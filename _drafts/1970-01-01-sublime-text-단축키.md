---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'sublime text: 단축키'
categories:
  - devtool
tags:
  - devtool
  - sublimetext
---

Build 3126 기준

## 커서/포커스 이동

- `alt + -`: Jump Back. 이전 포커스로 이동. 이클립스의 `alt + ←`와 비슷
- `alt + shift +  + `: Jump Forward. 다음 포커스로 이동. 이클립스의 `alt + →`와 비슷
- `f12`: Goto Definition. 현재 커서가 있는 함수나 메서드의 선언부로 이동. 제한적으로 작동하는 기능(Syntax 보기 형태와 문서의 내용이 알맞아야 함)이다.
- `shift + f12`: Goto Reference. 함수나 메서드를 사용(참조)하고 있는 라인으로 이동.
- `ctrl + 0`: 사이드 바로 포커스 이동

## 편집

- `ctrl + j`: 라인 단위 병합
- `F9`: 대소문자 무시하고 라인 단위 알파벳 오름차순 정렬
- `ctrl + F9`: 라인 단위 알파벳 오름차순 정렬
- `ctrl + m`: 괄호
- `ctrl + shift + m`: 괄호

## 멀티 커서

- `ctrl + d`: 선택한 단어 기준 멀티 커서
- `ctrl + alt + 방향키 위/아래`: 위나 아래로 멀티 커서

## overlay

- `` ctrl + ` ``: 콘솔창
- `ctrl + shift + p`: 서브라임 빠른 명령어 탐색창 열기
- `ctrl + p`: (폴더 열기 이후)빠른 파일 탐색창 열기
- `ctrl + r`: 함수 단위 탐색창 열기
- `ctrl + g`: 라인 이동
- `ctrl + ;`: 키워드 탐색창 열기

## 확장 메뉴 조합

- `ctrl + k`: 확장 메뉴
- `ctrl + k`, `ctrl + u`: 대문자 변환
- `ctrl + k`, `ctrl + l`: 소문자 변환
- `ctrl + k`, `ctrl + b`: 사이드 바 토글
