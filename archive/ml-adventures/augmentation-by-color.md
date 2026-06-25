---
layout: page
title:  "Training Image Augmentation by Color"
date:   2021-09-01 15:49:32 +0200
permalink: /archive/ml-adventures/augmentation-by-color/
---
*Data augmentation:*
> in data analysis are techniques used to increase the amount of data by adding slightly
> modified copies of already existing data or newly created synthetic data from existing data.
> It acts as a regularizer and helps reduce overfitting when training a machine learning model.
[from Wikipedia](https://en.wikipedia.org/wiki/Data_augmentation)

The images I have don't have any uniform distribution of colors per block type - so 
I was afraid my networks learn to recognize the block type by their colors.

One common advice I've got was to augment the data by manipulating the colors somehow.
<!--more-->

## Greyscale
The first proposal was to convert the images to grayscale. I did not like it too much - because
it reduces the input data. That reduction can remove the unwanted correlation
but it does not have to, and it can also erase the shape data if it conflates the color
of the block and the background.

## Color swaps and reversions
Then there was the proposal to reverse and/or swap random color channels.
I tried that with the following code:
{% highlight python %}
def rev_color(image, color):
    channel = image[:,:,color]
    rev_channel = 255 - channel
    image[:,:,color] = rev_channel
    return image

def swap_color(image, i):
    b,g,r = cv2.split(image)
    if i == 0:
        new_channels = [g,b,r]
    elif i == 1:
        new_channels = [r,g,b]
    else:
        new_channels = [b,r,g]
    new_image = cv2.merge(new_channels)
    return new_image
{% endhighlight %}
Not very elegant - but it worked.

For the following original photo
<p>
<img alt="original colors" src="/assets/images/20201109_131838_478_2047_1228_2797.png">
</p>
A green and blue channels swap resulted in:
<p>
<img alt="swapped colors" src="/assets/images/20201109_131838_478_2047_1228_2797_r0.png">
</p>
And reversion of the blue channel resulted in:
<p>
<img alt="swapped colors" src="/assets/images/20201109_131838_478_2047_1228_2797_r0_rev.png">
</p>

## "HSV rotation"
Yet another proposal was to convert the color into HSV and then 'turn' the H channel by a random angle.
Here is my code for that:
{% highlight python %}
def move_colors(image, diff_color):
    hsv = cv2.cvtColor(image,cv2.COLOR_BGR2HSV)
    h,s,v = cv2.split(hsv)
    hnew = np.mod(h + diff_color, 180).astype(np.uint8)
    hsv_new = cv2.merge([hnew,s,v])
    bgr_new = cv2.cvtColor(hsv_new, cv2.COLOR_HSV2BGR)
    return bgr_new
{% endhighlight %}

And the result of 'turning the color' by 81 degrees:
<p>
<img alt="swapped colors" src="/assets/images/20201109_131838_478_2047_1228_2797_81.png">
</p>

I trained my network on the original + augmented images (separately for reversions and swaps).
This resulted in doubling the amount of training data,
but there were no meanigful improvement in accuracy.

