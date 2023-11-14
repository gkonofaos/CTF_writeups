# Golfer - Part 1 - HTB reverse challenge

---
## Introduction

- **Challenge name** : Golfer - part 1 
- **Challenge description** : ''A friend gave you an odd executable file, in fact it is very tiny for a simple ELF, what secret can this file hide?''
- **Challenge category** => Reverse
- **Challenge points** => 20 / easy

---
## First Look
Starting of with the `file` command to get a grasp of what we are dealing with i got the following result : 
`golfer: ELF 32-bit`

This doesn't give us alot of information and probably it is compressed or stripped.
Using the `readelf -h` command we get the following: 

![swappy-20231114_210014](https://github.com/gkonofaos/CTF_writeups/assets/112202449/cf51b3f3-48ff-4c65-a477-e0cb47ad6643)

As we can see the flags for Data, Version , OS/ABI dont seem correct

Next i tried the Classic `Strings` Command :

`a4fTUH}yR{l
g_30Br`

Ok , some very familiar characters pop up i can distinguish the **HTB{}** and it isnt hard to put together the **g0lf3r** part as well, so the first thing that popped into my mind was that maybe this was a reordering of the letters and with some trial and error i could brute force the flag 
but nothing came out as the letters the were left off didn't make any word.

---
## Static Analysis
Then i tried loading it up to Ghidra and see if anything  pops up but nothing helpful came of it.


## Dynamic Analysis
Then i tried using *Cutter* to debug it and see what happens step by step:

![swappy-20231114_211542](https://github.com/gkonofaos/CTF_writeups/assets/112202449/2d694977-6e24-4b81-8edc-9831b2f0752b)

As we can  see from the disassembled code the first instruction that gets executed is a `jmp  0x8000127` :

![swappy-20231114_211706](https://github.com/gkonofaos/CTF_writeups/assets/112202449/ca85c557-1ae7-4077-b825-794db7df69b3)


And then the program **exits**.

After a few iterations of debugging trying to understand if anything happens from that *jmp* i decided to see what the code bellow the jmp does since its never executed.
Using the right-click -> edit -> NOP instruction on the jmp instruction i patched the code like this.


![swappy-20231114_211835](https://github.com/gkonofaos/CTF_writeups/assets/112202449/a1e4c8fa-c880-41e0-89cd-4d28a6b6097d)
 

Running it finally gave us the flag :

<details>
  <summary>Spoiler warning</summary>
  
 

![swappy-20231114_212024](https://github.com/gkonofaos/CTF_writeups/assets/112202449/12edd6af-a22f-4f7b-ac1b-45e40d97b3d3)
  
</details>


