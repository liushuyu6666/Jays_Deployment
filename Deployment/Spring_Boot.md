- [Overview](#overview)
- [Steps](#steps)
  - [Build the project](#build-the-project)
    - [Setup the environment](#setup-the-environment)
    - [Build the project](#build-the-project-1)
  - [Run the project](#run-the-project)
    - [Grant execution permission](#grant-execution-permission)
    - [Launch the project](#launch-the-project)
    - [Check the status](#check-the-status)
  - [Other peripherals](#other-peripherals)
    - [Install Database](#install-database)


# Overview
This section outlines the manual deployment process for a Spring Boot project. The instructions below utilize the **Gradle** build automation tool and demonstrate deployment on an **Ubuntu** environment.

# Steps
## Build the project
Deploying a Spring Boot project involves two vital components: a build automation tool and the indispensable OpenJDK.
1. **Build Automation Tool (e.g., Gradle)** (like Gradle) is responsible for managing your project's build process, including fetching dependencies, compiling code, and creating distribution artifacts. It uses the JDK tools (like `javac` for compiling) to carry out these tasks 
2. **OpenJDK (Java Development Kit)** provides the necessary tools and runtime environment to compile and execute Java code. When you build and run a Spring Boot application (or any Java application) using Gradle, Gradle uses the JDK to compile your source code into bytecode and package it into a deployable format. When you run the application, the JDK provides the Java Virtual Machine (JVM) to execute the bytecode. Even in the presence of Gradle, the necessity of OpenJDK remains steadfast.

### Setup the environment
1. Ensure Gradle Compatibility:
   1. Navigate to your project's root directory containing the `gradlew` script.
   2. Determine your project's Gradle version using:
    ```bash
    ./gradlew --version
    ```
   3. Check server's Gradle version:
    ```bash
    gradle -v
    ```
2. Upgrade Gradle (if needed):
   1. Use the following commands to upgrade Gradle:
    ```bash
    curl -s "https://get.sdkman.io" | bash
    source "$HOME/.sdkman/bin/sdkman-init.sh"
    sdk install gradle <version>
    ```
   2. After upgrade, update the Gradle version in scripts:
    ```bash
    ./gradlew wrapper --gradle-version <version>
    ```
3. Verify JDK Compatibility:
   1. Check `sourceCompatibility` in `build.gradle` for required JDK version of the project.
   2. Use
    ```bash
    apt search openjdk
    java -version
    ```
    to check server's Java version.
4. Install OpenJDK:
   1. Install the required OpenJDK version (e.g., OpenJDK 19):
    ```bash
    sudo apt install openjdk-19-jdk
    ```
   2. Optional: If needed, uninstall an old JDK version:
    ```bash
    sudo apt remove --purge openjdk-11-jdk
    sudo apt autoremove
    ```

### Build the project
1. Open a terminal and navigate to the project's root directory.
2. Execute the command
    ```bash
    gradle build
    ```
    to initiate the build process.
3. Upon successful completion, the compiled JAR or WAR file will reside in the `build/libs` directory within your project folder.
4. Locate the compiled JAR/WAR file. By default, it might be named `demo-0.0.1-SNAPSHOT.jar`.
5. Optionally, move the JAR/WAR file to your preferred deployment location.

## Run the project
### Grant execution permission
Before running the project, ensure proper execution permissions are granted. Follow these steps:
1. Check Permissions: In the terminal, navigate to your project's directory and use the command:
    ```bash
    ls -l <Your_JAR_Package_Name>
    ```
    Observe the permission string in the output. If it appears as `-rwxrwxr--`, execution permission is absent.
2. Grant Permission: To allow execution, use the command:
    ```bash
    chmod +x <Your_JAR_Package_Name>
    ```
3. Verify Permission: Confirm the updated permission string, now displayed as `-rwxrwxr-x`, indicating execution access.

### Launch the project
To start the Spring Boot server in the background as a daemon process, follow these steps:
1. Navigate to the project directory containing the JAR file using the terminal.
2. Run the following command:
    ```bash
    nohup java -jar demo-0.0.1-SNAPSHOT.jar > output.log &
    ```
    This will start the server in the background and create an `output.log` file to capture the server's output.
3. To monitor the server logs in real-time, use the command:
    ```bash
    tail -f output.log
    ```
    This will display the log output as it's being written to the file.

### Check the status
Verify the successful deployment by confirming that the program is listening on port `8080`. Use the following command:
```bash
sudo lsof -i :8080
```

## Other peripherals
### Install Database
Since we are using MongoDB, we should install MongoDB:
   1. Follow [the official instruction](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/) for installing MongoDB on Ubuntu.
   2. Start MongoDB using the command:
    ```bash
    sudo systemctl start mongod
    ```
    or
    ```bash
    sudo service mongod start
    ```
   3. Check the status of MongoDB using the command:
    ```bash
    sudo systemctl status mongod
    ```
    or
    ```bash
    sudo service mongod status
    ```
   4. Enable MongoDB to run on system boot with the command:
    ```bash
    sudo systemctl enable mongod
    ```

   
