# Releasing software is hard
First let's start with 2 commonly heard statements:
> Enterprise doesn't do DevOps!

> Releasing is hard!

_- signed, everybody in every enterprise company ever_

Whilst it might be true historically that Enterprise companies tend to have quite a lot of bureaucracy around everything (especially releasing), it doesn't mean that we can't look to improve the pace at which we deliver working software to our customers. In fact, if we follow some pretty simple ideas we might just improve our quality as well!

Releasing doesn't have to be hard! Let me explain.

# What is DevOps?

> DevOps is
> a set of practices
> that works to
> *automate processes*
> so software teams can
> *build, test, and release*
> software *faster and more reliably.*

_Paraphrased from [Atlassian](https://www.atlassian.com/devops)_

# How do can I drive DevOps behaviour in my team?
Read on, oh reader, and I shall tell you more about:
* The DevOps mindset
* A simple release process
* A few simple DevOps coding practices

# What's a "DevOps" mindset?
Sounds a bit wanky, right?.. so what is it?
Well, driving change, especially around automating processes in traditionally bureaucratic companies can be difficult!

To drive DevOps practices there are a few things to keep in mind.
If you can keep these at the forefront of your team's mind, then most other things can be derived from that line of thinking.

## End goal
What are we looking to achieve with this DevOps shift?
Well, we are hoping to:
* Reduce the time it takes for our work (including bug fixes) to land in our users hands
* Reduce the time it takes to become aware of a problem in production
* Reduce the risk and longevity of downtime due to a release
* Reduce the impact of any particular bug introduced

These may sound like lofty, all encompassing goals but these are realistic improvements we should all be aiming for.

## Git master is sacred
You must ensure that the latest version of your code is always deployable. If you need to "cut a release" and branch off master, you will be less inclined to release on a regular (read: daily or on-merge basis).

If your code is not "ready to release" at anytime, your ability to safely respond when _shit hits the fan_ is heavily affected.

Think about it, if you first need to rollback your code to a "known" safe place, then this is yet another step in the process before you can even begin fixing your production incident.

In order to keep master "production ready", all merges to master must also be ready for production. 

This is achieved by:
- A Production Mindset
  - if you know your code could go straight to prod, you will take more care with it
- Automation
  - Anything that gives you confidence to release, should be automated and it should prevent branches merging to master if they fail
- Coding Practices
  - Feature Toggles, backward compatibility and other techniques can help increase prod readiness

As we get into the coding practices, I'll give some examples of ways to help keep your code production ready.

## Deploy and run what you build
The person best placed to understand issues with a feature is the person that just wrote code for that feature.
If that person is in charge of dealing with any customer issues with that piece of work they will:
* Better understand the problems when they occur
* Be more inclined to take care when writing the code
* Have better context and incentive to improve the supportability of that code in the future

By getting your Devs at least involved in this process, your software quality will be given a good chance to improve, if only through empathy for the user and themselves!

## Small changes are best
If you spend a year developing software before giving it to a user, you are spending a year building up risk. Any piece of that years (or even months) development could (and likely does) have an undiscovered bug.

If anything goes wrong (and it will), you will have to sift through a whole year of code and features to discover the issue.

On the other hand, if you make a single change and deploy it to production you will likely have much more confidence about deploying to production. Why? Well, if it breaks, it is much easier to have confidence around which change caused the issue... And you can simply rollback to the previous version (or even write a test, fix the bug and redeploy).

Due to our new confidence in finding issues and rolling back changes quickly, the *risk* of any 1 change is _drastically reduced_.  We also _massively increase_ our ability to avoid downtime due to an introduced bug.
This helps gives us the confidence we need to release at a much faster cadence.

Simply stated: 
> *Small change == less risk == fast release*

For further reading here is an interesting Kent Beck article about taking small changes to the extreme: [Testing the boundaries of collaboration] (https://increment.com/testing/testing-the-boundaries-of-collaboration/)

## Automated testing is key!

The other lynch pin in our DevOps plan is testing. If we don't have sufficient automated testing to give us confidence to release then the plan falls apart. If we rely on manual testing for this confidence, and this manual testing is performed AFTER merging to master, then your manual testing becomes a blocker to production. However, because you have merged to master, you don't just block the code your code for being released but any code merged after yours. This puts pressure on the QAs causing unnecessary delays or forcing them to drop their testing standards.. or just putting unnecessary stress on them!

This is not to say that manual exploratory testing doesn't have its place. It definitely does.
Just that with sufficient automated testing (combined with the coding practices described below), manual testing shouldn't block a release!

For 2 very interesting (but opposing) points of view on appropriate testing levels, check out talks/articles by:
Kent Beck
* [”Unit” tests](https://m.facebook.com/nt/screen/?params=%7B%22note_id%22%3A387720532357705%7D&path=%2Fnotes%2Fnote%2F&_rdr)
* [Is TDD dead?](https://martinfowler.com/articles/is-tdd-dead/)

Kent C. Dodds.
* [Write tests. Not too many. Mostly integration.](https://kentcdodds.com/blog/write-tests)


However, if your QAs are finding issues that if deployed to production would impact users, then you are in luck! Our guiding principles section will take you through several techniques to avoid blocking production deployments, whilst still maintaining safe and reliable production environments.
