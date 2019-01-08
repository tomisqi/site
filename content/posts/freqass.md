+++
title = "Frequency assignment in 5G NR"
description = ""
tags = [
    "nr",
    "telecom",
]
date = "2018-11-01"
+++

<span style="color:red">-This post is a work in progress-</span>

This post will be about some reflections I had when reading about the
frequency assignments in 5G NR.
I will specifically discuss frequency assignments on DL since this is the
area I've been reading about.

Frequency assignment is described in detail in TS 38.214.

## Resource allocation types
5G NR defines two ways to describe frequency assignments on DL. This assignment is part of the DCI, and is how the network tells the UE what frequency resources will be used for PDSCH. <br>
The two possible ways to describe frequency assignments are:

1. Resource allocation type 0
2. Resource allocation type 1

## Resource allocation type 0
When using resource allocation type 0 the network tells the UE the frequency resources to use by the way of a bitmap. <br>
Given that the 5G NR standard allows for a maximum of 275 resource blocks, an equal number of bits would be needed to encode this information if 1 bit per resource block were to be used. Instead a tradeoff is reached: the number of bits is limited so that the DCI can remain relatively small, but resolution is lost by allocating 1 bit per resource block group (RBG), which are essentially a collection of contiguous resource blocks.

The size of the RBG varies by bandwidth as is evidenced in Table 5.1.2.2.1-1 of TS 38.214

| Bandwidth Part Size | Configuration 1 | Configuration 2 |
| --------------------|:---------------:| ---------------:|
| 1 – 36              | 2               | 4               |
| 37 - 72             | 4               | 8               |
| 73 - 144            | 8               | 16              |
| 145 - 275           | 16              | 16              |

The values in the table represent the "nominal" size of the RBG and are denominated _P_. This means that there are non-nominal sizes, and indeed there are.<br>
Following the table, TS 38.214 explains that:

 - the size of the first RBG is []
 - the size of the last RBG is [] if [] and _P_ otherwise,
 - the size of all other RBGs is _P_

The first and second bullet from above are the "non-nominal" sizes of the RBG.

_Why are the non-nominal values what they are?_

I found that this comes from the fact that the start/end of RBGs need to be aligned regardless of the start of the BWP [].<br>
See the following figure for reference<br>
[insert figure]<br>
As can be seen, RBGs that are on the both ends of the bandwidth part are smaller if they are not aligned with the standard. In this way RBGs are aligned always regardless of start of BWP

_Why these RBG sizes?_

<...coming...>

## Resource allocation type 1

When using resource allocation type 1 the network tells the UE a start frequency resource and a number of contiguous frequency resources to use. <br>
This is different from type 0, wherein the UE is given the resources to use by the way of a bitmap. Also different from type 0 is that in type 1, there is no longer a need
for RBGs. Instead, the precise starting resource block and the number of contiguously allocated resource block is signaled in the DCI. Both pieces of data (start and number) are encoded in a value called
resource indication value (RIV).

_How many combinations do we have for start resource block / number of resource blocks given a certain bandwidth?_

Answering this question should shed light on the number of bits used in DCI to represent frequency assignments that use resource allocation type 1.

To answer this question, I find it useful to think of a simple scenario. <br>
Suppose our bandwidth was equal to 4. If our start RB is ___S___, there are four
such start RBs:<br>
![image1] (/images/allocType1_startRBs.png)
For each of those start RB ___S___, we have a different number of resource blocks to use. This number is limited by the number of resource blocks ahead of the start resource block.<br>
If ___S___ = 0, there four different allocation sizes (shown leftmost in the image below). If ___S___ = 1, there are three different allocation sized and so on.
![image2] (/images/allocType1.png)
Indeed, as can be seen in the image, in total we say that we have 1 + 2 + 3 + 4 different possible configurations for our frequency allocation.<br>
We can easily generalize this to say that given a bandwidth _B_ there will be 1 + 2 + 3 ... + (_B_ - 1) + _B_ possible different possible configurations.<br>
Using the sum of natural numbers, this is equivalent to

![eq1] (https://latex.codecogs.com/gif.latex?%5Cfrac%7BB*%28B&plus;1%29%7D%7B2%7D)

The above tells us how many combinations would be possible, but how many bits will we need in the DCI? <br>
We will need log2 number of bits (plus a ceil operator ![eq5] (https://latex.codecogs.com/gif.latex?%5Clceil%20.%20%5Crceil) to get the next integer)

![eq2] (https://latex.codecogs.com/gif.latex?%5Clceil%20log_2%7B%28%5Cfrac%7BB*%28B&plus;1%29%7D%7B2%7D%29%7D%20%5Crceil)

This is analogous to the formula found in TS 38.212,

![eq3] (https://latex.codecogs.com/gif.latex?%5Clceil%20log_2%20%7B%28%5Cfrac%7BN_%7BRB%7D%5E%7BDL%2CBWP%7D%20*%20%28N_%7BRB%7D%5E%7BDL%2CBWP%7D%20&plus;%201%29%7D%7B2%7D%29%7D%20%5Crceil)

where,

![eq4] (https://latex.codecogs.com/gif.latex?N_%7BRB%7D%5E%7BDL%2CBWP%7D)
        = The number of resource blocks in the bandwidth part


<br><br>
I find understanding how this formula comes along very rewarding.


<br><br>
<img src="/images/black64x64.png" alt="fin" width="16" align="right"/>
