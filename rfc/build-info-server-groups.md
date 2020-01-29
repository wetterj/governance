# Display Generic Build Information on Server Groups

| | |
|-|-|
| **Status**     | _**Proposed**, Accepted, Implemented, Obsolete_ |
| **RFC #**      |  |
| **Author(s)**  | James Wetter ([`@wetterj`](https://github.com/wetterj)), Vadim Atlygin ([`@vava`](https://github.com/vava)) |
| **SIG / WG**   | _Applicable SIG(s) or Working Group(s)_ |


## Overview

Currently Spinnaker cannot display generic information about the current build deployed to a server group in an easily accessible manner. This document proposes allowing key build information to be displayed in the ServerGroup box on the 'clusters' page to ensure it is quickly accessible.


### Goals and Non-Goals

Goals:



*   Provide a mechanism for generic build information to be communicated from any stage in a pipeline to the deck frontend.
*   Render this build information with the markdown react component in the ServerGroupHeader displayed on the clusters page.


## Motivation and Rationale

Currently Jenkins and Docker have integration that provides a link to the Jenkins build or Docker image for the build currently deployed to a server group. The link is directly visible on the 'clusters' page for an app making it easily accessible. This type of integration should be possible with other tools used to build projects such as Google Cloud Build. Ideally the mechanism should be as generic as possible allowing flexibility and extensibility.

The primary benefit is the ease with which a user can verify which build is currently deployed to each server group.

Early customers for this change will be Chrome-Ops.


## Design

To implement this change we need to add;



1.  A mechanism to get the build information from any stage in a pipeline to the UI component, and
1.  a react component to ServerGroupHeader.


### Dedicated Stage

We propose using a new stage that reads a .properties file from an artifact. The variables defined in this file will be added to the build context for subsequent stages. The variables defined in this file will also be passed to frontend as part of the ServerGroup model once the pipeline deployed to the corresponding servers.

Using a stage to fetch this data from an artifact allows any preceeding stage to generate the artifact, so the feature will not be limited to integration with any particular build infrastructure. It will also allow other UI elements to incorporate the contents of this file if needed in the future.


### React Component

ServerGroupHeaderProps will be extended to include a string of markdown providing generic build information. This string will be rendered with the existing markdown component in the deck project.
