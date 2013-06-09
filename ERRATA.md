# Errata

## Item 5

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

## Item 8

[Page 39]: There is a typo under 'Class-Specific Equality Methods'. The following is wrong:

 > Objective-C has is no strong type checking at compile time...

It should instead read:

 > Objective-C has no strong type checking at compile time...

## Item 36

[Page 185]: The `numberI` variable should be sent `retainCount`. The code should read:

```objc
NSNumber *numberI = @1;
NSLog(@"numberI retainCount = %lu", [number retainCount]);
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
