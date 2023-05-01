---
title: Domain Analysis
has_children: false
nav_order: 2
---

# Domain Analysis

## Knowledge crunching

In order to deal with the complexity of the project there must be a continuous exchange of information with the stakeholders. More specifically, the information related to the system and the knowledge crunching process itself will be recovered through interviews.

* **How does a user access the system?**\
User access to the system depends on whether they are accessing the system for the first time or they already have an account:
    - **Sign up**: this operation allows a user without an account to create one;
    - **Login**: this operation enables a user with an existing account to access the system.

* **What is a park?**\
In this system, park refers to the action of a user placing their vehicle in a single parking slot.

* **What is a parking slot?**\
A parking slot is the place where a person can park their own vehicle

* **What does it happen when a user wants to park its vehicle?**\
When a user sees a free parking slot, they occupy it with their vehicle. After that, they report the occupation to the system by selecting the hour in which the stop will end.

* **How can a user see the status of parking slots around the city?**\
The application displays a map with all the parking slots in a certain area, determined by the user's location and a specified range of interest. Each parking slot is shown with a marker in one of three possible colors: 
    - **red**: the parking slot is occupied
    - **orange**: the parking slot will be free in ten minutes
    - **green**: the parking slot is free

* **How does a user select the distance of interest in which they can see the parking slots?**\
The user can select the distance of interest specifying a range in which they want to see the parking slots status.

* **Can a user increase their parking time?**\
Users can increase the duration of their parking anytime. However, they can only choose a time after the previously selected end time (it is not possible to shorten the parking time).
* **Can a user free their parking slot anytime?**\
Yes, a user can free their current occupied parking slot anytime by using the app.

* **What happens if a user occupies a parking slot after the stop end?**\
In the first version of the system, nothing happens. In future expansions of the system, where a fee will be requested to park, the fee will be higher (possibly 5x) if they do not remove the vehicle after the stop end.

