---
layout: post
date: 2024-03-03 17:25:15 +0900
title: '[React] Next.js 기본'
categories:
  - react
tags:
  - nextjs
  - react
  - javascript
  - basics
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Next.js \| Docs](https://nextjs.org/docs)

#### 테스트 환경 정보

- Next.js 14.x.x
- 타입스크립트를 사용한다 가정하고 작성함


## 개요

Next.js(이하 넥스트) 사용방법 간단 정리.


## 스캐폴딩

기본:

```bash
npx create-next-app@latest PROJECT_NAME
```

공식 넥스트 템플릿을 기반으로 프로젝트를 설정하려면:

```bash
npx create-next-app@latest nextjs-dashboard --use-yarn --example "https://github.com/vercel/next-learn/tree/main/dashboard/starter-example"
```

이런식으로 저장소의 URL을 `--example` 옵션값으로 지정하면 됨

### 초기 파일 구조

```
root
 ├─.next
 ├─node_modules
 ├─public
 └─src
```

- `.next`: 빌드에 필요한 파일과 빌드 결과가 생성되는 경로
- `node_modules`: node 패키지 로컬 경로.
- `public`: 정적 파일이 위치하는 경로.
- `src`: 소스 코드 루트 경로. 이 아래에 App router에선 `app` 디렉터리가, Pages router에선 `page` 디렉터리가 있어야 한다. (`src` 디렉터리를 생략하도록 설정할 수 있음)


## 앱 라우터 App Router

App Router(이하 앱 라우터)는 넥스트 13.4 버전에서 도입된 새로운 라우팅 방식이다. 이전 버전의 라우팅은 Pages Router(이하 페이지 라우터)라고 부른다.

### 앱 라우터 vs 페이지 라우터

- [App router | Next.js](https://nextjs.org/docs/app/building-your-application/routing)
- [Pages router | Next.js](https://nextjs.org/docs/pages/building-your-application/routing)

개발자는 앱 라우터와 페이지 라우터 중 하나를 선택할 수 있다. 둘의 차이를 요약하면 다음과 같다:

앱 라우터:

- `src/app` 디렉터리에서 라우팅을 구성한다.
- `src/app/page.jsx`가 시작 파일이다.
- 파일 시스템 기반 라우팅을 사용한다. URL이 `A/B`인 경우 `src/app/A/B/pages.jsx` 파일을 찾는다.
- 기본적으로 서버 컴포넌트로 작동한다.

페이지 라우터:

- `src/pages` 디렉터리에서 라우팅을 구성한다.
- `src/pages/index.jsx`가 시작 파일이다.
- 파일 이름 기반 라우팅을 사용한다. URL이 `A/B`인 경우 `src/pages/A/B.jsx` 파일을 찾는다.
- 기본적으로 클라이언트 컴포넌트로 작동한다.

어느 한 쪽을 선택한다고 해서 반대쪽을 아예 사용 못하는 건 아니다. 구버전으로 개발된 소스에 `src/app` 디렉터리를 만들기만 하면 앱 라우터를 사용할 수 있다. 이 경우 앱 라우터가 우선권을 가져가긴 하지만 동일한 URL을 사용할 순 없다(*The App Router takes priority over the Pages Router. Routes across directories should not resolve to the same URL path and will cause a build-time error to prevent a conflict*). 실제로 `src/app/pages.jsx`와 `src/pages/index.jsx`가 동시에 존재하면 아래와 같은 에러가 발생한다:

```
Conflicting app and page file was found, please remove the conflicting files to continue:
  "src\pages\index.tsx" - "src\app\page.tsx"
```

이것은 `src\pages\a\b.tsx` 파일과 `src\app\a\b\page.tsx` 파일이 동시에 존재해도 마찬가지다. 따라서 앱 라우터와 페이지 라우터 둘 다 쓰고 싶다면 루트`/` URL을 포함한 URL에 각각의 라우터를 적용한 분리 작업이 필요하다.

### 'use client'

앱 라우터는 기본적으로 모든 컴포넌트가 서버 사이드 렌더링(SSR) 되는 서버 컴포넌트로 취급된다. 서버 컴포넌트는 다음과 같은 특징이 있다:

- 상태(state)와 라이프사이클 메서드(`useState`, `useReduce`)를 사용할 수 없다.
- 브라우저 API(`window`, `document`, `location`, `console` 등)에 접근할 수 없다.
- 이벤트 핸들러(`onClick`, `onSubmit`, ...)를 사용할 수 없다.
- `useEffect`, `useLayoutEffect` 같은 일부 훅을 사용할 수 없다.
- `useRef`로 DOM에 접근할 수 없다.

서버 컴포넌트는 서버에서 실행되기 때문에 발생하는 제약 사항들이다. 만약 이를 회피(?)하고 싶다면 `'use client'` 디렉티브를 파일 최상단에 추가하여, 클라이언트(브라우저)에서 작동해야 함을 넥스트에 알려줘야 한다:

```jsx
'use client';

import { useState } from 'react';

export default function Counter() {
  const path = usePathname(); // 서버 컴포넌트에서 사용 불가능한 코드
  console.log('path:', path)

  const [count, setCount] = useState(0); // 서버 컴포넌트에서 사용 불가능한 코드

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

이렇게 하면 클라이언트 컴포넌트가 된다. 하지만 넥스트는 하이브리드 방식으로 렌더링하며, **이 코드 또한 초기 렌더링은 서버에서 처리된다**. 이 과정에서 서버에서 실행할 수 없는 코드는 넥스트가 알아서 분할(*Code Splitting*)한다. 그 다음 사용자 상호작용과 업데이트를 클라이언트 측에서 처리한다고 이해하면 된다.

#### 브라우저 전용 API에 대한 접근

`'use client'`로 클라이언트 컴포넌트로 선언했다 하더라도 앱 라우터 방식에선 여전히 제약이 존재하는데, 대표적으로 `window` 같은 브라우저 전용 API를 쓸 수 없다는 점이다

```jsx
'use client';

export default function Page() {
  console.log('window.name', window.name);
  return <h2>about-us</h2>;
}
// Unhandled Runtime Error
// Error: window is not defined
```

이를 해결하려면 `useEffect` 훅으로 컴포넌트 렌더링 후 접근하도록 코드를 수정한다:

```jsx
'use client';

import { useEffect } from 'react';

export default function Page() {
  useEffect(() => {
    console.log('window.name', window.name);
  }, []);
  return <h2>about-us</h2>;
}
```

#### Hydration

리액트에서 *Hydration*이란 서버에서 렌더링된 초기 HTML과 클라이언트에서 실행되는 자바스크립트가 연결되는 과정을 말한다. 이는 브라우저에서 실행될 자바스크립트 코드가 없다면 hydration이 필요하지 않다는 것을 의미한다.

앞서 언급한 서버 컴포넌트의 제약 사항을 살펴보면, 모두 브라우저에서의 사용자 상호작용과 관련된 내용들이다. 이를 통해 서버 컴포넌트에는 브라우저에서 실행될 자바스크립트가 포함되지 않는다는 것을 알 수 있다. 따라서 Hydration이 필요한 컴포넌트는 오직 `'use client'` 디렉티브를 선언한 클라이언트 컴포넌트라는 결론이 나온다.

ℹ️ 클라이언트 사이드 렌더링(CSR)에서 Hydration은 필요하지 않지만, 서버 사이드 렌더링(SSR)의 **클라이언트 컴포넌트**에는 Hydration이 필요하다.


## 프레임워크 설정

### `import {inter} from '@/app/ui/fonts'`에서 `@`의 의미

`tsconfig.json` 파일의 `paths` 항목에서 정의한 루트 경로다:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

위 설정대로면 `@/app/ui/fonts` 파일의 실제 위치는 `root/src/app/ui/fonts.ts`다. 만약 실제 파일이 위치하는 경로가 `root/src/app`이 아니라 `root/app` 아래라면:

```json
"paths": {
  "@/*": ["./*"]
}
```

이렇게 바꿔야 함.

### next.config.js

- [Next.js | API Reference: next.config.js Options](https://nextjs.org/docs/pages/api-reference/next-config-js)
- [GitHub | next.js/packages/next/src/server/config-shared.ts at canary · vercel/next.js](https://github.com/vercel/next.js/blob/canary/packages/next/src/server/config-shared.ts)

next.config.js 혹은 next.config.mjs로 관리하는 Next.js의 빌드 설정 파일.

```js
const nextConfig = {
  reactStrictMode: true,
  output: undefined
};

export default nextConfig;
```

#### output

빌드 결과의 모양을 결정하는 옵션.

- `undefined`: 생략했을 때의 기본 값으로, Vercel 같은 플랫폼에 배포하는 용도이며 SSR, SSG를 사용하는 형태로 빌드된다. 이 방식은 빌드와 배포가 대충 자동으로 이뤄진다고 한다. Vercel 같은 경우 소스 파일만 배포하면 나머지는 자동화되어 있어서 `.next` 디렉터리를 따로 업로드할 필요 없다.
- `standalone`: 모든 필요한 종속성이 함께 번들링되는 방식. Docker 컨테이너에 배포할 때 사용한다. `.next/standalone`에 빌드 결과가 저장된다.
- `export`: 정적 사이트로 내보내기 위한 옵션으로 Node.js 환경이 아닐 때 사용한다. 따라서 서버 사이드 렌더링/로직을 사용할 수 없다. `out` 디렉터리에 빌드 결과가 저장된다.


## 환경 변수

[Next.js | Configuring: Environment Variables](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables)

넥스트는 `.env`로 시작하는 환경 변수 파일을 지원한다.

넥스트에서 환경 변수는 `.env`, `.env.production` 등의 파일에 작성한다. 이 변수는 빌드 시 `process.env`에 주입된다:

```bash
# .env
DB_HOST=localhost
DB_USER=myuser
DB_PASS=mypassword
```

```bash
> yarn dev
yarn run v1.22.22
$ next dev
   ▲ Next.js 14.1.4
   - Local:        http://localhost:3000
   - Environments: .env.local, .env.development, .env

 ✓ Ready in 3.2s
 ○ Compiling /_error ...
 ✓ Compiled /_error in 966ms (288 modules)
```

그리고 `process.env.[환경변수이름]`으로 접근한다:

```js
const db = {
  host: process.env.DB_HOST,
  username: process.env.DB_USER,
  password: process.env.DB_PASS,
};
```

환경 변수 파일로 인식하는 이름은 다음과 같다:

- `.env`: 개발 환경(공식 도움말에선 `NODE_ENV`로 표시함) 구분 없이 사용되는 기본 환경 변수 파일.
- `.env.development`: 개발 환경(`next dev`)일 때 불러오는 환경 변수 파일.
- `.env.production`: 프로덕션 환경(`next build`, `next start`)에 불러오는 환경 변수 파일이다.
- `.env.test`: 테스트(`next test`) 모드에서만 불러오는 환경 변수 파일.
- `.env.local`: 개발환경 구분 없이 사용되는 개발자 개인용 환경 변수 파일. 일반적으로 개인 API 키 같은 민감 정보를 설정하며, 버전 관리 대상에서 제외한다. 
- `.env.development.local` | `.env.production.local`: 각각 개발 환경이거나 프로덕션 환경일 때 불러오는 개인자 개인용 환경 변수 파일. `.env.local`과 달리 개발 환경 구분이 필요할 때 사용한다.

우선 순위가 높은 순으로 나열하면 다음과 같다:

- `.env.development.local`, `.env.production.local`
- `.env.local`
- `.env.development`, `.env.production`
- `.env`

만약 같은 이름의 환경 변수가 여러 파일에 존재하면, 불러온 파일 중 우선 순위가 가장 높은 파일의 환경 변수가 나머지를 덮어쓴다.

**TODO** 이 외에 `.env.dev`나 `.env.staging`처럼 별도의 환경 변수가 필요한 경우 추가 설정이 필요하다.

### 다른 환경 변수를 참조하기

달러`$`를 사용한다:

```bash
# .env
TWITTER_USER=nextjs
TWITTER_URL=https://twitter.com/$TWITTER_USER
```

```js
console.log(process.env.TWITTER_URL); // https://twitter.com/nextjs
```

### 브라우저에서 사용 가능한 환경 변수

환경 변수 이름이 `NEXT_PUBLIC_`로 시작하면 클라이언트 측 코드(=브라우저에서 실행될 코드)에 노출되는 환경 변수가 된다. 그렇지 않은 환경 변수는 서버 측 코드에서만 접근할 수 있다:

```bash
# 브라우저에서 접근 불가
ENV_VARIABLE="server_only_variable"

# 브라우저에서 접근 가능
NEXT_PUBLIC_ENV_VARIABLE="public_variable"
```


## 자주 쓰는 API

### useRouter

라우팅을 프로그래밍 방식으로 변경(programmatically change)하는 클라이언트 훅.

```tsx
'use client'
 
import { useRouter } from 'next/navigation'
 
export default function Page() {
  const router = useRouter()
 
  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  )
}
```

### usePathname

현재 URL의 경로 정보를 가져오는 클라이언트 훅. 

```tsx
'use client'
 
import { usePathname } from 'next/navigation'
 
export default function ExampleClientComponent() {
  const pathname = usePathname()
  return <p>Current pathname: {pathname}</p>
}
```

### useParams

동적 파라미터를 읽을 때 사용하는 클라이언트 훅

```tsx
'use client'
 
import { useParams } from 'next/navigation'
 
export default function ExampleClientComponent() {
  const params = useParams<{ tag: string; item: string }>()
 
  // Route -> /shop/[tag]/[item]
  // URL -> /shop/shoes/nike-air-max-97
  // `params` -> { tag: 'shoes', item: 'nike-air-max-97' }
  console.log(params)
 
  return <></>
}
```
