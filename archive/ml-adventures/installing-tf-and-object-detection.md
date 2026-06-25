---
layout: page
title:  "My script for installing Python TensorFlow Object Detection libs"
date:   2021-08-20 15:49:32 +0200
permalink: /archive/ml-adventures/installing-tf-and-object-detection/
---

I had a lot of trouble installing the Python TensorFlow libs required for object detection
in Ubuntu - then I wrote a script that works for me and my collaborators (Ubuntu 20.04 and 21.04):

[Python TensorFlow Object Detection installation](https://gist.github.com/zby/e3341751f54bc92ebcc1d9da08a9459a
)

It installs all the system prerequisites (like protobuf), CUDA, the python libs, and also downloads one neural network (because I worked with it).
<!--more-->

The crucial part is this:

`pip3 install pycocotools --no-build-isolation --no-binary :all:`

Apparently there is a pip bug that without the additional flags makes it installing binaries for numpy 1.20.xx and then linking pycocotools with it:
[from this SO answer](https://stackoverflow.com/questions/66060487/valueerror-numpy-ndarray-size-changed-may-indicate-binary-incompatibility-exp/66743692#66743692)

None of the online tutorials I found touched this problem. Maybe it will be fixed some day.
