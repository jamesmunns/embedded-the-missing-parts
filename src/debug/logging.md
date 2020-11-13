# printf/log Debugging

This is actually one of the most common debugging techniques used in practice, and while it tends to get a little bit of shade sometimes, there's a surprising amount of depth here.

When developing desktop programs, it's super common to just "print to the console" when you are developing. Whether that is messages like "Here", "Here2", "why are we here", or more structured logging with timestamps or line numbers, this is printf/log debugging!

Now, most embedded systems don't have a "console" like a desktop program, but the status quo here is typically using a serial port or UART to print text, and to use a USB-to-Serial adapter to see this logging information.

Most development boards have this built-in these days.

For the most part, this is pretty equivalent! You "print" text in your program, and you see the output on a command prompt (or console window) on your PC.

Many RTOS or other environment also automatically wire this up to `printf` in C, so you might not even have to set it up.

BUT, since we don't have an operating system like Linux managing our output console, there are a couple "gotchas" to use print debugging on embedded systems.

For one, the serial/uart link is generally relatively slow. Often this is 115200 baud.

Now, 115200 is more than fast enough for humans! I guarantee you can't read that fast. BUT, if you dump a lot of text to the console in a loop, this means your embedded system will do one of two things:

1. It will block (or wait) until it is done sending the message
2. It will buffer up the message you add, and slowly drain out the message in the background (if you have something to manage this for you with DMA/interrupts or an RTOS task.

The problem is that at 64MHz, it takes 22k times longer to send the data than to just copy memory.

Even if you have buffering, eventually your buffer will fill up, and then you have to wait, or lose messages!

If your system blocks to send, then you can introduce unexpected delays in your program that don't exist when you aren't printing. This can cause timing heisenbugs:

"When I print, it works! But when I don't print, it doesn't work!"

This is often because the delay from printing is enough time for something to complete, or for a message to be actually received, etc.

This is really confusing when it happens to beginners OR experienced folks!

Also, it's important to realize that formatting (in C, C++, AND Rust) is relatively "expensive" to do!

There's a huge difference between `printf("Hello")` and `printf("Hello %0.02f", 42.0)`, in terms of memory usage and runtime speed.

This adds to the timing instability or performance loss, and can significantly increase your code size.

There are often "formatting alternatives" or "lightweight printf" options, but they often reduce functionality (for better or worse).

Okay, but the story isn't all bad. To it's credit, there are a couple things print/log debugging can do that is MUCH better than alternatives, like debugging with GDB.

Firstly, the knowledge required for this is much lower, and more accessible for beginners.

The amount of clarity those simple "here", "here2", "lol", "what" messages can give when you are trying to figure out what a program is doing is amazingly powerful!

Plus you can take that log output, and analyze it "after the fact" to follow what your code was doing, and why!

Like with LED debugging, this works best using the scientific method:

* Guess at what your system is doing
* Add print statements
* Run, and analyze the results
* Repeat until everything is clear

I do this *all the time*, on embedded and on desktop!

Second: Debuggers like GDB typically require "stopping the world" to step through code at "human speed". This means that a lot of odd things can happen:

* Interrupts may not fire as normal
* You might miss messages or events
* Hardware can "time out"

(as a note, it is often possible to write gdb scripts to alleviate these issues, but I'd consider this fairly "arcane knowledge" still, unfortunately).

This isn't to say GDB debugging is bad, but it's just a different tool with different qualities to print debugging!

With print debugging, even with a slow UART, it is way less "timing intrusive" to log-as-you-go, and see the ordering of what your system does at "full speed".

For example if you are doing USB or Bluetooth, these protocols are very timing sensitive!

If you hit a breakpoint and pause for three seconds, you've totally lost your USB/BT connection! Now you have to reconnect to debug further.

With printing, you can probably log and move on, without disrupting the more timing sensitive parts (as long as you don't log too much).

Especially when combined with an accurate timestamp (provided by the device), this "sequential log of information" can make-or-break tracking down hard bugs.

In terms of mediating the cost of print debugging, there are a couple "power user" techniques that are great to add to your toolbox. At least for Cortex-M devices, these include:

* Semihosting
* ITM/SWO
* RTT
* Memoized logging
* Deferred Formatting

Semihosting is a technique of "logging over the debugger", instead of over a serial port.

From a plus side: It's very easy to implement, and if you have a JTAG/SWD debugger, you don't need a separate serial port! All these messages just go through the debugger.

The downside is it is SLOOOOOOW, especially with cheap debuggers. Sometimes taking 100s of ms just to send a single message!

But it can be useful for debugging before you even get a serial port working, and works on any Cortex-M chip!

The second is ITM/SWO, which is like a "debugger accelerated serial port". The upside is that it is MUCH faster than Semihosting, and only a bit more complicated to use.

The downside is it requires debugger HW support (the cheap ones usually don't), or a separate fast USB-UART.

But you can often drive these incredibly fast, definitely above the MHz speed, sometimes up to the 10s of MHz, which means the logging delay (without formatting) is much lower.

The third is RTT, which is somewhere in the middle.

Like Semihosting, it operates over your existing SWD/JTAG connection, so no additional HW needed. However it uses a more efficient technique (a ringbuffer on your MCU memory) to have much better performance.

Memoized logging is a technique that requires a little setup, but can help if you really need to log a lot of info.

Basically the idea is instead of printing something "human readable", you print a short "message ID" instead.

So instead of "Hello, world!", you just send the number 1.

An easy way to do this is to use an enum for "status codes", so you never log text, just numbers! This means you don't need `printf`, and send way fewer bytes over the wire.

The downside to this is that (typically) this requires a little more planning than just `printf`, because your device and host both need to speak a common protocol!

The last item here is "deferred logging". The gist here, is instead of formatting `printf("Hello: %0.02f", 42.0")` on the device (which takes time and resources), you just send "Hello: %0.02f" and 42.0 over the wire, and let a more powerful system do the formatting for you.

If you're developing embedded systems in Rust, you're in for a treat! Ferrous has a tool called `defmt` which does memoization and deferred logging for you automatically!

We also have a tool called `probe-run` which handles setting up RTT for you as well!

You can [check out the tools here](https://knurling.ferrous-systems.com/tools/), to get an idea of how they work.
