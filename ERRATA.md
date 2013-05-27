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

## Item 36

[Page 185]: The `numberI` variable should be sent `retainCount`. The code should read:

```objc
NSNumber *numberI = @1;
NSLog(@"numberI retainCount = %lu", [number retainCount]);
```
