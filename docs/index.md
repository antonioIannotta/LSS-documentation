# Smart parking

## Introduction

### Overview
The parking situation it's etimated to become more and more complicated in the next years. This increase of complexity is due to two main factors:
* the need for the cities to earn moeny from the available parking slot
* the number of cars in the city
These factors lead to the possibility for the citizen to trust on a parking system fully digitalized in which the payment system is secure and the availability of a certain parking slot is always shown in a reliable manner.

In this project the payment system issue has not been addressed.

### Requirements
In the following are reported the main requirements that the system must satisfy:
* The user must easily access to the system
* The system must show to the user the state of the several parking slot around the city.
* The system must allow the user to select the end of the stop after having parked the car/motorcycle.
* The system must allow the user to easily increment the stop duration

## Domain Analysis

### Knowledge crunching

In order to deal with the complexity of the project there have been a continuous exchange of information with the stakeholders. More specifically the information related to the system and the knowledge crunching process itself have been implementet through interviews.

* **How does a user access to the system?**\
The sign-up and sign-in operations are handled through the Google Auth tool. So the user only needs to have a Google account

* **What is a park?**\
In this application it's better to deal with the concept of park in the meaning of single slot where a certain person can park its car/motorcycle. So the user can park its car/motorcycle into a parking slot.

* **What does it happen when a user wants to park its car/motorcycle?**\
The user sees a free parking slot and occupy it with its car/motorcycle. After that from the app he/she selects the hour in which the stop will end.

* **How can a user see the parking slots status around the city?**\
The application shows every parking slot that is in a certain place. The place depends on the location of the user and on the distance of interest that the user specify. Every parking slot is shown in one of three possible colors: 
    - **red**: the parking slot is busy
    - **orange**: the parking slot will be free in few minutes
    - **green**: the parking slot is free

* **How does a user select the distance of interest in which he can see the parking slots?**\
The user can select the distance of interest specifying a range in which he wants to see the parking slots status.

* **Could a user increment the stop?**\
The user can increment the duration of the stop before the previous stop ends. More specifically he/she can only specify a time after the end stop time previously inserted.

### Ubiquitous language

The result of the previous knowledge crunching step has been the creation of a glossary with the goal to provide an ubiquitous language around the which develop the whole system. The glossary contains the following concepts:
* **Parking slot**: location where the user can park its own car/motorcycle
* **Occupy**: the action of moving a vehicle into a parking slot
* **Application**: Android application
* **End stop**: the time limit within the user must remove its vehicle from the parking slot.

### Use cases
After a proper domain analysis has been possible to detect the use cases and the entities that are involved into this system. The following imgage illustrates the uses cases for Smart Parking:\
![Alt text](../use_case/Use%20case%20diagram.png "Use cases")