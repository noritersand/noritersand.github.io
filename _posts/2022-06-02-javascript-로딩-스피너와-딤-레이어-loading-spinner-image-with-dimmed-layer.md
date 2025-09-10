---
layout: post
date: 2022-06-02 22:01:33 +0900
title: '[JavaScript] 로딩 스피너와 딤 레이어'
categories:
  - javascript
tags:
  - css
  - code-snippet
  - dimmed-layer
  - loading
  - z-index
  - background-image
---

* Kramdown table of contents
{:toc .toc}


## 개요

로딩 스피너와 딤 레이어를 설정하는 방법을 작성한 글.

ℹ️ 로딩 스피너(spinner)란 로딩 중임을 표현하기 위한 뱅뱅 도는 어떤 이미지를 의미한다. `spinner images`로 검색해 볼 것.


## 코드

```css
#loading {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #ffffff36;
  z-index: 9999;
  background-image: url("/image/loading.gif");
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-position: center;
}
```

```html
<div id="loading"></div>
```

```js
function onLoad() {
  let $ele = document.querySelector('#loading');
  $ele.style = 'display: block';
}

function onLoaded() {
  let $ele = document.querySelector('#loading');
  $ele.style = 'display: none';
}
```

살짝 흐릿한 레이어가 전면을 덮어서 로딩이 완료될 때까지 클릭 등을 막는 용도로 쓴다. 

여기에 대충 적당히 움직이는 이미지를 얹으면 끗.


## 개선 버전: 다중 비동기 작업 지원

카운터 방식으로 동시 진행 중인 모든 작업을 추적하고 마지막 작업 완료 시에만 로딩이 종료된다.

```js
/**
 * 동시 다중 비동기 작업을 지원하는 로딩 오버레이 (카운터 기반)
 */
class LoadingOverlay {
  constructor() {
    this.overlay = null;
    this.loadingCount = 0;  // 현재 진행 중인 작업 수
    this.previousActiveElement = null;
    this.currentMessage = '처리 중입니다...';
    this.createOverlay();
    this.initEventListeners();
  }

  createOverlay() {
    this.overlay = document.createElement('div');
    this.overlay.style.cssText = `
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.5);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 9999;
      opacity: 0;
      visibility: hidden;
      transition: opacity 0.3s ease, visibility 0.3s ease;
    `;
    
    this.overlay.innerHTML = `
      <div style="
        background: white;
        padding: 2rem;
        border-radius: 8px;
        text-align: center;
        box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
        max-width: 300px;
        width: 90%;
      ">
        <div style="
          width: 40px;
          height: 40px;
          border: 3px solid #f3f3f3;
          border-top: 3px solid #007bff;
          border-radius: 50%;
          animation: spin 1s linear infinite;
          margin: 0 auto 1rem;
        "></div>
        <p style="color: #666; font-size: 1rem; margin: 0;" class="loading-text">
          처리 중입니다...
        </p>
        <div style="color: #999; font-size: 0.8rem; margin-top: 0.5rem;" class="loading-counter">
          진행 중: 1개
        </div>
      </div>
    `;

    // 스피너 애니메이션 추가
    const style = document.createElement('style');
    style.textContent = `
      @keyframes spin {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
      }
    `;
    document.head.appendChild(style);
    document.body.appendChild(this.overlay);
  }

  initEventListeners() {
    // 키보드 입력 차단
    document.addEventListener('keydown', this.blockInput.bind(this));
    
    // 오버레이 클릭 차단
    this.overlay.addEventListener('click', (e) => {
      e.stopPropagation();
      e.preventDefault();
    });
  }

  show(message = '처리 중입니다...') {
    this.loadingCount++;
    
    // 첫 번째 작업이 시작될 때만 포커스 저장
    if (this.loadingCount === 1) {
      this.previousActiveElement = document.activeElement;
      this.overlay.style.opacity = '1';
      this.overlay.style.visibility = 'visible';
      document.body.style.overflow = 'hidden';
    }

    // 메시지와 카운터 업데이트
    this.updateDisplay(message);
    
    console.log(`[LoadingOverlay] 작업 시작 (${this.loadingCount}개 진행 중): ${message}`);
  }

  hide() {
    if (this.loadingCount <= 0) {
      console.warn('[LoadingOverlay] hide() 호출되었지만 진행 중인 작업이 없습니다.');
      return;
    }

    this.loadingCount--;
    console.log(`[LoadingOverlay] 작업 완료 (${this.loadingCount}개 남음)`);

    // 모든 작업이 완료되었을 때만 오버레이 숨김
    if (this.loadingCount === 0) {
      this.overlay.style.opacity = '0';
      this.overlay.style.visibility = 'hidden';
      document.body.style.overflow = '';
      
      // 포커스 복원
      if (this.previousActiveElement && this.previousActiveElement.focus) {
        this.previousActiveElement.focus();
      }
      
      console.log('[LoadingOverlay] 모든 작업 완료, 오버레이 숨김');
    } else {
      // 아직 진행 중인 작업이 있으면 카운터만 업데이트
      this.updateDisplay();
    }
  }

  updateDisplay(newMessage = null) {
    if (newMessage) {
      this.currentMessage = newMessage;
    }

    const loadingText = this.overlay.querySelector('.loading-text');
    const loadingCounter = this.overlay.querySelector('.loading-counter');
    
    loadingText.textContent = this.currentMessage;
    loadingCounter.textContent = `진행 중: ${this.loadingCount}개`;
  }

  forceReset() {
    console.warn('[LoadingOverlay] 강제 리셋 실행');
    this.loadingCount = 0;
    this.overlay.style.opacity = '0';
    this.overlay.style.visibility = 'hidden';
    document.body.style.overflow = '';
  }

  blockInput(e) {
    if (this.loadingCount === 0) return;
    
    // F5, F12, ESC 등 일부 키는 허용
    const allowedKeys = ['F5', 'F12', 'Escape'];
    if (!allowedKeys.includes(e.key)) {
      e.preventDefault();
      e.stopPropagation();
    }
  }

  isLoading() {
    return this.loadingCount > 0;
  }

  getLoadingCount() {
    return this.loadingCount;
  }
}
```

