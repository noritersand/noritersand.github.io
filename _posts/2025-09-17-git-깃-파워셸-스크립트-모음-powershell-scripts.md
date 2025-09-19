---
layout: post
date: 2025-09-17 16:15:33 +0900
title: '[Git] 깃 파워셸 스크립트 모음'
categories:
  - git
tags:
  - git
  - powershell
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}


## 파워셸 스크립트: 모든 리모트 추적 브랜치를 로컬 브랜치로 생성하는 스크립트

```bash
# 현재 로컬 브랜치 목록 가져오기
$localBranches = git branch --list | ForEach-Object { $_ -replace '^\*?\s+', '' }

# 모든 리모트 추적 브랜치 목록 가져오기
$remoteBranches = git branch -r | ForEach-Object { $_.Trim() } | Where-Object { $_ -notlike '*/HEAD*' }

# 각 리모트 브랜치에 대해 처리
foreach ($remoteBranch in $remoteBranches) {
    # 'origin/' 접두어 제거하여 로컬 브랜치 이름 생성
    $localBranch = $remoteBranch -replace '^[^/]+/', ''
    
    # 이미 로컬 브랜치가 존재하는지 확인하고, 로컬 브랜치 생성 및 리모트 브랜치로 추적 설정
    if ($localBranches -notcontains $localBranch) {
        Write-Host "Creating local branch: $localBranch from $remoteBranch"
        git branch $localBranch $remoteBranch
    }
    else {
        Write-Host "Branch already exists: $localBranch"
    }
}

Write-Host "Done creating local branches from remote tracking branches."
```


## 파워셸 스크립트: 한 달 이상 지난 머지된 리모트 브랜치 목록 보기

- 버전: 2.1
- 💾 파일 이름은 `cleanup-merged-branches-multi-repo.ps1`으로 저장할 것
- ⚠️ 이 스크립트는 로컬 깃 저장소 경로의 바로 상위 경로에서 실행해야 함
- Claude(Opus 4.1) 시켜서 개선했더니 이렇게 길어짐...

