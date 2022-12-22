# Events Overhaul
**Other names:** Events 2.0, Advanced Events\
**Expected release:** Winter 2023\
**Last updated:** 2022/12/22

## Table of Contents
- [Project Description and Motivations](#project-description-and-motivations)

## Project Description and Motivations
The current event system serves some very basic use cases to run only our weekly events. It started with handling event registration and sign ins/outs. We eventually extended it to include rebuy tracking. 

However, we still have been handling a lot of things manually. For example, setting and creating blind structures have to be redone every event, and also have to be remembered across terms. Blind timers have to be done either through a physical stopwatch or through online software.

There is also a lot of missing functionality. For example, table seating randomization which is particularly useful for the midterm and final tournaments. Automatic table balancing which can aid executives in running events.

## Project Design

### Current Features
The current feature set of the events system includes:
- Basic event details (name, start date/time, variable game input, additional info)
- Event registration
- Entry sign-in/sign-out
- Per-member rebuy tracking
- Entry searching

### New and Enhanced Features
Some of the features which would be improved are:
#### **Enhanced Event Details**
In addition to the basic details we currently have:
- Name
- Date
- Format (variable input)
- Additional details (textbox)
 
We are going to add the following additional information to event creation:
- Format (selector)
  - This turns the format field from a text field to a selector, with a list of pre-specified games. This will only be a list on the frontend so it can easily grow. The backend should not attempt to validate the format.

- Blind structure
  - This is the largest piece of new information we will be tracking. When creating a new event, users will be prompted with one of two options:
    - Option 1: The user can select from a list of previously saved blind structures. This will then load the saved blind structure into the app, and allow the user to edit the structure if needed. The user can then re-save the structure to overwrite it.
    - Option 2: The user can create a new blind structure from scratch. The user will have the option to save this structure for later use. If they choose not to save it the structure will be deleted after the event is finished.
