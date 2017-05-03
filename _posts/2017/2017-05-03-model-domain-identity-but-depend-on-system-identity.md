---
layout: post
title: Model Domain Identity but Depend on System Identity
categories: [Analysis, Software Architecture]
tags: [Analysis, Assumptions, Identifiers, Modeling, Data Ownership]
---

## Introduction

There have been many debates over the years, dating all the way back to when the idea for relational databases was conceived, on which type of identifier or key is best to use.  A natural key or surrogate key.  But as in most best practices or *which one is better* debates, the wrong question is being asked.  Asking an either or type question, is searching for a silver bullet answer.

What *should* be asked is what type of scenario or activity is each good for.  This can build some context around when it is more appropriate to use one over the other, playing just on their individual strengths.  With that context in place, each can be further generalized to having a single responsibility.  A cost is associated to the mixing of those responsibilities as we're not just playing to each other's strengths anymore.  Like many other ideas, its strengths, the things it is most valued for, are the very things that can turn into weaknesses if applied in the wrong context.

After identifying these responsibilities, along with some context of when and how to use each type of Id, cheaper and higher quality decisions can emerge.  But before we can do that, we need to find and test the assumptions that tend to lead us astray preventing us from coming to these deeper insights.

*Note: These concepts aren't bound to a relational database, or any particular technology for that matter, but to keep the terminology consistent, I'll refer to the terminology popularized by relational databases.  That is, Primary Key, Surrogate Key, and Natural Key.*

## Finding the assumptions

Natural keys are so appealing because they feel so natural (pun intended) when we think and speak about a business domain.  The terminology of a particular Id is used openly and freely by the business when they discuss their processes and the specific entities that are utilized within them.  They're telling you how they want to identify a particular entity.  All too often, it's these types of seemingly simple and innocent ideas that can cause us to have implicit assumptions around a domain concept that infers and imposes details into our solutions that don't really exist.  In this case, the learning about a natural key inferring a very specific type of implementation.  That it should be the primary key.

Why does this happen?  When we're doing analysis, the domain experts are concentrated in the problem domain.  Their view of the concepts and particular problems we're trying to solve are much more pure than ours.  We're only mere visitors in the problem domain.  Our primary domain, the solution domain, is consumed by technical details.  So much so, we tend to get lost in them, the technologies we're using, the hip new pattern we're trying to apply, etc. that we lose touch with the problems we're being tasked to solve.  Because of this, our view tends to be polluted with irrelevant and potentially damaging details during analysis.

When you listen to a domain expert speak about a concept or problem, how quick do you jump to a solution in your head polluting the ideas with all sorts of technical jargon?  Instead, try to separate the activity of domain learning from solution exploring.  With these separated, it's our job, not the domain expert's, to do this conscious mapping between the problem and solution domains.  Make this a deliberate and thoughtful process and not just an accidental process that re-enforces conclusions from imaginary and untested assumptions.

## Separating Responsibilities

What the business is really telling us is how *they* want to reference a particular entity.  This identifier is a concept of the problem domain.  A primary key is a concept of the solution domain and NOT the problem domain.  It is how *the system* wants to reference a particular entity.  This is an important point.  How an entity's data is partitioned and related to each other is a solution domain concern.  Put more bluntly, it is a technical or implementation detail.

While there should be alignment between the problem and solution domains on the partitioning of the data and its responsibilities, how the data is partitioned and how it is pulled together when it's needed, is a technical detail.  Business stakeholders and users of the software don't care how this data is partitioned or the fact it even is.  As long as the rules are upheld and they get the benefits they were anticipating, they will be as satisfied as they'll ever be.

After identifying these responsibilities, it should start to become clearer that a natural key *needs* to be modeled, but it *shouldn't* be the primary key.  The natural key can be an attribute on an entity just like any of its other attributes.  It can have rules around it like any other piece of data and in this case, the rule to be enforced is that of uniqueness.  But what is the uniqueness bound to?  A single entity?  Time?  The state of a process or another entity?  Point being, not only should the implementation of a primary key not be inferred by the requirement of uniqueness and identity, but additionally, determine, or at least consider, what the uniqueness rule of the data in the domain is bound to.

Adding to these responsibilities thus further strengthening the case to keep them separate, is the concept of data ownership.  The system does not own the natural keys in a domain, yet are treated as if it does when they are used as the primary key.  The only way for the system to own the Id is for it to generate it.  The concept the surrogate Id is for should have a logical meaning in the domain, but its value should not.  If it did, the system wouldn't own it.  The value should have no context to the domain whatsoever.  While database generated Ids can be used, it is most certainly not implied here.  Any Id generation algorithm will work so long as it provides uniqueness across the entities of a certain type, within the system.

