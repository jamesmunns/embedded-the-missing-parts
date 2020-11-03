# Eyeball Debugging

This one is the easiest form of debugging, because you don't need any special equipment. Usually you just need one or more LEDs, or the usual output of your system (motors, speaker, etc.)

Although your eyes aren't a great way to debug super fast things, like whether you are sending the correct data or not, there is a lot you can tell with just your eyes.

For example, you can blink an LED to show that the device is still alive, or performing some task.
If the LED stops blinking? You know it got stuck somewhere.

Have a couple LEDs? Make one blink for each task. This way you can see if just one part of your system is getting stuck.

Try and use something dynamic (like a blink) so you can still see "signs of life".

Another clever trick to see "how far in a sequence" a device gets is to use a couple LEDs to output a binary value that represents a state in a state machine.

If you have two LEDs and four states:

| LED1 | LED0 | State |
| :--- | :--- | :---- |
| Off  | Off  | 0     |
| Off  | On   | 1     |
| On   | Off  | 2     |
| On   | On   | 3     |

This doesn't work for states that change rapidly (faster than your eyes can see), but if your system is getting "stuck" somewhere, walking through the states in your state machine can show you exactly where it stopped!

It also really helps to take notes about what you change, and what the effects are. As soon as you take notes, its a science experiment!

Try to guess what the effect of the changes in your code will make on the resulting output. This way you can verify if that was correct or not.

Later, we can reuse these same techniques along with tools like an oscilloscope or logic analyzer to see even faster changing patterns.

You can also use things like RGB LEDs to show colors, which can help simplify things. "green" is easier to remember than OFF, ON, OFF.

You can also get secondary colors in there too, without even worrying about fancier techniques, like PWM:

| Red | Green | Blue | Color |
| :-- | :---- | :--- | :---- |
| Off | Off   | Off  | Black |
| Off | Off   | On   | Blue  |
| Off | On    | Off  | Green |
| Off | On    | On   | Turqoise |
| On  | Off   | Off  | Red |
| On  | Off   | On   | Purple |
| On  | On    | Off  | Orange/Yellow |
| On  | On    | On   | White |

Note: some people are colorblind, and so RGB is less useful. It's always good to know if anyone on your team has troubles with things like this, so you might choose to use 3 discrete LEDs instead of an RGB if this impacts your team.

Also good to keep in mind for released products.
On a recent project, I had a wireless device getting "stuck". I didn't have a debugger attached, so I wanted to see the problem at a glance.

I used Red with a long blink for "background task", Green for "Received Packet", and Blue for "Sent Packet". Short Red blink was for errors.

I could watch it blink through colors at different rates, and realized there wasn't a problem with my wireless device at all! It stopped receiving messages at some point. The problem was actually on the OTHER wireless device connected to my PC!

This problem was really intermittent, sometimes taking 1-6 hours to show up. But because it was blinking LEDs, I could just look from my couch whenever it was acting funny, and see exactly what was going on.

I wrote a library in Rust to help out with this called [`blinq`](https://github.com/jamesmunns/blinq).

It also lets you do stuff like have patterns with different lengths (short, long, medium blinks), or even stuff like morse code!

You can get a surprising amount of info from simple LEDs.

But in summary, it's helpful to think about the tools you have for observing at hand already, and to think about your expectations for how your system works vs. how it is actually behaving.

Sometimes you need fancy tools, sometimes a simple LED is even better.
