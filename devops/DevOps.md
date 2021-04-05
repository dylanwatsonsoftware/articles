# *DevOps in the real world*

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

# The release process
The process should be very simple and easy!

As simple as:
* Raise a pull request and review as normal
* Review change for user impact
* Verify release
* Deploy to prod

There may well be many additional steps and checks (and there should be!) but the process above includes only _manual_ steps. The rest should be automated.

## Raise and review pull request
This one should be simple. Review the PR for the things you'd usually look for. E.g:
* correctness
* suitability of design
* tests and testability
* style

## Review for user impact
After reviewing the change as usual, you must then review in the mindset of what will affect the user and what will affect the system.

If we want master to *always* be in a state that is ready to deploy to production, we much be sure that deploying to to production won't:
* Impact users if we have made a mistake
* Change important workflows for the user, without notifying them
* Make functionality available to them that has not been sufficiently tested or signed off 
 * This might be the Product Owner, QA, "the customer" or "the business" depending on your context
* Require multiple parts of the system to be deployed in a specific order
  * This will cause unnecessary coupling and long nights rolling back deployments of multiple systems.

Ideally, when we release we want to make the smallest possible change to the user, perhaps even completely nil.

We do this through techniques that reduce customer impact such as feature toggles and API backwards compatibility.

If we can avoid switching on functionality until it is truly ready, we can merge our work constantly.

This has several benefits:
* Smaller reviews allow developers to give better focus and feedback to reviews
* More consistent merges lessens the cost (and risk) of merge conflicts
* Merging often, gets changes into the hands of QAs earlier for exploratory testing
* Merging with the mindset of not making user facing changes until ready, allows others operating in the same code base to deploy and launch their features without being impacted by your changes.

## Verify release
This step is almost non-existent if you are deploying single changes into production (i.e continuous deployment), however given that this article is talking about Enterprise companies.. and that fact that you are reading this article, you probably aren't doing that.

So in the scenario, where you have multiple changes merged and you now want to deploy them to production, we must check what we are about to deploy to our users.

Since we are in an enterprise scenario, we might have multiple teams working on the same codebase at the same time. This means that when you deploy, everyones changes go with it. That's why it is crucial to have visibility of *exactly what changes* have been made to the codebase - not just which tasks _should_ be in the code based on your task tracking software (e.g JIRA).

Depending on your needs you should be looking for things like:
* Does the change need to be reviewed for security?
* Have we sanity checked that this deployment has minimal user impact?

If you find you are also asking the following questions, you may have a coding practice problem or lack of automation in your pipeline:
* Does this deployment introduce user-facing changes?
* Do we need to notify users about this deployment?
* Are we dependent on a change being made by a third-party?


## Deploy to prod
As soon as you have gained confidence in your release, you should deploy it.

Deploying to prod as soon as we gain confidence has several benefits:
* Delaying may reduce confidence, for no good reason
* Delaying increases the potential changes in the NEXT deployment, increasing the risk for any single deployment
* More changes means less visibility and confidence of an individual deployment
* If you have to rollback a deployment due to an issue, the larger the body of work being deployed the greater the number of features being "removed" from production is
* If (read: when) you have a production incident, it's much less risky to deploy only the bugfix into production - there is no need to further complicate a stressfull (and possibly late night) situation.

So... Deploy to production as soon and as frequently as you are able!

## Launch the feature
After potentially multiple production deployments, once the feature has been sufficiently tested and ready for use, it can be switched on in production.

This might entail, a configuration change in your repository or enabling the feature through some configuration API. At this point in time, depending on your company, you may also like to notify your users of the impending changes.

# Coding practices
## Continuous Delivery
Continuous Delivery is all about automating everything in your process. Everything, right up to the point where you make a manual decision to deploy to production.

The more that is automated, the easier your release process becomes, the more releases that you will be willing to do.

Whilst is might be difficult for highly controlled industries (e.g Government and Finance), at some point we could even automate the decision to deploy to production. We call this continuous deployment. This is just a natural progression of continuous delivery but isn't required to make serious strides towards faster easier releases.

The medium ground is having a tool with all the codified criteria you have to approve a release to go to production, then just click the button yourself. But given everything we've discussed about increasing risk by delaying, if you've got a tool to do the work, once you have confidence in that, why not just let the tool release when it's ready?

