# Events Overhaul
**Other names:** Events 2.0, Advanced Events\
**Expected release:** Winter 2023\
**Last updated:** 2023/02/21

## Table of Contents
- [Project Description and Motivations](#project-description-and-motivations)
- [Project Design](#project-design)
  - [Current Features](#current-features)
  - [Enhanced Features](#enhanced-features)
  - [New Features](#new-features)
- [Milestones](#milestones)
  - [Phase 1 (MVP)](#phase-1-mvp)


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

### Enhanced Features
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
  - Users will be able to input the small blind, big blind, ante, and time for each level. There will be an option to enable a BB ante instead of an entire table ante.
- Starting stack size
  - Will be used to calculate the average stack
- Table size
  - This is the number of players per table. This new field will be used for a table balancing feature which will always attempt to fill tables to the max table size.
- Point multiplier
  - This new field will add a multiplicative modifier on the points function to increase or decrease the amount of points someone gets for an event. This is particularly useful for increasing the amount of points given in special events like the midterm social.

#### **Improved Event Sign-In**
The current event sign-in process consists of a page with a list of all members registered for the semester. Members already sign-in for that event are not included in the list. Users then have to scroll through the entire list to find the member they would like to sign-in. Admins can select multiple members from this list and sign-in them all in at the same time. There is no search feature on this page so users must you their browsers find feature to find names.

The new event sign-in experience will consist of two side-by-side lists. On the left will be a list of current members registered in the semester. On the right will be all members that are currently registered in the event. Each section will have a search bar to filter the each separate list. Users will then select a *single* entry from the left list and there will be a button to sign them in. This action will move them from the left list to the right list and register them for the event. If a user selects a member on the right side there will be a button to sign them out of the event, moving them to the left list. The page will never switch or refresh during this process which allows a user to remain on the list for the entire duration of the sign-in process.

Some thought would need to be put in towards a mobile friendly view.

#### **Improved Tournament Lobby View**
The tournmanent lobby is what the current event info page shows. We will first be removing a large portion of the columns from the entries list. We will be removing the rebuys column as this will become a global event count instead of a count per entry. The "Signed Out At" column will also be removed. The "Place" column will be changed to a "Points" column and will only appear after the event has ended. The "Actions" column will be changed to a blank column and the buttons in each row will be replaced with clickable icons that appear on hover.

We also will need to improve the mobile view of this page as many execs perform sign outs via their phones. There are few mobile improvements we could make.

One of these is adding a on click event to a row in the entries list which will bring up a lot of the information we have removed. This includes the users signed out at timestamp, membership information, and some actions like signing them in/out or removing them from the event. This would be useful for both desktop and mobile users.

Secondly we could possibly show some global actions buttons at the top of the lobby screen when the user is on a mobile device. Clicking one of the actions will then popup some sort of search modal where the user would look for the entry they want to perform the action on.

#### **Automatic Signout**
One problem we currently have is for members who either forget or choose not to sign out. We used to just remove these people from the event but with the introduction of the new points based rankings this will decrease the total number of points. To combat this we have just been manually updating the entries in the database after the event has ended. The feature will instead once an event has been "ended", all not signed out entries will be signed out at the events start time. This will force them all into last place.

#### **Updated Point Calculation**
The current point calculation does not take into account the number of rebuys an event has. These rebuys are technically extra entries in the tournament and surviving in large field tournaments is extremely hard. Because of this players aren't being rewarded for the amount of effort they might put into winning these events. The new point calculation formula will become
```
(base_value * (event_size + rebuys) * point_multiplier) / FACTOR
```

#### **Rebuys**
We will be moving the rebuy count away from being per entry and instead being a global count on the event. We simply don't have a use for a per player rebuy count and it makes it extremely harder for executives to properly track rebuys which then destroys an accurate count of the budget on the website.

### New Features

Some net new features of this project are:

#### **Tournament Clock**
We are currently using a webapp called Poker Blind Timer to use for a tournament clock. This does some things very well. However it has multiple problems. One problem it has is that we have to reinput the structure for every event which usually takes 10-15 mins of wasted time. Also it cannot do more than 20 levels. After level 19 the clock just stops. There is no client side save point so if you lose the browser/computer we have to reinput the entire structure all over again.

To implement this we will add a tab to the event info page for the tournament clock. This will take in information from the event such as the blind structure and should display the following core information:
- Timer
- Level number
- Current SB/BB/Ante (Possibly a BB ante)
- Next level number
- Next SB/BB/Ante with next timer

Some nice to have information that we can pickup from the event details is
- Number of entries / number of rebuys / total number of entries combined
- Number of players left in the tournament
- Point payouts for the top X number of placements (we can just run the calculation beforehand pretty easily)
- The average stack size and size in BBs (can be calculated through the starting stack size, total number of entries and players remaining)

#### **Table Balancer**
This feature might end up getting axed for the MVP however I think it would be pretty useful to have. The basic idea would be to have some sort of visual aid to see how money tables are/should be open to keep a some what optimial size at each table. Ideally with perfect information (all entries signed out) this should be able to tell you:
- When to open a new table
- When you can break a table
- How many tables there are in total

This might have to end up being a very interactive feature to have it work as well as it needs to which is why it might be to much for the MVP.

## Milestones

### Phase 1 (MVP)
Phase 1 of this project will be to essentially mimic what we currently use for a blind clock with a few enhancments. This will include features such as
- Ability to create and attach blind structures to an event
- Ability to edit structures after being selected
- Blind timer which will show a timer, big blind, small blind, and ante.
- Buttons to advance or go back to previous levels
- Button to pause/start the timer