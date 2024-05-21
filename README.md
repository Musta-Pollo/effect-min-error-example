# Reproduced error
## Terminal
```
 ERROR  ReferenceError: Property 'TextDecoder' doesn't exist, js engine: hermes 
    at ContextNavigator (http://192.168.1.242:8081/node_modules/expo-router/entry.bundle//&platform=ios&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:144902:24)
    at ExpoRoot (http://192.168.1.242:8081/node_modules/expo-router/entry.bundle//&platform=ios&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:144858:28)
    at App
    at ErrorToastContainer (http://192.168.1.242:8081/node_modules/expo-router/entry.bundle//&platform=ios&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:187561:24)
    at ErrorOverlay
    at withDevTools(ErrorOverlay) (http://192.168.1.242:8081/node_modules/expo-router/entry.bundle//&platform=ios&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:187064:27)
    at RCTView
    at View (http://192.168.1.242:8081/node_modules/expo-router/entry.bundle//&platform=ios&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:40811:43)
    at RCTView
    at View (http://192.168.1.242:8081/node_modules/expo-router/entry.bundle//&platform=ios&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:40811:43)
    at AppContainer (http://192.168.1.242:8081/node_modules/expo-router/entry.bundle//&platform=ios&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:40654:25)
    at main(RootComponent) (http://192.168.1.242:8081/node_modules/expo-router/entry.bundle//&platform=ios&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:118984:28)

```

## Screenshot
![Screenshot 2024-05-21 at 7 53 12](https://github.com/Musta-Pollo/effect-min-error-example/assets/68862752/504eae5d-0682-4b68-ada0-a29d5248924c)

# Changes

The only changes made were adding polyfill to the root layout and adding a button with effect code onPress.

**Added code:**
``` typescript
 <Button
        onPress={async () => {
          const program = Effect.gen(function* () {
            Effect.log("Deleted completions start");
            const a = yield* Effect.promise(async () => {
              Promise.resolve("a");
            });
            yield* Effect.log("Deleted inside completions");
            Effect.timeout("100 millis");
            Effect.retry(
              Schedule.exponential(100).pipe(
                Schedule.compose(Schedule.recurs(3))
              )
            );
          });

          try {
            await Effect.runPromise(program);
          } catch (error) {
            console.error("An error occurredbbbb:", error);
          }
        }}
        title="Click me"
      ></Button>
```

