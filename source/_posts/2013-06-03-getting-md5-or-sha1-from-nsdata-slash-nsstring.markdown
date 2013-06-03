---
layout: post
title: "Getting MD5 or SHA1 from NSData/NSString"
date: 2013-06-03 18:58
comments: true
categories: [DEV,iOS] 
---

Calculating the `md5` or `sha1` hash from `NSData/NSString` in iOS sdk is pretty simple. 

####Step 1:
The very first thing you need to do is import CommonCrypto’s CommonDigest.h

```objectivec
#import <CommonCrypto/CommonDigest.h>
```

####Step 2:
Here is Code （SHA1 from NSData）:

```objectivec
NSString *file = [[NSBundle mainBundle] pathForResource:@"offlineLoading@2x.png" ofType:nil];
NSData *data = [NSData dataWithContentsOfFile:file];
uint8_t digest[CC_SHA1_DIGEST_LENGTH];

CC_SHA1(data.bytes, data.length, digest);
NSMutableString* output = [NSMutableString stringWithCapacity:CC_SHA1_DIGEST_LENGTH * 2];

for(int i = 0; i < CC_SHA1_DIGEST_LENGTH; i++) {
    [output appendFormat:@"%02x", digest[i]];
}
```

SHA1 from NSString:

```objectivec
- (NSString*)sha1:(NSString*)input {
	const char *cstr = [input cStringUsingEncoding:NSUTF8StringEncoding];
 	NSData *data = [NSData dataWithBytes:cstr length:input.length];
 
 	uint8_t digest[CC_SHA1_DIGEST_LENGTH];
 
	CC_SHA1(data.bytes, data.length, digest);
 
 	NSMutableString* output = [NSMutableString stringWithCapacity:CC_SHA1_DIGEST_LENGTH * 2];
 
 	for(int i = 0; i < CC_SHA1_DIGEST_LENGTH; i++)
 		[output appendFormat:@"%02x", digest[i]];
 
 	return output; 
}
```

MD5 from NSString:

```objectivec
- (NSString *) md5:(NSString *) input {
 	const char *cStr = [input UTF8String];
 	unsigned char digest[16];
 	CC_MD5( cStr, strlen(cStr), digest ); // This is the md5 call
 
 	NSMutableString *output = [NSMutableString stringWithCapacity:CC_MD5_DIGEST_LENGTH * 2];
 
 	for(int i = 0; i < CC_MD5_DIGEST_LENGTH; i++)
 		[output appendFormat:@"%02x", digest[i]];
 
 	return  output;
}
```

