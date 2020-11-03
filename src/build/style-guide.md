# A Style Guide

This one tends to get a bit contentious, but it doesn't have to be! A style guide covers a couple things, mostly how you format your code (tabs, spaces, etc).

Now, I'm going to start off by saying: the actual style you choose doesn't really make sense. Tabs, spaces, same line brackets, whatever. Do whatever your team can agree on.

But be strict about it! The rules can change over time, but the key here is (again) consistency!

Basically, if you ever have a debate about how to format something, just immediately talk to the team, make the decision, and stick with it.

You should definitely write all your decisions down somewhere everyone on your project can see.

`docs/style-guide.txt` is a good start!
Now, there are also now tools to automate this, so your team doesn't do it manually.

For rust there is `cargo fmt`, for C/C++ there are tools like `uncrustify` or `clang-tidy`. Find one that automates the things you want, or has a default you (mostly) like!

And make sure you have "one touch formatting" as well! In Rust, this is usually just `cargo fmt`, but for C/C++ you might want to have another script that does this for you, so everyone knows how to do it.

Set the expectation that every change in version control is formatted.

Having a tool to do this is a "great equalizer", it's not person A vs person B's opinions, you just do what the tool does. It takes the thought off the problem.

It also makes it easy to document your style: It's documented in the config file for your formatting tool!
Having a consistent code formatting helps in a couple ways:

1. It makes odd things harder harder to hide. You'd be surprised at how good people are at noticing something "looks odd", even if they don't know why! Capitalize on this.
2. It reduces unproductive discussions during code review. Don't waste time making 20 formatting nitpicks, just one "run the formatter" comment, then don't talk about format in code review!
3. It makes changing the style the same as changing your code: If you want to change the rules? Okay! Open a pull request to change the formatter config file, and discuss the pros/cons there. Now you have a record of why decisions were made too!

As a note: you should probably have team policy on what to do whenever you change the formatting rules.

Do you fix the whole repo at once? Or do you reformat things as you go?

I have a preference to the first, but your team may disagree. Write down your decision!

Code formatters aren't perfect, and you may want to exclude some files or blocks of code from automatic formatting. This is pretty easy to do with most formatters, but make sure you have a good reason before doing it!

Code formatters also have the side effect of "touching" lots of your code at once.

I suggest keeping 'code formatting' as separate commits or separate pull requests to any functional change to the code. This makes the changes easier to review.

I tend to make my functional changes, open a review, get feedback, then do a formatting commit before merging the PR. This works for me, but find something that works with your team!

While you're writing down style, you might also want to think about documenting how you do other things in your project consistently:

* What do your commit messages look like?
* What do your issues/tasks look like?
* How are files and folders named?

These are things that are harder to automate, but worth writing down! Again, consistency is the most important policy, so decide *something* for now, and write it down!

Any time you have a "should we do X or Y" discussion, WRITE IT DOWN and never have it again.

Don't worry about capturing everything at the start, just keep it up to date! Again, this helps bring people on to your team smoothly, and will make your code reviews (we'll get to those later) a bit more stress free.
