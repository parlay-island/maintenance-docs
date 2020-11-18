# Maintenance FAQs

## Where are dependencies?

The backend has all dependencies in a Pipfile.

The frontend has all dependencies in a package.json file.

The Unity application contains all dependencies in Parlay/Packages/packages-lock.json.

## Why are costs increasing?

There are many answers to this question! [Look at the billing console](./#observe-costs) to get a sense of what is costing you the most.

Some ideas:

* Are EC2 instances costing you money? That might mean that you have instances that are running that you don't want to run. If you do not have a current development team, try to [shut down the stage environments](./#shut-down-the-stage-applications). Or, you might want to change your Instance Type to a more cost-effective type.
* Is ELB costing you money? You might want to move to Single Instances instead of using load balancing.

## How do I switch to a new database?

The database settings used are pulled from environment variables. If you are working locally, just edit the `.env` file. If you are working in an elastic beanstalk application, change the configuration environment variables to reflect the new database. 

## I'm getting lots of 500 errors from the backend. What do I do?

{% hint style="warning" %}
Fixing these issues might be difficult to someone without development experience.
{% endhint %}

The bad thing about 500 errors is that it's hard to tell what's wrong without good logging. The current status of the backend's logging is non-existent, so here are a few tips.

1. Try to [reset the database auto-sequence](./#reset-the-database-auto-sequence).
2. Use stage to debug the issue.
   1. Start a stage environment if not started already.
   2. Make sure that the debug environment variable in elastic beanstalk is set to `True`.
   3. Make sure that this environment is linked to the production database \(check that the database variables in Configuration are the same as in production\), so that accessed data from the backend API call are the same. Change this back after you finish!
   4. Replicate the backend API call that caused the issue and because it's in debug mode, the API will return a web page that shows where the problem is occurring in the code base.

## How do I shut everything down?

{% hint style="warning" %}
Remember to take a snapshot of the database if you might want to keep the data currently stored in the application.
{% endhint %}

Repeat the [shut down stage applications](./#shut-down-the-stage-applications) instructions, but for the applications that have 'prod' in their name.



## A lot of people are using the application, and everything is getting slower. What do I do?

Try to [turn on auto-scaling](./#auto-scale-one-of-the-elastic-beanstalk-applications) for the backend or the frontend! If that doesn't work, you might need to increase the max number of instances in the auto-scaling settings for the slow application. Additionally, you can try to change to a different instance type that is bigger.

