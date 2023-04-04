---
title: DevOps Practices
has_children: false
nav_order: 6
---

# DevOps Practices

In order to provide a well-organized development process, two separate repositories have been created on Github for the frontend and backend of the project. By creating isolated repositories, each team can manage its own codebase and pipelines without interfering with the other. This ensures that each team is able to focus on its repository, allowing the frontend and the backend to grow independently.


The platforms and tools that we choose are the following:
  * **GitHub**
  * **GitHub Actions**
  * **Docker**

## Backend
As previously said have been chosen two different CI/CD pipelines, corresponding to the two repositories created on GitHub. For the pipeline related to the backend repository have been created two workflows for the two main branches:
* **/dev**: there is a workflow that begins its execution everytime there's a push on **/dev** branch. This workflow is helpful because it is in charge to execute the tests calling a **gradle build**.
* **/master**: here there's the most important workflow, because at every push on the branch **/master** the workflow is in charge to push a docker image previously defined into a docker file on DockerHub. It's important to report the main features of the defined dockerfile:
    - it starts from an **Ubuntu** image.
    - install all the needed packages and then clone the repository, checkount on master and then executes the **./gradlew build**. This is done before the image is pushed. When the container is pulled and executed the application starts executing the **./gradlew run** and exposing the API on 8080 port.

## Frontend
The frontend project uses the [Trunk Based Development](https://trunkbaseddevelopment.com) source-control branching model. TBD encourages continuous integration and delivery by minimizing the use of long-lived feature branches, as each developer should merge their changes to the main branch frequently by creating pull requests. This approach avoids the complexity of maintaining multiple branches and allows the team to detect and resolve conflicts early, as well as ensuring that everyone is working on the latest version of the codebase. 

In order to enforce this practice we remove the possibility to push changes directly to the trunk.

### Pull Request Merges

We want to ensure that our codebase remains as clean and organized as possible. By using rebase and merge, we ensure that the commit history remains linear and easy to follow, which makes it trivial to identify and fix any issues that may arise down the line.

In order to enforce this practice we only allow the "rebase and merge" strategy, so as to incorporate changes which are contained in a pull request.

### Pull Request Requirements
To preserve the quality of the code and prevent a significant decrease in test coverage due to changes in the codebase, certain criteria must be met by each pull request prior to merging. Specifically:
* All tests must succeed. This helps to ensure that code changes do not introduce any new bugs or break any existing functionality. This is critical for maintaining the integrity of the project and preventing issues from arising in production.
* A minimum code coverage of 60% for the whole project and patch is achieved. This ensures that any changes are covered by new tests. (checked by Codecov)
* The analysis of the pull request with [SonarQube](https://www.sonarsource.com/products/sonarqube/) reports no code smells, no bugs, no duplicated code and no technical debt.
* All commits and the pull request names follow the [Conventional Commits specification](https://www.conventionalcommits.org/en/v1.0.0/). (checked by [Semantic PRs](https://github.com/marketplace/semantic-prs))
* No sensitive information is present in the pull request (checked by [GitGuardian](https://www.gitguardian.com)).

### Dependency updates
[Renovate](https://github.com/renovatebot/renovate) 
To ensure that the project remains up-to-date without the need for manual intervention, we decide to use Renovate. Renovate is a tool that automatically creates pull requests when it identifies an outdated dependency, and if all the above-mentioned requirements are met, the pull request can be merged (it is automatically merged if it is a MINOR version change).

### Release
Each time a pull request is merged into the `main` branch, a pull request named `chore(main): release x.y.z` is updated with a changelog created from conventional commits by the use of [Release Please](https://github.com/googleapis/release-please) with a new version, generated according to semantic versioning. This follows the convention of MAJOR.MINOR.PATCH and helps to ensure that the project is versioned consistently over time and not based on a "magic" new feature.

Once this pull request is merged, the VERSION.txt file is automatically updated, the documentation is generated with [Dokka](https://github.com/Kotlin/dokka), and the new version of the app is released in [Firebase App Distribution](https://firebase.google.com/docs/app-distribution). This automated process helps to ensure that releases are consistent and free from human errors.
