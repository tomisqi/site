+++
title = "Freq assignment in DCI format 1-1"
description = ""
tags = [
    "nr",
    "telecom",
]
date = "2018-11-01"
+++

I was recently reading about DCI Format 1-1 in TS 38.212 used in 5G-NR when I came across the
frequency domain resource assignment. There were a couple of formulas that at first sight
I couldn't explain, but upon reflecting on it, they have become clear.

## Format 1-1
Format 1-1 is used in 5G-NR for scheduling PDSCH in one cell. Among other things,
it includes the frequency domain assignment which is how the  network tells the UE
what frequency resources will be used for PDSCH. <br>
There are two allocation types for frequency resources:

1. Resource allocation type 0
2. Resource allocation type 1

I will explain resource allocation type 1, and I will visit type 0 in the future.

## Resource allocation type 1

Resource allocation type 1 takes the approach of telling the UE: "starting from this
frequency resource, use this many frequency resources". The UE is in essence given
a start and a run. This is different from type 0, wherein the UE is given
the resources to use by the way of a bitmap.

In TS 38.212, we see that the number of bits in DCI for allocation type 1
is given by:

![eq1] (https://latex.codecogs.com/gif.latex?%5Clceil%20log_2%20%7B%28%5Cfrac%7BN_%7BRB%7D%5E%7BDL%2CBWP%7D%20*%20%28N_%7BRB%7D%5E%7BDL%2CBWP%7D%20&plus;%201%29%7D%7B2%7D%29%7D%20%5Crceil)

where,

![eq2] (https://latex.codecogs.com/gif.latex?N_%7BRB%7D%5E%7BDL%2CBWP%7D)
        = The number of resource blocks in the bandwidth part


To deduce how this formula comes along, I find it useful to think of a simple scenario. <br>
Suppose our bandwidth was equal to 4. If our start RB is ___S___, there are four
such start RBs:<br>
![image1] (/images/allocType1_startRBs.png)
For each of those start RB ___S___, we have a different number of resource blocks to use. This number is limited
by the number of resource blocks ahead of the start resource block.
As an example, in the case of ___S___ = 0, there four different allocation sizes. This is shown leftmost in
the image below.

![image2] (/images/allocType1.png)

Indeed, as can be seen in the image, in total we say that we have 1 + 2 + 3 + 4 different
possible configurations for our frequency allocation.<br>
We can easily generalize this to say that given a bandwidth B there will be 1 + 2 + 3 ... + (B - 1) + B
possible different possible configurations.<br>
Using the sum of natural numbers, this is equivalent to

![eq3] (https://latex.codecogs.com/gif.latex?%5Cfrac%7BB*%28B&plus;1%29%7D%7B2%7D)

How many bits will we need to represent the above number? We will need the log2
number of bits

![eq4] (https://latex.codecogs.com/gif.latex?log_2%7B%28%5Cfrac%7BB*%28B&plus;1%29%7D%7B2%7D%29%7D)

log2 does not return an integer necessarily, so we take the next integer using
the ceil operator ![eq5] (https://latex.codecogs.com/gif.latex?%5Clceil%20.%20%5Crceil)

![eq6] (https://latex.codecogs.com/gif.latex?%5Clceil%20log_2%7B%28%5Cfrac%7BB*%28B&plus;1%29%7D%7B2%7D%29%7D%20%5Crceil)

The above is identical to formula found in TS 38.212, by substituting

![eq6] (https://latex.codecogs.com/gif.latex?B%20%3D%20N_%7BRB%7D%5E%7BDL%2CBWP%7D)

<br><br>
<img src="/images/black64x64.png" alt="fin" width="16" align="right"/>
