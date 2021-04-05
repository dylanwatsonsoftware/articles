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
