### Complete Guide to Gradle for DevOps and CI/CD Integration

#### What is Gradle?

Gradle is an open-source build automation tool that is used primarily for Java projects, although it can be used to build, test, and deploy software written in other languages. It combines the best features of Ant and Maven and is designed for multi-project builds. Gradle uses a domain-specific language (DSL) based on Groovy and Kotlin to describe the build logic.

#### Why Use Gradle in DevOps?

- **Declarative Builds**: Uses a DSL for describing builds, making them easier to read and maintain.
- **Incremental Builds**: Supports incremental builds, which improves build performance.
- **Dependency Management**: Advanced dependency management and conflict resolution.
- **Plugins**: Rich ecosystem of plugins that extend its capabilities.
- **CI/CD Integration**: Seamless integration with CI/CD pipelines and other tools.

### Installing Gradle

1. **Windows/Mac/Linux**:
   - Download from the official website: [gradle.org](https://gradle.org/releases/)
   - Extract the downloaded file and set the `GRADLE_HOME` environment variable to the extracted folder.
   - Add `GRADLE_HOME/bin` to the `PATH` environment variable.

2. **Using SDKMAN!**:
   ```sh
   sdk install gradle
   ```

3. **Homebrew (macOS)**:
   ```sh
   brew install gradle
   ```

### Basic Gradle Concepts

- **Project**: A Gradle project represents a thing that is to be built.
- **Task**: A single piece of work executed by Gradle (e.g., compile, test).
- **Build Script**: A script that contains the instructions for building the project (`build.gradle`).

### Creating a Basic Gradle Project

1. **Initialize a New Gradle Project**:
   ```sh
   gradle init --type java-application
   ```

2. **Project Structure**:
   ```
   ├── build.gradle
   ├── settings.gradle
   ├── src
   │   └── main
   │       └── java
   │           └── App.java
   │   └── test
   │       └── java
   │           └── AppTest.java
   ```

3. **build.gradle**:
   ```groovy
   plugins {
       id 'application'
   }

   repositories {
       mavenCentral()
   }

   dependencies {
       implementation 'com.google.guava:guava:31.0.1-jre'
       testImplementation 'junit:junit:4.13.2'
   }

   application {
       mainClassName = 'App'
   }
   ```

4. **settings.gradle**:
   ```groovy
   rootProject.name = 'my-gradle-project'
   ```

### Building and Running the Project

- **Build the Project**:
  ```sh
  gradle build
  ```

- **Run the Application**:
  ```sh
  gradle run
  ```

- **Run Tests**:
  ```sh
  gradle test
  ```

### Integrating Gradle with CI/CD

#### Using Jenkins

1. **Install Gradle Plugin**:
   - Go to `Manage Jenkins` > `Manage Plugins` > `Available`
   - Search for `Gradle Plugin` and install it.

2. **Configure Gradle in Jenkins**:
   - Go to `Manage Jenkins` > `Global Tool Configuration`
   - Add a new Gradle installation and specify the version.

3. **Create a New Jenkins Job**:
   - Select `Freestyle project`
   - In the `Build` section, add a `Invoke Gradle script` build step
   - Specify the tasks to run, e.g., `clean build`

4. **Jenkins Pipeline with Gradle**:
   ```groovy
   pipeline {
       agent any
       tools {
           gradle 'Gradle-6.8.3' // Specify the Gradle installation name
       }
       stages {
           stage('Build') {
               steps {
                   script {
                       gradle 'clean build'
                   }
               }
           }
           stage('Test') {
               steps {
                   script {
                       gradle 'test'
                   }
               }
           }
           stage('Deploy') {
               steps {
                   // Add your deployment steps here
               }
           }
       }
   }
   ```

#### Using GitLab CI/CD

1. **.gitlab-ci.yml**:
   ```yaml
   image: gradle:6.8.3-jdk11

   stages:
     - build
     - test
     - deploy

   build:
     stage: build
     script:
       - gradle clean build

   test:
     stage: test
     script:
       - gradle test

   deploy:
     stage: deploy
     script:
       - ./deploy.sh
   ```

#### Using GitHub Actions

1. **.github/workflows/gradle.yml**:
   ```yaml
   name: Java CI with Gradle

   on:
     push:
       branches: [ main ]
     pull_request:
       branches: [ main ]

   jobs:
     build:
       runs-on: ubuntu-latest

       steps:
       - name: Checkout repository
         uses: actions/checkout@v2

       - name: Set up JDK 11
         uses: actions/setup-java@v2
         with:
           java-version: '11'

       - name: Grant execute permission for gradlew
         run: chmod +x gradlew

       - name: Build with Gradle
         run: ./gradlew build
   ```

### Advanced Gradle Features

#### Custom Tasks

1. **Creating Custom Tasks**:
   ```groovy
   task hello {
       doLast {
           println 'Hello, Gradle!'
       }
   }
   ```

2. **Running Custom Tasks**:
   ```sh
   gradle hello
   ```

#### Multi-Project Builds

1. **settings.gradle**:
   ```groovy
   rootProject.name = 'multi-project'
   include 'projectA', 'projectB'
   ```

2. **projectA/build.gradle**:
   ```groovy
   // Configure projectA specific settings
   ```

3. **projectB/build.gradle**:
   ```groovy
   // Configure projectB specific settings
   ```

4. **Running Multi-Project Build**:
   ```sh
   gradle build
   ```

#### Dependency Management

1. **Adding Dependencies**:
   ```groovy
   dependencies {
       implementation 'org.springframework:spring-core:5.3.8'
       testImplementation 'org.junit.jupiter:junit-jupiter-api:5.7.2'
   }
   ```

2. **Configuring Repositories**:
   ```groovy
   repositories {
       mavenCentral()
       jcenter()
   }
   ```

### Conclusion

Gradle is a powerful build tool that can streamline your DevOps workflows through automation, efficient builds, and seamless integration with CI/CD pipelines. This guide covers the basics of Gradle, including setup, basic usage, and integration with popular CI/CD tools like Jenkins, GitLab CI/CD, and GitHub Actions. For more advanced configurations, refer to the official [Gradle documentation](https://docs.gradle.org/current/userguide/userguide.html).
