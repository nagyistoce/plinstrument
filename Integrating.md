### Example main.m ###

```
int main (int argc, char *argv[]) {
    NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
    PLInstrumentConsoleResultHandler *handler = [[[PLInstrumentConsoleResultHandler alloc] init] autorelease];
    PLInstrumentRunner *runner = [[[PLInstrumentRunner alloc] initWithResultHandler: handler] autorelease];

    if (argc > 1) {
        /* Run one case */
        id cls = objc_getClass(argv[1]);
        if (cls == nil) {
            fprintf(stderr, "No such class %s\n", argv[1]);
            return EXIT_FAILURE;
        }
        [runner runCase: [[cls new] autorelease]];
    } else {
        /* Run all instrument cases */
        [runner runAllCases];
    }

    [pool release];

    return EXIT_SUCCESS;
}
```