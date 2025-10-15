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

```powershell
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

- 버전: 4.8
- 💾 파일 이름: `cleanup-merged-branches-multi-repo.ps1`
- ⚠️ 이 스크립트는 로컬 깃 저장소 경로의 바로 상위 경로에서 실행해야 함. 아니면 `-Path`로 그 경로를 지정하던지...

```powershell
param(
    [int]$Days = 30,
    [string]$Path = ".",
    [string]$DefaultBranch = "main", # DefaultBranch를 "main"으로 기본값 설정
    [string[]]$ExcludeBranches = @(),
    [int]$TimeoutSeconds = 600,
    [string]$RemoteName = "origin",
    [switch]$Help
)

# 도움말 표시
if ($Help) {
    Write-Host "=== Git 브랜치 정리 대상 식별 도구 v4.8 - 도움말 ===" -ForegroundColor Cyan
    Write-Host ""
    Write-Host "설명:" -ForegroundColor Yellow
    Write-Host "  지정된 경로의 모든 하위 폴더를 재귀적으로 검색하여, 머지된 지 오래된"
    Write-Host "  원격 브랜치를 찾아 목록을 출력합니다. 브랜치를 삭제하지 않습니다."
    Write-Host ""
    
    Write-Host "기본 사용법:" -ForegroundColor Yellow
    Write-Host "  .\cleanup-merged-branches.ps1           # 기본 설정으로 실행 (현재 폴더와 하위)"
    Write-Host "  .\cleanup-merged-branches.ps1 -Help     # 이 도움말 표시"
    Write-Host "  .\cleanup-merged-branches.ps1 -Path 'C:\my-projects'  # 특정 경로의 모든 하위 저장소 검색" -ForegroundColor Gray
    Write-Host ""
    
    Write-Host "주요 옵션:" -ForegroundColor Yellow
    Write-Host "  -Days <숫자>                    # 기준 일수 (기본값: 30)"
    Write-Host "  -Path <경로>                    # 검색할 폴더 경로 (기본값: 현재 폴더)"
    Write-Host "  -DefaultBranch <브랜치명>       # 기본 브랜치명 (기본값: main)"
    Write-Host "  -RemoteName <원격저장소명>      # 원격 저장소 이름 (기본값: origin)"
    Write-Host "  -ExcludeBranches <패턴들>       # 제외할 브랜치 패턴 배열"
    Write-Host "  -TimeoutSeconds <숫자>          # 네트워크 타임아웃 (기본값: 600초)"
    Write-Host ""
    
    Write-Host "주의사항:" -ForegroundColor Red
    Write-Host "  • 이 스크립트는 삭제 대상을 식별만 하며, 실제 삭제는 직접 수행해야 합니다."
    Write-Host "  • git branch --merged 기준으로 머지 여부를 판단함"
    Write-Host ""
    
    exit 0
}

# 스크립트 시작 시간 기록
$scriptStartTime = Get-Date
$timestamp = Get-Date -Format 'yyyyMMdd-HHmmss'

# Git 명령 실행 함수 (타임아웃 및 작업 디렉터리 처리 포함)
function Invoke-GitCommand {
    param(
        [string[]]$Arguments,
        [int]$TimeoutSec = 600,
        [string]$WorkingDirectory = $null
    )
    
    $process = New-Object System.Diagnostics.Process
    $process.StartInfo.FileName = "git.exe"
    $process.StartInfo.Arguments = $Arguments -join " "
    $process.StartInfo.UseShellExecute = $false
    $process.StartInfo.RedirectStandardOutput = $true
    $process.StartInfo.RedirectStandardError = $true
    $process.StartInfo.CreateNoWindow = $true
    
    if ($WorkingDirectory) {
        $process.StartInfo.WorkingDirectory = $WorkingDirectory
    }
    
    try {
        $process.Start() | Out-Null
        $result = $process.WaitForExit($TimeoutSec * 1000)
        
        if (-not $result) {
            $process.Kill()
            return @{
                Success = $false
                Output = "Timeout ($TimeoutSec seconds) exceeded."
                ExitCode = -1
            }
        }
        
        $output = $process.StandardOutput.ReadToEnd() + $process.StandardError.ReadToEnd()
        
        return @{
            Success = ($process.ExitCode -eq 0)
            Output = $output.Trim()
            ExitCode = $process.ExitCode
        }
    } catch {
        return @{
            Success = $false
            Output = $_.Exception.Message
            ExitCode = -1
        }
    } finally {
        if ($process -ne $null) {
            $process.Dispose()
        }
    }
}

# 브랜치 머지 상태 확인
function Test-BranchMerged {
    param(
        [string]$BranchName,
        [string]$MainBranch,
        [string]$RemoteName,
        [string]$WorkingDirectory
    )
    
    $fullBranchRef = "$RemoteName/$BranchName"
    $fullMainRef = "$RemoteName/$MainBranch"
    
    $merged = Invoke-GitCommand @("branch", "-r", "--merged", $fullMainRef) -WorkingDirectory $WorkingDirectory
    if ($merged.Success) {
        $mergedBranches = $merged.Output -split "`r?`n" | ForEach-Object { $_.Trim() }
        return ($mergedBranches -contains $fullBranchRef)
    }
    
    return $false
}