## Backwards Compatibility
When developing APIs, as with any piece of software, you want to ensure that you can deploy your change at any time. You don't want to force all consumers of your API to be deployed at the same time as the API (even if there's only one consumer). This would create a [lock-step deployment](https://www.google.com/amp/s/blog.steadycoding.com/problems-with-lock-step-deployment/amp/).

Coupling your backend and frontend deployments can cause massive headaches. If you discover an issue with either of them you'll need to rollback both. Creating extra work and delaying all the other pieces of work in both systems.

To avoid this we must only make *non-breaking* API changes.

This means:
* Only add endpoints, don't remove them
* Never add a mandatory parameter to your requests 
* Never remove a field from a response
* Never rename a field or endpoint (This will actually have the same effect as removing).

Ok ok, I know that sounds a bit dramatic. When I say never, I mean before making these removals you must consider the impact it will have to production systems before making this change.

In order to do that, first update all your consumers to handle your change (not using the field, or sending the new field).. then and only then can you make your API change.

## Keystone Interfaces
One good way to allow your code to be merged and tested without affecting the user is with a [Keystone Interface](https://martinfowler.com/bliki/KeystoneInterface.html).

Basically this entails building all the functionality (with all required tests) for a particular feature (over multiple PRs) then, only once everything is ready, we build the UI (or API endpoint) to expose the functionality. This is obviously simplest when the feature can be exposed via a single button but can work well for all kinds of features. In this case, the button becomes the Keystone that is placed at the end and allows us access to the functionality.

This method is very powerful. As Martin Fowler states, it can even be used to hide whole pages of functionality, we can just use a link to that page as the Keystone. It can work just as well in an API where all the functionality is slowly fleshed out, including DB schema and migrations and then once it's ready and tested, raise a PR to include the change in an existing endpoint. For internal APIs, where you control all consumers you probably don't even need to go to this much effort... Just add the new endpoint.

I've previously felt odd about this particular technique as it feels odd to have code in your codebase that doesn't get used yet (See [Dead Code](https://refactoring.guru/smells/dead-code), however as long as the feature is still being worked on, this approach can have some serious benefits and avoid some of the complexities and effort with more involved techniques like feature toggles.

## Dark Launches
Martin Fowler also mentions [Dark Launching](https://martinfowler.com/bliki/DarkLaunching.html), which is the idea of calling the code for a new feature without the user being able to tell. If done correctly, this could allow you to test out new functionality and it's performance impacts in production without overly affecting the user.

You might wish to expand some existing functionality to display more information to the user. You could flesh this extra info out, collecting it and gathering production stats on performance or errors (obviously you need to ensure the errors don't affect the user). Once you're happy, add the code to expose the new functionality to the user.. a bit like a Keystone Interface again.

## Feature Toggles
Once you have considered all other avenues (such as Dark Launching and Keystone Interfaces), in order to avoid launching your feature until you're ready you might need to use a [Feature Toggle]( https://martinfowler.com/articles/feature-toggles.html).

It's often still useful to think about your functionality in terms of Keystones however. If your feature allows it, create a Keystone and put your feature toggle around that. Once you have to finished building out and testing your feature (with the feature toggled on for you), you can switch on the toggle.. exposing your feature toggled Keystone to the world.

Not all changes allow you to toggle your code at the UI level but the deeper down in the code you place your toggle, the more intertwined with the test of the code it will be and the harder it will be to test and gain confidence in it. 

So place your toggles at the highest level of abstraction you can in your code and try to avoid littering several of the same toggles throughout your code.

## If you can't see it, it didn't happen
Lastly, but possibly the most important point is that you must have the ability to see what it going on in production. That means alerting, logging and monitoring.

It should go without saying that if we want to reduce the time taken to become aware of a production issue then we need to be monitoring production for issues!

In the modern software space, you will be inundated with tools that can get you visibility of your production system.
Let's take a look at some of the types of tools to watch out for.

### Frontend App

* Alerting for errors
* Http call logging
* User tracking
* Event Analytics
* Demographic analysis

There are so many things that you COULD track and as you build out your system you will likely need to know more and more about how your system is really used.
The key thing is that your chosen solution allones you to continually expand out your knowledge of the system as required.

Some tools that I've seen be used with success:
* Sentry (I can't recommend this enough!)
* Rollbar
* Google Analytics
* Firebase
* LogRocket
* AppSignal

### Backend Services
Again there are plenty of things you could track, hopefully some (like performance or resource usage) are already handled by your platform.

As with frontend monitoring, it doesn't matter so much what you choose, just have something in place that can expand as your visibility requirements increase.. and they will.

Here are a few backend monitoring tools that I've seen work well:
* Splunk (Really great at alerting, querying and data transformation)
* ELK (Elasticsearch & Kibana)
* Prometheus
* AWS CloudWatch
* xMatters
* PagerDuty
* ServiceNow
* DataDog

# Conclusion
There are many tools out there to help us release faster, 