```bash
param(
    [int]$Days = 30,
    [boolean]$DryRun = $true,
    [string]$Path = ".",
    [string]$DefaultBranch = "main",
    [string[]]$ExcludeBranches = @(),
    [int]$TimeoutSeconds = 30,
    [boolean]$StrictMode = $false,  # 더 엄격한 머지 체크
    [switch]$Help  # 도움말 표시
)

# 도움말 표시
if ($Help) {
    Write-Host "=== Git 브랜치 정리 도구 v2.1 - 도움말 ===" -ForegroundColor Cyan
    Write-Host ""
    Write-Host "설명:" -ForegroundColor Yellow
    Write-Host "  여러 Git 저장소에서 머지된 지 오래된 원격 브랜치를 찾아 정리하는 도구"
    Write-Host ""
    
    Write-Host "기본 사용법:" -ForegroundColor Yellow
    Write-Host "  .\cleanup-merged-branches-multi-repo.ps1        # 기본 설정으로 실행 (30일, DryRun)"
    Write-Host "  .\cleanup-merged-branches-multi-repo.ps1 -Help  # 이 도움말 표시"
    Write-Host ""
    
    Write-Host "주요 옵션:" -ForegroundColor Yellow
    Write-Host "  -Days <숫자>                              # 기준 일수 (기본값: 30)"
    Write-Host "  -DryRun <`$true|`$false>                  # 미리보기 모드 (기본값: `$true)"
    Write-Host "  -Path <경로>                              # 검색할 폴더 경로 (기본값: 현재 폴더)"
    Write-Host "  -DefaultBranch <브랜치명>                 # 기본 브랜치명 (기본값: main)"
    Write-Host "  -ExcludeBranches <브랜치패턴들>           # 제외할 브랜치 패턴 배열"
    Write-Host "  -TimeoutSeconds <숫자>                    # 네트워크 타임아웃 (기본값: 30)"
    Write-Host "  -StrictMode <`$true|`$false>              # 엄격한 머지 체크 (기본값: `$false)"
    Write-Host ""
    
    Write-Host "사용 예시:" -ForegroundColor Green
    Write-Host ""
    Write-Host "  # 60일 이상 된 브랜치 확인" -ForegroundColor Cyan
    Write-Host "  .\cleanup-merged-branches-multi-repo.ps1 -Days 60"
    Write-Host ""
    Write-Host "  # 특정 경로의 하위 저장소들 확인" -ForegroundColor Cyan
    Write-Host "  .\cleanup-merged-branches-multi-repo.ps1 -Path `"C:\MyProjects`""
    Write-Host ""
    Write-Host "  # 특정 브랜치 패턴 제외 (와일드카드 지원)" -ForegroundColor Cyan
    Write-Host "  .\cleanup-merged-branches-multi-repo.ps1 -ExcludeBranches @('release*','hotfix*','feature/important*')"
    Write-Host ""
    Write-Host "  # 엄격한 체크 모드 (더 정확하지만 느림)" -ForegroundColor Cyan
    Write-Host "  .\cleanup-merged-branches-multi-repo.ps1 -StrictMode `$true"
    Write-Host ""
    Write-Host "  # 네트워크가 느린 환경용 타임아웃 증가" -ForegroundColor Cyan
    Write-Host "  .\cleanup-merged-branches-multi-repo.ps1 -TimeoutSeconds 60"
    Write-Host ""
    Write-Host "  # 실제 삭제 실행 (위험! 신중히 사용)" -ForegroundColor Red
    Write-Host "  .\cleanup-merged-branches-multi-repo.ps1 -DryRun `$false" -ForegroundColor Red
    Write-Host ""
    
    Write-Host "고급 조합 예시:" -ForegroundColor Green
    Write-Host ""
    Write-Host "  # 90일 이상, 엄격 모드, 특정 브랜치 제외" -ForegroundColor Cyan
    Write-Host "  .\cleanup-merged-branches-multi-repo.ps1 -Days 90 -StrictMode `$true -ExcludeBranches @('release*','hotfix*')"
    Write-Host ""
    Write-Host "  # 프로덕션 환경용 안전한 설정" -ForegroundColor Cyan
    Write-Host "  .\cleanup-merged-branches-multi-repo.ps1 -Days 180 -StrictMode `$true -TimeoutSeconds 60 -DryRun `$true"
    Write-Host ""
    
    Write-Host "주의사항:" -ForegroundColor Red
    Write-Host "  • 항상 -DryRun `$true로 먼저 테스트할 것"
    Write-Host "  • 중요한 브랜치는 -ExcludeBranches에 추가할 것"
    Write-Host "  • -StrictMode는 더 정확하지만 실행 시간이 오래 걸림"
    Write-Host "  • 실제 삭제는 되돌릴 수 없으니 신중히 할 것"
    Write-Host "  • 네트워크 상태가 불안정하면 -TimeoutSeconds 값 증가"
    Write-Host "  • main과 master 브랜치는 자동으로 보호됨"
    Write-Host ""
    
    Write-Host "출력 파일:" -ForegroundColor Yellow
    Write-Host "  • CSV 리포트: git-cleanup-report-YYYYMMDD-HHMMSS.csv"
    Write-Host "  • 요약 리포트: git-cleanup-summary-YYYYMMDD-HHMMSS.txt"
    Write-Host "  • 실행 로그: git-cleanup-YYYYMMDD-HHMMSS.log"
    Write-Host ""
    
    exit 0
}

# 로그 파일 시작
$logPath = "git-cleanup-$(Get-Date -Format 'yyyyMMdd-HHmmss').log"
Start-Transcript -Path $logPath -Append

# 에러 처리를 위한 함수들 - 보안 개선
function Test-GitCommand {
    param([string[]]$Arguments)
    try {
        # & 연산자로 안전하게 실행
        $result = & git $Arguments 2>&1
        $success = $LASTEXITCODE -eq 0
        
        # 에러 출력과 정상 출력 분리
        if ($result -is [System.Management.Automation.ErrorRecord[]]) {
            return @{ 
                Success = $false
                Output = $null
                ExitCode = $LASTEXITCODE
                Error = ($result | Out-String)
            }
        }
        
        return @{ 
            Success = $success
            Output = $result
            ExitCode = $LASTEXITCODE 
        }
    } catch {
        return @{ 
            Success = $false
            Output = $null
            ExitCode = -1
            Error = $_.Exception.Message 
        }
    }
}

