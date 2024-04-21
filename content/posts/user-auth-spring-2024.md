+++
author = "OpenGWAS"
title = "User authentication is now live"
date = "2024-04-21"
description = "After the transition period, users will need to sign in and generate a token to be provided along with their request. Users will be given free allowance, and the cost of each request will be deducted from your allowance. This article explains the process in detail."
tags = [
    "auth",
]
+++

Details below are correct at the time of writing.

## Overview

As mentioned in our previous article [High server load of API](/posts/high-server-load-of-api/) we are now trialling a user authentication system to access the API. Doing this brings us into line with how most high load APIs are managed, and should help us maintain this resource to remain free and open. It costs us a significant amount in time and money to keep the resource up, so finding ways to keep this sustainable is very important. There are a few specific reasons for requiring authentication:

1. We can prevent unfair anonymous usage
2. We can contact users to help with alternative solutions who are using the API inappropriately
3. We can put in mild usage restrictions per user to ensure that the service remains available to everyone. In this way everyone is granted an allowance (see below), and we anticipate that with normal usage most users will never exceed that allowance.
4. Understanding who our userbase is will help us make the service better.

After all the changes, the workflow will be like this:

1. User signs in to our website, generates a random string ('token') and saves it to their user environment.
2. The package or script sends the request to OpenGWAS API, carrying the token.
3. OpenGWAS identifies users by the token, calculates the cost of the request, deducts it from the user's allowance and finally fulfills the request if there is still allowance left.

The token is valid for a while and user can request a new one at any time. Allowance will be reset periodically and does not carry over.

## Sign in

You can [sign in](https://api.opengwas.io/) via Microsoft, GitHub or email verification. You will need to provide your email address and first and last name if you are using email verification, or we will fetch those directly from Microsoft or GitHub. You personal data will be protected by UK GDPR.

There is no specific 'sign up' process. The first time you sign in successfully using any of the methods above, your information is recorded. And there is no 'password' to memorise.

OpenGWAS will infer your affiliation during sign in. This is from either the organisational profile of your Microsoft account, or the domain of your email address (e.g. bristol.ac.uk). If you sign in using an organisational account via Microsoft, we will also receive your department and job title information (if any). We store these information for analytical and monitoring purposes only, and will not share them with anyone else.

## Your token

Once you have successfully signed in, you can request a token, which will be valid for a while. This is your proof of identity and you should never share it with others.

You can only have one valid token at a time but you can reset it at anytime.

You can use the same token on unlimited number of your devices, package installations, scripts etc. To use your token, add it into your **request header** under the key `Authorization` with value `Bearer your_token`. Read more about how to do this in the [Authentication](https://api.opengwas.io/api/#authentication) section of API tutorial. You can also read about how to use the token in R packages e.g. [https://mrcieu.github.io/ieugwasr/articles/guide.html].

## How do tokens work?

We are using a system known as JSON Web Tokens (JWT). Briefly, there are two steps to using JWT - authentication, and authorisation. 

- **Authentication**: You authenticate who you are when you sign in, we verify your identity and generate a token. This token is then sent back to you, and you can use it to prove your identity in future requests.
- **Authorisation**: You use the token to prove your identity in future requests. API endpoints will authorise a query based on the token that you have sent. We check the token to see if it is valid, and if it is, we allow you to access the requested endpoint.

## Your allowance and the costs

You will be given periodical allowance based on the method of your most recent sign-in, free of charge. Every time you send a request, OpenGWAS will work out the cost using a set of rules and deduct the cost from your allowance.

The allowance for your account: 
- is replenished periodically, and you can make as many requests as possible (and reasonable) within the time span until you exhaust your allowance
- does not carry over to the next time span
- can only be reset by the system (resetting your token will **not** reset your allowance)

The cost of each request:
- is nil or flat for a few endpoints, and varies for the others based on the procedure and the amount of resources requested
- is calculated when OpenGWAS accepts it
- identical requests will be counted separately
- queries will contribute to the allowance regardless of the result

Read [Allowance and cost](http://api.opengwas.io/api/#allowance) in the API tutorial for more information.

## Legacy of Google authentication

Previously, a small number of users depended on using Google OAuth2 authentication to access private datasets via the API. Google authentication is now being phased out, and will be completely replaced by the access token system described above.

## What's next: The transition period

You will have until 30th April 2024 to update the packages (developed by MRC IEU) and implement the auth flow in your own script. From Wednesday 1st May 2024, you need to prove your identity (authenticate) to use our service, even if you are querying a public dataset. See [https://mrcieu.github.io/ieugwasr/articles/guide.html] for more information on how to use the token in R packages.

Read [Authentication](https://api.opengwas.io/api/#authentication) in the API tutorial for more information.