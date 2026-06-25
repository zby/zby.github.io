---
layout: page
title:  "Debugging Machine Learning"
date:   2021-09-01 15:49:32 +0200
permalink: /archive/ml-adventures/debugging-ml/
---

Neural networks are close to being black boxes. You train a network and check if it works - but if it doesn't
there is no debugging to find out the cause of that.
You can only guess, and try with different data, and try some other random changes to see if that improves the score.

I have been training my networks on batches of data - and the accuracy of its predictions were
improving. Untill it stopped. I did not know why. I suspected that maybe the data is biased
and the networks learned something they shouldn't
(that is overfitting probably) and the new batches are different so it does not work on them.
I tried measuring that bias - as described in
[Unwanted Correlations](/archive/ml-adventures/unwanted-correlations/) and it proved difficult.
But then I noticed that the images that don't work have the special property that
they end up in my data pipeline as rectangles - while all the rest is processed into
squares.
<!--more-->

And then I noticed that he bbox is evidently expanded in the direction of the smaller side.

My first fix was to pad the images with black - so that they become squares too. That worked
but I was not satisfied. So I looked into the code and voila - width and height are mismatched in 
one place. As long as the pictures are squares they are equal and the mismatch does not matter.

That showed a big improvement - the training on the new data works and the accuracy is better
but it is still far from good, and I still don't know how much training data
I need to reach reasonable accuracy.
