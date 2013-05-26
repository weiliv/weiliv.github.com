---
layout: post
title: "Modern Objective-C Syntax"
date: 2013-05-26 19:06
comments: true
categories: [DEV,iOS]
---

Apple Inc's WWDC2012 involved lots of new properties of Objective-C, helping developpers write code with much more efficiency. What a pity that it is so late since I learned about thar from the WWDC2012 conference videos published on itunes.

###Object Literals

That's awsome! Object literals enable you define NSNumber/NSArray/NSdictionary conveneiently. Let's review the tranditional method to define those objects:

``` objectivec
NSNumber * number = [NSNumber numberWithInt:1];
NSArray * array = [NSArray arrayWithObjects:@"one", @"two", nil];
NSDictionary * dict = [NSDictionary dictionaryWithObjectsAndKeys:@"value1", @"key1", @"value2", @"key2", nil];
```

Now the above code can be refactered as follows, 

``` objectivec
NSNumber * number = @1;
NSArray * array = @[@"one", @"two"];
NSDictionary * dict = @{@"key1":@"value1", @"key2":@"value2"};
```
So neat!:) There is no longer need to append disgusting "nil" at the end of array and now the dictionay KV object's key can be place first! More examples as follows:

``` objectivec
// integer
NSNumber *fortyTwo = @42;             // same as [NSNumber numberWithInt:42]
NSNumber *fortyTwoUnsigned = @42U;    // same as [NSNumber numberWithUnsignedInt:42U]
NSNumber *fortyTwoLong = @42L;        // same as [NSNumber numberWithLong:42L]
NSNumber *fortyTwoLongLong = @42LL;   // same as [NSNumber numberWithLongLong:42LL]

// float
NSNumber *piFloat = @3.141592654F;    // same as [NSNumber numberWithFloat:3.141592654F]
NSNumber *piDouble = @3.1415926535;   // same as [NSNumber numberWithDouble:3.1415926535]

// bool
NSNumber *yesNumber = @YES;           // same as [NSNumber numberWithBool:YES]
NSNumber *noNumber = @NO;             // same as [NSNumber numberWithBool:NO]

// array with zero object
NSArray * array = @[];                // same as [NSArray array]

// dictonary with zero KV
NSDictionary * dict = @{};            // same as [NSDictionary dictionary]
```
Aha, the code now are much more readable and beautiful~

###Tranverse Elements

While the ```for in``` and ```for(int i = 0)``` are familiar to us, and tranversing a dictionary is always anoyed us as:

``` objectivec
NSDictionary * dict = …
NSArray * keys = [dict allKeys];
for (NSString * key in keys) {
    NSString * value = [dict objectForKey:key];
}
```

Throw that away! Now you can tranverse elements with block!

``` objectivec
[lines enumerateObjectsUsingBlock:^(NSString * obj, NSUInteger idx, BOOL *stop) {
}];

[_urlArguments enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {
}];
```
See? The block also support NSDictionary!

###Subscripting Method

This new feature involed in WWDC2012 allows you get/set objects in array with bracket ```[]```. In other words, the previous ``` [array objectAtIndex:idx]``` and ```[array replaceObjectAtIndex:idx withObject:obj]``` can be rewitten as ```array[idx]``` and ```array[idx] = obj```. Here are some examples:

``` objectivec
NSArray * array = @[ @"111", @"222", @"333"];
for (int i = 0; i < 3; ++i) {
    NSLog(@"array[i] = %@", array[i]);
}

NSMutableDictionary * dict =[@{  @1: @"value1",
                                 @2: @"value2",
                                 @3: @"value3" } mutableCopy];
for (int i = 0; i < 3; ++i) {
    NSLog(@"dict[%d] = %@", i, dict[@(i+1)]);
    dict[@(i+1)] = [NSString stringWithFormat:@"new %@", dict[@(i+1)]];
}

[dict enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {
    NSLog(@"dict[%@] = %@", key, dict[key]);
}];
```

So the feature also works with NSDictionary. Moreover, you can provide this ```[]``` feature in your own class, just implmenting these two method:

``` objectivec
- (id)objectAtIndexedSubscript:(NSUInterger)idx;
- (void)setObject:(id)value atIndexedSubscript:(NSUInteger)idx;
```

In fact, this feature is not unfamiliar in other language, like C++ or Java. Maybe Objective-C was born in 1980s, it never improve that.
But, better late than never!~ Objective-C is always moving forward!
