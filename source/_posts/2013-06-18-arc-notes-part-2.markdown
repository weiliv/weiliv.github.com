---
layout: post
title: "ARC Notes - Part 2 - More about Lifecycles"
date: 2013-06-18 18:39
comments: true
categories: [iOS] 
---

## *__strong*

Let's talk over the vars declared as *__strong* or declared default as *__strong*.

```objectivec
{
    /*
     * create object and take its ownership
     */
    id __strong obj = [[NSObject alloc] init];

    /*
     * obj always has ownership inside here
     */
}
    /*
     * out of the lifecycle, the object of obj has been released
     */
```

```objectivec
{
    /*
     * create object but has no ownership
     */
    id __strong obj = [NSMutableArray array];

    /*
     * because obj is __strong, it hold the object
     * compiler put obj in autoreleasepool
     */
}
    /*
     * out of the lifecycle, the object of obj has been released
     */
```

Though the var *obj* is not obtained by *alloc/new/copy/mutableCopy* functions, the *obj* is *__strong* reference and can be holder. That's all what compiler do.

In addition, var's default type is *__stong*, and if it reference to new values, the old one is released. *obj = nil* means release current object referenced by *obj*.

## __weak

### about compiler

And the *__weak* vars is put into *autoreleasepool* when it is being used. Why? Just turn to its definition~ What will happen when the object referenced by weak vars is released/destroyed? That'll crash! So the compiler put it to an autoreleasepool to solve the problem.

### retain cycle

In most of the situations, it's enough to user *__strong*. But when it comes to **retain cycle**, *__weak* takes its place.

### outlet

There's an advantage when outlet user *weak property*. It can simplify the process of viewDidUnload/didReceiveMemeoryWarning.

```objectivec
@property (nonatomic, strong) IBOutlet UILabel *label;

-(void)didReceiveMemoryWarning {
	[super didReceiveMemoryWarning];
	if (self.isViewLoaded && !self.view.window) {
        self.view = nil;
        self.label = nil; // release object
    }
}
```

If no *self.label = nil* here, there is a leak of memory. But when use weak property:

```objectivec
@property (nonatomic, weak) IBOutlet UILabel *label;

-(void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    if (self.isViewLoaded && !self.view.window) {
        self.view = nil;
        //nothing to do here...
    }
}
```

But not all outlet should use weak property, such as self.vew … It depends.