# 브랜치 활동 정보 조회 (단순화된 버전)
function Get-BranchActivityInfo {
    param(
        [string]$BranchName,
        [string]$RemoteName,
        [string]$WorkingDirectory
    )
    
    $fullBranchRef = "$RemoteName/$BranchName"
    
    # 마지막 커밋 날짜만 확인
    $lastCommitLogResult = Invoke-GitCommand @(
        "log",
        "--format=%ci",
        "--max-count=1",
        $fullBranchRef
    ) -WorkingDirectory $WorkingDirectory
    
    $lastCommitDate = $null
    if ($lastCommitLogResult.Success) {
        try {
            $lastCommitDate = [DateTime]::Parse($lastCommitLogResult.Output)
        } catch {
        }
    }
    
    if (-not $lastCommitDate) {
        return $null
    }
    
    return @{
        LastCommit = $lastCommitDate
        ActivityDate = $lastCommitDate
    }
}

# 메인 처리 로직
$thresholdDate = (Get-Date).AddDays(-$Days)
$totalBranchesFound = 0
$repositories = @()
$errors = @()

Write-Host "=== Git 브랜치 정리 대상 식별 도구 v4.8 ===" -ForegroundColor Cyan
Write-Host "검색 경로: $((Get-Item $Path).FullName)" -ForegroundColor Gray
Write-Host "기준 날짜: $($thresholdDate.ToString('yyyy-MM-dd')) ($($Days)일 전)" -ForegroundColor Gray
Write-Host "원격 저장소: $RemoteName" -ForegroundColor Gray
if ($ExcludeBranches.Count -gt 0) {
    Write-Host "제외 패턴: $($ExcludeBranches -join ', ')" -ForegroundColor Yellow
}
Write-Host ""

$originalLocation = Get-Location

