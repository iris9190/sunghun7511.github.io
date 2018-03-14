---
layout: post
title: "kramdown 사용법"
img: postimg/programming.jpg
date: 2018-03-14 15:54:27 +0900
description: Jekyll 에서 사용하는 kramdown 문법 에 대해 알아본다.
categories: Jekyll, Kramdown, Markdown
---
# kramdown 문법

## 개요

Git blog를 사용하려고 보니 kramdown 라는 마크다운 문법을 사용한다고 해서 앞으로 사용하기 위해서 정리를 하였다.

[출처](http://gjchoi.github.io/env/Kramdown%28%EB%A7%88%ED%81%AC%EB%8B%A4%EC%9A%B4%29-%EC%82%AC%EC%9A%A9%EB%B2%95/)

## 문법

### 헤더 (Header)

* `===` 는 h1
* `---` 는 h2
* `#` 는 h1
* `##` 는 h2
* `###` 는 h3
* `####` 는 h4
* `#####` 는 h5
* `######` 는 h6

### 강조

`*이텔릭체*` -> *이텔릭체*

`**진하게**` -> **진하게**

* 이 때 `*`는 `_`로 대체될 수 있다. 예 : `_이텔릭체_`

### 링크

[Go to Google](https://google.com)

[Go to Google](https://google.com "google")

[Go to Google][Google  Go]

[Google Go]: https://google.com "google"

[Go to Google]

[Go to Google]: https://google.com "google"

```
[Go to Google](https://google.com)

[Go to Google](https://google.com "google")

[Go to Google][Google  Go]

[Google Go]: https://google.com "google"

[Go to Google]

[Go to Google]: https://google.com "google"
```

### 이미지

![이미지이름](/assets/img/SHGroup.png)

```
![이미지이름](/assets/img/SHGroup.png)
```

### 인라인 코드

`` ` `` 를 사용해서 감싸면 된다. `` ` `` 자체를 사용하고 싶다면 ``` `` ```로 감싸주면 된다.

` ### 이런식으로 무시가 가능하다. `

여러 줄은 ` ``` ` 을 사용한다.

### 줄바꿈

줄바꿈은 문장 제일 끝에 스페이스 바 두 번을 치면 된다.  
다음과 같이 줄바꿈이 된다.

### 블록 인용구

> 다음과 같은 블록 인용구를 사용할 수 있다.

```
> 다음과 같은 블록 인용구를 사용할 수 있다.
```

### 코드 블럭

    띄어쓰기 4줄을 사용하거나

~~~~~~~~
~를 매우 많이 사용하면 <pre><code> 태그들로 감싸져서 
문자를 그대로 보이게 할 수 있다.
~~~~~~~~

```
    띄어쓰기 4줄을 사용하거나

~~~~~~~~
~를 매우 많이 사용하면 <pre><code> 태그들로 감싸져서 
문자를 그대로 보이게 할 수 있다.
~~~~~~~~
```

### 프로그램 코드 블럭

블럭 인용구의 시작부분 바로 뒤에 언어 이름을 붙이면 가능하다.

```c
#include <stdio.h>

int main(void){
    printf("Hello World!\n");
    return 0;
}
```
~~~~~~~~ java
package com.SHGroup.Test;

public class Test {
    public static void main(String[] args){
        System.out.println("Hello World!");
    }
}
~~~~~~~~

~~~~~~~
```c
#include <stdio.h>

int main(void){
    printf("Hello World!\n");
    return 0;
}
```
~~~~~~~

```
~~~~~~~~ java
package com.SHGroup.Test;

public class Test {
    public static void main(String[] args){
        System.out.println("Hello World!");
    }
}
~~~~~~~~
```

### 가로선

* * *
---
- - -
--------

```
* * *
---
- - -
--------
```

### 리스트

1. 첫 번째
2. 두 번째
3. 세 번째

* 순서 없음
* 순서가 없다.
* N 번째가 없다.
  * 이후에 리스트를 더 붙일 수 있다.
    * 리스트

```
1. 첫 번째
2. 두 번째
3. 세 번째

* 순서 없음
* 순서가 없다.
* N 번째가 없다.
  * 이후에 리스트를 더 붙일 수 있다.
    * 리스트
```

### 각주

이것은 엄청나게 대단한 내용이다[^1]

```
이것은 엄청나게 대단한 내용이다[^1]


[^1]: 이것이 바로 그 각주라는 것인데, `[^1]` 처럼 사용한다.
```

[^1]: 이것이 바로 그 각주라는 것인데, `[^1]` 처럼 사용한다.