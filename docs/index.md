# Smart parking

## Introduction

### Overview
In many cities around the world parking has become a significant issue. As the number of cars on the road continues to grow, finding a place to park can be a real challenge. In this context, the parking situation is expected to become increasingly complicated in the next years.

A fully digitalized parking system that guarantees reliable information on parking space availability can be a viable solution. Such a system would enable citizens to trust that they can find a parking space when they need it.

In order to do that we want to create an application that allows users to find parking slots nearby, occupy one of them and, if needed, extend the occupation.
The application should have a user-friendly interface with several screens, including a login screen, a signup screen for new users, a password change screen and a home page for finding available parking slots nearby. 
The project should be developed using the Domain Driven Design (DDD) approach, with a focus on identifying and modeling the key entities, repositories and rules involved in the parking and user domain.

## Domain Analysis

### Knowledge crunching

In order to deal with the complexity of the project there have been a continuous exchange of information with the stakeholders. More specifically, the information related to the system and the knowledge crunching process itself have been recovered through interviews.

* **How does a user access the system?**\
User access to the system depends on whether they are accessing the system for the first time or already have an account:
    - **Sign up**: this operation allows a user without an account to create one;
    - **Login**: this operation enables a user with an existing account to access the system.

* **What is a park?**\
In this system, park refers to the action of a user placing their vehicle into a single parking slot.

* **What is a parking slot?**\
A parking slot is the place where a person can park its own vehicle

* **What does it happen when a user wants to park its vehicle?**\
When a user sees a free parking slot they occupy it with its vehicle. After that they report the occupation to the system by selecting the hour in which the stop will end.

* **How can a user see the status of parking slots around the city?**\
The application displays a map with all parking slots in a certain area, determined by the user's location and a specified range of interest. Each parking slot is shown with a marker in one of three possible colors: 
    - **red**: the parking slot is occupied
    - **orange**: the parking slot will be free in ten minutes
    - **green**: the parking slot is free

* **How does a user select the distance of interest in which he can see the parking slots?**\
The user can select the distance of interest specifying a range in which he wants to see the parking slots status.

* **Can a user increase their parking time?**\
Users can increase the duration of their parking anytime. However, they can only specify a time after the previously selected end time (it is not possible to shorten the parking time).
* **Can a user free their parking slot anytime?**\
Yes, a user can free its current occupied parking slot anytime by using the app.

* **What does it happen if a user occupies a parking slot after the stop end?**\
In the first version of the system nothing. In future expansions of the system, where a fee will be requested to park, the fee will be higher (maybe 5x) if they don't remove the vehicle after the stop end.