function Test-BranchMerged {
    param(
        [string]$BranchName,
        [string]$MainBranch,
        [boolean]$StrictMode = $false
    )
    
    # 브랜치명 이스케이프 처리
    $safeBranchName = $BranchName -replace '([\[\]{}()*+?.\\^$|#])', '\$1'
    
    if ($StrictMode) {
        # 더 엄격한 체크: merge-base 사용
        $mergeBaseResult = Test-GitCommand @("merge-base", "--is-ancestor", "origin/$BranchName", "origin/$MainBranch")
        return $mergeBaseResult.Success
    } else {
        # 기본 체크: merged 옵션 사용
        $mergedResult = Test-GitCommand @("branch", "-r", "--merged", "origin/$MainBranch")
        if (-not $mergedResult.Success) {
            return $false
        }
        
        # 개선된 문자열 매칭
        $targetBranch = "origin/$BranchName"
        $normalizedOutput = $mergedResult.Output -split "`r?`n" | ForEach-Object { $_.Trim() }
        return $normalizedOutput -contains $targetBranch
    }
}

function Get-BranchLastActivity {
    param([string]$BranchName)
    
    # 마지막 커밋 날짜
    $lastCommitResult = Test-GitCommand @("log", "-1", "--format=%ci", "origin/$BranchName")
    if (-not $lastCommitResult.Success) {
        return $null
    }
    
    $lastCommitDate = $null
    try {
        # 다양한 날짜 형식 시도
        $dateString = $lastCommitResult.Output | Select-Object -First 1
        
        # ISO 8601 형식 시도
        try {
            $lastCommitDate = [DateTime]::ParseExact($dateString, "yyyy-MM-dd HH:mm:ss zzz", [System.Globalization.CultureInfo]::InvariantCulture)
        } catch {
            # 일반 Parse 시도
            $lastCommitDate = [DateTime]::Parse($dateString, [System.Globalization.CultureInfo]::InvariantCulture)
        }
    } catch {
        Write-Host "      ⚠️  날짜 파싱 실패: $BranchName ($dateString)" -ForegroundColor Yellow
        return $null
    }
    
    # 머지 커밋 찾기 - 안전한 방식으로 개선
    $mergeDate = $null
    
    # 브랜치명을 이스케이프 처리
    $escapedBranchName = $BranchName -replace '([\[\]{}()*+?.\\^$|#])', '\$1'
    
    # 여러 패턴으로 머지 커밋 검색
    $mergeSearches = @(
        @("log", "--merges", "--format=%ci|%s", "--grep=$BranchName", "-1"),
        @("log", "--merges", "--format=%ci|%s", "--grep=Merge.*$escapedBranchName", "-1"),
        @("log", "--merges", "--format=%ci|%s", "--grep=$escapedBranchName.*into", "-1")
    )
    
    foreach ($searchArgs in $mergeSearches) {
        $mergeResult = Test-GitCommand $searchArgs
        if ($mergeResult.Success -and $mergeResult.Output) {
            try {
                $mergeLine = $mergeResult.Output | Select-Object -First 1
                if ($mergeLine -match '^([^|]+)\|') {
                    $mergeDateStr = $matches[1]
                    $mergeDate = [DateTime]::Parse($mergeDateStr, [System.Globalization.CultureInfo]::InvariantCulture)
                    break
                }
            } catch {
                continue
            }
        }
    }
    
    return @{
        LastCommit = $lastCommitDate
        MergeDate = $mergeDate
        ActivityDate = if ($mergeDate) { $mergeDate } else { $lastCommitDate }
    }
}

