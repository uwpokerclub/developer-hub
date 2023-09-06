# University of Waterloo Poker Studies Club Developer Hub

## Table of Contents
- [Introduction](#introduction)
- [What We Do](#what-we-do)
- [Our Motto](#our-motto)
- [Repositories](#repositories)
  - [App](#app)
  - [API](#api)
- [Roadmap](#roadmap)
- [How We Work](#how-we-work)
  - [Tickets](#tickets)
  - [Sprints](#sprints)
  - [Sprint Board](#sprint-board)

## Introduction
Welcome to the University of Waterloo Poker Studies Club Developer Hub. This repository will be your main source for information about our project as well as what we are currently working on. You can also find some of our processes to help when you are contributing to the project.

## What We Do
Our primary focus is the development of [our website](https://uwpokerclub.com). This includes our public facing pages where prospecting members can checkout how great the club is, and our managment dashboard where our executive members can easily run our events.

## Our Motto
The main goal of our team is to proivde students with relevant experience in the software engineering world. So a lot of what we do mimics that of an actual engineering team at your average company. This includes sprints, planning and preping items to be worked on.

Our motto for development is: 

> If it's easy/safe enough to do it yourself, you might as well do it yourself. 

This means that if an implementation of a feature can written by you without using third party libraries/applications easily and it is safe to do so (in terms of security), write it yourself. You'll learn much more about a specific area when you try and do it on your own then rely on someone else to do it for you. However, for extremely complex systems or systems that have security implications (think password hashing as an example), we would much rather use a third party library to do this for us.

## Wiki
Checkout the [Developer WIki](https://github.com/uwpokerclub/tickets/wiki) for some useful information about our processes and best practices.

## Repositories
We have two main repositories that are used to develop the website.

### App
[App](https://github.com/uwpokerclub/app) houses the frontend application of our website. This repository is written in Typescript and uses React to create a single page app. You can view the [contributor guidelines here]() for repository specific information.

### API
[API](https://github.com/uwpokerclub/api) is our REST API that helps server information dynamically to the frontend application. Currently the API is only used for the management dashboard. You can view the [contributor guidelines here]() for repository specific information.

## Roadmap
Our roadmap includes some of our big name features of things we would like to add to the website. [You can view our product roadmap here.](https://github.com/orgs/uwpokerclub/projects/1)
## How We Work
Below is all of the information you will need to contribute to our website as well as suggest new ideas and report bugs. Please ensure you read and understand this information as contributions not following these guidelines may be rejected.

### Tickets
A *ticket* is just a single item of work. These are represented using Github Issues. All tickets go through this repository as a central location for tracking, so no issues will be located in any other repo. Many tickets related to a similar overall feature may be grouped into an "Epic", which is just a single issue that is used to track all related tickets for that feature. Our epics are issues with the `epic` label.

If you would like to suggest a new feature for the application or report a bug, simply create a new issue in this repository and fill out the relevant information.

### Sprints
A sprint is a short time-boxed period where we complete a set amount of work. Our team works in 2-week sprints, i.e. we have 2 weeks to complete the tickets we set out to complete in that sprint. If a ticket isn't completeled by the end of the 2 week period, it simply carries over to the next sprint to be completed then.

We use the following rough schedule:

#### Ticket Evaluation
**Time**: Continuous \
**Description**: This is the triaging phase. New tickets that have been created will be evaluated to see if they are worthy of *potentially* being worked on. This includes making sure tickets have the necessary required information as well as the feature request is accepted and in the case of bug reports, the bug is reproducible. Once a ticket has gone through this process it will be marked as `"Ready"`.

#### Sprint Preparation
**Time**: Start of a sprint \
**Description**: Tickets previously marked as `"Ready"` will then be re-evaluated. Tickets that are seen as high priority will then be marked as `"Selected"`. This simply means that this ticket can *possibly* be selected for the next sprint, however it is not guaranteed to be selected. Items deemed not a high priority simply remain in the `"Ready"` state until they are eventually become a priority or they are no longer relevant, and thus are closed/removed.

#### Sprint Planning
**Time**: End of a sprint \
**Description**: Tickets marked as `"Selected"` will then be assigned to developers as well as [story pointed](https://www.simplilearn.com/story-points-in-agile-article) and then be marked with a `"Next Sprint"` identitfier, simply meaning this ticket will be completed in the next sprint. Not all tickets that have been `"Selected"` will make it into the next sprint (based on developer workload and carry-over items). Items that have not been selected for the next sprint will remain in `"Selected"` in prep for the next sprint planning phase.

At the beginning of the next sprint, all tickets that were marked with `"Next Sprint"` will marked as `"Todo"` and then implementation can begin.

### Sprint Board
The sprint board is simply a kanban board that we used to move tickets into the different phases of development. [You can view the sprint board here.](https://github.com/orgs/uwpokerclub/projects/4/views/2)\
**Todo**: Implementation has not started. \
**In Progress**: Implementation has started for this ticket. Testing by the assigned developer is also occuring. \
**In Review**: Implementation has completed and a PR has been created pending secondary review and approval. \
**In QA**: Implementation has been approved and the PR has been merged. Implementation is now being tested by the assigned developer and reviewer again. \
**Done**: Implementation has successfully passed QA requirements and can be released.

## Code of Conduct
Please ensure you read through our code of conduct to keep our community safe.

[Code of Conduct](./CODE_OF_CONDUCT.md)
