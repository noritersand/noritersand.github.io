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

{% raw %}

#### 참고 문서

- [Next.js \| Docs](https://nextjs.org/docs)

#### 테스트 환경 정보

- Next.js 14.x.x
- Next.js 15.x.x
- 타입스크립트를 사용한다 가정하고 작성함


## 개요

넥스트(Next.js) 사용방법 간단 정리.


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


## 페이지 라우터 Pages Router vs 앱 라우터 App Router

- [Pages router \| Next.js](https://nextjs.org/docs/pages/building-your-application/routing)
- [App router \| Next.js](https://nextjs.org/docs/app/building-your-application/routing)

앱 라우터(App router)는 넥스트 13.4 버전에서 도입된 새로운 라우팅 방식이다. 이전 버전의 라우팅은 페이지 라우터(Pages router)라고 부른다.

개발자는 페이지 라우터와 앱 라우터 중 하나를 선택할 수 있다. 둘의 차이를 요약하면 다음과 같다:

페이지 라우터:

- `src/pages` 디렉터리에서 라우팅을 구성한다.
- `src/pages/index.tsx`가 루트 경로 인덱스 파일이다.
- 파일 이름 기반 라우팅을 사용한다. URL이 `A/B`인 경우 `src/pages/A/B.tsx` 파일을 찾는다.
- 서버 컴포넌트 개념이 없다.
- `_app.tsx`는 모든 페이지를 감싸는 루트 컴포넌트이며 리액트 렌더링 계층에서 가장 상위에 위치한다.
- `_document.tsx`가 SSR 시점의 HTML 뼈대룰 작성하는 파일이다.
- 데이터 패칭(data fetching)으로 `getServerSideProps()`, `getStaticProps()`, `getInitialProps()` 등의 API를 사용한다.

앱 라우터:

- `src/app` 디렉터리에서 라우팅을 구성한다.
- `src/app/page.tsx`가 루트 경로 인덱스 파일이다.
- 파일 시스템 기반 라우팅을 사용한다. URL이 `A/B`인 경우 `src/app/A/B/page.tsx` 파일을 찾는다.
- 기본적으로 서버 컴포넌트로 작동한다.
- `layout.tsx`에서 반복되는 레이아웃이나 전역 스타일 등을 정의한다. (페이지 라우터의 `_app.tsx`와 `_document.tsx`를 합쳤다고 보면 됨)
- 데이터 패칭으로 `fetch`와 `async를` 통한 서버 컴포넌트 내 데이터 패칭, 또는 `use`를 활용하는 패턴이 있다.

어느 한 쪽을 선택한다고 해서 반대쪽을 아예 사용 못하는 건 아니다. 구버전으로 개발된 소스에 `src/app` 디렉터리를 만들기만 하면 앱 라우터를 사용할 수 있다. 

🚨 두 방식을 모두 사용할 경우 앱 라우터가 우선권을 가져가긴 하지만 동일한 URL을 사용할 순 없다(*The App Router takes priority over the Pages Router. Routes across directories should not resolve to the same URL path and will cause a build-time error to prevent a conflict*). 실제로 `src/app/page.tsx`와 `src/pages/index.tsx`가 동시에 존재하면 아래와 같은 에러가 발생한다:

```
Conflicting app and page file was found, please remove the conflicting files to continue:
  "src\pages\index.tsx" - "src\app\page.tsx"
```

이것은 `src/pages/A/B.tsx` 파일과 `src/app/A/B/page.tsx` 파일이 동시에 존재해도 마찬가지다.

### layout.tsx

앱 라우터에선 반복되는 레이아웃이나 전역 스타일을 `layout.tsx`에서 정의한다. `layout`이란 이름은 정해진 규칙이라 변경할 수 없으며, 확장자는 다음 넷 중 하나여야 한다: `.ts`, `.js`, `.tsx`, `.jsx`

루트 경로의 `layout.tsx`는 사이트 전체에 적용되는 공통 레이아웃 파일이다. 특정 경로 아래에서만 적용되는 레이아웃 파일을 별도로 추가할 수 있는데, 이를 하위 레이아웃 혹은 중첩 레이아웃(nested layout)이라 한다. 예를 들어 `src/app/A/B/layout.tsx` 파일은 `/A/B` 경로 아래의 페이지에만 적용된다. 

하위 레이아웃은 상위 레이아웃 내부에서 중첩 적용된다. 예를 들어 파일 구조가 아래와 같을 때:

- `src/app/layout.tsx`
- `src/app/A/layout.tsx`
- `src/app/A/B/layout.tsx`
- `src/app/A/B/page.tsx`

URL `/A/B`의 페이지를 보여줄 때는 `src/app/A/B/page.tsx`의 내용을 `src/app/A/B/layout.tsx`, `src/app/A/layout.tsx`, `src/app/layout.tsx` 순으로 감싸지는 식이다.

### 라우트 그룹 Route Groups

파일을 디렉터리로 분리하고 싶지만 URL에는 노출하고 싶지 않을 수도 있는데 이 때 사용하는 기능이다. 디렉터리 이름을 괄호`()`로 감싸면 된다. e.g., `(product)`, `(overview)`, ...

🚨 아래처럼 동일한 최종 경로(`/C`)를 차지하는 라우트 그룹은 넥스트가 어느 라우트를 렌더링해야 할지 판단할 수 없어 빌드 에러를 일으킨다:

- `src/app/(A)/C/page.tsx`
- `src/app/(B)/C/page.tsx`

ℹ️ 넥스트에서 '라우트'는 보통 URL 경로 또는 페이지 한 개를 의미한다.

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

### 브라우저 전용 API에 대한 접근

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

### Hydration

리액트에서 *Hydration*이란 서버에서 렌더링된 초기 HTML과 클라이언트에서 실행되는 자바스크립트가 연결되는 과정을 말한다. 이는 브라우저에서 실행될 자바스크립트 코드가 없다면 hydration이 필요하지 않다는 것을 의미한다.

앞서 언급한 서버 컴포넌트의 제약 사항을 살펴보면, 모두 브라우저에서의 사용자 상호작용과 관련된 내용들이다. 이를 통해 서버 컴포넌트에는 브라우저에서 실행될 자바스크립트가 포함되지 않는다는 것을 알 수 있다. 따라서 hydration이 필요한 컴포넌트는 오직 `'use client'` 디렉티브를 선언한 클라이언트 컴포넌트라는 말이 된다.

ℹ️ 클라이언트 사이드 렌더링(CSR)에서 hydration은 필요하지 않지만, 서버 사이드 렌더링(SSR)의 **클라이언트 컴포넌트**에는 hydration이 필요하다.

### 'use server'

[Directives: use server \| Next.js](https://nextjs.org/docs/app/api-reference/directives/use-server)

`'use server'` 지시어는 서버 측에서 실행될 함수나 파일을 지정한다. 지시어는 함수나 파일의 맨 위에 위치해야 한다.

ℹ️ 사실 이것은 리액트의 기능임:

- [Server Actions – React](https://react.dev/reference/rsc/server-actions)
- ['use server' directive – React](https://react.dev/reference/rsc/use-server)


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

- [Next.js \| API Reference: next.config.js Options](https://nextjs.org/docs/pages/api-reference/next-config-js)
- [GitHub \| next.js/packages/next/src/server/config-shared.ts at canary · vercel/next.js](https://github.com/vercel/next.js/blob/canary/packages/next/src/server/config-shared.ts)

next.config.js 혹은 next.config.mjs로 관리하는 Next.js의 빌드 설정 파일.

```js
const nextConfig = {
  reactStrictMode: true,
  output: undefined
};

export default nextConfig;
```

#### reactStrictMode

리액트의 Strict Mode를 활성화할지를 결정하는 프로퍼티.

#### output

빌드 결과의 모양을 결정하는 옵션.

```js
module.exports = {
  output: 'standalone',
}
```

- `undefined`: 생략했을 때의 기본 값으로, Vercel 같은 플랫폼에 배포하는 용도이며 SSR을 사용한다. Vercel 같은 경우 소스 파일만 배포하면 나머지는 자동화되어 있어서 `.next` 디렉터리를 따로 업로드할 필요 없다.
- `standalone`: 모든 필요한 종속성이 함께 번들링되는 방식. Docker 컨테이너에 배포할 때 사용한다. `.next/standalone`에 빌드 결과가 저장된다.
- `export`: 정적 사이트로 내보내기 위한 옵션으로 Node.js 환경이 아닐 때 사용한다. 따라서 서버 사이드 렌더링/로직을 사용할 수 없다. `out` 디렉터리에 빌드 결과가 저장된다.

⚠️ `export`는 보통 웹 서버를 통한 단순 배포 용도로 사용한다. 하지만 웹 서버는 `https://host/dashboard` 같은 URL을 이해하지 못하고 404로 응답하게 된다. 따라서 들어오는 요청의 path 값에 `.html` 붙여 파일을 찾도록 하는 추가 작업이 필요하다. 아래는 nginx일 경우의 설정이다:

```
location / {
    try_files $uri $uri/ $uri.html =404;
}
```

#### trailingSlash

넥스트는 기본적으로 URL을 리다이렉션할 때 트레일링 슬래시(URL 경로 바로 뒤에 `/`가 붙는 것)를 제거하는데, 이 값을 `true`로 설정하면 반대로 작동한다. 가령 `/about` 경로는 `/about/`으로 바뀐다.

`output: 'export'` 일 때 넥스트는 슬러그 이름으로 HTML 파일을 생성한다. 가령 `/about` 슬러그가 있으면 `/about.html` 파일이 생성되는 식이다. 하지만 `trailingSlash`가 `true`면 `/about.html` 대신 `/about/index.html`을 생성한다.


#### redirects

특정 요청을 다른 경로로 리다이렉션하는 옵션이다.

```js
module.exports = {
  async redirects() {
    return [
      {
        source: '/about',
        destination: '/',
        permanent: true,
      },
    ]
  },
}
```

- `source`: 리다이렉션 대상이 될 요청 경로 패턴
- `destination`: 보낼 주소
- `permanent`: 브라우저에 이 리다이렉션이 영구적인지 아닌지를 결정한다. `true`면 308 Permanent Redirect로 응답하고 `false`면 307 Temporary Redirect로 응답한다.

`source`와 `destination`에는 와일드카드`*`와 정규식을 이용한 패스 매칭(path matches)을 지원한다. 예를 들면 `source`에 `/blog/:slug*`를, `destination`에 `/news/:slug*`라고 작성하면, 요청 경로 중 `/blob`만 `/news`로 바뀌고 나머지는 그대로인 주소로 리다이렉션하게 된다. 

#### rewrites

요청 경로를 다른 경로에 매핑한다. 포워딩과 유사하지만 약간 다르며, 웹 서버에서 제공하는 rewrite와도 다르다. Next.js에선 들어오는 요청을 내부에서 다른 주소로 매핑하는데, 이 과정에서 클라이언트, 그러니까 브라우저는 URL 변경을 감지하지 못한다. 이를 *URL masking*이나 *URL hiding*이라 부른다. 클라이언트의 특정 요청을 Next.js 서버에서 감지하고 매핑된 다른 주소로 대신 요청을 보내 데이터를 받아오는 방식이라서 가능한 것. 이를 이용해서 외부에 노출되면 안되는 (API 키 같은) 중요한 값을 숨길 수 있다.

```js
module.exports = {
  async rewrites() {
    return [
      {
        source: '/about',
        destination: '/',
      },
    ]
  },
}
```

- `source`: rewrites할 요청 경로 패턴
- `destination`: 매핑할 주소

`redirects`와 마찬가지로 `source`와 `destination`에는 와일드카드`*`와 정규식을 이용한 패스 매칭(path matches)을 지원한다.

⚠️ `redirects`와 `rewrites`는 HTML 내보내기 방식(`next.config.js`의 `output` 옵션이 `export`일 때)에서 사용할 수 없음


## 환경 변수

[Next.js \| Configuring: Environment Variables](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables)

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


## 데이터 받아오기 Data Fetching

이 글에서 데이터 받아오기는 단순히 어딘가에 데이터를 요청하고 그걸 받아오는 것을 말하는 게 아니라, SSR 시점에 fetching을 완료하고 그걸 컴포넌트에 그려넣는 방법을 말한다.

앱 라우터 방식에선 async 함수 컴포넌트가 JSX를 반환하기 전에 원하는 데이터를 받아오면 된다. 이 작업은 비동기가 아니기 때문에 `useState()`가 필요하지 않으며, SSR 시점에 완료되는 작업이기 때문에 `useEffect()` 또한 필요하지 않다:

```jsx
// 코드 출처: https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating

async function getData() {
  const res = await fetch('https://api.example.com/...')
  // The return value is *not* serialized
  // You can return Date, Map, Set, etc.
 
  if (!res.ok) {
    // This will activate the closest `error.js` Error Boundary
    throw new Error('Failed to fetch data')
  }
 
  return res.json()
}
 
export default async function Page() {
  const data = await getData()
 
  return <main></main>
}
```

반면 페이지 라우터 방식에선 `getServerSideProps()` 함수를 이용한다. 아래는 SSR 시점에 데이터를 받아와 렌더링하는 코드다:

```tsx
// 코드 출처: https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props

import type { InferGetServerSidePropsType, GetServerSideProps } from 'next'
 
type Data = {
  message: string
}
 
export const getServerSideProps = (async () => {
  // Fetch data from external API
  const res = await fetch('https://example.com/api/data')
  const data: Data = await res.json()
  // Pass data to the page via props
  return { props: { data } }
}) satisfies GetServerSideProps<{ data: Data }>
 
export default function Page({
  data,
}: InferGetServerSidePropsType<typeof getServerSideProps>) {
  return (
    <main>
      <p>{data.message}</p>
    </main>
  )
}
```

⚠️ HTML 내보내기로 빌드하면 SSR 렌더링은 사용할 수 없음


## 라우트 핸들러 Route Handlers

GET, POST 같은 HTTP 메서드에 대응하는 서버 사이드 함수. 클라이언트의 요청을 받아 (필요한 경우) 로직을 처리하고 JSON 등의 데이터로 응답하는 엔드 포인트를 이르는 말.

파일 이름은 `route.ts|js`여야 한다. 내보내는 함수는 여러 개일 수 있으며, 이름은 반드시 HTTP 메서드와 동일해야 한다:

```ts
export async function GET(request: Request) {
  // GET 로직
}

export async function POST(request: Request) {
  // POST 로직
}

// PUT, DEL, ...
```


## 다이나믹 라우트 Dynamic Routes

[Routing: Dynamic Routes \| Next.js](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)

URL의 특정 부분을 동적 파라미터로 처리하는 기능. `[slug]` 같은 대괄호 문법을 사용하여, 파일 이름에 다이나믹 세그먼트(Dynamic Segment)를 설정할 수 있다. 이렇게 파일 경로와 파라미터를 동시에 정의하고, 해당 경로에 따라 동적으로 페이지를 렌더링하는 기능이다.

```jsx
export default function Page({ params }) {
  return <div>My Post: {params.slug}</div>
}
```

예를 들어 `app/product/[id].js`라는 파일은 `/product/123` 같은 URL을 처리할 수 있다. `app/product/[category]/[id].js`라는 파일을 만들어 `/product/laptop/123` 같은 다중 파라미터 처리도 가능하다.

```jsx
export default function ProductPage({ params }) {
  const { category, id } = params;

  return (
    <div>
      <h1>Category: {category}</h1>
      <p>Product ID: {id}</p>
    </div>
  );
}
```

[...slug]와 같은 문법(공식 용어는 Catch-all Segments)으로 경로의 나머지 부분을 모두 '캐치'할 수 있다. 예를 들어 `app/[...slug].js`는 `/blog/2024/september/my-post`와 같은 URL을 처리한다.

```jsx
export default function BlogPostPage({ params }) {
  const { slug } = params;

  return (
    <div>
      <h1>Blog Post</h1>
      <p>Full Slug: {slug.join('/')}</p>
      <p>Year: {slug[0]}</p>
      <p>Month: {slug[1]}</p>
      <p>Title: {slug[2]}</p>
    </div>
  );
}
```

Next.js 설정이 `output: 'export'`인 경우 다이나믹 라우트 기능에 제약이 따른다. 미리 페이지를 만들어놔야 한다고 하는데... 자세한 설명은 **TODO**

라우트 핸들러에선 다이나믹 세그먼트를 아래처럼 처리한다:

```tsx
// 코드 출처: https://nextjs.org/docs/app/building-your-application/routing/route-handlers#dynamic-route-segments
export async function GET(
  request: Request,
  { params }: { params: Promise<{ slug: string }> }
) {
  const slug = (await params).slug // 'a', 'b', or 'c'
}
```


## 자주 쓰는 API

### useRouter

라우팅을 프로그래밍 방식으로 변경(programmatically change)하는 클라이언트 훅.

```jsx
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

```jsx
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

### dynamic()

컴포넌트를 동적으로 가져올 때 사용하는 함수. [React.lazy()](https://react.dev/reference/react/lazy)와 [Suspense](https://react.dev/reference/react/Suspense)의 합성이며 비슷한 방식으로 작동한다.

SSR 제어를 위해 쓰기도 한다:

```jsx
import dynamic from 'next/dynamic';

const DynamicComponent = dynamic(() => import('../components/DynamicComponent'), {
  loading: () => <p>Loading...</p>,
  ssr: false,
});

function MyPage() {
  return (
    <div>
      <h1>My Page</h1>
      <DynamicComponent />
    </div>
  );
}

export default MyPage;
```

`loading`으로 불러 오기 전의 내용을 지정했으며, `ssr: false`로 사전 렌더링(pre-rendering)을 비활성화하는 코드다.


{% endraw %}
