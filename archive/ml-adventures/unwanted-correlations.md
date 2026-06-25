---
layout: page
title:  "Unwanted Correlations"
date:   2021-08-30 15:49:32 +0200
permalink: /archive/ml-adventures/unwanted-correlations/
---

I am training neural networks to detect some objects by their shapes. The problem is that the photo sets are not color-balanced - that is
there are correlations between the object color and the object type - so the networks are probably learning it instead of recognizing the shapes.

I would like to have two tools - fist is to measure the bad correlation in the data set. The second is a tool for helping choosing
photos from a photo set to create a more color balanced data set. The second one might be difficult.

This looks like a very common and generic problem - but I have trouble in finding good solutions for it.
<!--more-->

It is a bit complicated because there are three color channels (RGB) and also the color variable is semi-continuous one.

Are there any tutorials about that? Libraries in Python?

To simplify it I guess I could do it for each color channel separately and also divide the color values into maybe 8 buckets to make the color
variable discrete. Then I could probably use chi squared, which is available in scikit-learn, for that - but I still need to wrap my head around
that because the lib only expects a sequence not a table. Or maybe we could use Logistic Regression, also available in scikit-learn. Logistic Regression
is about building a predictor that learns the block class from its color - exactly what I need. But it is not immediately evident if there is a way
to get a measure of how good is the predictor and also there might be problems with speed.