## Making Educated Decisions

Have you ever thought you understood something, made a decision, and by the time your project was done, wished you had made a different one?  This is a very common experience, because in most cases, we're fairly new to a particular domain when we start a project.  Our understanding of it and the concepts from it that are represented in the software will change over time.  Problem is, more often than not, our naive assumptions about the rules and concepts are tested too late in the game.  When we receive requests to enhance or change the software *after* we already built something around those assumptions.

Agile, or any type of iterative development practice for that matter, doesn't inherently solve this.  Those practices are great for receiving quick post-game feedback on decisions made on small pieces of work.  But some decisions, if initially wrong, can be fairly costly to change.  Those decisions should not be solely made on the *simplest thing that could possibly work* mantra.  Instead, those decisions should be evaluated and your assumptions tested through a series of trade-offs resulting in the *simplest educated thing that could possibly work*.  Without the trade-offs to build enough context, a better, but slightly more complicated solution, might not even be considered.  Or even worse, when it is, it could turn a cheap decision into an expensive one.

During analysis, it is very easy to mistake slowly changing pieces of data as being immutable.  This is the exact type of data that we falsely assume won't change and will masquerade itself as a natural key.  Using this value as a natural key is a bet against change instead of a preparation to allow it.  Even if your assumptions are *currently* right, this doesn't mean the business won't ask for a change in the future that will invalidate them.  A value in the domain is immutable, until it isn't.

A great example of this is a "code" that is used in a domain to identify an entity.  Something like a Product Code.  While they uniquely identify a particular entity, they tend to have format requirements associated with them that are external to the software.  More often than not, the people building the software are completely unaware of this.  When marketing wants to change the format, the separate system the value is originating in changes the format, or just a simple typo occurs, that "immutable" code in your system, will have to change.

## Isolating Identity Value Changes

If what's in question is the stability of our understanding of a concept or the value in the domain and how it might change over time, more analysis is probably in order and it might be best to defer the decision.  If a decision must be made, the reversibility of that decision should be considered.  Stating it another way, what would be the cost and impact if we made the decision incorrectly?  If a value was used as a natural key and was duplicated across a system, changing this would be fairly significant.  The size of the request to change this is unfortunately disproportional to the cost of change.

To mitigate this risk, the decision and its various options should be made less significant in the architecture.  This is accomplished by encapsulating the decision thus making the cost of change more reasonable.  This is exactly what good architecture is all about.

>Architecture represents the significant design decisions that shape a system, where significant is measured by cost of change. -Grady Booch

By isolating and encapsulating responsibilities, when the details change, the cost of the change is proportional to the size of the request.  With the domain data isolated to one place, it is open to change as freely as it wants.  The separation of system identity and domain identity creates flexibility.  If they were one and the same, change would be a fairly difficult distributed change.  The isolation turns the change into a cheap local change.  Only the immutable, system generated, and system owned Id is shared and duplicated within the system.

Even with this flexibility *within* your system, if these values do change, there's probably going to be issues *outside* of your system.  While this is true, neither using a surrogate key or natural key will solve this.  We were never trying to eliminate the problem though.  Just control the cost of it.

### Technical Implications

Obviously, having one Id is more simple than having two Ids for the same entity.  But keep in mind, they each have their single responsibility.  The natural key is for the end-user while the surrogate key is for the system.  Correlation between the two should only need to occur a maximum of once per business process.  Depending on the UI design and architecture, this could be further lowered.  Additionally, the correlation code in the software could be re-used so it's only written once.

After the correlation has happened, the rest of the process can use the internal system Id for the entity.  As far as viewing the natural key on various UI screens, it can be treated and composed like any other piece of data in the system.

## Summary

Some initial decisions aren't as reversible or malleable as others, and as a result, carry a fairly significant cost if wrong.  The risk of a high cost of change can mitigated by evaluating the relevant trade-offs through an educated decision making process.  In this case, for a small investment, by creating a second identifier owned and generated by the system, the cost of change is controlled, reasonable, and proportional to the size of the request from the business.

This is accomplished by insulating the identity values from business concerns and by only depending on the internal system generated surrogate Ids when operating within the system.  The natural key is still modeled and accounted for, but is treated like any other piece of data that an entity has associated with it.