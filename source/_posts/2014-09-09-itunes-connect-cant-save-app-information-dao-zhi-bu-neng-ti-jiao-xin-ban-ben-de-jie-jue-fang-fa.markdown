---
layout: post
title: "iTunes Connect: Can't save app information 导致不能提交新版本的解决方法"
date: 2014-09-09 18:18
comments: true
categories: [iOS]
---
  
  
iTunesConnect改版后UI上更酷炫了，但是功能上却出现了一些让人抓狂的bug。
      
周末提交app给apple审核，打包上传后发现点击“Submit For Review”总是出现下面的错误：
	
*Your app information could not be saved. Try agein. If the problem persists, cantact us.*

然后就遵循提示试了几次，还是出现相同的bug。Chrome抓包显示的是 **We've got a server error ... 500**  一度想等它恢复。 后来去so上看有木有遇到相同问题的小伙伴，还真有，不过大多是版本号冲突的原因，我也尝试着更改版本号，问题依旧。。

后来发现一韩国哥们也遇到了相同的问题，于是怀疑是不是包里面包含的中文字符导致不能识别，然后就发现了问题所在，**Product Name**不能为中文！之后把XCode-Targets-Packaging下的Product Name改成了英文，相应的在info.plist文件中修改Bundle name和Bundle display name为之前的中文app名。之后打包上传，顺利地Submit For Review了。

Apple， 还我为你折腾的这两三个小时啊！