---
layout: default
title:  "Being a well balanced developer - Part 2 (Own your product/feature release)"
description: Sharing my views on how to be a well balanced developer - Part 2. Productionalising the code
date:   2020-06-22 00:00:00 -0000
categories: main
---

# Being a well balanced developer - Part 2 
## Own your product/feature release. Embrace DevOps.

Without passion for the code we write and product we develop, we never can be a well balanced developer. Unless and until we see the product in action, we can't quinch the thirst of having developed a good product. 

If we just stop at coding and developing the product, our curiosity to see how our code works in production will be dependent on others like release / devops team. Cases where there is a delay in releasing the feature may give a blow to your enthusiasm. If the same delay in release happens consecutively for few time, the curiosity will become frustration (I have been there). The frustration will gradually lead to loss of interest in the product, as we know that the feature is going to be stalled because of various reasons.

Eventhough there may be very valid reasons, our sub conciousness won't accept that.

To mitigate this, we as developers should own our feature releases. Even If there is a dedicated devops team, they shouldn't interfere in the release of features in usual cases. However there can be exceptions, for example devops team can stall a release due to infrastructure update and similar reasons.

If you have reached this far, this article hopes to have ignited the fire of owning your releases 😉.

Having ignited the fire, let us go over couple of well known ways to achieve the ownership of our releases:

1. Continuous Integration / Continuous deployment: By this we tend to reduce the number of of steps involved in deployment as deployments need to be continuous.

2. Work with the DevOps team to create a framework to release the feature instead of working with them to release the feature. 

3. Always factor in for any db migrations that will be required and account for the infrastructure. 

For example, your release may require a db migration that involves changing the type of a column in table which has millions of record. In this case, If deploying directly, your instance health check may fail as DB migration may be running for a time greater than the grace time given for health check. 

Make sure the CD is capable of catering to these needs. And perform the required changes, in this case increase the grace period of health check

4. When developing feature, make sure to think along with the constraints of infrastructure. And make concious decisions. A feature that requires introduction of new parts to the infrastructure will take more time to deploy than that of a feautre that doesn't require any change to infrastructure. 

5. Make sure you as a developer have enough permissions to deploy without much hiccups. For example, if it's required by the project to make chagnes to environment variables constantly, make sure you have a framework in place to change/add environment variables. And add changes like these to the TODO list of the feature to say it as completed.

6. Remember the first point of setting up continuous deployment CD farmework? Make sure the pipeline has end to end test that is not flaky and covers the critical parts of the product. Believe me those parts of the product that we think may not be affected will be the first that will be affected 😬 .

7. Try to reduce the code changes required for small changes by extracting them into environment vairables.

8. Above all try to create lean features by scoping them well. It's a rule of a thumb that a feature that makes 10 changes to the product will take more time than the feature that makes 3 changes to the product.

As an objective try to reduce the **_code to cash_** time. Hope this article showed some light on how to see your code running in production and thus get a feeling of satisfaction and accomplishment.