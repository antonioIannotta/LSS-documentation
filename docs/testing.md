---
title: Testing
has_children: false
nav_order: 8
---

# Testing

## Frontend
Running tests for an Android application often requires the use of an emulator, which can significantly increase the time it takes to complete testing creating a cumbersome and slow environment for developers, which can discourage them from regularly testing their code. This could result in more issues being introduced into the codebase, ultimately leading to longer and more complicated merge processes.

To avoid this scenario, we have decided to write only tests that run without an emulator by making use of [Roboelectric](https://robolectric.org/). This approach reduces the overall testing time reducing also the costs associated with the pipelines.

![alt text](https://github.com/antonioiannotta/LSS-documentation/blob/main/testing/frontend_coverage.png?raw=true "Frontend Coverage")
Even with this approach, we were able to reach good coverage in all the modules except for the presentation one, where, since Jetpack Compose is still pretty young, there is still [an issue](https://github.com/jacoco/jacoco/issues/1208) concerning coverage.