try {
    # 모든 하위 디렉터리를 재귀적으로 검색하여 Git 저장소 찾기
    $directories = Get-ChildItem -Path $Path -Recurse -Directory -ErrorAction SilentlyContinue | Where-Object {
        Test-Path (Join-Path $_.FullName ".git")
    }
    
    $totalDirs = $directories.Count
    
    if ($totalDirs -eq 0) {
        Write-Host "⚠️  Git 저장소를 찾을 수 없습니다. 경로를 다시 확인해주세요." -ForegroundColor Yellow
        exit 0
    }
    
    Write-Host "📊 총 $($totalDirs)개의 Git 저장소 발견" -ForegroundColor Green
    Write-Host ""
    
    $currentDir = 0
    
    # 각 저장소 처리
    foreach ($dir in $directories) {
        $currentDir++
        
        Write-Host "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━" -ForegroundColor DarkGray
        Write-Host "📁 저장소 [$currentDir/$totalDirs]: $($dir.Name)" -ForegroundColor Green
        
        # 디버깅 정보 출력
        Write-Host "   ℹ️ Git 명령 실행 디렉터리: $($dir.FullName)" -ForegroundColor DarkGray
        
        # Git 상태 확인
        $status = Invoke-GitCommand @("status", "--porcelain") -TimeoutSec 10 -WorkingDirectory $dir.FullName
        if (-not $status.Success) {
            Write-Host "   ❌ Git 저장소가 아니거나 손상됨: $($status.Output)" -ForegroundColor Red
            $errors += "[$($dir.Name)] Git 상태 확인 실패: $($status.Output)"
            continue
        }
        
        # 원격 정보 업데이트
        Write-Host "   🔄 원격 정보 업데이트 중..." -ForegroundColor Gray
        $fetch = Invoke-GitCommand @("fetch", "--prune", "--quiet") -WorkingDirectory $dir.FullName
        if (-not $fetch.Success) {
            Write-Host "   ⚠️  원격 업데이트 실패 - 로컬 정보로 진행" -ForegroundColor Yellow
            $errors += "[$($dir.Name)] 원격 업데이트 실패"
        }
        
        # 기본 브랜치 결정
        # 사용자가 지정한 DefaultBranch를 우선적으로 사용
        $mainBranch = $DefaultBranch
        
        Write-Host "   📋 기본 브랜치: $mainBranch" -ForegroundColor Cyan
        
        $remoteRefs = Invoke-GitCommand @("ls-remote", "--heads", $RemoteName) -WorkingDirectory $dir.FullName
        if (-not $remoteRefs.Success) {
            Write-Host "   ❌ 원격 브랜치 조회 실패" -ForegroundColor Red
            $errors += "[$($dir.Name)] 원격 브랜치 조회 실패"
            continue
        }
        
        $remoteBranches = $remoteRefs.Output -split "`r?`n" | ForEach-Object {
            if ($_ -match 'refs/heads/(.+)$') { $matches[1] }
        } | Where-Object { $_ }
        
        Write-Host "   🔍 $($remoteBranches.Count)개 원격 브랜치 검사 중..." -ForegroundColor Gray
        
        $branchesToDelete = @()
        $skippedBranches = @()
        
        # 모든 원격 브랜치 검사
        foreach ($branchName in $remoteBranches) {
            # 기본 브랜치와 동일하면 보호
            if ($branchName -eq $mainBranch) {
                continue
            }
            
            # 사용자 제외 패턴 확인
            $shouldExclude = $false
            foreach ($pattern in $ExcludeBranches) {
                if ($branchName -like $pattern) {
                    $shouldExclude = $true
                    $skippedBranches += "$branchName (패턴: $pattern)"
                    break
                }
            }
            if ($shouldExclude) { continue }
            
            # 머지 상태 확인
            if (-not (Test-BranchMerged -BranchName $branchName `
                                        -MainBranch $mainBranch `
                                        -RemoteName $RemoteName `
                                        -WorkingDirectory $dir.FullName)) {
                continue
            }
            
            # 활동 정보 조회
            $activityInfo = Get-BranchActivityInfo -BranchName $branchName `
                                                   -RemoteName $RemoteName `
                                                   -WorkingDirectory $dir.FullName
            
            if (-not $activityInfo -or -not $activityInfo.ActivityDate) {
                $skippedBranches += "$branchName (날짜 정보 없음)"
                continue
            }
            
            $daysSinceActivity = (Get-Date) - $activityInfo.ActivityDate
            
            if ($daysSinceActivity.TotalDays -gt $Days) {
                $branchInfo = [PSCustomObject]@{
                    Branch = $branchName
                    LastCommit = $activityInfo.LastCommit
                    ActivityDate = $activityInfo.ActivityDate
                    DaysOld = [int]$daysSinceActivity.TotalDays
                }
                $branchesToDelete += $branchInfo
            }
        }
        
        # 결과 출력
        if ($branchesToDelete.Count -gt 0) {
            Write-Host "   ⚠️  삭제 대상: $($branchesToDelete.Count)개 브랜치" -ForegroundColor Yellow
            foreach ($branch in $branchesToDelete | Sort-Object Branch) { # 브랜치 이름으로 정렬
                $dateStr = if ($branch.ActivityDate) { $branch.ActivityDate.ToString('yyyy-MM-dd') } else { "N/A" }
                Write-Host "      ✅ $($branch.Branch) ($($branch.DaysOld)일 전, $dateStr)" -ForegroundColor Red
            }
            
            $repoInfo = [PSCustomObject]@{
                Repository = $dir.Name
                Path = $dir.FullName
                MainBranch = $mainBranch
                Branches = $branchesToDelete
            }
            $repositories += $repoInfo
            $totalBranchesFound += $branchesToDelete.Count
        } else {
            Write-Host "   ✅ 삭제 대상 브랜치 없음" -ForegroundColor Green
        }
        
        if ($skippedBranches.Count -gt 0) {
            Write-Host "   ℹ️  건너뛴 브랜치: $($skippedBranches.Count)개" -ForegroundColor Blue
            if ($skippedBranches.Count -le 3) {
                foreach ($skipped in $skippedBranches) {
                    Write-Host "      - $skipped" -ForegroundColor DarkGray
                }
            }
        }
    }
    
    Write-Host "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━" -ForegroundColor DarkGray
    Write-Host ""
    
    # 오류 요약
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
        Write-Host "   총 $($repositories.Count)개 저장소에서 $($totalBranchesFound)개 브랜치 발견" -ForegroundColor Yellow
        
        # 실행 시간
        $executionTime = (Get-Date) - $scriptStartTime
        Write-Host "   실행 시간: $([int]$executionTime.TotalSeconds)초" -ForegroundColor Gray
        Write-Host ""
        
        # 가장 오래된 브랜치 표시
        $topOldBranches = $repositories | ForEach-Object {
            $repo = $_
            $repo.Branches | ForEach-Object {
                $_ | Add-Member -NotePropertyName "Repository" -NotePropertyValue $repo.Repository -PassThru
            }
        } | Sort-Object DaysOld -Descending | Select-Object -First 5
        
        if ($topOldBranches.Count -gt 0) {
            Write-Host "🏆 가장 오래된 브랜치들:" -ForegroundColor Magenta
            foreach ($branch in $topOldBranches) {
                Write-Host "   $($branch.Repository)/$($branch.Branch) - $($branch.DaysOld)일" -ForegroundColor Red
            }
            Write-Host ""
        }
        
        Write-Host "💡 팁: 위 목록을 확인하고, 필요시 다음 명령어로 수동 삭제하세요." -ForegroundColor Cyan
        Write-Host "  git push <remote_name> --delete <branch_name>" -ForegroundColor Gray
    } else {
        Write-Host "✅ 모든 저장소가 깨끗합니다 - 삭제할 브랜치가 없습니다" -ForegroundColor Green
    }
    
} finally {
    Set-Location $originalLocation
}

Write-Host "`n스크립트 실행 완료" -ForegroundColor Gray
```
