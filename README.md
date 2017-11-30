[ ![jitpack](https://jitpack.io/v/MinnDevelopment/Java-DiscordRPC.svg) ](https://jitpack.io/#MinnDevelopment/Java-DiscordRPC)
# Java-DiscordRPC

This library contains Java bindings for [Discord's official RPC SDK](https://github.com/discordapp/discord-rpc) using JNA.

This project provides binaries for `linux-x86-64`, `win32-x86-64` and `darwin`.

If, on macOS, you get the following message which can be ignored: `Error in LSRegisterURL: -10811`

## Documentation

You can see the official discord documentation in the [API Documentation](https://discordapp.com/developers/docs/rich-presence/how-to).
<br>Alternatively you may visist the javadoc at [jitpack](https://jitpack.io/com/github/MinnDevelopment/Java-DiscordRPC/master-SNAPSHOT/javadoc/index.html).

## Examples

### Basics

The library can be used just like SDK. This means you can almost copy the exact code used in the official documentation.

```java
import club.minnced.discord.rpc.*;

public class Main {
    public static void main(String[] args) {
        DiscordRPC lib = DiscordRPC.INSTANCE;
        String applicationId = "";
        String steamId = "";
        DiscordEventHandlers handlers = new DiscordEventHandlers();
        handlers.ready = () -> System.out.println("Ready!");
        lib.Discord_Initialize(applicationId, handlers, true, steamId);
        DiscordRichPresence presence = new DiscordRichPresence();
        presence.startTimestamp = System.currentTimeMillis() / 1000; // epoch second
        presence.details = "Testing RPC";
        lib.Discord_UpdatePresence(presence);
        // in a worker thread
        new Thread(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                lib.Discord_RunCallbacks();
                try {
                    Thread.sleep(2000);
                } catch (InterruptedException ignored) {}
            }
        }, "RPC-Callback-Handler").start();
    }
}
```

> Note: To reveal this presence you have to start a window for your application.

### Community Examples

If you want to add an example to the [example directory](https://github.com/MinnDevelopment/Java-DiscordRPC/tree/master/examples)
you can do so by making a PR with your package and source.
All I ask from you is that the example does not abuse the API in any way and that your example can compile. 
If you would like to add examples in other JVM languages you must first add support via the `build.gradle` file.

For example, `examples/java/club/minnced/rpc/examples/MyGame.java` would be your example file relative to the project root directory.

## License

Java-DiscordRPC is licensed under the Apache 2.0 License. The base DiscordRPC is licensed under the MIT license.

## Contributing

Find something that is lacking? Fork the project and pull request!
