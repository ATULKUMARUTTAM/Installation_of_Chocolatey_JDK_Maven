# Installation Guide for Chocolatey, JDK, Maven, and Docker

This guide provides detailed, step-by-step instructions for installing **Chocolatey**, **JDK (Java Development Kit)**, **Maven**, and **Docker Desktop** on **Windows** and **macOS**. It is designed for researchers, developers, and beginners setting up a development environment for projects like Spring Boot or machine learning workflows. The guide includes verification steps, troubleshooting tips, and instructions for creating a Spring Boot project with Docker.

## Prerequisites

- **Operating System**: Windows 10/11 (64-bit) or macOS 10.15 (Catalina) or later.
- **Admin/Root Privileges**: Required for installing software and modifying system settings.
- **Internet Connection**: Necessary for downloading installers and packages.
- **Disk Space**: At least 10 GB free for tools and dependencies.
- **Memory**: 8 GB RAM minimum (16 GB recommended for Docker and development).
- **Windows-Specific**: Windows Subsystem for Linux (WSL2) for Docker Desktop.
- **macOS-Specific**: Rosetta 2 for Apple Silicon (M1/M2) if running non-native software.

---

## Windows Installation

### Step 1: Open PowerShell with Admin Privileges

1. **Open PowerShell**:
   - Press `Win + S`, type `PowerShell`, right-click on **Windows PowerShell**, and select **Run as administrator**.
   - Confirm the User Account Control (UAC) prompt by clicking **Yes**.
2. **Verify PowerShell Version**:
   ```powershell
   $PSVersionTable.PSVersion
   ```
   - **Expected Output**: PowerShell version 5.1 or higher (included in Windows 10/11).
   - **If it fails**, update PowerShell or ensure you have admin privileges.

### Step 2: Install Chocolatey

**Chocolatey** is a package manager for Windows, simplifying software installation.

1. **Set Execution Policy**:
   - In the admin PowerShell, run:
     ```powershell
     Set-ExecutionPolicy Bypass -Scope Process -Force
     ```
   - This allows running the Chocolatey installation script.
2. **Install Chocolatey**:
   - Run:
     ```powershell
     [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
     ```
   - Wait for the installation to complete.
3. **Verify Installation**:
   ```powershell
   choco -v
   ```
   - **Expected Output**: A version number (e.g., `2.3.0`).
   - **If it fails**, check internet connectivity and retry.

### Step 3: Install JDK (Java Development Kit)

**JDK 21** is recommended for modern Java applications, including Spring Boot.

1. **Install OpenJDK via Chocolatey**:
   ```powershell
   choco install openjdk --version=21 -y
   ```
   - The `-y` flag auto-confirms prompts.
   - Installs to `C:\Program Files\OpenJDK\openjdk-21` (default path).
2. **Set JAVA_HOME Environment Variable**:
   ```powershell
   [System.Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\Program Files\OpenJDK\openjdk-21", "User")
   ```
   - Add JDK to PATH (if not automatically added):
     ```powershell
     [System.Environment]::SetEnvironmentVariable("Path", "$($Env:Path);$($Env:JAVA_HOME)\bin", "User")
     ```
3. **Verify JDK Installation**:
   ```powershell
   java -version
   ```
   - **Expected Output**: OpenJDK 21 details (e.g., `openjdk 21.0.2 2023-01-17`).
   - **If it fails**, check `JAVA_HOME` with `echo $Env:JAVA_HOME`.

### Step 4: Install Maven

**Maven** is a build automation tool for Java projects.

1. **Install Maven via Chocolatey**:
   ```powershell
   choco install maven -y
   ```
   - Installs to `C:\ProgramData\chocolatey\lib\maven\apache-maven-<version>`.
2. **Refresh Environment Variables**:
   ```powershell
   refreshenv
   ```
   - Updates the PowerShell session with new variables.
3. **Verify Maven Installation**:
   ```powershell
   mvn -v
   ```
   - **Expected Output**: Maven version and Java details (e.g., `Apache Maven 3.9.6`).
   - **If it fails**, ensure `JAVA_HOME` is set and Maven’s `bin` is in the PATH.

### Step 5: Install Docker Desktop

**Docker Desktop** enables containerization for running applications.

1. **Enable WSL2** (required for Docker Desktop):
   ```powershell
   wsl --install
   ```
   - Restart your system if prompted.
   - Verify WSL2:
     ```powershell
     wsl --version
     ```
