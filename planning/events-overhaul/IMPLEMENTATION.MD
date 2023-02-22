# Events Overhaul Implementation Details

## Table of Contents
- [Document Description](#document-description)
- [Phase 1 (MVP)](#phase-1-mvp)
  - [Database Implementation](#database-implementation)
  - [HTTP API Implementation](#http-api-implementation)

## Document Description

This document outlines the implementation specific details of a project. This document should only provide the high level implementation changes such as database model changes, process changes, and logical changes to the codebase. There should be minimal code provided in this document unless it is being used to show some sort of example.


## Phase 1 (MVP)

### Database Implementation

The first thing we are going to add is two new tables to handle the blind structures themselves. These tables will be named `structures` and `blinds`. The `structures` table has a one to many relationship with `blinds` i.e. a structure groups many blinds together. The `blinds` hold all of the information for a single level in a structure. The `index` field of `blinds` will be used to order the blinds in a specific structure.

![](https://www.plantuml.com/plantuml/svg/VP31QiGW54NtViKZgtQHVuXjJIerffYaa5MnKOgWBg928Pr_7mCPCeimstCUztgjGfHBJCwYsSeGe8zBccDQT80L0HW5FH4KC_WKjCLYWWyoPUwbq_25nUix5i2x0VZ86BoH6eziWA905xG7lbZZrUEmSElypXTr9oXf-AOE4PwCUjukdRGMA1z8GuxUB7xCxncMFkep7eqxvVcf-W_w5ZEMxM7o6dJtCsNvl-x3KAMzIivU0G00)

Next in order to support linking of structures to events, we will need to add a new field to events for the structure id.

![](https://www.plantuml.com/plantuml/svg/TP31IiGm68JlUOevAbvyX0tTlnfCiqkQ2Ekb5FiB1Tk5vAyWuhiRNGXxyPQPV3DCiiio9jcMTyMhpF89-YY7Z2y5p1DwyaPR7Btfj3_XYKx5NyU5yQnzywXzaM_dj8noPQnduNobCo-SXTDG2cCqUxG7R-Z1_TR1KqkUN4DzvVBDFDqgu6_QC8t22AQZFkZk-09t273HMkofrT46DD6NWZ1KvX8jgRxJrXeNAdglxifLsbvbIppKz_qtgAANHTzgn-jK_ke7)

### HTTP API Implementation

Structures will need to support creation, editing, and listing. For now we will not need to delete structures from any interface. To accomplish this we will add a new subroute `/events/structures` that will support the following requests.

#### Create a new structure

Submitting a request to this endpoint will create a new record in the structure table. Once this is complete, the API will iterate through every element of the blinds array to set the index, and then it will bulk insert these records into the database with the newly created structure ID.

```json5
// POST /events/structures
// Example body:
{
  "name": "Super Turbo",
  "blinds": [
    {
      "small": 25,  // small blind
      "big": 50,    // big blind
      "ante": 0,    // ante
      "time": 12,   // level time (in minutes) (1-60)
    },
    {
      "small": 50,
      "big": 100,
      "ante": 0,
      "time": 8,
    }
  ]
}
```

#### Retrieve structure info

This endpoint will retrieve the structure name and all of the blind levels associated with this structure. The blinds will have to be sorted by index before sending as a response, so they levels are always in the correct order.
```json5
// GET /events/structures/${structureID}
// Example response:
{
  "name": "Super Turbo",
  "blinds": [
    {
      "small": 25,  // small blind
      "big": 50,    // big blind
      "ante": 0,    // ante
      "time": 12,   // level time (in minutes) (1-60)
    },
    {
      "small": 50,
      "big": 100,
      "ante": 0,
      "time": 8,
    }
  ]
}
```

#### List all structures

This endpoint is pretty self-explanatory. It will list all structures and their names. This endpoint will not list the blind levels for each structure. This data should be retrieved from another request.

```json5
// GET /events/structures
// Example response:
[
  {
    "id": 1,
    "name": "Super Turbo"
  },
  {
    "id": 2,
    "name": "Midterm Social"
  }
]
```

#### Updating structures

When updating a structure, we should do a whole replacement of the existing object in the database. Thus instead of this being a PATCH request it will be a PUT request. We will support updating of the structures name, as well as each individual level in the structure. Once the request is through, first delete all blinds associated with the structure id, then insert all of blinds from the request.

```json5
// PUT /events/structures/${structureID}
// Example body:
{
  "name": "New Super Turbo",
  "blinds": [
    {
      "small": 25,  // small blind
      "big": 50,    // big blind
      "ante": 0,    // ante
      "time": 12,   // level time (in minutes) (1-60)
    },
    {
      "small": 100,
      "big": 200,
      "ante": 0,
      "time": 8,
    }
  ]
}
```

#### Other API Changes

Other changes that need to be made is the addition of a `structureId` field to the following routes:
- Create event
- Get event
- List events

### App Workflow Changes