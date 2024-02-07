+++
author = "OpenGWAS"
title = "User authentication is now live"
date = "2024-01-19"
description = "Users will need to sign in and generate a token to be provided along with their request. Users will be given free allowance every hour, and costs of each request will be deducted from the allowance. This article explains the process in detail."
tags = [
    "auth",
]
+++

Details below are correct at the time of writing.

## Overview

As mentioned in our previous article [High server load of API](/posts/high-server-load-of-api/), a few users have been exploiting OpenGWAS, making it unavailable for others who have a genuine interest in GWAS. We have designed and implemented the user authentication system and the cost model.

After all the changes, the workflow will be like this:

1. User signs in to our website, generates a random string ('token') and saves it to the R or Python packages or their own script.
2. The package or script sends the request to OpenGWAS API, carrying the token.
3. OpenGWAS identifies users by the token, calculates the cost of the request, deducts it from the user's allowance and finally fulfills the request if there is still allowance left.

Token is valid for a while and user can request a new one at any time. Allowance will be replenished every hour and does not carry over.

## Sign in

You can [sign in](https://api.opengwas.io/) via Microsoft SSO, GitHub and email verification. You will need to provide your email address and first and last name if you are using email verification, or we will fetch those directly from Microsoft or GitHub. You personal data will be protected by UK GDPR.

There is no specific 'sign up' process. The first time you sign in successfully using any of the methods above, your information is recorded. And there is no 'password' to memorise.

OpenGWAS will infer your affiliation during sign in. This is from either the organisational profile of your Microsoft account, or the domain of your email address (e.g. bristol.ac.uk). If OpenGWAS believes that you are a member of an organisation (whether or not it is an academic institution), you will be categorised as an **Organisational** user and will receive 5x allowance compared with a **Personal** user. If you sign in using an organisational account via Microsoft, we will also receive your department and job title information (if any).

## Your token

Once you have successfully signed in, you can request a token, which will be valid for a while. This is your proof of identity and you should never share it with others.

You can only have one valid token at a time but you can reset it at anytime.

You can use the same token on unlimited number of your devices, package installations, scripts etc. To use your token, add it into your **request header** under the key `Authorization` with value `Bearer: your_token`. Read more about how to do this in the [Authentication](https://api.opengwas.io/api/#authentication) section of API tutorial.

Packages developed by MRC IEU will be updated shortly to reflect this change.

## Your allowance and the costs

You will be given allowance per hour based on you user tier, free of charge. Every time you send a request, OpenGWAS will work out the cost of it using a set of rules and deduct the cost from your allowance.

The allowance for your account: 
- is replenished every hour, and you can make as many requests as possible within the hour until you exhaust your allowance
- does not carry over to the next hour
- can only be reset by the system (resetting your token will **not** replenish your allowance, as they are irrelevant)

The cost of each request:
- is nil or flat for a few endpoints, and varies for the others based on the procedure and the amount of resources requested
- is calculated when OpenGWAS accepts it (identical requests will be charged separately)

Read [Allowance and cost](http://localhost:8019/api/#allowance) in the API tutorial for more information.