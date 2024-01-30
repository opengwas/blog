+++
author = "OpenGWAS"
title = "High server load of API"
date = "2023-11-09"
description = "502 Bad Gateway errors result from misuse of our service. To address this, users will need to prove their identity to use OpenGWAS from 2024."
tags = [
    "error",
    "auth",
]
+++

## The issue

We are aware that users have been encountering '502 Bad Gateway' or the generic 500 errors recently. These occur when you are either visiting the OpenGWAS API website or using R or Python packages (e.g. ieugwasr, TwoSampleMR) which call our API.

We discovered further that a few users have modified our packages to bypass the built-in cap on max number of retries. These packages are then distributed and used by more people, making the circumstance worse.

Please be reminded that although most of our software packages are licensed under MIT, there is a fair-use policy for the OpenGWAS service. It is clearly stated in our [Terms of use](https://gwas.mrcieu.ac.uk/terms/) that:

> The University grants you a non-exclusive, non-transferable revocable licence to access and use the Platform for private or non-commercial research purposes only.

> You agree that you will not attempt to download GWAS Data in bulk through API queries or otherwise use the Platform in a way that would or might adversely affect the performance or operation of the Platform for other users.

## What's next

To address this issue, we have made a decision to forbid anonymous use of our service. 
User will have to prove their identity along with their requests. Users will also receive free 'allowance' for each time period, and most of the requests will be 'costed' against the allowance.

More details will be made available in early 2024 and restrictions will apply afterwards.

The OpenGWAS team appreciates your continued support.