2. **Download Docker Desktop**:
   - Visit: [Docker Desktop](https://www.docker.com/products/docker-desktop/)
   - Download the Windows installer (`Docker Desktop Installer.exe`).
3. **Install Docker Desktop**:
   - Run the installer.
   - Follow the wizard, ensuring **Enable WSL2 integration** is checked.
   - Restart your system if prompted.
4. **Verify Docker Installation**:
   ```powershell
   docker -v
   ```
   - **Expected Output**: Docker version (e.g., `Docker version 27.1.1, build 1234567`).
   - Run a test container:
     ```powershell
     docker run hello-world
     ```
   - **If it fails**, ensure WSL2 is enabled and Docker Desktop is running.

### Troubleshooting (Windows)

- **Chocolatey Installation Fails**: Check internet connectivity or run `choco upgrade chocolatey -y`.
- **JDK/Maven Not Found**: Verify `JAVA_HOME` (`echo $Env:JAVA_HOME`) and PATH (`echo $Env:Path`).
- **Docker Fails to Start**: Ensure WSL2 and Hyper-V are enabled (`dism.exe /Online /Enable-Feature /All /FeatureName:Microsoft-Hyper-V`).

---

## macOS Installation

### Step 1: Open Terminal

1. **Open Terminal**:
   - Press `Cmd + T` or search for **Terminal** in Spotlight (`Cmd + Space`, type `terminal`).
   - Ensure admin privileges (password may be required for `sudo`).
2. **Verify Shell**:
   ```bash
   echo $SHELL
   ```
   - **Expected Output**: `/bin/zsh` (default on macOS Monterey or later) or `/bin/bash`.
   - **If it fails**, ensure Terminal is launched correctly.

### Step 2: Install Homebrew

**Homebrew** is a package manager for macOS, equivalent to Chocolatey.

1. **Install Homebrew**:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
   - Enter your password and follow prompts.
   - May install Xcode Command Line Tools (requires internet).
2. **Add Homebrew to PATH** (if not automatic):
   - For Apple Silicon (M1/M2):
     ```bash
     echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
     eval "$(/opt/homebrew/bin/brew shellenv)"
     ```
   - For Intel Macs, Homebrew’s `/usr/local/bin` is typically in PATH.
3. **Verify Installation**:
   ```bash
   brew -v
   ```
   - **Expected Output**: Version number (e.g., `Homebrew 4.2.0`).
   - **If it fails**, restart Terminal, run `brew update`, or install Xcode Command Line Tools (`xcode-select --install`).

### Step 3: Install JDK (Java Development Kit)

**JDK 21** is recommended for modern Java applications.

1. **Install OpenJDK 21 via Homebrew**:
   ```bash
   brew install openjdk@21
   ```
   - Installs to `/opt/homebrew/Cellar/openjdk@21/<version>` (Apple Silicon) or `/usr/local/Cellar/openjdk@21/<version>` (Intel).
2. **Set JAVA_HOME**:
   - For Zsh (default shell):
     ```bash
     echo 'export JAVA_HOME=$(/usr/libexec/java_home -v 21)' >> ~/.zshrc
     source ~/.zshrc
     ```
   - For Bash:
     ```bash
     echo 'export JAVA_HOME=$(/usr/libexec/java_home -v 21)' >> ~/.bash_profile
     source ~/.bash_profile
     ```
3. **Verify JDK Installation**:
   ```bash
   java -version
   ```
   - **Expected Output**: OpenJDK 21 details (e.g., `openjdk 21.0.2 2023-01-17`).
   - **If it fails**, check `JAVA_HOME` with `echo $JAVA_HOME`.

### Step 4: Install Maven

**Maven** is a build automation tool for Java projects.

1. **Install Maven via Homebrew**:
   ```bash
   brew install maven
   ```
   - Installs to `/opt/homebrew/Cellar/maven/<version>` (Apple Silicon) or `/usr/local/Cellar/maven/<version>` (Intel).
2. **Verify Maven Installation**:
   ```bash
   mvn -v
   ```
   - **Expected Output**: Maven version and Java details (e.g., `Apache Maven 3.9.6`).
   - **If it fails**, ensure `JAVA_HOME` is set and Homebrew’s `bin` is in PATH (`echo $PATH`).

### Step 5: Install Docker Desktop

**Docker Desktop** enables containerization.

1. **Check for Rosetta 2 (Apple Silicon Only)**:
   ```bash
   softwareupdate --install-rosetta --agree-to-license
   ```
2. **Download Docker Desktop**:
   - Visit: [Docker Desktop](https://www.docker.com/products/docker-desktop/)
   - Download the macOS `.dmg` for your architecture (Apple Silicon or Intel).
3. **Install Docker Desktop**:
   - Open the `.dmg` and drag **Docker** to **Applications**.
   - Launch Docker Desktop from Applications.
   - Grant permissions (e.g., file access).
4. **Verify Docker Installation**:
   ```bash
   docker -v
   ```
   - **Expected Output**: Docker version (e.g., `Docker version 27.1.1, build 1234567`).
   - Run a test container:
     ```bash
     docker run hello-world
     ```
   - **If it fails**, ensure Docker Desktop is running (menu bar whale icon) and Rosetta 2 is installed.

### Troubleshooting (macOS)

- **Homebrew Installation Fails**: Run `brew update` or install Xcode Command Line Tools (`xcode-select --install`).
- **JDK/Maven Not Found**: Verify `JAVA_HOME` (`echo $JAVA_HOME`) and PATH (`echo $PATH`).
- **Docker Fails to Start**: Ensure Docker Desktop is running and Rosetta 2 is installed on Apple Silicon.

---

## Spring Boot Project Setup (Windows and macOS)

Use these tools to set up a Spring Boot project with Docker.

### Step 1: Create a Spring Boot Project

1. **Generate Project**:
   - Visit [Spring Initializr](https://start.spring.io/).
   - Configure:
     - Build Tool: Maven
     - Language: Java
     - Java Version: 21
     - Dependencies: Spring Web, H2 Database, Spring Data JPA
   - Download and extract to:
     - Windows: `C:\Projects\myapp`
     - macOS: `~/Projects/myapp`
2. **Update `pom.xml`**:
   ```xml
   <plugin>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-maven-plugin</artifactId>
     <version>3.2.5</version>
     <executions>
       <execution>
         <goals>
           <goal>build-image</goal>
         </goals>
       </execution>
     </executions>
   </plugin>
   ```
3. **Create `application.properties`**:
   - In `src/main/resources/application.properties`:
     ```properties
     server.port=8080
     spring.datasource.url=jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
     spring.datasource.driver-class-name=org.h2.Driver
     spring.datasource.username=sa
     spring.datasource.password=
     spring.jpa.hibernate.ddl-auto=update
     ```

### Step 2: Build Docker Image

1. **Navigate to Project**:
   - Windows:
     ```powershell
     cd C:\Projects\myapp
     ```
   - macOS:
     ```bash
     cd ~/Projects/myapp
     ```
2. **Build Image**:
   ```bash
   mvn spring-boot:build-image -Dspring-boot.build-image.imageName=yourusername/yourappname
   ```
   - Example: `aku/ecom-application`.
   - mvn spring-boot:build-image -D"spring-boot.build-image.imageName=aku/ecom-app"


### Step 3: Run Docker Container

```bash
docker run -p 8080:8080 yourusername/yourappname
```

- Test in browser/Postman:
  - API: `http://localhost:8080/api/users` (if implemented)
  - H2 Console: `http://localhost:8080/h2-console`

### Step 4: Push to Docker Hub

1. **Log in**:
   ```bash
   docker login
   ```
2. **Push Image**:
   ```bash
   docker push yourusername/yourappname
   ```

### Step 5: Pull and Run Elsewhere

```bash
docker pull yourusername/yourappname
docker run -p 8080:8080 yourusername/yourappname
```

### Optional: Custom Dockerfile

1. **Create `Dockerfile`**:
   ```dockerfile
   FROM eclipse-temurin:21-jdk
   ARG JAR_FILE=target/*.jar
   COPY ${JAR_FILE} app.jar
   ENTRYPOINT ["java","-jar","/app.jar"]
   ```
2. **Build Manually**:
   ```bash
   mvn clean package
   docker build -t yourusername/yourappname .
   ```

---

## Summary of Commands

| Task                          | Windows (PowerShell)                                      | macOS (Terminal)                                      |
|-------------------------------|----------------------------------------------------------|------------------------------------------------------|
| Install Package Manager       | `Set-ExecutionPolicy ...; iex ...` (Chocolatey)          | `/bin/bash -c "$(curl ...)"` (Homebrew)              |
| Install JDK                   | `choco install openjdk --version=21 -y`                 | `brew install openjdk@21`                            |
| Set JAVA_HOME                 | `[System.Environment]::SetEnvironmentVariable("JAVA_HOME", ...)` | `echo 'export JAVA_HOME=...'`                 |
| Install Maven                 | `choco install maven -y`                                | `brew install maven`                                 |
| Install Docker                | Download `Docker Desktop Installer.exe`                 | Download `Docker.dmg`                                |
| Verify Installations           | `choco -v`, `java -version`, `mvn -v`, `docker -v`      | `brew -v`, `java -version`, `mvn -v`, `docker -v`    |
| Build Docker Image            | `mvn spring-boot:build-image ...`                       | Same as Windows                                      |
| Run Docker Container          | `docker run -p 8080:8080 ...`                           | Same as Windows                                      |
| Push to Docker Hub            | `docker push ...`                                       | Same as Windows                                      |

---