function Get-SafeBranchName {
    param([string]$BranchName)
    
    # 브랜치명의 특수문자 이스케이프
    return $BranchName -replace '(["\$`])', '`$1'
}

# 메인 로직 시작
$cutoffDate = (Get-Date).AddDays(-$Days)
$totalBranchesFound = 0
$repositories = @()
$errors = @()

Write-Host "=== 개선된 Git 브랜치 정리 도구 v2.1 ===" -ForegroundColor Cyan
Write-Host "검색 경로: $((Get-Item $Path).FullName)" -ForegroundColor Gray
Write-Host "기준 날짜: $($cutoffDate.ToString('yyyy-MM-dd')) ($Days일 전)" -ForegroundColor Gray
if ($ExcludeBranches.Count -gt 0) {
    Write-Host "제외 브랜치: $($ExcludeBranches -join ', ')" -ForegroundColor Yellow
}
if ($StrictMode) {
    Write-Host "엄격 모드: 활성화" -ForegroundColor Magenta
}
Write-Host "로그 파일: $logPath" -ForegroundColor Gray
Write-Host ""

$originalLocation = Get-Location

try {
    $directories = Get-ChildItem -Path $Path -Directory
    $totalDirs = $directories.Count
    $currentDir = 0

    foreach ($dir in $directories) {
        $currentDir++
        
        $gitPath = Join-Path $dir.FullName ".git"
        if (-not (Test-Path $gitPath)) {
            continue
        }
        
        Write-Host "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━" -ForegroundColor DarkGray
        Write-Host "📁 저장소 [$currentDir/$totalDirs]: $($dir.Name)" -ForegroundColor Green
        
        Set-Location $dir.FullName
        
        # Git 저장소 상태 확인
        $statusResult = Test-GitCommand @("status", "--porcelain")
        if (-not $statusResult.Success) {
            Write-Host "   ❌ Git 저장소가 아니거나 손상됨" -ForegroundColor Red
            $errors += "[$($dir.Name)] Git 상태 확인 실패"
            continue
        }
        
        # 원격 정보 업데이트 (타임아웃 적용)
        Write-Host "   🔄 원격 정보 업데이트 중..." -ForegroundColor Gray
        $job = Start-Job -ScriptBlock { 
            Set-Location $using:dir.FullName
            git fetch --prune --quiet 2>&1 
            return $LASTEXITCODE
        }
        
        $completed = Wait-Job $job -Timeout $TimeoutSeconds
        if ($completed) {
            $fetchResult = Receive-Job $job
            Stop-Job $job -PassThru | Remove-Job
            if ($fetchResult -ne 0) {
                Write-Host "   ⚠️  원격 저장소 업데이트 실패 - 로컬 정보로 진행" -ForegroundColor Yellow
                $errors += "[$($dir.Name)] 원격 업데이트 실패"
            }
        } else {
            Stop-Job $job -PassThru | Remove-Job
            Write-Host "   ⚠️  원격 업데이트 시간초과 - 로컬 정보로 진행" -ForegroundColor Yellow
            $errors += "[$($dir.Name)] 원격 업데이트 시간초과"
        }
        
        # 기본 브랜치 확인
        $remoteBranchesResult = Test-GitCommand @("branch", "-r")
        if (-not $remoteBranchesResult.Success) {
            Write-Host "   ❌ 원격 브랜치 목록 조회 실패" -ForegroundColor Red
            $errors += "[$($dir.Name)] 원격 브랜치 조회 실패"
            continue
        }
        
        # 기본 브랜치 확인 (개선된 문자열 검사)
        $remoteBranches = $remoteBranchesResult.Output
        $normalizedBranches = $remoteBranches | ForEach-Object { $_.Trim() }
        
        $hasOriginMain = $normalizedBranches -contains 'origin/main'
        $hasOriginMaster = $normalizedBranches -contains 'origin/master'
        
        $mainBranch = if ($hasOriginMain) { "main" } 
                     elseif ($hasOriginMaster) { "master" }
                     else { $DefaultBranch }
        
        # 메인 브랜치 존재 확인
        $mainBranchCheck = Test-GitCommand @("show-ref", "--verify", "refs/remotes/origin/$mainBranch")
        if (-not $mainBranchCheck.Success) {
            Write-Host "   ⚠️  기본 브랜치 '$mainBranch'를 찾을 수 없음" -ForegroundColor Yellow
            $errors += "[$($dir.Name)] 기본 브랜치 '$mainBranch' 없음"
            continue
        }
        
        Write-Host "   📋 기본 브랜치: $mainBranch" -ForegroundColor Cyan
        
        $branchesToDelete = @()
        $skippedBranches = @()
        
        # 사용자 제외 브랜치만 적용
        $userExcludes = $ExcludeBranches
        
        # 원격 브랜치 목록 가져오기 (개선된 파싱)
        $allRemoteBranches = $normalizedBranches | 
            ForEach-Object { 
                # HEAD -> origin/xxx 같은 심볼릭 참조 제외
                if ($_ -notmatch '->' -and $_ -match '^origin/(.+)$') {
                    $branchName = $matches[1]
                    # HEAD 제외
                    if ($branchName -ne 'HEAD') {
                        $branchName
                    }
                }
            } |
            Where-Object { $_ }  # null 값 제거
        
        Write-Host "   🔍 총 $($allRemoteBranches.Count)개 원격 브랜치 발견" -ForegroundColor Gray
        
        # 진행률 표시를 위한 변수
        $progress = 0
        $totalBranches = $allRemoteBranches.Count
        
        foreach ($branchName in $allRemoteBranches) {
            $progress++
            
            # 진행률 표시 (10개 이상일 때만)
            if ($totalBranches -ge 10) {
                Write-Progress -Activity "브랜치 검사 중 - $($dir.Name)" `
                              -Status "$branchName" `
                              -PercentComplete (($progress / $totalBranches) * 100) `
                              -CurrentOperation "$progress / $totalBranches"
            }
            
            # 기본 브랜치와 main/master는 무조건 제외 (건너뛴 브랜치로 카운트하지 않음)
            if ($branchName -eq $mainBranch -or $branchName -eq 'main' -or $branchName -eq 'master') {
                continue
            }
            
            # 사용자 제외 브랜치 확인
            $shouldExclude = $false
            foreach ($exclude in $userExcludes) {
                if ($branchName -like $exclude) {
                    $shouldExclude = $true
                    $skippedBranches += "$branchName (제외 패턴: $exclude)"
                    break
                }
            }
            if ($shouldExclude) { continue }
            
            # 머지 여부 확인
            if (-not (Test-BranchMerged $branchName $mainBranch $StrictMode)) {
                continue
            }
            
            # 브랜치 활동 정보 가져오기
            $activityInfo = Get-BranchLastActivity $branchName
            if (-not $activityInfo -or -not $activityInfo.ActivityDate) {
                $skippedBranches += "$branchName (날짜 정보 없음)"
                continue
            }
            
            $daysSinceActivity = (Get-Date) - $activityInfo.ActivityDate
            
            if ($daysSinceActivity.TotalDays -gt $Days) {
                $branchInfo = [PSCustomObject]@{
                    Branch = $branchName
                    LastCommit = if ($activityInfo.LastCommit) { $activityInfo.LastCommit.ToString('yyyy-MM-dd HH:mm') } else { "N/A" }
                    MergeDate = if ($activityInfo.MergeDate) { $activityInfo.MergeDate.ToString('yyyy-MM-dd HH:mm') } else { "추정됨" }
                    ActivityDate = $activityInfo.ActivityDate.ToString('yyyy-MM-dd HH:mm')
                    DaysOld = [int]$daysSinceActivity.TotalDays
                    MergeDetected = ($activityInfo.MergeDate -ne $null)
                }
                $branchesToDelete += $branchInfo
            }
        }
        
        # 진행률 완료
        if ($totalBranches -ge 10) {
            Write-Progress -Activity "브랜치 검사 중 - $($dir.Name)" -Completed
        }
        
        # 결과 출력
        if ($branchesToDelete.Count -gt 0) {
            Write-Host "   ⚠️  삭제 대상: $($branchesToDelete.Count)개 브랜치" -ForegroundColor Yellow
            foreach ($branch in $branchesToDelete | Sort-Object DaysOld -Descending) {
                $mergeIndicator = if ($branch.MergeDetected) { "✓" } else { "~" }
                Write-Host "      $mergeIndicator $($branch.Branch) ($($branch.DaysOld)일 전, $($branch.ActivityDate))" -ForegroundColor Red
            }
            
            $repoInfo = [PSCustomObject]@{
                Repository = $dir.Name
                Path = $dir.FullName
                MainBranch = $mainBranch
                Branches = $branchesToDelete
                SkippedCount = $skippedBranches.Count
            }
            $repositories += $repoInfo
            $totalBranchesFound += $branchesToDelete.Count
        } else {
            Write-Host "   ✅ 삭제 대상 브랜치 없음" -ForegroundColor Green
        }
        
        if ($skippedBranches.Count -gt 0) {
            Write-Host "   ℹ️  건너뛴 브랜치: $($skippedBranches.Count)개" -ForegroundColor Blue
            # 건너뛴 브랜치 목록 표시
            foreach ($skipped in $skippedBranches | Select-Object -First 5) {
                Write-Host "      - $skipped" -ForegroundColor DarkGray
            }
            if ($skippedBranches.Count -gt 5) {
                Write-Host "      ... 외 $($skippedBranches.Count - 5)개" -ForegroundColor DarkGray
            }
        }
    }
    
    Write-Host "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━" -ForegroundColor DarkGray
    Write-Host ""
    
    # 에러 요약
    if ($errors.Count -gt 0) {
        Write-Host "⚠️  발생한 문제들:" -ForegroundColor Yellow
        foreach ($error in $errors) {
            Write-Host "   - $error" -ForegroundColor Yellow
        }
        Write-Host ""
    }
    
    # 최종 요약
    if ($totalBranchesFound -gt 0) {
        Write-Host "📊 요약" -ForegroundColor Cyan
        Write-Host "   총 $($repositories.Count)개 저장소에서 $totalBranchesFound개 브랜치 발견" -ForegroundColor Yellow
        
        # 상위 5개 오래된 브랜치 표시 (버그 수정)
        $topOldBranches = $repositories | ForEach-Object { 
            $repo = $_
            $repo.Branches | ForEach-Object { 
                $_ | Add-Member -NotePropertyName "Repository" -NotePropertyValue $repo.Repository -PassThru 
            }
        } | Sort-Object DaysOld -Descending | Select-Object -First 5
        
        if ($topOldBranches.Count -gt 0) {
            Write-Host "`n🏆 가장 오래된 브랜치들:" -ForegroundColor Magenta
            foreach ($branch in $topOldBranches) {
                Write-Host "   $($branch.Repository)/$($branch.Branch) - $($branch.DaysOld)일" -ForegroundColor Red
            }
        }
        
        Write-Host ""
        
        if (-not $DryRun) {
            Write-Host "⚠️  경고: 실제 삭제 모드가 활성화되어 있습니다!" -ForegroundColor Red
            Write-Host "이 작업은 되돌릴 수 없습니다." -ForegroundColor Red
            Write-Host ""
            
            # 삭제할 브랜치 목록 표시
            Write-Host "삭제 예정 브랜치:" -ForegroundColor Yellow
            $repositories | ForEach-Object {
                Write-Host "  [$($_.Repository)]" -ForegroundColor Cyan
                $_.Branches | ForEach-Object {
                    Write-Host "    - $($_.Branch)" -ForegroundColor Gray
                }
            }
            Write-Host ""
            
            # 더 안전한 확인 프로세스
            $confirmCode = Get-Random -Minimum 1000 -Maximum 9999
            Write-Host "확인 코드: $confirmCode" -ForegroundColor Yellow
            $userInput = Read-Host "위 $totalBranchesFound 개의 브랜치를 삭제하려면 확인 코드를 입력하세요"
            
            if ($userInput -eq $confirmCode.ToString()) {
                foreach ($repo in $repositories) {
                    Write-Host "`n🗑️  저장소: $($repo.Repository)" -ForegroundColor Yellow
                    Set-Location $repo.Path
                    
                    $successCount = 0
                    $failCount = 0
                    
                    foreach ($branch in $repo.Branches) {
                        Write-Host "   삭제 중: $($branch.Branch)" -ForegroundColor Red
                        
                        # 브랜치명 이스케이프 처리
                        $safeBranchName = Get-SafeBranchName $branch.Branch
                        $deleteResult = Test-GitCommand @("push", "origin", "--delete", "$safeBranchName")
                        
                        if ($deleteResult.Success) {
                            Write-Host "     ✅ 삭제 완료" -ForegroundColor Green
                            $successCount++
                        } else {
                            $errorMsg = if ($deleteResult.Error) { $deleteResult.Error } else { "알 수 없는 오류" }
                            Write-Host "     ❌ 삭제 실패: $errorMsg" -ForegroundColor Red
                            $failCount++
                        }
                    }
                    
                    Write-Host "   결과: 성공 $successCount, 실패 $failCount" -ForegroundColor Cyan
                }
                Write-Host "`n✅ 모든 작업 완료!" -ForegroundColor Green
            } else {
                Write-Host "❌ 확인 코드가 일치하지 않습니다. 삭제 취소됨" -ForegroundColor Yellow
            }
        } else {
            Write-Host "💡 팁: 실제 삭제를 원하면 -DryRun `$false 옵션을 사용하세요" -ForegroundColor Cyan
        }
    } else {
        Write-Host "✅ 모든 저장소가 깨끗합니다 - 삭제할 브랜치가 없습니다" -ForegroundColor Green
    }
    
} finally {
    Set-Location $originalLocation
}

# 상세 리포트 생성
if ($repositories.Count -gt 0) {
    Write-Host ""
    $exportReport = Read-Host "상세 CSV 리포트를 생성하시겠습니까? (y/n)"
    if ($exportReport -eq 'y') {
        $reportPath = "git-cleanup-report-$(Get-Date -Format 'yyyyMMdd-HHmmss').csv"
        $reportData = @()
        
        foreach ($repo in $repositories) {
            foreach ($branch in $repo.Branches) {
                $reportData += [PSCustomObject]@{
                    Repository = $repo.Repository
                    Branch = $branch.Branch
                    LastCommit = $branch.LastCommit
                    MergeDate = $branch.MergeDate
                    ActivityDate = $branch.ActivityDate
                    DaysOld = $branch.DaysOld
                    MergeDetected = $branch.MergeDetected
                    MainBranch = $repo.MainBranch
                }
            }
        }
        
        $reportData | Export-Csv -Path $reportPath -NoTypeInformation -Encoding UTF8
        Write-Host "📄 상세 리포트 저장완료: $reportPath" -ForegroundColor Green
        
        # 요약 통계도 함께 저장
        $summaryPath = "git-cleanup-summary-$(Get-Date -Format 'yyyyMMdd-HHmmss').txt"
        $summary = @"
Git 브랜치 정리 리포트 - $(Get-Date)
==========================================

총 검사 저장소: $($repositories.Count)개
총 발견 브랜치: $totalBranchesFound개
기준 날짜: $($cutoffDate.ToString('yyyy-MM-dd'))
엄격 모드: $(if($StrictMode){'활성화'}else{'비활성화'})

저장소별 요약:
$($repositories | ForEach-Object { "- $($_.Repository): $($_.Branches.Count)개 브랜치" } | Out-String)

발생한 오류: $($errors.Count)개
$($errors | ForEach-Object { "- $_" } | Out-String)
"@
        
        $summary | Out-File -FilePath $summaryPath -Encoding UTF8
        Write-Host "📄 요약 리포트 저장완료: $summaryPath" -ForegroundColor Green
    }
}

# 로그 종료
Stop-Transcript

Write-Host "`n📝 실행 로그가 저장되었습니다: $logPath" -ForegroundColor Gray
```
