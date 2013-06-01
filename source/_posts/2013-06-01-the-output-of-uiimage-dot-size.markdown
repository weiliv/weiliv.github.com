---
layout: post
title: "the Output of UIImage.size"
date: 2013-06-01 14:11
comments: true
categories: 
---
The `size` reported by `UIImage` is in points, not pixels. You need to take into account the `scale` property of `UIImage`.

In other words, if `test.jpg` is 1000x1000 then `UIImage.size` will report 1000x1000. If `test@2x.png` is 2000x2000 then `UIImage.size` will also report 1000x1000. But in the 2nd case, `UIImage.scale` will report 2.

`CGImageGetWidth` reports its width in pixels, not points. Or o convert UIImage points into pixels, you need to multiply by scale.

``` objectivec
UIImage *image = [UIImage imageNamed:@"someImage.png"];
NSInteger pixelsInWidth = CGImageGetWidth(image.CGImage);
//or
pixelsInWidth = image.size.width * image.scale;
```