### Mockups
The development team creates mockups of the frontend based on the information collected during the interview. These mock-ups are then reviewed and discussed by the team to ensure their accuracy.
It is also possible to find a navigable prototype of the GUI [here](https://www.figma.com/proto/qgRlc0HsejXutHe3X2PEs6/Smart-Parking?page-id=0%3A1&node-id=1-1022&viewport=-255%2C4%2C0.76&scaling=min-zoom&starting-point-node-id=1%3A1022).

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/mockups/login.png?raw=true"/>
  <figcaption>Login screen mockup</figcaption>
</figure>
<br/>
<br/>

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/mockups/sign_up.png?raw=true"/>
  <figcaption>Sign up screen mockup</figcaption>
</figure>
<br/>
<br/>

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/mockups/parking_slots.png?raw=true"/>
  <figcaption>Parking slots map (Home) mockup</figcaption>
</figure>
<br/>
<br/>

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/mockups/change_password.png?raw=true"/>
  <figcaption>Change password screen mockup</figcaption>
</figure>
<br/>
<br/>

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/mockups/parking_slot_occupied.png?raw=true"/>
  <figcaption>Parking slot occupied mockup</figcaption>
</figure>
<br/>
<br/>

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/mockups/parking_slot_occupied_current.png?raw=true"/>
  <figcaption>Parking slot occupied by current user mockup</figcaption>
</figure>
<br/>
<br/>

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/mockups/parking_slot_free.png?raw=true"/>
  <figcaption>Parking slot free mockup</figcaption>
</figure>
<br/>
<br/>

## Ubiquitous language
After completing the previous step of knowledge crunching, a dictionary is created with the aim of establishing a common language for the entire system's development. The dictionary comprises the following terms:

| Term | Definition |
| ---- | ---------- |
| Backend | The part of the system that communicates with the frontend via APIs and reads and writes data into a database. |
| Duration of parking | The length of time a user occupies a parking slot. |
| Free parking slot | A parking slot that is currently not occupied by a vehicle. |
| Frontend | The part of the system that the user interacts with directly, including the user interface and any client-side logic. |
| Login | The operation that enables a user with an existing account to access the system. |
| Occupied parking slot | A parking slot that is currently occupied by a vehicle. |
| Park | The action of a user placing their vehicle into a single parking slot. |
| Parking slot | The place where a user can park their own vehicle. |
| Parking slots map | A map that shows all the parking slots nearby with a marker. |
| Parking slot marker | A marker used to identify the position of a parking slot in a map, it can be of three different colours: green if the parking slot is free, orange if it will be free in ten minutes, red if none of the other cases apply. |
| Parking slot status | The current status of the parking slot: occupied or not occupied. |
| Range of interest | The distance selected by users in the map to determine the area in which they want to see the parking slots. |
| Sign up | The operation that allows a user without an account to create one. |
| User | A person who interacts with the system to find and occupy parking slots. |
| User credentials | An email-password pair used by the user to log in to the system |
| Vehicle | The object that a user parks in a parking slot, such as a car, truck, or motorcycle. |
| System | The union of the backend and the frontend. |
| Stop end | The time selected by users to indicate when they will stop occupying a parking slot. |

## Use cases
Following a thorough domain analysis, the use cases and entities involved in this system are identified. The diagram below depicts the use cases for the system: 
![alt text](https://github.com/antonioiannotta/LSS-documentation/blob/main/use_case/use_cases.png?raw=true "Use cases") \
From the previous image it is possible to see how, in this system, there are two main actors involved in the operations:
* **User**: the user is the actor that is intended to use the application in order to look for free parking slots, to set an end stop time and to increment this time.
* **Client**: the client is the actor in charge of handling all the requests that are made by the user. More specifically, the client is intended to handle the operation of registration and access to the system. The client is also in charge of handling a proper visualization of the status of the parking slots. The client is also responsible for handling the setting of the end stop time or the increment of this time.


It is possible to notice, from the picture, the following operations:
* **User sign up**: The user creates an account with name, email, and password, to access the system.
* **User login**: The user logs in of their account with their username and password to access the system.
* **User password change**: The user changes the password used to access the system by providing the current one and a new one.
* **User deletion**: The user requests the deletion of all their personal information from the system.
* **User logout**: The user logs out of their account from the frontend.
* **View parking slots**: The user views a map of available parking slots near their current location. The map shows the markers of the parking slots.
* **View parking slot**: The user views the location and the current status of a specific parking slot.
* **View current parking slot**: The user views the location and the current status of the parking slot they are currently occupying.
* **Occupy parking slot**: The user occupies a parking slot until the stop end.
* **Increment parking slot occupation**: The user sets a new stop end for the parking slot they are currently occupying.
* **Free parking slot**: The user frees the parking slot they are currently occupying.

## User stories
After collecting the main use cases for the system, several user stories are developed to describe the system's functionality under specific circumstances. More specifically, the user stories illustrated are the following:
* **US1:** As a user, I want to sign up to the system with my email and I want the system to save my user credentials so that I can login directly the next time.
* **US2:** As a user, I want to log in to the system with my user credentials so that I can find and occupy a parking slot.
* **US3:** As a user, I want to see nearby parking slots on the map so that I can determine whether it is occupied or free.
* **US4:** As a user, I want to view the details of a parking slot so that I can know when it becomes free.
* **US5:** As a user, I want to occupy a parking slot for a certain period of time so that I can park my vehicle.
* **US6:** As a user, I want to be able to increment the occupation of my current parking slot if I need to park my vehicle for a longer period of time.

Below, you will find the illustrated user stories:

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/user%20stories/user_sign_up.png?raw=true"/>
  <figcaption>US1: User sign up</figcaption>
</figure>
<br/>

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/user%20stories/user_login.png?raw=true"/>
  <figcaption>US2: User login</figcaption>
</figure>
<br/>

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/user%20stories/view_parking_slots.png?raw=true"/>
  <figcaption>US3: View nearby parking slots</figcaption>
</figure>
<br/>

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/user%20stories/view_parking_slot.png?raw=true"/>
  <figcaption>US4: View parking slot</figcaption>
</figure>
<br/>

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/user%20stories/occupy_parking_slot.png?raw=true"/>
  <figcaption>US5: Occupy parking slot</figcaption>
</figure>
<br/>

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/user%20stories/increment_parking_slot_occupation.png?raw=true"/>
  <figcaption>US6: Increment parking slot occupation</figcaption>
</figure>
<br/>