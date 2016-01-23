---
layout: post
title: Redesigning vs. Refactoring Software Solutions
categories: [Refactoring, Analysis]
tags: [Redesigning, Refactoring, Analysis, Maintainability]
---

##Introduction
Many of the terms we frequently use to communicate software development ideas, concepts, and techniques, tend to be overloaded and generalized.  This often results in the misuse of the term which decreases the clarity of the problem, solution, or task at hand.  Recognizing this give us an opportunity to restate what we actually mean when we use the term and what we would actually like to achieve.  

More often than not, when we do this, we realize we meant something else entirely.  Implicit meaning and their benefits are further realized when focusing on the contextual usage of the term.  Communication clarity increases with specific intent yielding better practices or solutions.

While the list of terms that fit this criteria are endless, I'd like to focus on the term *refactoring*,  what it means, how it is overloaded, and then relabel one of its usages with a different term to encapsulate a different set of activities that result in similar benefits to that of *refactoring*. 

##Overloading the term 'Refactoring'

A non-exhaustive list of ways I've heard the term *refactoring* used in practice:

- To improve a design
- To replace a design
- To change the UI
- To change a process
- To make any change to existing code
- To replace a solution
- To change the requirements
- As a feature on a Product Roadmap

What is interesting is that in all cases, the term *refactoring* is used to refer to a beneficial change to the software.  All of them have the motivation to increase some qualities of the software to provide some value.  If that is the common denominator, one could argue that you could call anything and everything in software development *refactoring* as long as it delivers value.  That is a pretty overloaded term!

##Defining 'Refactoring'
*Note: This is a brief summary of Refactoring, only highlighting a few key properties to provide a basis for the rest of the post.  It is not by all means an extensive list of where, when, and why to perform Refactorings.*

