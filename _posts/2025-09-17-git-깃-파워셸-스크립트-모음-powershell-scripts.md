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


## 파워셸 스크립트: 한 달 이상 지난 머지된 리모트 브랜치 목록 보기

- ⚠️ 이 스크립트는 실제 로컬 깃 저장소 경로의 바로 상위 경로에서 실행해야 함
- 💾 파일 이름은 `cleanup-merged-branches-multi-repo.ps1`으로 저장할 것

```bash
# 여러 Git 저장소에서 머지된 지 30일 넘은 브랜치 찾기
param(
    [int]$Days = 30,
    [boolean]$DryRun = $true,
    [string]$Path = ".",
    [string]$DefaultBranch = "main",  # master을 사용하는 경우 변경 가능
    [string[]]$ExcludeBranches = @()    # 제외할 브랜치 목록
)

$cutoffDate = (Get-Date).AddDays(-$Days)
$totalBranchesFound = 0
$repositories = @()

Write-Host "=== 여러 저장소에서 $Days 일 이상 된 머지 브랜치 검색 ===" -ForegroundColor Cyan
Write-Host "검색 경로: $((Get-Item $Path).FullName)" -ForegroundColor Gray
if ($ExcludeBranches.Count -gt 0) {
    Write-Host "제외 브랜치: $($ExcludeBranches -join ', ')" -ForegroundColor Yellow
}
Write-Host ""

# 현재 디렉터리 저장 (스크립트 종료 시 복귀용)
$originalLocation = Get-Location

try {
    # 하위 디렉터리 검색
    $directories = Get-ChildItem -Path $Path -Directory

    foreach ($dir in $directories) {
        # .git 폴더가 있는지 확인
        $gitPath = Join-Path $dir.FullName ".git"
        if (Test-Path $gitPath) {
            Write-Host "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━" -ForegroundColor DarkGray
            Write-Host "📁 저장소: $($dir.Name)" -ForegroundColor Green
            
            # 해당 저장소로 이동
            Set-Location $dir.FullName
            
            # 원격 정보 업데이트
            Write-Host "   원격 정보 업데이트 중..." -ForegroundColor Gray
            git fetch --prune --quiet 2>$null
            
            # 기본 브랜치 확인 (master 또는 main)
            $remoteBranches = git branch -r 2>$null
            $mainBranch = if ($remoteBranches -match "origin/main") { "main" } 
                         elseif ($remoteBranches -match "origin/master") { "master" }
                         else { $DefaultBranch }
            
            $branchesToDelete = @()
            
            # 기본 제외 브랜치 + 사용자 지정 제외 브랜치를 위한 패턴 생성
            $defaultExcludes = @("main", "master", "develop", "HEAD")
            $allExcludes = $defaultExcludes + $ExcludeBranches
            $excludePattern = ($allExcludes | ForEach-Object { "origin/$_" }) -join '|'
            
            # 머지된 원격 브랜치 목록 가져오기
            $mergedBranches = git branch -r --merged origin/$mainBranch 2>$null | 
                Where-Object { $_ -notmatch $excludePattern } |
                ForEach-Object { $_.Trim() }
            
            if ($mergedBranches.Count -eq 0) {
                Write-Host "   ✓ 삭제 대상 브랜치 없음" -ForegroundColor Gray
                continue
            }
            
            foreach ($branch in $mergedBranches) {
                # origin/ 제거
                $branchName = $branch -replace '^origin/', ''
                
                # 사용자 지정 제외 브랜치 재확인 (와일드카드 지원)
                $shouldExclude = $false
                foreach ($exclude in $ExcludeBranches) {
                    if ($branchName -like $exclude) {
                        $shouldExclude = $true
                        break
                    }
                }
                
                if ($shouldExclude) {
                    continue
                }
                
                # 브랜치의 마지막 커밋 날짜 가져오기
                $lastCommitDate = git log -1 --format=%ci $branch 2>$null
                if ($lastCommitDate) {
                    $commitDate = [DateTime]::Parse($lastCommitDate)
                    
                    # 머지 커밋 찾기
                    $mergeCommit = git log origin/$mainBranch --merges --grep="$branchName" --format="%ci" -1 2>$null
                    
                    if ($mergeCommit) {
                        $mergeDate = [DateTime]::Parse($mergeCommit)
                        $daysSinceMerge = (Get-Date) - $mergeDate
                        
                        if ($daysSinceMerge.Days -gt $Days) {
                            $branchInfo = [PSCustomObject]@{
                                Branch = $branchName
                                MergeDate = $mergeDate.ToString('yyyy-MM-dd')
                                DaysOld = $daysSinceMerge.Days
                            }
                            $branchesToDelete += $branchInfo
                        }
                    }
                    # 머지 커밋을 못 찾은 경우 마지막 커밋 날짜로 판단
                    elseif ($commitDate -lt $cutoffDate) {
                        $daysSinceCommit = (Get-Date) - $commitDate
                        $branchInfo = [PSCustomObject]@{
                            Branch = $branchName
                            MergeDate = $commitDate.ToString('yyyy-MM-dd')
                            DaysOld = $daysSinceCommit.Days
                        }
                        $branchesToDelete += $branchInfo
                    }
                }
            }
            
            if ($branchesToDelete.Count -gt 0) {
                Write-Host "   ⚠️  삭제 대상: $($branchesToDelete.Count)개 브랜치" -ForegroundColor Yellow
                foreach ($branch in $branchesToDelete) {
                    Write-Host "      - $($branch.Branch) ($($branch.DaysOld)일 전)" -ForegroundColor Red
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
                Write-Host "   ✓ 삭제 대상 브랜치 없음" -ForegroundColor Gray
            }
        }
    }
    
    Write-Host "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━" -ForegroundColor DarkGray
    Write-Host ""
    
    # 요약
    if ($totalBranchesFound -gt 0) {
        Write-Host "📊 요약" -ForegroundColor Cyan
        Write-Host "   총 $($repositories.Count)개 저장소에서 $totalBranchesFound개 브랜치 발견" -ForegroundColor Yellow
        Write-Host ""
        
        if (-not $DryRun) {
            Write-Host "⚠️  경고: 실제 삭제 모드" -ForegroundColor Red
            $confirm = Read-Host "정말로 모든 브랜치를 삭제할까? (yes/no)"
            
            if ($confirm -eq 'yes') {
                foreach ($repo in $repositories) {
                    Write-Host "`n🗑️  저장소: $($repo.Repository)" -ForegroundColor Yellow
                    Set-Location $repo.Path
                    
                    foreach ($branch in $repo.Branches) {
                        Write-Host "   삭제 중: $($branch.Branch)" -ForegroundColor Red
                        git push origin --delete $branch.Branch 2>&1 | Out-Null
                        if ($?) {
                            Write-Host "     ✓ 삭제 완료" -ForegroundColor Green
                        } else {
                            Write-Host "     ✗ 삭제 실패" -ForegroundColor Red
                        }
                    }
                }
                Write-Host "`n✅ 모든 작업 완료!" -ForegroundColor Green
            } else {
                Write-Host "❌ 취소됨" -ForegroundColor Yellow
            }
        } else {
            Write-Host "ℹ  도움말"
            Write-Host ""
            Write-Host "# 60일 이상 된 브랜치 확인" -ForegroundColor Cyan
            Write-Host ".\cleanup-merged-branches-multi-repo.ps1 -Days 60" -ForegroundColor Gray
            Write-Host ""
            Write-Host "# 특정 경로의 하위 저장소 확인" -ForegroundColor Cyan
            Write-Host ".\cleanup-merged-branches-multi-repo.ps1 -Path `"C:\MyProjects`"" -ForegroundColor Gray
            Write-Host ""
            Write-Host "# 특정 브랜치 제외 (와일드카드 지원)" -ForegroundColor Cyan
            Write-Host ".\cleanup-merged-branches-multi-repo.ps1 -ExcludeBranches 'release*', 'hotfix*'" -ForegroundColor Gray
            Write-Host ""
            Write-Host "# 실제 삭제 실행 (주의!)" -ForegroundColor Cyan
            Write-Host ".\cleanup-merged-branches-multi-repo.ps1 -DryRun `$false" -ForegroundColor Gray
            Write-Host ""
            Write-Host "# main 브랜치만 확인하려면" -ForegroundColor Cyan
            Write-Host ".\cleanup-merged-branches-multi-repo.ps1 -DefaultBranch main" -ForegroundColor Gray
        }
    } else {
        Write-Host "✅ 모든 저장소가 깨끗함 - 삭제할 브랜치 없음" -ForegroundColor Green
    }
    
} finally {
    # 원래 위치로 복귀
    Set-Location $originalLocation
}

# 상세 리포트 생성 (선택사항)
if ($repositories.Count -gt 0) {
    Write-Host ""
    $exportReport = Read-Host "CSV 리포트를 생성할까요? (y/n)"
    if ($exportReport -eq 'y') {
        $reportPath = "merged-branches-report-$(Get-Date -Format 'yyyyMMdd-HHmmss').csv"
        $reportData = @()
        
        foreach ($repo in $repositories) {
            foreach ($branch in $repo.Branches) {
                $reportData += [PSCustomObject]@{
                    Repository = $repo.Repository
                    Branch = $branch.Branch
                    MergeDate = $branch.MergeDate
                    DaysOld = $branch.DaysOld
                    MainBranch = $repo.MainBranch
                }
            }
        }
        
        $reportData | Export-Csv -Path $reportPath -NoTypeInformation -Encoding UTF8
        Write-Host "📄 리포트 저장: $reportPath" -ForegroundColor Green
    }
}
```


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
