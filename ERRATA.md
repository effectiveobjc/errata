# Errata

## Item 5

[Page 19]: Typo: `UIVewAutoresizing` should read `UIViewAutoresizing`.

[Page 23]: The switch statement is missing `case` before each section. The code should read:

```objc
switch (_currentState) {
    case EOCConnectionStateDisconnected:
        // Handle disconnected state
        break;
    case EOCConnectionStateConnecting:
        // Handle connecting state
        break;
    case EOCConnectionStateConnected:
        // Handle connected state
        break;
}
```

## Item 7

[Page 34]: The second bullet point in the paragraph about differences mentions that direct access to the ivar will retain the new value and release the old value. This is only true if under ARC, which of course, you should be using (Item 30). Without ARC, the old value will not be released and the new value will not be retain during the setting.

[Page 35]: There is an issue with the setter `setLastName:` which would result in an infinite loop if run. Instead, it should be:

```objc
- (void)setLastName:(NSString*)lastName {
	if (![lastName isEqualToString:@"Smith"]) {
        [NSException raise:NSInvalidArgumentException
                    format:@"Last name must be Smith"];
	}
    super.lastName = lastName;
}
```

Note how it is calling `super`'s implementation of `setLastName:` through the property accessor.

## Item 8

[Page 39]: There is a typo under 'Class-Specific Equality Methods'. The following is wrong:

 > Objective-C has is no strong type checking at compile time...

It should instead read:

 > Objective-C has no strong type checking at compile time...

## Item 12

[Page 63]: Figure 2.4 should have a fifth box on the right, with `IMPe` written in it. Then the arrow should point from `capitalizedString` to `IMPe` box, leaving nothing pointing to `IMPc`. On the following page in the first paragraph, it states that the selector for `capitalizedString` has changed, hence this change to the figure.

## Item 30

[Page 158]: There is a typo in the code. The final two portions of code should read like this:

```objc
@interface EOCClass : NSObject {
    id _object;
}
@end

@implementation EOCClass
- (void)setup {
    _object = [EOCOtherClass new];
}
@end
```

```objc
- (void)setup {
    id tmp = [EOCOtherClass new];
    [_object release];
    _object = [tmp retain];
    [tmp release];
}
```

## Item 34

[Page 174]: There is a typo in the final paragraph. Instead of "autorelease po00ol" it should read "autorelease pool".

## Item 36

[Page 185]: The `numberI` variable should be sent `retainCount`. The code should read:

```objc
NSNumber *numberI = @1;
NSLog(@"numberI retainCount = %lu", [number retainCount]);
```

## Item 37

[Page 188]: Bad choice of words in the following:

> This block defines a variable whose name is someBlock.

It would be better if this read:

> This code defines a variable whose name is someBlock.


## Item 39

[Page 202]: There is a typo in the `typedef`. It should read:

```objc
typedef void(^EOCNetworkFetcherProgressHandler)(float progress);
```

## Item 41

[Page 211]: When using `dispatch_barrier_async`, one should be sure to use a custom concurrent queue, rather than a global one. Therefore the code should look like this:

```objc
_syncQueue = dispatch_queue_create("com.effectiveobjectivec.sync", DISPATCH_QUEUE_CONCURRENT);

- (NSString*)someString {
    __block NSString *localSomeString;
    dispatch_sync(_syncQueue, ^{
        localSomeString = _someString;
    });
    return localSomeString;
}

- (void)setSomeString:(NSString*)someString {
    dispatch_barrier_async(_syncQueue, ^{
        _someString = someString;
    });
}
```
