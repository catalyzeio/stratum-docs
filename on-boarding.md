---
title: The Platform On-boarding
category: getting-started
Summary: This article is intended to walk new users through self-service on-boarding
---

# On-boarding

As of August 16th 2017, Datica now offers customers the ability to completely on-board themselves with no support intervention. That means new customers can register for an account, input their billing details, spec out their application environment, provision, and deploy. We're even giving our Developer tier customers a 14-day free trial!

**Let's get started**

## Creating an account
First you'll need to register for a Datica Platform account in order to build a new environment. To register go [here](https://product.datica.com/compliant-cloud/register) and sign up with your email and a password of your choosing. Once you sign up you'll be sent an email confirmation. Click the link in that confirmation to make your registration process complete.

## Signing in
Now that you have an account and have confirmed your email you can sign in. Once you're signed in you should see an empty dashboard similar to the image below.

![Empty Dashboard](images/empty_dashboard.png)

## Onboarding - Introduction
Next you should click on the button in the welcome message that says "Create an environment". Once you click that you'll be taken to on-boarding. In this first view we're explaining from a highlevel what is required to on-board, which includes:

- Filling out your billing details
- Specing out your applicaiton environment
- And then finally deploying your new application

![On-boarding - Introduction](images/onboarding_intro.png)

## Onboarding - Billing & Organization Creation
Once you've read through the steps necessary to on-board and are ready to continue click the "Continue" button or "Billing" in the navigation.

Now that you're on the billing tab you'll need to create a new organization. In the dropdown under "Organization:" select "Create new organization". This will present an additional input for your "Organization Name:". We use these details for billing and contracting, so it's important you use the legal name of your entity here.

![On-boarding - Billing](images/onboarding_billing.png)

After you've typed out the name of your organization the next step is to choose your plan. Developer tier plans qualify for a 14-day free trial, so long as your environment fits inside of the available infrastructure (more on that later). Growth tier plans do not qualify for a 14 day free trial, however they do receive a number of other service-level benefits. For a  more in depth view of the difference between the two plans, please see our [pricing page](https://datica.com/pricing).

Once a plan is selected you'll have access to input your billing details in the form of a credit card. **Note:** We will never charge you without clearly indicating that we are doing so. We require a credit card up front to spec out an environment, however we will not charge you until that environment is provisioned. The one exception to that rule is when a customer's environment qualifies for a 14-day free trial, for which we will not charge you until the free trial is over.

## Onboarding - Environment Creation
Now we're cookin'! So you've created a new organization, have input your billing details, and now you're ready to create a new environment.

**The first step is to name your environment.** This name should be semantic and concise.

Once you have your name picked out **the next thing you'll want to do is select the services you need**. Datica's Platform abstracts away many of infrastructure complexities that you'd otherwise have to deal. What this means is that you'll get things like Load Balancing, Logging, Monitoring, Intrusion Detection, Backups, Disaster Recovery, and much more all out of the box. There is no additional configuration need on Datica to make these things work.

As such, the only thing you need to do during this step is simply select the building blocks required to make your application run. See the image below as an example:

![On-boarding - Environment Creation](images/onboarding_environment_spec.png)

We call these building blocks services. Each environment comes with the following services automatically:

- Logging: This service is required for full HIPAA and HITRUST compliance. Datica utilizes an open source tool called Kibana to provide a logging dashboard for our customers.
- Monitoring: Another service required for full HIPAA and HITRUST compliance, monitoring is provided via an open source dashboard for Sensu called uchiwa.
- Service proxy: The Service Proxy is a special service in each environment that is responsible for routing traffic from the outside world into your environment’s network, to specific code services. This is configured using sites. The service proxy is the only service exposed outside of your environment’s network - all external (non-console, non-VPN) traffic goes through it and is proxied to other services.

**Note:** For your first Datica environment these services are covered under the base fee. For each additional environment they are billed at our standard services size fee ($100/month per 1GB of Ram).

Outside of our built-in utility services each customer has the opportunity to add a number of other services. These include databases, caches, messaging services, and code services alike. In the example above you'll see two services selected:

- PostgreSQL
- code

Both of these services are scaled to a size of 2. This is highly recommended. This will ensure your application is highly available.

You'll also notice that the size of these services is set to 1. This means that each service will have a dedicated 1GB of Ram. Let's look at the math on that…

A free trial comes with 6GB of Ram. In the example above there are two services. Each with a scale of 2 and a size of 1. That means we've used a total of 4GB out of our allotted 6. For quick reference see the environment creation sidebar. At the top you'll see your bucket of available ram. Now to be clear, you can add additional ram, however anything outside for the allotted 6GB will void the free trial.
