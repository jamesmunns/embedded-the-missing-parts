# One Touch Builds

What does it mean to have a "one touch build"? Anyone on your team should be able to make a working firmware with one click, or one shell command or script.

This seems like a really simple thing, but if you have to:

* open the IDE
* change a config value
* copy the hex
* run a specific objcopy command
* edit the hex file in one specific place
* merge in some font data or images or something

Someone is going to make a mistake!

Open source tools like cargo, make, cmake, ninja, or whatever make this way easier to automate! Wrap it in a shell script or whatever you need.

IDEs like IAR are a little harder, but many still have a "headless" or CLI mode you can use, if you read the docs.

Have different configurations? RAM builds? Debug builds? Semi-debugging "whoops" builds?

Have a single command or button for each! You want to make sure your team is all building the firmware in the EXACT same way.

You'll notice consistency as a running theme here.
You want to eliminate potential sources of human error.

`make` or `make release` is a lot more direct than a 10 step list from an email 6 months ago.

Plus, you can put this in version control, and document it in your build environment!

Now when someone new starts on your team, or you bring in a contractor, you don't have to sit with them for a day (or week) to get their environment set up! Just send them the docs, and when "make release" works, they are good to go!

This step is also really important for later when we start talking about testing, or continuous integration. That's still a bit far off, but we're starting to take the first steps in that direction!

Again, what tool you use doesn't matter. Just do something, and document it!

Oh, and this script/makefile/cargo.toml or whatever is also important documentation!

It documents all the command line arguments you use in all of your tools, so you can't forget what flags you pass to your linker, or how you merge your image assets into the final binary.

This is documentation you don't even have to think about, but it's just as important as the tool versioning document you wrote.
