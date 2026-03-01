---
layout: post
date: 2025-04-14 12:08:05 +0900
title: '[React] SWR'
categories:
  - react
tags:
  - react
  - javascript
  - swr
  - vercel
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [React Hooks for Data Fetching – SWR](https://swr.vercel.app/)

#### 테스트 환경 정보

- x.x.x


## 개요

> SWR은 먼저 캐시(stale)로부터 데이터를 반환한 후, fetch 요청(revalidate)을 하고, 최종적으로 최신화된 데이터를 가져오는 전략입니다.

SWR은 Next.js 팀이 만든 리액트 훅으로 `useEffect` 훅을 대체하여 사용할 수 있다.

'SWR'이란 이름은 [HTTP RFC 5861](https://datatracker.ietf.org/doc/html/rfc5861)의 HTTP 캐시 무효 전략인 `stale-while-revalidate`에서 유래되었다고 한다. 

```jsx
import React, {useEffect} from 'react';
import useSWR from 'swr';

// 모의 fetcher 함수: 2초 후에 데이터 반환
const fetcher = async (key) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({
        id: 1,
        name: 'Test User',
        email: 'test@example.com',
      });
    }, 2000);
  });
};

export default function UseSwrTest() {
  const { data, error, isLoading } = useSWR('my-awesome-key', fetcher);
  
  return (
    <section>
      <h2>useSWR Test</h2>
      {
        error ? (
          <div>failed to load</div>
        ) : isLoading ? (
          <div>loading...</div>
        ) : (
          <>
            <p>hello, {data?.name}!</p>
            <p>Email: {data?.email}</p>
          </>
        )
      }
    </section>
  );
}
```


## useSWR()

```
const { data, error, isLoading, isValidating, mutate } = useSWR(key, fetcher, options)
```

- `key`: 요청을 구분하는 고유한 키 값
- `fetcher`: 실제 데이터를 가져오는 비동기 콜백 함수. 이 함수의 반환 타입은 `Promise`여야 하고, 첫 번째 인자에 `key`로 지정한 값이 그대로 전달된다(공식 가이드에선 `url`이라 표시함). `fetcher`는 선택 사항이라 생략할 수 있는데, 이 경우 `fallback`이나 `fallbackData`로 초기 데이터를 설정하고, `mutate()` 함수를 통해 데이터를 변경해야 한다.
- `options`: 선택 사항. 생략하면 기본 설정으로 작동함.
  - `suspense`: 기본값 `false`.
  - `fetcher(args)`: 
  - `revalidateIfStale`: 캐시된 데이터가 '오래되었다(stale)'고 판단될 때 백그라운드에서 데이터를 자동으로 재검증할지 여부. 기본값은 `true`.
  - `revalidateOnMount`: 컴포넌트가 마운트될 때마다 데이터를 재검증할지 여부. 기본값은 `true`.
  - `revalidateOnFocus`: 브라우저 창에 다시 포커스가 생겼을 때 데이터를 재검증할지 여부. 기본값은 `true`.
  - `revalidateOnReconnect`: 인터넷이 오프라인이었다가 다시 연결되었을 때 데이터를 재검증할지 여부. 기본값은 `true`.
  - `refreshInterval`: 밀리초로 지정하는 데이터 갱신 간격. 숫자 혹은 숫자를 반환하는 함수를 지정할 수 있다. `0`으로 설정하거나 생략하면 비활성화 된다.
  - `refreshWhenHidden`: 기본값 `false`.
  - `refreshWhenOffline`: 기본값 `false`.
  - `shouldRetryOnError`: 기본값 `true`.
  - `dedupingInterval`: 중복 요청을 방지하는 최소 시간. 이 시간 내에 발생한 요청을 막고 캐시 데이터를 반환한다. 단위는 밀리초, `0`으로 설정하면 비활성화된다. 생략했을 때의 기본값은 `2000`(2초).
  - `focusThrottleInterval`: 기본값 `5000`.
  - `loadingTimeout`: 기본값 `3000`.
  - `errorRetryInterval`: 기본값 `5000`.
  - `errorRetryCount`: 
  - `fallback`: 
  - `fallbackData`: 
  - `keepPreviousData`: 기본값 `false`.
  - `onLoadingSlow(key, config)`: 
  - `onSuccess(data, key, config)`: 
  - `onError(err, key, config)`: 
  - `onErrorRetry(err, key, config, revalidate, revalidateOps)`: 
  - `onDiscarded(key)`: 
  - `compare(a, b)`: 
  - `isPaused()`: 
  - `use`: 

반환값은:

- `data`: 
- `error`: 
- `isLoading`: 
- `isValidating`: 
- `mutate`: `fetcher`를 사용하지 않고 수동으로 데이터를 변경하는 함수.


### 키 외에 다른 인수에도 의존하는 경우

SWR은 키를 기준으로 데이터 고유성을 판단한다. 그런데 만약 키 말고도 호출에 필요한 인수가 더 있다면, 아래처럼 배열을 사용하여 키에 해당 인수를 포함해야 한다:

```js
// 코드 출처: https://swr.vercel.app/docs/arguments#multiple-arguments

// ❌ 잘못된 방법 - token이 바뀌어도 같은 캐시 사용
useSWR('my-awesome-key', key => fetchWithToken(key, token))

// ✅ 올바른 방법 - token마다 별도 캐시
const {data: user} = useSWR(['my-awesome-key', token], ([key, token]) => fetchWithToken(key, token))
```

예를 들어, 아래처럼 특정 상위 코드와 연관된 코드 항목들을 검색한다고 할 때, 배열로 키를 정의하면 전달한 상위 코드마다 캐시가 따로 적용된다:

```js
function fetchCodeItems(upperCode) {
  return fetch(`/system/code-items?code=${upperCode}`);
}

// ... 생략

const {data: response} = useSWR([`my-awesome-key`, upperCode], ([key, upperCode]) => fetchCodeItems(upperCode));
```


## useEffect와 비교

**TODO**
