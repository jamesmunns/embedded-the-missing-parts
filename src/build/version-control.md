# Version Control

You'd be surprised how many embedded teams out there STILL aren't using version control, either git, svn, hg, or whatever else. The tool you use doesn't matter, but version control is important for a couple reasons:
You need a way to refer to specific versions of your software on a change-by-change basis.

What version did you send to be tested? What is the version that is on the unit that is acting funny?

Is it the real 0.4.0, or did someone just forget to bump the version number?

You also want to see how your project changed over time, and at some point, figure out where/why some feature or bug got introduced. Version control is like having "save points" for your code, and let you easily "roll back time" to test things out, or go back to a known-good.

Version control also is a necessary building block for a bunch of the other things we are going to talk about later. Without having some kind of version control, this all gets much more complicated.

It's okay if you never learned these tools! A lot of universities didn't start teaching them until relatively recently (they didn't when I was in school 2007-2011).

But seriously, getting moderately good at version control is like learning a software development superpower.

Ideally, your whole team should learn a little version control, even the hardware and non-technical members of your team. This helps them work more seamlessly with your firmware team to test and verify things.

You should also talk to your team about how you use version control (or any tool, really), and come up with an agreed-upon approach.

When do you do merges? When do you rebase? Should the code on `main` always build? always pass tests? Is `main` only for production releases?

The actual strategy doesn't matter, but consistency does! So pick something, write it down, and change it if you need to! (this will be a recurring theme for the whole book).

[@PhilKoopman](https://twitter.com/PhilKoopman) calls this having "just enough paper", and I love this term and use it with my clients a lot.

Also, side note, you should go buy Koopman's "Better Embedded System Software" book. It is seriously one of the best references for introducing teams to important topics inside and outside of the safety critical domain.

It explains a lot of systems stuff you should know amazingly.

Speaking about "just enough paper", that brings us to the next topic!
