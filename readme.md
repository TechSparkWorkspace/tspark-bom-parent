# TechSpark Platform BOM

Welcome to the TechSpark Platform BOM repository! This project provides a centralized Bill of Materials (BOM) for
managing dependency versions across all TechSpark microservices built with Spring Boot. Using this BOM ensures
consistency and simplifies dependency management in your projects.

## Prerequisites

Before you start, ensure you have the following installed:

- Java Development Kit (JDK) 17 or higher
- Gradle 7.x or higher (optional, since the project includes the Gradle Wrapper)
- Git for version control

## Cloning the Repository

To clone the repository, run the following command:

```
git clone https://github.com/TechSparkWorkspace/tspark-bom-parent.git
```

Navigate to the project directory:

```shell
cd tspark-bom-parent
```

## Building the Project

Build the project using the Gradle Wrapper to ensure the correct Gradle version is used:

```shell
./gradlew build
```

On Windows, use:

```shell
gradlew.bat build
```

This command compiles the project and checks for any issues.

## Publishing the Artifact to Maven Repository

### Publishing to Local Maven Repository

To publish the BOM to your local Maven repository (`~/.m2/repository`), run:

```shell
./gradlew publishToMavenLocal
```

This makes the BOM available locally for other projects on your machine.

### Publishing to Remote Maven Repository

#### Step 1: Configure Repository Credentials

Set up your Maven repository credentials in the `gradle.properties` file. You can place this file either in the project
root (do not commit it to version control) or in your Gradle user home directory (`~/.gradle/gradle.properties`).

Add the following properties:

```properties
# gradle.properties
mavenRepoUrl=https://your-maven-repo-url
mavenRepoUsername=your-username
mavenRepoPassword=your-password
```

*Note:* Replace `https://your-maven-repo-url`, `your-username`, and `your-password` with your actual repository URL and
credentials.

#### Step 2: Update `build.gradle` File

Ensure your `build.gradle` file is configured to use the repository credentials:

```groovy
publishing {
    publications {
        mavenJava(MavenPublication) {
            // Existing POM configuration (no changes needed here)
        }
    }
    repositories {
        maven {
            name = 'MyCompanyRepo'
            url = mavenRepoUrl
            credentials {
                username = mavenRepoUsername
                password = mavenRepoPassword
            }
        }
    }
}
```

This setup tells Gradle to publish to the repository specified in mavenRepoUrl using the provided credentials.

#### Step 3: Publish to the Remote Repository

Run the following command to publish the BOM:

```shell
./gradlew publish
```

This uploads the artifact to your remote Maven repository.

## Using the BOM in Your Projects

After publishing, you can import the BOM into your microservice projects to centralize dependency management.

### 1. Add the Repository to Your Project

In your project's `build.gradle` file, add your Maven repository:

```groovy
repositories {
    mavenCentral()
    maven {
        url 'https://your-maven-repo-url' // Replace with your repository URL
        credentials {
            username = 'your-username'
            password = 'your-password'
        }
    }
}
```

### 2. Import the BOM

In your `dependencies` block, import the BOM:

```groovy
dependencies {
    implementation platform('org.techspark:techspark-bom:1.0.0')

    // Define dependencies without versions
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    // Add other dependencies as needed
}
```

This ensures your project uses the dependency versions specified in the BOM.

### 3. Verify Dependencies

Run the following command to verify the right version of dependencies are included.

```shell
./gradlew dependencies
```


