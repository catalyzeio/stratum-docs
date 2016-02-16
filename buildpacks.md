# What is a buildpack?

## What is a buildpack?

A buildpack is a framework for constructing your application's software stack. It is extremely flexible and is intended to operate as an agent to your environment. The buildpack agent will attempt to understand your stack and install any dependencies required by it.

## How does a buildpack work?

Stratum detects your code type automatically and assigns the proper buildpack accordingly. When you push your code to Stratum the appropriate buildpack creates an executable slug that includes the dependencies for your environment.

## Why should you use buildpacks for an application?

Buildpack's provide a standardized way to describe and manage your application's software stack. The entire stack can be maintained within a single version-controlled repository, making it easy to make changes and view changes to the application.
