# Compiler Warnings

Compiler warnings are a very important thing to manage, especially for languages like C/C++ (vs Rust) where safety is a bit more optional.

For greenfield/new projects, you should start your project with as many warnings turned on as possible! This is valuable feedback, and the compiler trying to save your butt while you're developing!

I highly recommend this post from [@MemfaultHQ](https://twitter.com/MemfaultHQ) as a good primer if you're not sure what to set:

https://interrupt.memfault.com/blog/best-and-worst-gcc-clang-compiler-flags

It talks about which set of warnings and errors have the best relevance, as well as "signal to noise ratio".

Add these to your "one touch build" scripts!

Furthermore, I'd like to reinforce that by default, you should treat all warnings as errors (-Werror in C/C++). AT LEAST in your "official builds", but it is a good habit to keep. You can always disable warnings on a line-by-line or file-by-file basis.

In Rust, I'd generally suggest `#![deny(warnings)]` for APPLICATIONS, but not for libraries. Once you have automated testing, this is something that can be configured on the command line as well.

This thread is a good discussion about this:

https://www.reddit.com/r/rust/comments/f5xpib/psa_denywarnings_is_actively_harmful/

For legacy code, or code "out of your control", like LwIP, or vendor provided HALs, warnings are a bit more complicated.

I'd suggest you run the code at least once with the "strict" warning settings, and spend a reasonable amount of time reviewing these.

This is part of the due diligence required when using someone else's code! Your customers don't care if the bug was in someone else's code, it's in your hardware!

You should fix any bugs you find (and report or PR the issue/fixes!), and repeat this any time you update the lib.

Once you've done this, you might want to change/reduce the warning levels for these library files, but retain the warnings on the rest of your code.

This isn't okay for avionics, but I'd say it's "above average" for consumer projects.

Ideally if you use C/C++, you should try having your code built by at least two compilers. This is useful for a couple reasons:

1. It keeps you honest about writing portable code.
2. Different compilers have different warnings. You want all the info you can get!
3. When using GCC/Clang, this is a great way to get a "second opinion" for free (in terms of cost).

That being said, sometimes it can be some work to keep code totally portable, and building on both platforms. You can cheat a bit with your second compiler...

For example, if my main compiler is GCC, I don't really care if Clang produces a working binary at the end (that's a 'nice to have'). This can help with some of the painful portability details - just stub out the hard parts for now, and clean up later if you can.

If you are using a proprietary compiler, having a (mostly) working build using open source tools is also a good backup plan, if you decide to switch off that platform in the future.

OSS compilers also tend to update at a faster rate, so you can try to get new warnings.

Going back to disabling "noisy" warnings, wherever you do this (in a Makefile, in the code), you should get in the habit about documenting WHY you are disabling this, and if possible, what would need to change to re-enable this in the future.

`// Disable noisy warning` isn't useful to anyone.

`// Disabling W0234 warning as we read 'uninitalized' values from RAM, which are placed there by the bootloader. This can be removed if we change to XYZ configuration scheme` is useful to EVERYONE, even if you never remove that.

My opinion is: You can always break or bend the rules, but you should always expect to justify why. It should take more effort to break the rules than to follow them, so you only break them when it is absolutely necessary.
