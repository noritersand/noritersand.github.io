---
layout: post
date: 2022-01-06 23:22:31 +0900
title: '[devtool] IntelliJ 개꿀팁 방출: Live Templates'
categories:
  - eclipse
tags:
  - devtool
  - intellij
  - live-templates
  - honey-tip
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [공식 가이드: Configure live templates](https://www.jetbrains.com/help/idea/using-live-templates.html#live_templates_configure)
- [공식 가이드: Live template variables](https://www.jetbrains.com/help/idea/template-variables.html)

#### 버전 정보

- IntelliJ IDEA 2021.3

## 개요

인텔리제이의 라이브 템플릿 소개와 맴대로 추가/수정하는 방법 설명.

## 라이브 템플릿이란

인텔리제이의 자동 완성은 요래요래 분류되는데:

- Code Completion
- Postfix Completion
- File and Code Templates
- Live Templates

이 중 라이브 템플릿은 특정 키워드를 입력한 후 발동 키를 누르면 미리 정해진 설정에 맞춰 코드가 완성되는 기능이다. 이클립스의 Content Assist와 동일함.

![](/images/intellij-live-templates-hi.gif)

윈도우 OS일 때 라이브 템플릿의 단축키는 <kbd>ctrl + space</kbd>이며 그 다음 (필요하면 방향키로 이동해서) <kbd>enter</kbd> 혹은 <kbd>tab</kbd>으로 선택한다.

여기까지는 누가 알려주지 않아도 아는 내용이쥬?

## 템플릿 커스터마이징

필요한 템플릿을 마음대로 추가/수정하는 방법을 알아보자.

환경 설정 경로: Settings<kbd>ctrl + alt + s</kbd> > Editor > Live Templates

![](/images/intellij-settings-live-templates.png)

각 번호별 설명:

1. 템플릿이 활성화될 언어
2. 새 템플릿 추가 버튼
3. 접두어. 이 템플릿이 어떤 키워드에 반응해야 하는지를 의미
4. 라이브 템플릿 선택 창에서 설명으로 표시되는 텍스트(비워도 됨)
5. **변수**를 사용할 때 해당 **변수**가 어떻게 작동해야 하는지 정의하는 메뉴. 위 스샷에선 작성한 **변수**가 없어서 비활성화돼 있음.
6. 이 템플릿이 문맥상 어느 위치에서 활성화 되는지 선택

## 변수가 뭔데?

변수에는 인텔리제이가 알아서 적절한 값으로 채워줄 내용을 정의한다.

예를 들어 접두어 `fori`는 Template text가 요렇게 생겼다:

```java
for(int $INDEX$ = 0; $INDEX$ < $LIMIT$; $INDEX$++) {
  $END$
}
```

그리고 Edit variables를 눌러보면:

![](/images/intellij-settings-live-templates-edit-var.png)

이렇게 돼 있는데 Name 열에선 변수의 이름을, Expression 열에선 해당 변수가 어떤 값으로 채워져야 하는지를 결정한다.

이 경우 `$INDEX$` 자리는 적절한 인덱스 변수명(i, j, k, ...)으로 채워지고 Expression이 비어있는 `$LIMIT$` 자리는 아무것도 채워지지 않는다. (대신 탭 포커싱이 됨)

![](/images/intellij-live-templates-fori.gif)

변수의 이름은 사전에 정의된 변수 두 가지를 제외하고 아무렇게나 지어도 된다.

### 사전 정의 변수

- `$END$`: 코드 자동 완성 후 캐럿이 위치할 자리를 지정
- `$SELECTION$`: 특정 코드를 선택(드래그)한 뒤 Surround With<kbd>ctrl + alt + t</kbd>로 라이브 템플릿을 선택하면 `$SELECTION$` 자리에 선택했던 코드가 자동으로 입력됨

## 설정 예시

### logger

우선 클래스 변수 logger를 자동으로 완성해 주는 템플릿이다.

Abbreviation은 `logger`로, Template Text는 아래처럼 작성한다:

```java
private static final Logger logger = LoggerFactory.getLogger($className$.class);
```

그 다음 `$className$` 변수의 값은 `className()`으로, 적용 범위는 `declaration`으로 선택한다.

- `className()`: 문맥 상 현재 클래스의 이름을 채워넣는다.
- `declaration`: 이 템플릿이 클래스나 메서드의 선언부에서만 활성화된다.

![](/images/intellij-live-templates-logger.gif)

### logger.info

자주 사용하게 될 로그 출력용 템플릿.

Abbreviation은 `li`로, Template Text는 아래처럼 작성한다:

```java
logger.info("{}", $aaa$);
```

`$aaa$`변수의 값은 `completeSmart()`, 적용 범위는 `expression`을 선택한다.

- `completeSmart()`: 자동 완성 후 문맥에 맞는 변수 선택 툴팁을 띄우게 함.
- `expression`: 이 템플릿이 메서드 구현부 중 표현식을 작성할 수 있는 지역일 때만 활성화 됨.

![](/images/intellij-live-templates-li.gif)

### logger.error

catch 구문 내에서의 Exception 메시지 출력 템플릿.

Abbreviation은 `le`로, Template Text는 아래처럼 작성한다:

```java
logger.error($var$.getMessage(), $var$);
```

`$var$` 변수의 값은 `variableOfType("Exception")`, 적용 범위는 `expression`을 선택한다.

- `variableOfType("Exception")`: 문맥에 맞는 Exception의 서브 타입 변수를 채워 넣는다.

![](/images/intellij-live-templates-le.gif)

변수의 Expression들이 어떻게 작동하는 지는 [공식 가이드](https://www.jetbrains.com/help/idea/template-variables.html#predefined_functions)를 볼 것.

-끝-
