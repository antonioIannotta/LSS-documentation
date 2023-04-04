---
title: Implementation
has_children: false
nav_order: 7
---

# Implementation

## Backend

The technologies we use for the implementation of the backend are the following:
* [Ktor](https://ktor.io/): this is the web framework of the Kotlin language we use to implement the Rest API.
* [MongoDB](https://www.mongodb.com/): this is the NoSQL databse we choose for the purpose of this application.
* [Gradle](https://gradle.org/): as build automation tool we choose Gradle.

In the *Design* section we reported that the application is composed of two modules: one related to the **user** and the other one to the **parking slot**.\ 
The user operations are all those operations related to the access of the user to the system. Parking slot operations are all those operations that are related to the management of parking slot status. These operations are mapped into two submodules for the implementation side corresponding to two several collections into the database.
Considering the implementation aspects each module is a single Gradle module. More specifically there are three modules:
* **Parking system**: this is the main module, in which there's the access point for the application and the framework layer from the previous paragraph. 
 - **user**: the submodule related to the operations for the user access to the system.
 - **parking-slot**: the submodule related to the operations for the parking-slot management.


## Frontend
In this paragraph, we discuss the implementation details of the frontend, which uses a great number of libraries and frameworks such as:
* [Jetpack Compose](https://developer.android.com/jetpack/compose);
* [Koin](https://insert-koin.io);
* [KotlinX Serialization](https://github.com/Kotlin/kotlinx.serialization);
* [KotlinX DateTime](https://github.com/Kotlin/kotlinx-datetime);
* [Retrofit](https://square.github.io/retrofit/);
* [Google Maps SDK](https://developers.google.com/maps/documentation/android-sdk/maps-compose).
At this point we try to present these libraries and we discuss why we think each one is a good fit for the app.

### Jetpack Compose
Jetpack Compose was chosen as the UI toolkit for building the app. It allows developers to get rid of XML layout files and use newer, easier-to-test declarative UIs. XML layouts force us to use a lot of different files and deal with lower-level things like view id. Moreover, even if Jetpack Compose is quite young, it is greatly integrated with the Android ecosystem and already offers wrappers for all the common functionality needed. Finally, it provides multiplatform support, so it can be deployed also to Web and Desktop platforms, even if that is not necessary for this project.

```kotlin
Column(
    modifier = Modifier
        .padding(32.dp)
        .fillMaxHeight(),
    verticalArrangement = Arrangement.spacedBy(
        10.dp,
        alignment = Alignment.CenterVertically
    )
) {
    Text(
        text = stringResource(R.string.screen_title_login),
        fontSize = 40.sp,
    )

    EmailTextField(
        uiState = uiState,
        onEmailChange = onEmailChange
    )
    PasswordTextField(
        uiState = uiState,
        onPasswordChange = onPasswordChange
    )
    SubmitButton(
        uiState = uiState,
        onSubmit = onSubmit
    )

    SignUpButton(
        uiState = uiState,
        onSignUpClick = onSignUpClick
    )
}
```
<p align="center">Jetpack Compose - Login Form</p>

### Koin
In the design chapter, we decided to use the service locator pattern. A key enabler of this pattern is a dependency-injection-framework. The service-locator pattern can be risky: if you forget to declare a dependency the app will crash at runtime. To avoid that, Koin provides a lot of utilities to test that all the dependencies are registered in tests to avoid bugs sneaking into production.

```kotlin
koinApplication {
    modules(domainModule)
    modules(presentationModule)
    checkModules {
        withInstance<UserRepository>(mockk())
        withInstance<ParkingSlotRepository>(mockk())
    }
}
```
<p align="center">Koin Dependency Testing</p>

### KotlinX Serialization
Kotlin Serialization is a library that allows developers to serialize Kotlin data classes to and from JSON. It is enough to annotate Kotlin data classes as `Serializable` to make them readable from JSON. This aspect, paired with the usage of DTOs, allows us to write very little code for it.


### KotlinX DateTime
kotlinx-datetime is chosen as the date and time library for the Android app. We choose this library because, like Koin, Kotlin Serialization, and Jetpack Compose, it offers multiplatform support and is also deeply integrated with the Kotlin ecosystem, so we do not have to write serializers and deserializers for dates.

### Retrofit
Retrofit is chosen as the networking library for the Android app. It is a networking library for Android that simplifies the process of making HTTP requests and handling responses. It allows us to write data sources easily, since we only have to annotate the fields in the methods.

```kotlin
@GET("parking-slot/")
suspend fun getParkingSlots(
    @Query("latitude")
    latitude: Double,
    @Query("longitude")
    longitude: Double,
    @Query("radius")
    radius: Double,
): List<ParkingSlotDto>
```
<p align="center">Retrofit - getParkingSlots method</p>


### Google Maps SDK
We use the Google Maps SDK for showing the parking slots map. The Google Maps SDK works by using a token that must be included in the app source code. Inserting it in the repository is too risky, as someone could simply take it and use it in their projects. This is why we store it in Github Secrets and add it with the pipelines to the `local.properties` file before building the app.

```
GoogleMap(
    cameraPositionState = cameraPositionState,
    modifier = modifier,
    properties = MapProperties(
        mapType = MapType.NORMAL,
        minZoomPreference = 17f,
        isMyLocationEnabled = true,
    ),
    uiSettings = MapUiSettings(
        indoorLevelPickerEnabled = false,
        mapToolbarEnabled = false,
    ),
)
```
<p align="center">Google Maps SDK usage</p>