```js
const loadingOverlay = new LoadingOverlay();
```

### 사용 패턴

#### 수동 관리

```js
try {
  loadingOverlay.show('작업1 시작');
  loadingOverlay.show('작업2 시작'); // 카운터: 2
  
  await task1();
  loadingOverlay.hide(); // 카운터: 1
  
  await task2();
  loadingOverlay.hide(); // 카운터: 0, 오버레이 숨김
} catch (error) {
  loadingOverlay.forceReset(); // 에러시 강제 리셋
}
```

#### 자동 관리

```js
// 자동으로 show/hide를 관리하는 래퍼 함수
async function withLoading(asyncFn, message = '처리 중입니다...') {
  try {
    loadingOverlay.show(message);
    return await asyncFn();
  } finally {
    loadingOverlay.hide();
  }
}
```

```js
// 단일 작업
await withLoading(fetchData, '데이터 로딩 중...');

// 동시 다중 작업
await Promise.all([
  withLoading(task1, '작업1 중...'),
  withLoading(task2, '작업2 중...'),
  withLoading(task3, '작업3 중...')
]);
```

동시 실행 예시:

```js
async function fetchUserData() {
  return withLoading(async () => {
    const response = await fetch('/api/users');
    return response.json();
  }, '사용자 데이터 로딩 중...');
}

async function uploadFile(file) {
  return withLoading(async () => {
    const formData = new FormData();
    formData.append('file', file);
    const response = await fetch('/api/upload', {
      method: 'POST',
      body: formData
    });
    return response.json();
  }, '파일 업로드 중...');
}

async function handleMultipleOperations() {
  // 이 세 작업이 동시에 실행되어도 오버레이가 올바르게 관리됨
  const [userData, fileResult, otherData] = await Promise.all([
    fetchUserData(),
    uploadFile(someFile),
    withLoading(() => fetch('/api/other').then(r => r.json()), '기타 데이터 로딩...')
  ]);
  
  return { userData, fileResult, otherData };
}
```

폼 제출 시 사용 예시:

```js
document.addEventListener('DOMContentLoaded', () => {
  const form = document.getElementById('myForm');
  if (form) {
    form.addEventListener('submit', async (e) => {
      e.preventDefault();
      
      try {
        await withLoading(async () => {
          // 실제 폼 제출 로직
          await new Promise(resolve => setTimeout(resolve, 2000));
        }, '폼 전송 중...');
        
        alert('제출 완료!');
      } catch (error) {
        alert('제출 실패: ' + error.message);
      }
    });
  }
});
```
