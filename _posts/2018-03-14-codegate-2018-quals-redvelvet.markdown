---
layout: post
title: "Codegate 2018 Quals RedVelvet"
img: codegate2018/RedVelvet/postimg.png
date: 2018-03-14 19:14:30 +0900
description: Codegate 2018 Quals Reversing RedVelvet Write-up
categories: Codegate Reversing Writeup
---
# Codegate 2018 Prequal Reversing RedVelvet

## 개요

```
RedVelvet - 182pts (Rev)
Happiness:)
Solve 28
```

[Download](https://s3.ap-northeast-2.amazonaws.com/codegate2018/afbea1c0a463d63cd6f00389a3b2fe88)

> 다운로드가 안된다면 [이곳](https://github.com/sunghun7511/Writeup/blob/master/ctf/codegate/2018-Prequal/Reversing/RedVelvet/afbea1c0a463d63cd6f00389a3b2fe88)에서 해주세요!

(`Junior` 으로 참가하여서 일반부와는 문제의 점수가 다를 수 있습니다.)

## 풀이

이 바이너리를 가상머신에서 실행시키면 입력을 한 번 받고 바로 종료한다.

먼저, IDA 를 통해서 바이너리를 분석해보았다.

![source](https://github.com/sunghun7511/Writeup/raw/master/ctf/codegate/2018-Prequal/Reversing/RedVelvet/source.PNG)

27 글자만큼의 입력을 받고, `func1` ~ `func15` 까지 거친 후 입력값을 `SHA256` 해시를 한 이후, 플래그의 해시로 추정되는 `s2` 와 같은지 검사한다.

각 `func` 함수들 안에는 만약 플래그의 조건이 맞지 않다면 종료하는 구문이 있었기에, `angr` 를 사용하였다.

아래와 같은 코드를 짜고, 실행시켰다.

```python
from angr import *

p = Project("./RedVelvet", load_options={'auto_load_libs': False})
ex = p.surveyors.Explorer(find=(0x0000000000401546, ), avoid=(0x00000000004007D0,))
ex.run()

print("\"" + ex.found[0].state.posix.dumps(0) + "\"")
print(ex.found[0].state.posix.dumps(0).encode("hex"))
'''
```

출력은 다음과 같이 나왔다.

```
What_You_Wanna_Be?:)_lc_la**\x02\x0e\x8a\x8aJ\x0e\x8a\x8a\x1a\x1a\x02\x08\x0e*JJ\x8a*\x0e\n\x8a*JJ\x02\x02J\x8a\x0b\x8a*\x08\x00
```

그런데 뒷부분이 조금 이상하게 나왔다.

이 바이너리에서 받는 입력은 27글자이다. 널바이트를 포함한다면, `What_You_Wanna_Be?:)_lc_la` 까지이다.

그래서 `What_You_Wanna_Be?:)_lc_la` 를 그대로 넣어봤지만 올바르지 않다고 떴고,

조금의 게싱을 동원하여 `lc_la` 를 `la_la` 로 바꾼 후에 넣어봤더니

```
shgroup@ubuntu:/mnt/hgfs/Writeup/ctf/codegate/2018-Prequal/Reversing/RedVelvet$ ./RedVelvet
Your flag : What_You_Wanna_Be?:)_la_la
HAPPINESS:)
HAPPINESS:)
HAPPINESS:)
HAPPINESS:)
HAPPINESS:)
HAPPINESS:)
HAPPINESS:)
HAPPINESS:)
HAPPINESS:)
HAPPINESS:)
HAPPINESS:)
HAPPINESS:)
HAPPINESS:)
HAPPINESS:)
HAPPINESS:)
flag : {" What_You_Wanna_Be?:)_la_la "}
```

다음과 같이 맞다고 하였다.

## Flag

`What_You_Wanna_Be?:)_la_la`