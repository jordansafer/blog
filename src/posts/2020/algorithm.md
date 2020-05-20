---
title: Online algorithm for simple moving average with  l̶o̶g̶(̶n̶)̶ O(1) storage (avg n sample history)
date: 2020-02-14
layout: post.pug
---

Disclaimer: Somebody has probably come up with this before, and they are not the top result on google. And their work is not described in the Wikipedia article on moving averages.

I tried to come up with an online algorithm for a moving average of 100 numbers and found that I was storing 100 numbers just to keep the average. 

Here's an example with a 5 number moving average, where the numbers are:
3,4,5,6,2,7,8,9,52,579,0 .....
We can add the first 5 numbers 3 + 4 + 5 + 6 + 2 = 20/5 = 4 and store an average of 4. Now we are presented with 7. How do we update our average? We need to subtract the oldest number (in this case, 3/5). But all we have stored is 4. So we are stuck. If we wanted to approximate, we could subtract 4/5, and drop an average sample, but we want to be precise.

So if we want a 100-number moving average, we have no choice but to store the last 100 numbers.

fun nextAverage(newestNumber, previous100, previousAverage):
let previous99::oldestNumber = previous100 // abuse of cons :)
let current100 = newestNumber::previous99 // correct use of cons
let currentAverage = previousAverage - oldestNumber/100 + newestNumber/100
in (currentAverage, current100)

So now I thought we could somehow logarithmically improve this with powers of 2, by storing partial sums!
But if we store the sums of the last 1,2,4,8 numbers, it doesn't help us. We still need to know the oldest number we are shifting out each time we process new input.

Thought: What if we don't average the same amount of numbers each time?
Strategy: We still store powers of 2, but they are non-overlapping.

Example stream of numbers: 3,4,5,6,2,7,8,9,5,2,5,7,9,0,1,3 .....
Stored numbers at each turn: 3 .. 0,7 .. 5,7 .. 0,0,18 .. 2,0,18 .. 0,9,18 .. 8,9,18 
.. 0,0,0,44 .. 5,0,0,44 .. 0,7,0,44 .. 5,7,0,44 .. 0,0,19,44 .. 9,0,19,44 .. 0,9,19,44 
.. 1,9,19,44 .. 0,0,0,32!
Notice how we are accumulating a sum of past numbers as powers of 2. We have a "sum of 1" place, "sum of 2" place, "sum of 4" place, and "sum of 8" place. And notice how we discard the old 8 when we finish the next 8!

If we consider a full cycle, starting with all zeros except for the latest "sum of 8", we will have a sum of 8,9,10,11,12,13,14,15,8 available to us as we go through the cycle. Meanwhile, we are keeping 4 numbers stored. This means that on average, we have a sum of the recent 11.5 most numbers available, with only 4 numbers stored!

This improves even more as we increase to 5,6,7 numbers (5 => 23.5, 6 => 47.5, 7 => 95.5). The tradeoff is that we are averaging over a variable number of numbers.

Actually, realizing this only requires 2 numbers. The old group and the new group we are working on. There is a tradeoff of # of groups and variability in number of samples in our average. For 2 numbers, we will store newGroupSampleCount, newGroupSum, and oldGroupSum.

Average samples =( groupSize + (2*groupSize - 1))/2 = 1.5 *gS - 0.5
100 = 1.5gS - 0.5, 100.5/1.5 = 67. So we should use groupSize = 67 to get 100 samples on average. We could use gS = 100 to get 100 samples minimum.

fun nextAverage(nextSample, newGroupSampleCount, newGroupSum, oldGroupSum)
let nextSum = newGroupSum + nextSample
let nextCount = newGroupSampleCount + 1
if nextCount = 67 then (0, 0, nextSum, nextSum/67)
else (nextCount, nextSum, oldGroupSum, (nextSum + oldGroupSum)/(nextCount + 67))

This has a drawback because of overflow, but we could store sums at sum/100 (scale down by 100) and just multiply by 100/actualCount on each iteration.

In conclusion, if you care more about consistency than accuracy, you probably want to use exponential moving averages, which are explained nicely by Wikipedia. You still have minimum storage and a reasonably accurate moving average of N-ish samples (old samples drop off in impact).