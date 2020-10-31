# A Documented Build Environment

This one is a stepping stone for later, but is actually really important to do as early as possible in any non-trivial project.

The gist here is:

You should have a list of every tool you use. List the tools, the versions you use, and how you use them.

Not just GCC, WHICH VERSION of GCC? The 2019-q4 release? Or some locally built custom fork? If possible, keep the installer (or a SHA of it) somewhere too.

You probably use more tools than you think!

* compiler
* linker
* debugger/tools like openocd/j-link
* binary utilities like objcopy/nm
* serial port tools
* and more

You also should have a "reference" environment, e.g. "Ubuntu 20.04" or "Windows 10 64-bit". Document it!

Now, you probably don't need to document the editor you use (unless it's an IDE, like IAR that is also a compiler), but you do probably want to document "side" things that are relevant, like your version of git, etc.
Now, in avionics, we would have periodic checks that EVERYONE had the right versions installed, especially when running offical tests, or building official firmwares.

This isn't usually necessary, but you might want to have one "reference" PC or VM set up EXACTLY like this.
Why? Because you should be doing all your "official builds" on this reference machine. Sending a unit for test? Shipping a firmware?

USE YOUR REFERENCE ENVIRONMENT!

This makes your firmware MUCH more reproducible!

If a firmware is acting funny, check if you are using the reference environment! Did you upgrade GCC? Or start using a different linker?

That might be the source of your problem! At least it's something to check.

Later on, we'll replace this environment with an official "build server", but having one clear reference PC or VM (or container) sitting around is good enough for now.

Keep this PC/VM somewhere safe after the project ends, too! You want to be able to re-create if needed.

If you need to update the firmware "with just one simple fix" in 2-3 years, or even 3-6 months, can you tell me what tools you used?

Do you still have the 2018-q2 installer for arm-none-eabi-gcc? Will there still be a reliable download link somewhere?

This is where VM snapshots or exported containers shine: You can put them on a redundant backup or cloud server somewhere, and take them off the shelf later. (Keep a copy of your VM tool too!)

Oh, and the libraries you use are part of this environment too! What version of LwIP are you using? The vendors BSP/HAL? that one library you randomly downloaded from github?

That should be either:

* In version control (that you own, not someone elses repo), OR
* In your docs

Plus, you have version control! Why not just put this in a text file in your source code repo? It's a good excuse to start a `docs/` folder. Call the file `software-development-plan.txt`, and impress your managers!

Plus then you can't lose it, it's with the code!

Going back to [@PhilKoopman](https://twitter.com/PhilKoopman)'s "Just enough paper", you don't need a fancy document or tool to capture this, use whatever you have already! Text file in a repo? Great! Page on your company wiki? Sure, as long as people actually look at that.

You gotta keep it up to date, though, and this is important! It's okay to change versions as you go, but make sure you always keep the doc up to date, and your "reference environment" up to date with what's in the doc!

You should double-check this on every "official" build.
