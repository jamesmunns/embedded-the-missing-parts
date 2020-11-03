# Code Reviews

I'm going to break Code Review into two main kinds:

1. Continuous Reviews
2. Inspection Reviews

The first of these, you are more likely to have run into. The goal of Continuous Reviews is to focus on the (ideally small) changes that are made to the software, one at a time.

If you do reviews during a Pull or Merge Request on GitHub/GitLab, these are a good example!

By focusing primarily on the changes, you get a better chance to see "micro" level changes. Did someone change what a function does? Did someone add a bunch of new code? Does what they changed make sense?

These reviews are great for giving directed feedback (could this be done more simply? Is this adequately tested?), and for checking hard-to-see in the "large scale" bugs, like off-by-one errors, or forgetting to validate some input or check whether a pointer is null (in C/C++).

These reviews typically aren't always great for seeing "the big picture" though. Has this file gotten too large? Is some struct or class doing "too much" now? Does it still make sense with the original plan? That can be hard to see when it happens one change at a time.

The good thing is that these reviews are quick! You only have to review 10s or 100s of lines of changes at a time usually, which means you can get feedback from others quickly. That's a valuable part of the process! You should generally make sure every change gets a review first.

The other kind of reviews, Inspection Reviews, are less common outside of the safety critical industry, but I think they are a valuable counterpart to Continuous Reviews.

Inspection Reviews focus on the "macro scale", rather than the "micro scale".

This is a great place to look at your system architecture, where resources are being used, whether your software is consistent, whether there are large scale fixes needed that could cause problems for your project in the future.

Inspection reviews typically involve:

Having multiple people sit down with the code (on a shared screen or monitor, or even printed out on paper), as well as any documentation, architecture diagrams, or any other planning docs you have done.

You want to have someone "drive" and explain each part of the code, and allow the team to ask "why" questions, as well as considering "does this make sense?"

This is not the time for nitpicks, this is the time to put on your "systems engineering hat".

Think about how the different parts of your system interact with each other, either through message passing, function calls, shared memory, semaphores/mutexes, or any other way.

Is the system matching the original design? Or is it growing in an odd organic way?

Inspection reviews can take quite a bit longer to complete (hours, days), and will involve multiple people.

Because of this, it's hard to run them too often. I'd suggest AT LEAST doing this while getting ready for a big release (e.g. testing build, alpha release), or right after.

If it's a while before you release, maybe consider having one every 4-6 weeks or so, depending on how much is going on with your project. Sometimes it helps you from spending weeks working in the wrong direction!

For both kinds of review, there is one important concept to keep in mind: **Respect**.

You want Code Review to be valuable, and that means you want people to give honest feedback and you want people to be receptive to this feedback

You're working together for the project to succeed.

This is NEVER the place to make fun of someone's code, to insult what they have done, or any other unprofessional behavior.

Reviews are an AMAZING chance to share knowledge. If the reviewer can't understand the code/change, that's a sign you need better comments/docs!

Sidenote: I draw a distinction between "business formal" and "professional" in reviews here. Emojis are welcome! Shitposting is not.

You want people to *look forward* to reviews, because it is a chance for them to share what they've done, and get valuable feedback!

Also, new or "junior" developers can sometimes be the best reviewers! They are still learning, and that is a chance for established or more experienced devs to share knowledge, and to spend time looking at the changes they've made from a teaching lens.

I know I've caught bugs of my own while explaining why my code was right (spoiler: it wasn't) to an intern. We both learned a lot that day.

Sure, senior devs can help you see things too, but any second set of eyes will bring a positive influence to your code.

You should also automate any steps of the review you possibly can.

* Code formatting? Automate it.
* Code LINTing (we'll talk about that later)? Automate it.
* Do the tests pass? Automate it.

Review time is EXPENSIVE, use it on the important stuff that you can't automate.

This also helps to retain as much of your "frustration budget" as possible. People are more interested in getting interesting feedback.

Getting 37 comments about "this should be 4 spaces" is a good way to sap all the energy people have on reviews, and bring you no value.

I find that there are a few common obstacles to effective reviews:

* Not enough time
* Not enough (experienced) people
* Not sure what to do during a review

These are all valid in some cases, but reviews are *IMPORTANT*. You need to make room for them.

Regarding "Not Enough Time": This is largely a management issue. The time it takes to do a review, and iterate on the feedback, should be included in time estimates. You should include them, and your manager or client should expect and respect them.

Code reviews help you catch issues EARLY, before they get buried deep into hard-to-reproduce situations. They help you spend less time testing, less time debugging, and less time recalling devices from the field.

(Good) managers think a lot about risk, in the context of a project. Reviews are one of the best ways to significantly de-risk a project, with respect to defects, and schedule slips.

It's easier to budget 10% extra time for code reviews, than an "oops" 60% schedule overrun.

Regarding "Not enough (experienced) people", this is one I unfortunately see on a lot of embedded projects. It happens the most when you only have one "main" developer on a team, and maybe 1-2 people who sometimes help out.

This is a dangerous place to be (speaking to managers).

Ideally, no project would only ever have one developer on it. It makes projects risky to that developer getting sick, or getting pulled off to some other urgent project.

Still, if you find yourself in this situation, there are options:

If your company has similarly skilled embedded people, just on another project, consider finding a "review buddy" on another team. You should learn about their project, and they should learn about yours.

In a pinch/crunch, you can also join the other project on short notice.

If you are the ONLY embedded person in your team or group, this is also a liability. Try to find someone, anyone, who is interested in embedded or the kind of project you are working on. Can't find anyone? Insist your manager does reviews with you until they hire someone else.

These reviews will be like having an intern review your code: You should explain EVERYTHING to them, the goal isn't necessarily for them to make good comments, but to instead ask questions that make you think about your code in a new way, maybe uncovering bugs.

Regarding "Not sure what to do during a review": This one is totally valid! If you've never worked on a project with good reviews, it can be hard to feel like you're "doing it right".

When in doubt, just have a conversation about your changes!

Ask about things you don't understand! Anything you talk about would probably be good as comments for the long term as well.

See something that smells funny? Ask about it! Chances are you'll catch something odd, or something that could be more clear. Or you'll learn something!

Also, take note of the kinds of questions that were valuable in a review.

Eventually, it will make sense to collect these into a review checklist you can use to help jog your memory and ask good questions.

Do all pointers get checked for NULL? Is input always validated?

This checklist is also a great training tool for new interns. Eventually I'll write a good baseline checklist to start with, but often, the details of your project makes it hard to have a "one size fits all" checklist. What works for avionics may not work for consumer devices.

Really at the end of the day, the usefulness of reviews boil down to a one thing:

It's good to have different perspectives of the code that you write.

You get one for each of:

* You write it
* You test it
* You explain it to someone else
* Someone else gives their perspective

Reviews give you a chance for (at least) two new perspectives on your code (per reviewer!). Don't waste these! Going from one perspective to three is a *huge* strategic advantage, and increases the chances of getting things right the first time.

So! That's reviews. Think about this the next time you are tempted to just type "Looks Good To Me" and hit merge without thinking about it, or when you think about committing directly to `main` without opening a PR first.
