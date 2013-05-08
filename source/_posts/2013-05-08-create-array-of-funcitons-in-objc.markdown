---
layout: post
title: "Create array of funcitons in ObjC"
date: 2013-05-08 13:59
comments: true
categories: [DEV, iOS] 
---

A requirement as follows,    
We need to test all the DotNet Http request APIs in one simple tester app. But that will anoy me much if single API is called line after line. So an array constructed with API functions will walk around that duplication of labor.    
Here is the code:   
``` objectivec
+ (NSArray *)createAPIFunctionsArray {
	NSMutableArray *selectorArray = [NSMutableArray array];
	[selectorArray addObject:[NSValue valueWithPointer:@selector(initInfoWithBlock:)]];
    //...
    return selectorArray;
}
```    

It looks like funtion pointer in C, showing as @selector in ObjC. We use the selector array like this:  
  
``` objectivec
[XXX performSelector:[[selectorArray objectAtIndex:self.index] pointerValue] withObject:^(RequestStatusData *resultData){
	//code block as param here ...
	//...
	
}];
```	    

