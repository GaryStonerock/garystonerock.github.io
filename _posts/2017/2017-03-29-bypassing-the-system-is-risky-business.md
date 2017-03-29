---
layout: post
title: Bypassing the System is Risky Business
categories: [Analysis, Architecture, Data]
tags: [Analysis, Risk, Migration, Scripts, Bugs, Data Corruption, Architecture, Systems Thinking, Data, Behavior Centric Thinking]
---

## Introduction

From time to time, cases will arise when a system contains data in a state that the business considers to be invalid or undesirable.  A common cause of this is when a bug in the software alters a set of data in an unexpected or incorrect manner.  A less common cause is when an edge case that the software doesn't currently support is experienced creating a gap between what is reflected in the system and the real world.  Regardless of the cause or case, more often than not, there can be negative business consequences if the invalid data isn't corrected.

All too often, the response to these problems is to write scripts that bypass the system to update the data to a different and desired state.  This data-centric thinking, viewing the problem as a data update problem, tends to misguide and emphasize riskier solutions by narrowing the problem focus in the wrong area.

>Bypassing the system is when you go directly to the database, circumventing the rules of the system, to apply an update to the data.

This definition does not discriminate in terms of technologies.  Bypassing the system isn't limited to using the natural built-in language, api, etc. of the database of choice.  A system is bypassed when *any* technology is used to circumvent the rules of the system.

Through those quick and short-sighted solutions, the business can open itself up to much more risk than it had with the problem they were originally facing.  Fortunately, there are much more direct solutions to these problems that help control and mitigate risk as opposed to increasing it.

## The Risk

>A software system defines rules and behaviors around data to constrain it in order to optimize for some business benefit in a controlled, reliable, repeatable, and automated way.

These are the very benefits that are being undermined when a system is bypassed.  Different code is run against the data to correct it than the code that has been written to enforce its validity and maintain its integrity.  This can further corrupt the data or corrupt even more data that was actually valid.  Outside of the obvious problems with data corruption, one often overlooked problem is the fact that it can introduce undefined behaviors in the system as the invalid state was not something designed for or considered.

### Increasing the Risk

Today, systems are becoming larger, more complicated, and dealing with a lot more data than they have in the past.  This is due to the digital transformation most businesses are going through to stay competitive in their respected markets.  These systems aren't just about capturing data and executing some logic around it.  Enterprise wide processes are being modeled and built directly into these systems.

This type of problem complexity is handled by splitting the large problems into many smaller problems.  This means multiple components in a system need to work together to solve a larger problem.  If you're bypassing the system, each component's responsibility, how they interact, and what their impact and influence on each other is, must be understood and considered.

Another way problem complexity is handled is through having multiple models to optimize or accommodate a particular scenario.  If data is replicated or passed between these models and the behaviors of the system are bypassed, the introduction of inconsistent data is probable.  Data also tends to flow through multiple models and business processes in a system.  If this design is bypassed, inconsistent states or incorrect decisions in the related business processes are also probable.

In addition to advancements in the architecture space, many new technologies have been introduced and popularized in reaction to these types of demands.  Using just one of these is often too limiting, so it's fairly common to see newer systems being built with many different technologies and architectures.  Databases are only able to enforce a very small subset of the rules of the systems being built today so it's very common to see business rules being implemented in many different technologies on many different tiers.

### Real Consequences

An insurance provider needed to update the amount they would reimburse customers for a particular product if it was defective.  To update the amount, they bypassed the system using a SQL script.  Because the rules of the system were not adhered to, data became corrupt.  It took 5 business days for the company to realize it's mistake.  During that time, they lost $1.8 million dollars due to the corrupt data incorrectly influencing decisions.

Another company, a software platform provider, already in the habit of executing scripts against a particular area of the system, executed a script that introduced duplicate records.  This went unnoticed for a week or so until that data was actually needed.  Given the software didn't expect duplicate records, certain business operations failed causing the specific department in the business to come to a screeching halt.  This had a direct impact on the customer and the platform provider's ability to deliver what they agreed to in the time frame they agreed to delivery it in.

In both cases, the change seemed simple in isolation, but had a very large negative impact on cost, time, and the ability for an organization to function as expected.  Even worse and unfortunately common when a system is bypassed, the impact was delayed.  The cost of these problems to the business increases based on how valuable it is for the data to be correct and that cost can be amplified through a delayed impact.

Taking a step back, have you ever experienced problems, no matter how big or how small, as a result of bypassing a system?  Unfortunately, chances are, you have.

## Leverage the System

Who owns the data?  Who allowed the data to get into its current form?  Who has the authority to transition the data from one state to another?  You got it.  *The System*

Instead of viewing the problem as a data update problem, take a behavior-centric view of it.  Ask yourself questions in terms of behaviors.  What behavior is wrong that is producing this undesired data, that when adjusted, will correct it?  What sets of behaviors can be applied to transform the data into the desired state?  Look at how the data in question is being used.  What are the rules and invariants currently in the system that must always be true about the data?  What else depends on the data?  Is it pushed to other processes or models?

Investigate how the behaviors in the system can be leveraged to resolve the issue.  More often than not, if you can leverage what the system already knows how to do, you can narrow your focus to only one component.  Interacting with that single component will ensure the data maintains its integrity and the downstream processes are eventually consistent with the desired state changes.

How do you interact with that component?  Well, who owns the data again?  By taking a behavior-centric view of the problem, it should start becoming clearer that we need to extend the software in order to properly address the issue.  Extend the system to detect the problem condition and then *migrate* the data into the desired state by interacting with the relevant component(s).  The only new behavior is the detection of the problem condition.  The existing code and the behaviors it implements, are leveraged to ensure the system is kept in a valid and deterministic state.

It is important that unless there was a bug that was causing the data to be in the undesired state, that the production code is not *modified* but *extended*.  This is both the single-responsibility and open-closed principles in action.  The extension's only responsibility is to detect the problem and to send commands into the system to apply the corrections.  The problems with changing working code is it introduces risk of breaking changes or missing dependencies that should change as well.

>Changing working code is a bad idea. If you change working code, it is less likely to work after. -Greg Young

In the case of a bug or an enhancement to the software, the code changes and migrations should be packaged, versioned, and tested together.

By modeling the problem as a set of behaviors, we're able to simplify the problem, partition the problem, isolate responsibilities, leverage existing behaviors, and maintain data integrity and consistency thus reducing and controlling risk.

## Summary

Code written that bypasses the system can easily corrupt more data, introduce inconsistencies, and/or introduce undefined behaviors.  This is because the rules of the system are circumvented.  Even if the rules are duplicated, it is very prone to mistakes through incorrect translation, missing context, or misunderstood assumptions.  Scripts that bypass the system tend to deal with many accidental details or unrelated problems whereas if the system can be leveraged, the focus will be just on the essential details enabling a simple and direct solution to be found. 

When bypassing the system, the increased risk on the business, that if was known, would be largely unacceptable in most cases.  The risk is unnecessary and can be mitigated by leveraging the system to do what it already knows how to do.