While there are many formal definitions with slightly different wording, I'd like to use Martin Fowler's definition from his [Refactoring Book](http://www.amazon.com/Refactoring-Improving-Design-Existing-Code/dp/0201485672/ "Refactoring Book"):
>A change made to the internal structure of software to make it **easier to understand** and **cheaper to modify** *without changing its observable behavior*.

If we adhere to this definition strictly, any change without the intent to increase the maintainability of the software or any change that alters the behavior of the software is **not** a *refactor*.  Why should we care about making this distinction?  As I was stating above, we'll be able to maximize its benefits by narrowing the focus of our efforts.

Maintainability, and as a result *refactoring*, are purely an economics concern.  If you only ever need to build something once, and the size of the thing you're building is small enough to manage with incurring some short term costs, then *refactoring* is probably something you should avoid doing.  This would also be the case in a scenario where there's a system, part of a system, etc. that doesn't change very often.  Why make a change if the value of the change isn't greater than the cost of the change?

As most of us have experienced, software systems tend to live long lives in ever-changing environments that require software to actually be *soft*.  If we're going to have to change the software in the future, we should be making changes in such a way that make modifications easier, or at least, not harder.  As Kent Beck put it so well in Martin's [Refactoring Book](http://www.amazon.com/Refactoring-Improving-Design-Existing-Code/dp/0201485672/ "Refactoring Book"):

>If you can get today's work done today, but you do it in such a way that you can't possibly get tomorrow's work done tomorrow, you lose.

*Again I want to emphasize that not everything in the software is equally important nor require this constant nurturing.  How to uncover those details, balance various tradeoffs, or create boundaries to encapsulate and isolate those details are outside the scope of this post.*

The practice of *refactoring* makes the software more maintainable by improving its internal quality or reducing its cost.  This is achieved by:

- *Preventing design decay:*  Design decay occurs when changes are made that decreases the clarity of the design and its intent.  This makes it harder for developers to understand where, what, and why a change should be made.
- *Removing accidental complexity or artificial constraints:*  The current shape or structure of a design was created with less information then what is currently known.  As a result, there could be a misalignment between the problem domain and solution domain.  A *square peg, round hole* situation.
- *Ensuring [technical debt](http://martinfowler.com/bliki/TechnicalDebt.html "Technical Debt") stays below the [payoff line](http://martinfowler.com/bliki/DesignStaminaHypothesis.html "Design Payoff Line"):*  If the debt of a solution crosses that payoff line, it becomes impossible to payoff that debt.  Two indicators we've crossed that line are exponentially growing timelines or the development staff using phrases like "It would be faster to just build this from scratch."
- *Providing opportunities to realize implicit concepts:*  Software development is a learning process.  If we have a better understanding of the problem domain we're in now than when we did when we created a design, it's in our best interest to share and document that knowledge in the design.  The debt accrued here could be correlated to the *prudent* and *inadvertent* quadrant in Martin's [technical debt quadrant](http://martinfowler.com/bliki/TechnicalDebtQuadrant.html "Technical Debt Quadrant").

While there are many different reasons you might decide to do a *refactoring*, I'd like to focus on the case where you're adding new functionality and enhancing the software in some way.  Kent Beck has a very nice metaphor for this case and the process one should follow.  There's 2 hats; one of which is the *refactoring* hat while the other is the *adding function* hat.  They are mutually exclusive meaning you can only ever wear one hat at a time.  The trigger to switch to the *refactoring* hat is if you try to add something and the existing structure makes the process awkward in some way.  At that point, one should stop and modify the existing structure to make the new code easier to add.

##Shifting focus

If we can now agree what *refactoring* is and isn't, how can we define changes to the software that also are about maintainability (among other things) but **do** change the behavior of the software?  Is this something we would even care about?  When and why would we be doing this?  I would argue this case is just as important, if not more important, than what *refactoring* accomplishes.

It's ironic that as a group, we (in the software development community) are very logical, yet when we review existing code, we make 2 illogical assumptions:

1. The solution, code, etc. is crap
2. The code, logic, etc. is correct

While I'm not going to dig into that first assumption here, I *think* both of those assumptions *might* be triggered by the same thing - it's the easiest thing to do.  It requires time to understand a solution and the context in which the critical decisions were made.  If *you* never heard anyone complain about it, it **must** be good..right?!

Coincidentally, I came across a [post](http://www.sandimetz.com/blog/2016/1/20/the-wrong-abstraction "The Wrong Abstraction") by Sandi Metz a few days on a different topic in which she made the same assertion:
>Existing code exerts a powerful influence. Its very presence argues that it is both correct and necessary.

We tend to assume what we wrote met the business needs, it's what they wanted, and that will always be the case.  More often than not, if we just asked them, we might hear that they don't like the way it is, never did, or how it's not applicable anymore.

Making these false assumptions causes the same problems, from a maintainability perspective, that a development team not employing *refactoring* techniques does, but on much a larger scale.  Instead of building solutions with workarounds and patches driven by false technical complexity, they are driven by false domain complexity.  If we're going to make any assumptions, I would prefer to assume everything is wrong.  Not as a driver of negativity, but rather a driver to have conversations with our business stakeholders.

One of the main reasons a developer would be reviewing existing code would be they were just tasked with enhancing the software in some way and they needed to figure out how to do it.  Instead of making those false assumptions during the review process, they could take the opportunity to perform a little analysis to educate themselves with both the domain and the context in which the existing solutions were applied.  Here are some things that they might find out if they just asked: (literally all of these have happened to me just in the past few months)

- The business has evolved or customer needs have changed rendering the existing solution ineffective or outdated.
- The addition of the feature or change fundamentally changes the problem we've already solved rendering the existing solution ineffective, outdated, or just invalid.
- The basis of the existing solution was a less-desirable solution for the business but was chosen as a result of other pressures such as time to market.
- The person asking for the enhancement might not be aware of the properties and side-effects of existing solutions and how they relate to what they are asking for.
- Our domain knowledge has reached a level where we realize the existing solution isn't what we should have done to begin with.  This is also that *prudent* and *inadvertent* quadrant in Martin's [technical debt quadrant](http://martinfowler.com/bliki/TechnicalDebtQuadrant.html "Technical Debt Quadrant").

In all these cases, there are multiple behaviors of the system that we *do want to change* that will *bring value* to the business and if we don't change, will hurt the *maintainability* of the software.  I thought a while about a term or set of terms I could use to communicate a practice that would describe this case.  What I came up with was the *3 R's*.

##Revisit, Restate, Redesign

What I am proposing is, when enhancing software, *revisit* the related problems,  *restate* those problems with the addition of the details of the new problem, then *redesign* the solution or set of solutions to better align with the current state of the business.  This could be the **third hat**!

I came across the perfect definition of *revisit* from [Google](https://www.google.com/?gws_rd=ssl#q=Revisiting "Revisiting Definition") to convey my idea:
>consider (a situation or problem) again or from a different perspective

It's so important for us to do this.  We're the ones that are seeing all the details of the current solutions and there's a reason people tend forget all those details.  Businesses and their rules are complex!  It's our responsibility to bring visibility to those details, ask questions, and start the conversations.  Question everything!  The existing design!  The existing requirements!  The existing problems!  Heck...even the new problem!  

*Restating* the problem(s) allows us to view the problem space different thus allowing us to explore different solutions we might not have realized.  Accidental (or outdated) constraints can be removed that were causing friction or imposing too much complexity in a solution.  Additional constraints could be added to break apart and isolate problems further simplifying domain complexities.  *Redesigning* the solution to re-align with those problems might be a minor change or an entirely different solution altogether.

If the new problem and existing solutions have a relationship, this process will yield a more holistic solution.  Most importantly, it delivers value in two ways: by increasing its maintainability and by delivering a refined solution to the business optimizing for what they care about!  

##Summary

There were 2 important ideas I was hoping to convey:

1. The maintainability of a system isn't just about the quality of the technical artifacts.  While we have many architectures, patterns, and techniques for increasing maintainability such as *refactoring*, it's hard to beat iterative analysis over the life-time of enhancing a system.  More often than not, it is the basis on how we should divide our systems and the problems it's solving.  Low level tactical techniques such as *refactoring* won't be as effective without higher level complementary techniques.  While my proposal was a third hat, *the 3 R's*, the foundation of it is about continual collaboration and partnership with our business stakeholders ensuring we're delivering the most value we possibly can.
2. Always be explicit with context.  This so important in the world of software development because we communicate with so many different people with radically different backgrounds.  Whether they are software development terms, or the terminology that is used in the problem domain you are currently working in, important nuanced differences lay in the implicit semantic differences of how others interpret them.  We saw this with the term *refactoring*, how it is misused or misinterpreted, and how we can uncover hidden, yet meaningful ideas once we define a context.  With that being said, I'll leave you with the historic quote from Phil Karlton:
>There are two hard things in computer science: cache invalidation, **naming things**, and off-by-one errors.