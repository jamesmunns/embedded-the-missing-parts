# Reset Reason

Embedded systems, especially early in the development process, tend to reboot pretty often. They can reboot for a lot of reasons:

* A firmware update
* A hardware bug
* A software bug
* Low battery
* Coming in and out of sleep mode

And a lot more reasons!

The problem is, you can't always tell why. Your system was sitting there all fine, and then it blinked the "reboot" sequence, or you saw the boot screen flash for a second.

What was that? An off-by-one error in the code? Did the power supply hiccup? Have your device tell you!

Often, your CPU will have status register that reports some reasons. These often include:

* Power brown-out
* Debugger commanded reboot
* Clean Power-On
* Watchdog timeout
* Reboot due to exceptions

These are one cause for resets, but there are often others.

If you hit a `panic!()`, an `unwrap!()`, or a failed `assert` in your code, you may do a software reset that looks the same as rebooting into the bootloader.

For these reasons, you may also want to store some trace of this BEFORE you reboot.

Often, the easiest way to do this is to put it in a chunk of RAM that doesn't get initialized on boot. The idea is that:

* when you panic, write a flag to uninit memory
* Cause a soft reset
* On boot, check for that flag
* If you see it, report it then clear the flag

You also may want to write a "magic word" to memory, to ensure that you can tell the difference between a random set-bit in memory, and an intentionally set flag. Another option is to have a longer section of data, and include a CRC.

You should wipe the CRC or magic word on read.

By the way, I wrote a panic handler in Rust to manage this for cortex-m devices, it's called panic-persist. You can see it here: https://docs.rs/panic-persist

It stores the panic message to a reserved RAM section, so you can retrieve it after boot and send over a serial port or such.

But why are we doing this all AFTER the reboot? If we could set a flag, why can't we just log the error when it occurs?

Well for some reasons, like a power outage, we may not get a chance to respond! And if you overflow the stack, we might not be able to actually do anything.

So instead, we try to do as little as possible in the "bad" times, and instead push that to the next reboot, which is likely the next time our system is in a "reasonable" condition.

At this point, we can decide whether to print to UART, send over ETH, or write to flash.

Especially in the case of writing to flash, this is something that can take a comparatively long time! If we're relying on "reboot quickly to get to a safe state", then we don't want to be waiting for 10s or 100s of milliseconds to reboot! We want to go now!

I also talked a lot about *clearing* the message when we're done. This is also important to avoid any false positives, such as a "watchdog reset flag" that stays set even though we rebooted intentionally for some other reason.

Faults are another area where we may not be able to always see all information after a reboot unless we store it.

Certain faults, like an invalid memory access, division by zero, or secure zone violation can all be a sign of a software bug.

These are signs of a potentially serious software defect! We can register handlers for faults like a HardFault, and use that handler to write the fault status registers to RAM, so we can retrieve them after our next boot.

We can also grab even more information, like the contents of our stack pointer, link register, or even look at unwinding the stack to figure out how we got to the unfortunately place that we are right now.

This can be invaluable info when hunting down a heisenbug!

In the future, we'll talk way more about "post mortem" debugging, as well as having persistent logs to notice long term patterns or trends, like the system always rebooting unexpectedly exactly every 49.71 days.

We could choose to start our logging or diagnostic anywhere, but let's be honest: everybody boots. It's a good chance to gather more information in a seemingly benign situation, and to set us up with the ability to grab important info we may need for critical debugging later.
