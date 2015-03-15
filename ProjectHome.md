PLInstrument provides a reproducible instrumentation library, modeled on xUnit. The library is intended to facilitate the instrumentation of performance critical code, and provide comparable results over the lifetime of the code base.

The library was developed primarily for the purpose of measuring code runtime on the iPhone, where CPU resources are significantly constrained.

### Examples: ###

#### An instrument class ####

```
@interface PLAffineInstrument : PLInstrumentCase @end

@implementation PLAffineInstrument
- (PLInstrumentResult *) instrumentMirrorTransform {
    PLIAbsoluteTime start, finish;
    int iterations = 25000;

    start = PLICurrentTime();
    for (int i = 0 ; i < iterations; i++) {
        CGAffineTransform mirrorTransform;
        mirrorTransform = CGAffineTransformMakeTranslation(0.0, 200.0f);
        mirrorTransform = CGAffineTransformScale(mirrorTransform, 1.0, -1.0);
    }
    finish = PLICurrentTime();

    return [PLInstrumentResult resultWithStartTime: start endTime: finish iterations: iterations];
}
@end
```

**Results:**

```
Instrumentation suite 'PLCoreGraphicsDemoInstruments' started at
    2008-12-20 18:51:46 -0800
Instrumentation case -[PLCoreGraphicsDemoInstruments instrumentMirrorTransform]
    completed (0.710000 μs/iteration) at 2008-12-20 18:51:46 -0800
Instrumentation suite 'PLCoreGraphicsDemoInstruments' finished at
    2008-12-20 18:51:53 -0800
```

#### Running all instruments in a project ####

```
#import <PLInstrument/PLInstrument.h>

// If not using ARC, add the appropriate calls to -autorelease
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        /* Set up the handler and runner */
        PLInstrumentConsoleResultHandler *handler = [[PLInstrumentConsoleResultHandler alloc] init];
        PLInstrumentRunner *runner = [[PLInstrumentRunner alloc] initWithResultHandler: handler];

        /* Run all instrument cases */
        [runner runAllCases];
    }

    return 0;
}
```

### Download ###

  * [1.0.1 Binary](http://plinstrument.googlecode.com/files/PLInstrument-1.0.1.dmg)

### Documentation ###

  * [Online API documentation](http://plinstrument.googlecode.com/svn/tags/plinstrument-1.0.1/docs/index.html)