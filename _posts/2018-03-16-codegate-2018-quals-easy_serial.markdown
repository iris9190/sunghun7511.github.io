---
layout: post
title: "Codegate 2018 Quals easy_serial"
img: codegate2018/easy_serial/postimg.png
date: 2018-03-16 15:13:08 +0900
description: Codegate 2018 Quals Reversing easy_serial Write-up
categories: Codegate Reversing Writeup
---
# Codegate 2018 Prequal Reversing easy_serial

## 개요

```
easy_serial - 924pts (Rev)
Solve 2
```

[Download](https://s3.ap-northeast-2.amazonaws.com/codegate2018/f771eb6211d0f2afd59376c3af8f786a)

> 다운로드가 안된다면 [이곳](https://github.com/sunghun7511/Writeup/blob/master/ctf/codegate/2018-Prequal/Reversing/easy_serial/21ad6600a0045e8091c81706c6907d1d)에서 해주세요!

(`Junior` 으로 참가하여서 일반부와는 문제의 점수가 다를 수 있습니다.)

## 풀이

바이너리를 분석을 위해 IDA 를 사용하였다.

리눅스 64비트 바이너리 파일이였다.

함수가 엄청 많았는데, 분석을 하다보니 하스켈로 짜여진 바이너리라는 것을 알게 되었다.

왜냐하면

![functions](https://github.com/sunghun7511/Writeup/raw/master/ctf/codegate/2018-Prequal/Reversing/easy_serial/functions.PNG)

몇몇 함수 이름들이 `hs_` 로 시작했었고, `hs_main` 함수가 있었기 때문이다.

하스켈 디컴파일러가 있지 않을까 하는 마음에 깃헙에서 찾아봤는데, 있긴 있었다.

[hsdecomp](https://github.com/gereeter/hsdecomp) ([Jonathan S](https://github.com/gereeter) 님에게 감사드립니다. ^-^)

먼저 이를 사용하려고 클론을 하고, `python ./runner.py ../easy` 를 입력했는데, 오류가 났다..

하지만 어느정도 동작은 하는 것을 발견하였고, 오류가 나는 부분을 고치기로 하였다.

오류나는 부분을 찾아 들어가면서 오류를 고치기 위해 조금의 삽질을 하였다.

> 만약 고쳐진 하스켈 디컴파일러를 사용하고 싶으시다면 [이곳](https://github.com/sunghun7511/hsdecomp)에 올려놓았다.

이게 하스켈 디컴파일러의 출력입니다.

```haskell
Main_main_closure=>> $fMonadIO
    (putStrLn (unpackCString# "Input Serial Key >>> "))
    (>>= $fMonadIO
        getLine
        (\s1dZ_info_arg_0 ->
            >> $fMonadIO
                (putStrLn (++ (unpackCString# "your serial key >>> ") (++ s1b7_info (++ (unpackCString# "_") (++ s1b9_info (++ (unpackCString# "_") s1bb_info))))))
                (case && (== $fEqInt (ord (!! s1b7_info loc_7172456)) (I# 70)) (&& (== $fEqInt (ord (!! s1b7_info loc_7172472)) (I# 108)) (&& (== $fEqInt (ord (!! s1b7_info loc_7172488)) (I# 97)) (&& (== $fEqInt (ord (!! s1b7_info loc_7172504)) (I# 103)) (&& (== $fEqInt (ord (!! s1b7_info loc_7172520)) (I# 123)) (&& (== $fEqInt (ord (!! s1b7_info loc_7172536)) (I# 83)) (&& (== $fEqInt (ord (!! s1b7_info loc_7172552)) (I# 48)) (&& (== $fEqInt (ord (!! s1b7_info loc_7172568)) (I# 109)) (&& (== $fEqInt (ord (!! s1b7_info loc_7172584)) (I# 101)) (&& (== $fEqInt (ord (!! s1b7_info loc_7172600)) (I# 48)) (&& (== $fEqInt (ord (!! s1b7_info (I# 10))) (I# 102)) (&& (== $fEqInt (ord (!! s1b7_info (I# 11))) (I# 85)) (== $fEqInt (ord (!! s1b7_info (I# 12))) (I# 53))))))))))))) of
                    <tag 1> -> putStrLn (unpackCString# ":p"),
                    c1ni_info_case_tag_DEFAULT_arg_0@_DEFAULT -> case == ($fEq[] $fEqChar) (reverse s1b9_info) (: (C# 103) (: (C# 110) (: (C# 105) (: (C# 107) (: loc_7168872 (: loc_7168872 (: (C# 76) (: (C# 51) (: (C# 114) (: (C# 52) [])))))))))) of
                        False -> putStrLn (unpackCString# ":p"),
                        True -> case && (== $fEqChar (!! s1bb_info loc_7172456) (!! s1b3_info loc_7172456)) (&& (== $fEqChar (!! s1bb_info loc_7172472) (!! s1b4_info (I# 19))) (&& (== $fEqChar (!! s1bb_info loc_7172488) (!! s1b3_info (I# 19))) (&& (== $fEqChar (!! s1bb_info loc_7172504) (!! s1b4_info loc_7172568)) (&& (== $fEqChar (!! s1bb_info loc_7172520) (!! s1b2_info loc_7172488)) (&& (== $fEqChar (!! s1bb_info loc_7172536) (!! s1b3_info (I# 18))) (&& (== $fEqChar (!! s1bb_info loc_7172552) (!! s1b4_info (I# 19))) (&& (== $fEqChar (!! s1bb_info loc_7172568) (!! s1b2_info loc_7172504)) (&& (== $fEqChar (!! s1bb_info loc_7172584) (!! s1b4_info (I# 17))) (== $fEqChar (!! s1bb_info loc_7172600) (!! s1b4_info (I# 18))))))))))) of
                            <tag 1> -> putStrLn (unpackCString# ":p"),
                            c1tb_info_case_tag_DEFAULT_arg_0@_DEFAULT -> putStrLn (unpackCString# "Correct Serial Key! Auth Flag!")
                )
        )
    )
s1b4_info=unpackCString# "abcdefghijklmnopqrstuvwxyz"
loc_7172600=I# 9
s1bb_info=!! s1b5_info loc_7172488
loc_7172488=I# 2
s1b5_info=splitOn $fEqChar (unpackCString# "#") s1dZ_info_arg_0
loc_7172584=I# 8
loc_7172504=I# 3
s1b2_info=unpackCString# "1234567890"
loc_7172568=I# 7
loc_7172552=I# 6
s1b3_info=unpackCString# "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
loc_7172536=I# 5
loc_7172520=I# 4
loc_7172472=I# 1
loc_7172456=I# 0
loc_7168872=C# 48
s1b9_info=!! s1b5_info loc_7172472
s1b7_info=!! s1b5_info loc_7172456
```

바로 보기는 힘들어 보여서 조금 더 보기 쉽게 고쳤다.

```haskell
Main_main_closure=>> $fMonadIO
    (putStrLn (unpackCString# "Input Serial Key >>> "))
    (>>= $fMonadIO
        getLine
        (\s1dZ_info_arg_0 ->
            >> $fMonadIO
                (putStrLn(++ (unpackCString# "your serial key >>> ") (++ s1b7_info (++ (unpackCString# "_") (++ s1b9_info (++ (unpackCString# "_") s1bb_info))))))

                (case
                    && (== $fEqInt (ord (!! s1b7_info I# 0)) (I# 70)
                    (&& (== $fEqInt (ord (!! s1b7_info I# 1)) (I# 108))
                    (&& (== $fEqInt (ord (!! s1b7_info I# 2)) (I# 97))
                    (&& (== $fEqInt (ord (!! s1b7_info I# 3)) (I# 103))
                    (&& (== $fEqInt (ord (!! s1b7_info I# 4)) (I# 123))
                    (&& (== $fEqInt (ord (!! s1b7_info I# 5)) (I# 83))
                    (&& (== $fEqInt (ord (!! s1b7_info I# 6)) (I# 48))
                    (&& (== $fEqInt (ord (!! s1b7_info I# 7)) (I# 109))
                    (&& (== $fEqInt (ord (!! s1b7_info I# 8)) (I# 101))
                    (&& (== $fEqInt (ord (!! s1b7_info I# 9)) (I# 48))
                    (&& (== $fEqInt (ord (!! s1b7_info (I# 10))) (I# 102))
                    (&& (== $fEqInt (ord (!! s1b7_info (I# 11))) (I# 85))
                    (== $fEqInt (ord (!! s1b7_info (I# 12))) (I# 53))))))))))))) of

                    <tag 1> -> putStrLn (unpackCString# ":p"),
                    c1ni_info_case_tag_DEFAULT_arg_0@_DEFAULT ->
                    case == ($fEq[] $fEqChar) (reverse s1b9_info)
                        (: (C# 103) (: (C# 110) (: (C# 105) (: (C# 107) (: C# 48 (: C# 48 (: (C# 76) (: (C# 51) (: (C# 114) (: (C# 52) []))))))))))
                    of False -> putStrLn (unpackCString# ":p"), True -> case
                        && (== $fEqChar (!! s1bb_info I# 0) (!! uppercase_string I# 0))
                        (&& (== $fEqChar (!! s1bb_info I# 1) (!! lowercase_string (I# 19)))
                        (&& (== $fEqChar (!! s1bb_info I# 2) (!! uppercase_string (I# 19)))
                        (&& (== $fEqChar (!! s1bb_info I# 3) (!! lowercase_string I# 7))
                        (&& (== $fEqChar (!! s1bb_info I# 4) (!! number_string I# 2))
                        (&& (== $fEqChar (!! s1bb_info I# 5) (!! uppercase_string (I# 18)))
                        (&& (== $fEqChar (!! s1bb_info I# 6) (!! lowercase_string (I# 19)))
                        (&& (== $fEqChar (!! s1bb_info I# 7) (!! number_string I# 3))
                        (&& (== $fEqChar (!! s1bb_info I# 8) (!! lowercase_string (I# 17)))
                        (== $fEqChar (!! s1bb_info I# 9) (!! lowercase_string (I# 18)))))))))))
                        of <tag 1> -> putStrLn (unpackCString# ":p"),
                            c1tb_info_case_tag_DEFAULT_arg_0@_DEFAULT -> putStrLn (unpackCString# "Correct Serial Key! Auth Flag!")
                )
        )
    )
lowercase_string=unpackCString# "abcdefghijklmnopqrstuvwxyz"
number_string=unpackCString# "1234567890"
uppercase_string=unpackCString# "ABCDEFGHIJKLMNOPQRSTUVWXYZ"

s1b5_info=splitOn $fEqChar (unpackCString# "#") s1dZ_info_arg_0
s1bb_info=!! s1b5_info I# 2
s1b9_info=!! s1b5_info I# 1
s1b7_info=!! s1b5_info I# 0
```

소스를 보니 문자열을 입력받고, `#` 를 기준으로 문자열 세 가지로 나눈다.

그 이후 다음과 같이 세 번 검사한다.

1. 나눈 문자열 중 첫 번째 문자열은 미리 정해둔 값들과 같은지 검사한다.
2. 나눈 문자열 중 두 번째 문자열은 거꾸로 뒤집어서 미리 정해둔 값들과 검사한다.
3. 나눈 문자열 중 마지막 문자열은 미리 정의된 몇 가지 배열에서 n번째 값과 검사한다.
    * 미리 정의된 배열은 3가지 이다.
    * 소문자 문자열 배열 : `abcdefghijklmnopqrstuvwxyz`
    * 대문자 문자열 배열 : `ABCDEFGHIJKLMNOPQRSTUVWXYZ`
    * 숫자 배열 : `1234567890`

이제 이것을 통과할 수 있는 값을 얻기 위해 다음과 같은 파이썬 코드를 작성하였다.

```python
def chrs(cs):
    n = ""
    for c in cs:
        n += chr(c)
    return n

def num(a):
    return (chr(a + ord('1')))

def lo(a):
    return (chr(a + ord('a')))

def up(a):
    return (chr(a + ord('A')))

flag = ""

flag += chrs((70, 108, 97, 103, 123, 83, 48, 109, 101, 48, 102, 85, 53))

flag += "#"

flag += chrs((52, 114, 51, 76, 48, 48, 107, 105, 110, 103))

flag += "#"

flag += up(0) + lo(19) + up(19) + lo(7) + num(2) + up(18) + lo(19) + num(3) + lo(17) + lo(18)

print(flag)
```

그리고, 최종적으로 이 소스코드의 출력값은 다음과 같았다.

```
Flag{S0me0fU5#4r3L00king#AtTh3St4rs
```

(`}`는 왜 없지.... ㅇㅁㅇ)

### Flag

`S0me0fU5#4r3L00king#AtTh3St4rs`