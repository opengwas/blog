+++
author = "OpenGWAS"
title = "Managing traffic to the OpenGWAS API"
date = "2024-03-20"
description = "502 Bad Gateway errors result from misuse of our service. To address this, users will need to prove their identity to use OpenGWAS from 2024."
tags = [
    "error",
    "auth",
]
+++

## The issue

For several months we had been experiencing extremely high traffic to our servers. Under this level of load, API users have been encountering '502 Bad Gateway' or the generic 500 errors recently. These occur when you are either visiting the OpenGWAS API website or using R or Python packages (e.g. [ieugwasr](http://github.com/mrcieu/ieugwasr), [TwoSampleMR](http://github.com/mrcieu/TwoSampleMR), [ieugwaspy](http://github.com/mrcieu/ieugwaspy)) which call our API.

We also noticed that a few users have modified our packages to bypass the built-in cap on max number of retries. These packages are then distributed and used by more people, making the circumstance worse.

Please be reminded that although most of our software packages are licensed under MIT, there is a fair-use policy for the OpenGWAS service. It is clearly stated in our [Terms of use](https://gwas.mrcieu.ac.uk/terms/) that:

> The University grants you a non-exclusive, non-transferable revocable licence to access and use the Platform for private or non-commercial research purposes only.
> You agree that you will not attempt to download GWAS Data in bulk through API queries or otherwise use the Platform in a way that would or might adversely affect the performance or operation of the Platform for other users.

## What we have been doing

We've tried to address this in a few different ways

1. **Blocking very heavy users**: We have seen behaviour where users are trying to make many queries per second over a prolonged period, which implies they are running it in a parallel distributed manner. We now block the IP addresses of any users who make an unreasonable number of requests per second. The block is imposed for a few hours. This has helped a bit, but the problem was still persisting.
2. **Increasing the capacity of the API service**: Even with reasonable use per user, there are so many users that our API was not able to handle the number of requests. We have been working to migrate the API to a more scalable infrastructure, which has been a non-trivial task. At this point the new API deployment is now functional and it seems like the performance is much better - we are seeing far fewer examples of 502 errors.

## What's next

This is an open, free service. But scaling up the API means more in hosting costs, in addition to the costs of hosting the database and maintaining the software etc. So going forwards we are going to have to put in some fair use restrictions to ensure that the service remains available to everyone. This will mean that users will have to authenticate to use the service, and each user will have a usage quota in order to manage traffic. Anonymous usage for most queries will no longer by permitted.

We are working on making the registration process as painless as possible, and we will provide more details on this in the coming weeks.

Other than that, remember that the flat files can be downloaded from OpenGWAS without needing the API at all, with a number of tools to support - more info here: [gwasvcf](https://github.com/mrcieu/gwasvcf).