#### Mockups
The development team created mockups of the frontend based on the information collected during the interview. These mock-ups were then reviewed and discussed by the team to ensure their accuracy.
It is also possible to find a navigable prototype of the GUI [here](https://www.figma.com/proto/qgRlc0HsejXutHe3X2PEs6/Smart-Parking?page-id=0%3A1&node-id=1-1022&viewport=-255%2C4%2C0.76&scaling=min-zoom&starting-point-node-id=1%3A1022).

<figure>
  <img src="../mockups/login.jpg"/>
  <figcaption>Login screen mockup</figcaption>
</figure>

<figure>
  <img src="../mockups/sign_up.jpg"/>
  <figcaption>Sign up screen mockup</figcaption>
</figure>

<figure>
  <img src="../mockups/parking_slots.jpg"/>
  <figcaption>Parking slots map (Home) mockup</figcaption>
</figure>
### Ubiquitous language
After completing the previous step of knowledge crunching, a dictionary was created with the aim of establishing a common language for the entire system's development. The dictionary comprises the following terms:

| Term | Definition |
| ---- | ---------- |
| Backend | The part of the system that communicates with the frontend via APIs and reads and writes data into a database. |
| Duration of parking | The length of time a user occupies a parking slot. |
| Free parking slot | A parking slot that is currently not occupied by a vehicle. |
| Frontend | The part of the system that the user interacts with directly, including the user interface and any client-side logic. |
| Login | The operation that enables a user with an existing account to access the system. |
| Occupied parking slot | A parking slot that is currently occupied by a vehicle. |
| Park | The action of a user placing their vehicle into a single parking slot. |
| Parking slot | The place where a user can park its own vehicle. |
| Parking slots map | A map that shows all the parking slots nearby with a marker. |
| Parking slot marker | A marker used to identify the position of a parking slot in a map, it can be colored in three different ways: green if the parking slot is free, orange if it will be free in ten minutes, red if none of the other cases apply. |
| Parking slot status | The current status of the parking slot: occupied or not occupied. |
| Range of interest | The distance selected by the user in the map to specify the area in which they want to see the parking slots. |
| Sign up | The operation that allows a user without an account to create one. |
| User | A person who interacts with the system to find and occupy parking slots. |
| User credentials | A email-password pair used by the user to log in into the system |
| Vehicle | The object that a user parks in a parking slot, such as a car, truck, or motorcycle. |
| System | The union of the backend and the frontend. |
| Stop end | The time selected by the user to indicate when they will stop occupying a parking slot. |

### Use cases
Following a thorough domain analysis, the use cases and entities involved in this system were identified. The diagram below depicts the use cases for the system: 
![alt text](https://github.com/antonioiannotta/LSS-documentation/blob/main/use_case/use-cases.drawio.png?raw=true "Use cases") \
From the previous image is possible to see how in this system there are two main actors involved in the operations:\
* **User**: the user is the actor that is intended to use the application in order to look for free parking slot, to set an end stop time and to increment this time.
* **Client**: the client is the actor in charge to handle all the requests that are made by the user. More specifically the client is intended to handle the operation of registration and access to the system. The client is also in charge to handle a proper visualization of the status of the parking slots. It's also the client that is in charge to handle the setting of the end stop time or the increment of this time itself. \


It's possible to notice, from the picture, the following operations:
* **User sign up**: The user creates an account with name, email, and password, to access the system.
* **User login**: The user logs in to their account with their username and password to access the system.
* **User password change**: The user changes the password used to access the system by providing the current one and the new one.
* **User deletion**: The user requests the deletion of all their personal information from the system.
* **User logout**: The user logs out their account from the frontend.
* **View parking slots**: The user views a map of available parking slots near their current location. The map shows the markers of the parking slots.
* **View parking slot**: The user views the location and current status of a specific parking slot.
* **View current parking slot**: The user views the location and current status of the parking slot they are currently occupying.
* **Occupy parking slot**: The user occupies a parking slot until stop end.
* **Increment parking slot occupation**: The user sets a new stop end for the parking slot currently occupied.
* **Free parking slot**: The user frees the parking slot currently occupied.

### User stories
After collecting the main use cases for the system, several user stories were developed to describe the system's functionality under specific circumstances. More specifically the user stories illustrated are the followings:
* As a user, I want to access the system so that I can find and occupy a parking slot.
* As a user, I want to occupy a parking slot for a certain duration of time so that I can park my vehicle.
* As a user, I want to see the status of each parking slot on the map so that I can determine whether it is occupied or available.
* As a user, I want to extend the duration of my parking if I need to park my vehicle for a longer period of time.

Below, you will find the illustrated user stories:

![alt text](https://github.com/antonioiannotta/LSS-documentation/blob/main/user%20stories/The%20user%20access%20to%20the%20system_2023-02-07.png?raw=true "User stories access")

![alt text](https://github.com/antonioiannotta/LSS-documentation/blob/main/user%20stories/Parking%20operation_2023-02-07.png?raw=true "Occupy parking slot")

![alt text](https://github.com/antonioiannotta/LSS-documentation/blob/main/user%20stories/Status%20show_2023-02-07.png?raw=true "View parking slot statuses")

![alt text](https://github.com/antonioiannotta/LSS-documentation/blob/main/user%20stories/Parking%20extension_2023-02-07.png?raw=true "Increment parking slot occupation")

### Requirements

#### Business requirements
| ID | Requirement |
| -- | ----------- |
| BR1 | The application should provide an easy way for parking slot owners to manage their slots.

#### User requirements

| ID | Requirement |
| -- | ----------- |
| UR1 | Users should be able to create a new account by providing name, email, and password. |
| UR2 | Users should be able to log in to their account using their email and password. |
| UR3 | Users should be able to change their password. |
| UR4 | Users should be able to delete their account and have all associated data removed from the system. |
| UR5 | Users should be able to search for available parking slots near their current location. |
| UR6 | Users should be able to reserve a parking slot for a specific time period. |
| UR7 | Users should be able to extend the occupation of their parking slot. |
| UR8 | Users should be able to terminate parking slot occupation. |

#### Functional requirements

| ID | Requirement |
| -- | ----------- |
| FR1 | The system should allow users to register by providing name, email and password. |
| FR2 | The system should authenticate registered users by verifying their login credentials (email and password). |
| FR3 | The system should allow users to change their password at any time. |
| FR4 | The system should allow users to delete their account. |
| FR5 | The system should allow users to logout. |
| FR6 | The system should allow users to occupy a parking slot by selecting it from a map and specifying the end of stop. |
| FR7 | The system should allow users to extend the occupation of a parking slot for a specified period. |
| FR8 | The system should show to the user the state of the parking slots nearby him. |
| FR9 | The system should show to the user the parking slot currently occupied by him. |
| FR10 | The system should allow users to terminate the occupation of a parking slot. |

#### Non functional requirements

| ID | Requirement |
| -- | ----------- |
| NFR1 | The system should be easy to maintain and update. |
| NFR2 | The system should be able to scale horizontally in response to changing user demand. |
| NFR3 | The system response times should be fast enough to provide a smooth user experience. |

#### Implementation requirements

| ID | Requirement |
| -- | ----------- |
| NFR1 | The system should be designed using a modular architecture, following the principles of Domain-Driven Design (DDD). This includes clearly defined domain entities, services, and repositories. |
| NFR2 | The application should provide a RESTful API for accessing and modifying data. The API should use standard HTTP methods. |
| NFR3 | The deployment process of the backend and of the frontend should be automated. |

### Strategic Design

#### Subdomains
In Domain Driven Design in order to better manage the complexity of the domain you want to analyze, it is advisable to thoroughly explore the problem-domain by identifying subdomains.
We identified three subdomains:
* **Parking Management (Core)**: this is responsible for managing parking slots, their availability, and their occupancy status. It contains entities such as Parking Slot and Parking Slot State. It is a core subdomain because it contains the main entities and business logic related to the problem: this is the central domain of the application.
* **User Management (Generic)**: this is responsible for managing users, sign up, and login. It contains entities such as User, User Credentials, and Authentication Token. This subdomain is generic because the functionalities it provides are often needed in many different applications and are not specific to the domain of parking management.
* **Client (Supporting)**: this is responsible for handling user interactions, displaying information and sending requests to the backend. It is a supporting subdomain because it provides the user interface and handles user interactions, but it does not contain any business logic related to parking or user management. Its main role is to interact with the other bounded contexts to fulfill user requests and present information to the user.

![alt text](https://github.com/antonioiannotta/LSS-documentation/blob/main//strategic_design/core_domain_chart.png?raw=true "Core Domain Chart")

#### Bounded contexts

The subdomain **Parking Management** is composed of two bounded contexts described in the following bounded context canvases:

| Name | Parking Slot Management |
| Description | Responsible for managing parking slots and their availability. Contains entities such as Parking Slot and Parking Slot State. |
| Domain Roles | execution context |
| Strategic Classification - Domain | Core |
| Strategic Classification - Business Model | - |
| Strategic Classification - Evolution | Custom built |
| Ubiquitous Language | Parking slot, Parking slot status, Range of interest, Vehicle |
| Business Decisions | Possibility to add parking slots |
| Inbound Communications | - |
| Outbound Communication | User Management |

| Name | Parking Slot Occupation Management |
| Description | Responsible for managing parking slot occupation, including incrementing and freeing parking slots. |
| Domain Roles | execution context |
| Strategic Classification - Domain | Core |
| Strategic Classification - Business Model | Revenue generator |
| Strategic Classification - Evolution | Custom built |
| Ubiquitous Language | Occupied parking slot, Park, Parking slot, Parking slot status, Vehicle, Stop end |
| Business Decisions | - |
| Inbound Communications | - |
| Outbound Communication | Parking Slot Management |

The subdomain **User Management** is composed of two bounded contexts described in the following bounded context canvases:

| Name | User Management |
| Description | responsible for managing user and personal information. Contains entities such as User. |
| Domain Roles | execution context |
| Strategic Classification - Domain | Generic |
| Strategic Classification - Business Model | - |
| Strategic Classification - Evolution | Product |
| Ubiquitous Language | Login, Sign Up, User |
| Business Decisions | - |
| Inbound Communications | - |
| Outbound Communication | Authentication |

| Name | Authentication |
| Description | responsible for managing user authentication and authorization. Contains entities such as User Credentials. |
| Domain Roles | execution context |
| Strategic Classification - Domain | Generic |
| Strategic Classification - Business Model | Revenue generator |
| Strategic Classification - Evolution | Product |
| Ubiquitous Language | Login, Sign up, User credentials |
| Business Decisions | - |
| Inbound Communications | - |
| Outbound Communication | User Management |

The supporting subdomain **Client** is composed of two bounded contexts described in the following bounded context canvases:

| Name | User Interface |
| Description | responsible for handling user interactions and presenting information to the user. |
| Domain Roles | execution context |
| Strategic Classification - Domain | Supporting |
| Strategic Classification - Business Model | Engagement creator |
| Strategic Classification - Evolution | Custom built |
| Ubiquitous Language | Duration of parking, Free parking slot, Login, Occupied parking slot, Park, Parking slot, Parking slots map, Parking slot marker, Parking slot status, Range of interest, Sign up, User, Vehicle, Stop end |
| Business Decisions | The user interface should be localizable in different languages |
| Inbound Communications | - |
| Outbound Communication | Network Communication bounded-context |

| Name | Network Communication |
| Description | responsible for communication with the backend server, including sending and receiving data. |
| Domain Roles | execution context |
| Strategic Classification - Domain | Supporting |
| Strategic Classification - Business Model | - |
| Strategic Classification - Evolution | Commodity |
| Ubiquitous Language | Frontend, Backend |
| Business Decisions | - |
| Inbound Communications | - |
| Outbound Communication | Authentication, User Management, Parking slot, Parking slot management |

#### Bounded contexts canvas


#### Context Map
Bounded contexts relationship was modeled in a context map. The context map was generated using Context Mapper VSC's extension.
![alt text](https://github.com/antonioiannotta/LSS-documentation/blob/main/strategic_design/cm.png?raw=true "System context map")


## Design

### Architecture and implementation
The architecture decided to adopt is a simple Client/Server architecture, with the client (the Android app) that is in charge to handle the interaction with the user and the backend (the server) that is in charge to provide consistent data related to the parking slots and that is in charge to handle the registration/access procedures. \
A first general consideration is meaningful considering the separation between the logic/storage component (the backend) and the logic/presentation component (the frontend). This separation has been the result of a deep analysis and it resulted as the best choise because of the possibility to have independence in both architecture and implementation of each part. More specifically, adopting this philosophy of separation of concerns at the architectural side has been possible to choose the database that turned out to be the best for us and to choose the web service framework totally free.
It's important to have a deeper view on both side, frontend and backend, in order to better analyze their own architecture.

#### Backend
The backend is composed by the database and by the software that is in charge to define the logic behind the access to the database. More specifically there are two main component:
* **Database** two different databases was used for the application, one for the user authentication bounded context and one for the parking slot management bounded context. Databases was each stored on MongoDB Cloud.
* **Web Service**
While there's not too much to say about the database it's important to talk about the web service. There are two main routes:
* **/user**
* **/parking-slot**
Each of these routes is in charge to recall logic to handle either the user access/registration to the service or to retrieve/update from/to the database the infromation related to the parking slots.

##### Backend: general concepts
There have been several theoretical consideration that led to choose a specific typology of either database or web framework rather than the another one.
* **Database**: the choise of a NoSQL address has been the result of a simple consideration: since the data in this application have a core into the parking slots it has been wise to map this core even into the database. Adopting a NoSQL database like **MongoDB** made it easier to organize the several documents indexing each one by the parking slot identifier.
* **Web Service**: the choise of the web framework to use for the backend has been led by the consideration of have a single language across the whole application. In order to achieve this goal has been used Ktor as web framework (implemented in Kotlin). In order to implement the routes for the web service has been adopted a ReST approach in order to create ReSTful API. \

##### Backend: architecture
The architecture adopted for the backend has followed the rules of the **Clean architecture**. This architecture led to the detection of several layers for the application and has been applied to both the two modules that the backend has: **user** and **parking slot**. The layers detected are the following:
* **entity**
* **use cases**
* **interface adapter**
* **framework**

In the following is reported the architecture for each module:

###### User 
* Entity:
    - **User**: completely mapped in the database, it represents all used information about the user (email, password and name).
    - **User credentials**: represents a id-password pair used from the user to log him into the system (the user identifier is the email address).
    - **User info**: objects that represent the user's not sensitive information. It corresponds to a user object without password field.
* Use cases:
    - **Login**: a user provides his identified and password to access the system.
    - **create user**: a user wants to register him on the sistem.
    - **Get info**: a user wants see him account information.
    - **Change password**: a user wants to change his password with a new one. 
    - **Delete user**: a registered user wants to delete his account.
    - **Validate**: a email-password pair must be validated (checking if a user with this email exists in the database and if the associated password corresponds with the one in the email-password pair).
    - **Exists** an user email must be verified checking in the database if there's a registered user with this email

* Interface adapter:
    - handles the framework layer requests, translating them into use cases.

* Framework:
    - It's represented by KTOR web server and Mongo database.

###### Parking slot
* Entity:
    - **Paring Slot**: this entity is completely mapped into the database and      represents a single parking slot. Any other component of the parking slot module depends on the parking slot entity.

* Use cases:
    - **Occupy a slot**: this is the use case in which a user is intended to occupy a parking slot for a certain amount of time
    - **Increment occupation**: this is the use case in which a user is intended to extend the occupation of a certain parking slot.
    - **Free slot**: this is the use case in which a user free the parking slot previously occupied.

* Interface adapter:
    - This layer is in charge to handle the requests coming from the framework, that is the most external layer, and translate them into use cases.

* Framework:
    - In this case is represented by the web server and by the database choose for the purpose.

##### Backend: implementation
The technologies used for the implementation of the backend are the following:
* [Ktor](https://ktor.io/): this is the web framework of the Kotlin language used to implement the Rest API.
* [MongoDB](https://www.mongodb.com/): this is the NoSQL databse choose for the purpose of this application.
* [Gradle](https://gradle.org/): as build automation tool has been chosen Gradle.

In the previous section has been reported as the application is composed of two modules related to the functions for the **user** and the functions for the **parking slot**.\ 
The user operations are all those operations related to the access of the user to the system. The parking slot operations are all those operations that are related to the management of parking slot status. These operations are mapped into two submodules for the implementation side correspond to two several collections into the database.
Considering the implementation aspects each module is a single gradle module. More specifically there are three modules:
* **Parking system**: this is the main module, in which there's the access point for the application and the framework layer from the previous paragraph. 
    - **user**: the submodule related to the operations for the user access to the system.
    - **parking-slot**: the submodule related to the operations for the parking-slot management.

###### User

The 3 inner layers of CLEAN architecture are located in **user** submodule while the framework layer is in the core module due to the shared aspect with **parking-slot** submodule (there are framework features shared by both submodules, such as the application server and routing and authentication features). 

* **entity** package contains serializable data classes that implement user entities (User, UserInfo and UserCredentials).
* **use_cases** package contains an interfaace that describe the behaviour of all the use cases.
* **interface_adapter** package contains a class UserInterfaceAdapter that implements the use cases. It also includes two submodules: a model package and a utils package. The model subpackage contains data classes that define the request/response body structure that should be used from the framework layer. The utils subpackage contains utility functions such as email sender method and jwt handler methods (generation and validation).

###### Parking Slot
In order to talk about the implementation of the parking slot operations inside the submodule **parking-slot** is important to keep together implementation and clean architecture. The clean architecture previously presented is composed of four layers. The three inner layers (entity, use cases and interface adapter) correspond to three packages into the **parking-slot** submodule. The fourth layer (framework) is outside this module by the moment it is common to both **parking-slot** and **user** submodules. In the following are reported more specifically the element that fill any of the three layers of the **parking-slot** submodule:
* **entity**: in this package there are all those entities that are the domain basis for the parking-slot subproject. These core entities are:
    - **Position**: represents a single position expressed as latitude-longitude coordinates. This entity is helpful because it provides the exact location for each parking slot.
    - **Center**: represents the area around the which the user is interested about the status of the parking slots.
    - **StopEnd**: represents the end stop setted or incremented for a certain parking slot.
    - **ParkingSlot**: represents a single parking slot with its properties: the status (either occupied or not), the position expressed as latitude and longitude, the end stop time and the identifier (that is unique for each parking slot)
    All these entities are modelled as **Kotlin** classes. This choise derived by the consideration about the possible inheritance of these classes. By the moment these entities could be subjected to inheritance in future uses and future development of the domain the choise has been to model them as classes.

* **use_cases**: in this package are reported the use cases, representing the second layer of the clean architecture previosuly presentend. Remaining on the implementation side the use cases have been represented as method of an interface to be implemented by the interface adapter. This is because the interface adapted is in charge to implement the methods translating into it the framwework requests.

* **interface_adapter**: this package contains a class, **InterfaceAdapter**, that receives as constuctor parameter a collection of the MongoDB database, in this case. This class implements the UseCases interface and provide the results that are used into the framework package to provide the responds to the incoming requests from the client. It's important to notice how, implementing the Interface adapter in this way, the use cases remains independent from it and, in case of change of the Database, will only be necessary to change the constructor argument and some operation inside this class. This solution has been thought to be aligned to the SoC principle and, furthermore, to be coherent with the previous domain analysis.

#### Frontend
The frontend follows the Clean Architecture with a separation into four main modules:
* The domain module contains all the entities, use cases and repositories. It defines the core concepts and business rules of the app and use cases. The domain module does not depend on any other modules.
* The data module is responsible for managing the interaction with local storage, backend API and system services. It contains data sources, and repositories implementation. This module only depends on the domain module.
* The presentation module contains the user interface components and provides the main user-facing features of the app. This module only depends on the domain module.
* The app module depends on all the three previous modules and it is responsible for the launch of the app.

By separating the frontend into distinct modules, we can ensure that the different parts of the app are decoupled and can be developed and tested independently. Both the data module and the presentation module only depends on interface (Dependency Inversion Principle). To give the proper implementation to the modules that needs it we have used the service locator pattern.

##### Domain Module
Divisione nei due moduli
Entities
Use Cases
##### Data Module
Data Source
##### Presentation Module
MVVM

## Testing

### Frontend
Running tests for an Android application often requires the use of an emulator, which can significantly increase the time it takes to complete testing creating a cumbersome and slow environment for developers, which can discourage them from regularly testing their code. This could result in more issues being introduced into the codebase, ultimately leading to longer and more complicated merge processes.

To avoid this scenario, we have decided to write only tests that run without an emulator by making use of [Roboelectric](https://robolectric.org/). This approach reduces the overall testing time reducing also the costs associated with the pipelines.

![alt text](https://github.com/antonioiannotta/LSS-documentation/blob/main/testing/frontend_coverage.png?raw=true "Frontend Coverage")
Even with this approach, we were able to reach good coverage in all the modules except for the presentation one, where, since Jetpack Compose is still pretty young, there is still [an issue](https://github.com/jacoco/jacoco/issues/1208) concerning coverage.

## DevOps Practices

In order to provide a well-organized development process, two separate repositories have been created on Github for the frontend and backend of the project. By creating isolated repositories, each team can manage its own codebase and pipelines, without interfering with the other. This ensures that each team is able to focus on its repository allowing the frontend and the backend to grow independently.


The platforms and tools that have been chosen are the following:
  * **GitHub**
  * **GitHub Actions**
  * **Docker**

### Backend
As previously said have been chosen two different CI/CD pipelines, corresponding to the two repositories created on GitHub. For the pipeline related to the backend repository have been created two workflows for the two main branches:
* **/dev**: there is a workflow that begins its execution everytime there's a push on **/dev** branch. This workflow is helpful because it is in charge to execute the tests calling a **gradle build**.
* **/master**: here there's the most important workflow, because at every push on the branch **/master** the workflow is in charge to push a docker image previously defined into a docker file on DockerHub. It's important to report the main features of the defined dockerfile:
    - it starts from an **Ubuntu** image.
    - install all the needed packages and then clone the repository, checkount on master and then executes the **./gradlew build**. This is done before the image is pushed. When the container is pulled and executed the application starts executing the **./gradlew run** and exposing the API on 8080 port.

### Frontend
The frontend project uses the [Trunk Based Development](https://trunkbaseddevelopment.com) source-control branching model. TBD encourages continuous integration and delivery by minimizing the use of long-lived feature branches: each developer should merge their changes to the main branch frequently by creating pull requests.  This approach avoids the complexity of maintaining multiple branches and allows the team to detect and resolve conflicts early and ensure that everyone is working on the latest version of the codebase. 

In order to enforce this practice we have removed the possibility to push changes directly to the trunk.

#### Pull Request Merges

We wanted to ensure that our codebase remained as clean and organized as possible. By using rebase and merge, we ensure that the commit history remains linear and easy to follow, which makes it much easier to identify and fix any issues that arise down the line.

In order to enforce this practice we have allowed only "rebase and merge" strategy to incorporate changes contained in a pull request.

#### Pull Request Requirements
To preserve the quality of the code and prevent a significant decrease in test coverage due to changes in the codebase, certain criteria must be met by each pull request prior to merging. Specifically:
* All tests must succeed. This helps to ensure that the code changes didn't introduce any new bugs or break existing functionality. This is critical for maintaining the integrity of the project and preventing issues from arising in production.
* A minimum code coverage of 60% for the whole project and patch is achieved. This ensures that any changes is covered by new tests. (checked by Codecov)
* The analysis of the pull request with [SonarQube](https://www.sonarsource.com/products/sonarqube/) reports no code smells, no bugs, no duplicated code and no technical debt.
* All commits and the pull request name follows the [Conventional Commits specification](https://www.conventionalcommits.org/en/v1.0.0/). (checked by [Semantic PRs](https://github.com/marketplace/semantic-prs))
* No sensitive information is present in the pull request (checked by [GitGuardian](https://www.gitguardian.com)).

#### Dependency updates
[Renovate](https://github.com/renovatebot/renovate) 
To ensure that the project remains up-to-date without the need for manual intervention we have decided to use Renovate. Renovate is an automated tool that automatically creates pull requests when it identifies that a dependency is out of date, and if all the requirements above are met, the pull request can be merged (it is automatically merged if it is a MINOR version change).

#### Release
Each time a pull request is merged in the master a pull request named `chore(main): release x.y.z` is updated with a changelog created from conventional commits by the use of [Release Please](https://github.com/googleapis/release-please) with a new version generated according to semantic versioning. This follows the convention of MAJOR.MINOR.PATCH and helps to ensure that the project is versioned consistently over time and not based on some "magic" new feature.

Once this pull request is merged, the VERSION.txt file is automatically updated, the documentation is generated with [Dokka](https://github.com/Kotlin/dokka), and the new version of the app is released in [Firebase App Distribution](https://firebase.google.com/docs/app-distribution). This automated process helps to ensure that releases are consistent and free from human errors.

## Installation

### Frontend
Installing the application on an Android device can be done in a couple of ways. The first option is to download the apk file directly from the [Releases section](https://github.com/GZaccaroni/smart-parking-frontend/releases/latest) of the project's GitHub page. Once the apk file has been downloaded, it can be installed on the device by following the standard installation process.

Alternatively, the application can also be installed using [Firebase App Distribution](https://firebase.google.com/docs/app-distribution). To do this, permission must be requested at [this web page](https://appdistribution.firebase.dev/i/6533aa08a46f931c). Once permission has been granted, the application can be installed on the device using the Firebase App Distribution website. This approach offers the added benefit of being able to easily update the application.
