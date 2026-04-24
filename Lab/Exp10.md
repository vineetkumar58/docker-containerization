# Experiment 10: SonarQube — Static Code Analysis

---

## Table of Contents

1. [Theory & Introduction](#1-theory--introduction)
2. [Key Terminology](#2-key-terminology)
3. [Lab Architecture](#3-lab-architecture)
4. [SonarQube Server — "The Brain"](#4-sonarqube-server--the-brain)
5. [Sonar Scanner — "The Worker"](#5-sonar-scanner--the-worker)
6. [Why Both Are Required](#6-why-both-are-required)
7. [How the Token Works](#7-how-the-token-works)
8. [Prerequisites](#8-prerequisites)
9. [Step 1 — Start the SonarQube Server](#9-step-1--start-the-sonarqube-server)
10. [Step 2 — Create a Sample Java App with Code Issues](#10-step-2--create-a-sample-java-app-with-code-issues)
11. [Step 3 — Generate a Token (Manual UI Step)](#11-step-3--generate-a-token-manual-ui-step)
12. [Step 4 — Run the Scanner](#12-step-4--run-the-scanner)
13. [Step 5 — View Results in the Dashboard](#13-step-5--view-results-in-the-dashboard)
14. [Step 6 — Integrate with Jenkins (CI/CD)](#14-step-6--integrate-with-jenkins-cicd)
15. [Commands Quick Reference](#15-commands-quick-reference)
16. [Comparative Analysis](#16-comparative-analysis)
17. [Best Practices](#17-best-practices)
18. [Conclusion](#18-conclusion)

---

## 1. Theory & Introduction

### Problem Statement

Code bugs and security issues are often found too late — during testing or even after deployment. Manual code reviews are slow, inconsistent, and do not scale as teams grow.

### What is SonarQube?

**SonarQube** is an open-source platform that automatically scans your source code for:
- Bugs
- Security vulnerabilities
- Maintainability issues

...without running the code. This process is called **static analysis**.

### How SonarQube Solves the Problem

| Capability | Description |
|---|---|
| Continuous scanning | Scans code on every commit, giving immediate feedback |
| Quality Gates | Pass/fail checks before code is deployed |
| Technical Debt tracking | Estimates how long it would take to fix all issues |
| Multi-language support | Supports 20+ programming languages |
| Visual dashboard | Shows trends and history over time |

---

## 2. Key Terminology

| Term | Meaning |
|---|---|
| **Quality Gate** | A set of rules; code must pass before deployment |
| **Bug** | Code that will likely break or behave incorrectly at runtime |
| **Vulnerability** | A security weakness in the code |
| **Code Smell** | Code that works but is poorly written or hard to maintain |
| **Technical Debt** | Estimated time to fix all issues |
| **Coverage** | Percentage of code tested by unit tests |
| **Duplication** | Repeated code blocks (copy-paste code) |

---

## 3. Lab Architecture

SonarQube has two separate components — a **Server** (the brain) and a **Scanner** (the worker). Both are required.

```
┌─────────────────────────────────────────────────────────┐
│                    Your Machine / CI                    │
│                                                         │
│   ┌──────────────┐        ┌──────────────────────────┐  │
│   │  Your Code   │──────▶ │    Sonar Scanner         │  │
│   │  (Java, JS,  │ scans  │  (CLI / Maven / Jenkins) │  │
│   │   Python...) │        └────────────┬─────────────┘  │
│   └──────────────┘                     │ sends report   │
│                                        ▼                │
│                          ┌─────────────────────────┐    │
│                          │   SonarQube Server      │    │
│                          │   (runs on port 9000)   │    │
│                          │   ┌─────────────────┐   │    │
│                          │   │ Analysis Engine │   │    │
│                          │   │ Quality Gates   │   │    │
│                          │   │ Web Dashboard   │   │    │
│                          │   └────────┬────────┘   │    │
│                          └───────────┼─────────────┘    │
│                                      │ stores results   │
│                          ┌───────────▼─────────────┐    │
│                          │   PostgreSQL Database   │    │
│                          └─────────────────────────┘    │
└─────────────────────────────────────────────────────────┘
```

**Flow:**
```
[ Your Code ]
      │
      ▼
[ Sonar Scanner ]  ◀── reads code, detects issues
      │
      │  sends analysis report (HTTP + Token)
      ▼
[ SonarQube Server ]  ◀── validates, stores, displays results
      │
      ▼
[ PostgreSQL Database ]  ◀── persists everything
```

---

## 4. SonarQube Server — "The Brain"

The SonarQube Server is a web application that:

- Stores all analysis results in a database
- Applies code quality rules (bugs, vulnerabilities, smells)
- Shows a web dashboard at `http://localhost:9000`
- Enforces Quality Gates (pass/fail decisions)
- Tracks history and trends over time

> **Analogy:** Think of it like a teacher/examiner — it receives work, grades it, and shows the report.

### What runs inside the server:

| Component | Role |
|---|---|
| Analysis Engine | Applies the rules to submitted code |
| Web UI | Dashboard you browse to at port 9000 |
| PostgreSQL Database | Stores all analysis results persistently |

---

## 5. Sonar Scanner — "The Worker"

The Sonar Scanner is a tool that:

- Reads your source code files
- Detects issues based on the server's rule set
- Sends an analysis report to the SonarQube Server

> **Analogy:** Think of it like a student writing an exam — it does the work and submits it.

### Scanner types:

| Scanner Type | When to use |
|---|---|
| `sonar-scanner` (CLI) | Any project, run manually |
| Maven plugin (`mvn sonar:sonar`) | Java/Maven projects |
| Gradle plugin | Java/Gradle projects |
| Jenkins integration | CI/CD pipelines |
| GitHub Actions | Automated PRs |

### Quick Comparison: Server vs Scanner

| Feature | SonarQube Server | Sonar Scanner |
|---|---|---|
| Type | Server application | CLI / plugin |
| Role | Store & display results | Analyze code |
| Web UI | Yes (port 9000) | No |
| Runs on | Server / Docker container | Dev machine / CI |
| Required | Yes | Yes |

---

## 6. Why Both Are Required

```
  Only Server installed        Only Scanner installed
  ─────────────────────        ──────────────────────
  Dashboard is empty           Nowhere to send results
  No code gets analyzed        Analysis is wasted

  Both installed together
  ───────────────────────
  Full pipeline works ✓
```

---

## 7. How the Token Works

The scanner does not use a username/password. It uses a **token** for authentication.

```
[ Scanner ]
     │
     │  HTTP request + Token header
     ▼
[ SonarQube Server ]
     │
     ├── Token valid?   ──Yes──▶  Accept analysis, store results
     │
     └── Token invalid? ────────▶ Reject (scan fails)
```

**Token facts:**
- Generated once in the Server Web UI
- Used by the Scanner on every scan
- Shown **only once** — copy it immediately

**Three ways to pass the token to the scanner:**

```bash
# Option A — directly in the command
sonar-scanner -Dsonar.login=YOUR_TOKEN

# Option B — environment variable (preferred in CI)
export SONAR_TOKEN=YOUR_TOKEN
sonar-scanner -Dsonar.host.url=http://localhost:9000

# Option C — Maven flag
mvn sonar:sonar -Dsonar.login=YOUR_TOKEN
```

---

## 8. Prerequisites

Before starting the lab, make sure you have the following installed:

| Tool | Check command | Purpose |
|---|---|---|
| Docker Desktop | `docker --version` | Run SonarQube containers |
| Docker Compose | `docker-compose --version` | Orchestrate multi-container setup |
| Java (JDK 11+) | `java -version` | Required for Maven scanner |
| Maven | `mvn --version` | Run `mvn sonar:sonar` |
| Text Editor | — | Create and edit files |

> **Note:** Java and Maven are only needed for Option A (Maven scanner). If you use the Docker CLI scanner (Option B), only Docker is required.

---

## 9. Step 1 — Start the SonarQube Server

We use Docker Compose to start both the SonarQube server and its PostgreSQL database together.

### Create the project folder

```bash
mkdir sonar-lab
cd sonar-lab
```

### Create `docker-compose.yml`

```yaml
# docker-compose.yml
# Starts two containers:
#   1. sonar-db     → PostgreSQL database
#   2. sonarqube    → SonarQube server (web UI + analysis engine)

version: '3.8'

services:

  sonar-db:
    image: postgres:13
    container_name: sonar-db
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
      POSTGRES_DB: sonarqube
      POSTGRES_HOST_AUTH_METHOD: trust
    volumes:
      - sonar-db-data:/var/lib/postgresql/data
    networks:
      - sonarqube-lab

  sonarqube:
    image: sonarqube:lts-community
    container_name: sonarqube
    ports:
      - "9000:9000"
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://sonar-db:5432/sonarqube
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
    volumes:
      - sonar-data:/opt/sonarqube/data
      - sonar-extensions:/opt/sonarqube/extensions
    depends_on:
      - sonar-db
    networks:
      - sonarqube-lab

volumes:
  sonar-db-data:
  sonar-data:
  sonar-extensions:

networks:
  sonarqube-lab:
    driver: bridge
```

### Start both containers

```bash
docker-compose up -d
```

### Watch the logs until you see "SonarQube is up"

```bash
docker-compose logs -f sonarqube
```

> **Note:** This can take 2–3 minutes. Wait for the message `SonarQube is up` before proceeding.

### Access the Web UI

Open your browser at: **http://localhost:9000**

- Default login: `admin` / `admin`
- You will be prompted to change the password on first login.

---

## 10. Step 2 — Create a Sample Java App with Code Issues

We create a simple Java class that intentionally contains bugs, vulnerabilities, and code smells — so SonarQube has something to detect.

### Create the folder structure

```bash
mkdir -p sample-java-app/src/main/java/com/example
cd sample-java-app
```

### Create `src/main/java/com/example/Calculator.java`

```java
package com.example;

public class Calculator {

    // BUG: Division by zero is not handled
    // If someone calls divide(5, 0), this will crash at runtime.
    public int divide(int a, int b) {
        return a / b;
    }

    // CODE SMELL: Unused variable
    // The variable 'unused' is declared but never used.
    public int add(int a, int b) {
        int result = a + b;
        int unused = 100;   // ← code smell: delete this line
        return result;
    }

    // VULNERABILITY: SQL Injection risk
    // Building a query by concatenating user input is dangerous.
    // An attacker could pass: "1 OR 1=1" and get all users.
    public String getUser(String userId) {
        String query = "SELECT * FROM users WHERE id = " + userId;
        return query;
    }

    // CODE SMELL: Duplicated code
    // These two methods do exactly the same thing.
    public int multiply(int a, int b) {
        int result = 0;
        for (int i = 0; i < b; i++) {
            result = result + a;
        }
        return result;
    }

    public int multiplyAlt(int a, int b) {
        int result = 0;
        for (int i = 0; i < b; i++) {
            result = result + a;   // ← exact duplicate of multiply()
        }
        return result;
    }

    // BUG: Null pointer risk
    // If 'name' is null, calling .toUpperCase() will throw NullPointerException.
    public String getName(String name) {
        return name.toUpperCase();
    }

    // CODE SMELL: Empty catch block
    // The exception is caught but silently ignored.
    public void riskyOperation() {
        try {
            int x = 10 / 0;
        } catch (Exception e) {
            // ← never leave catch blocks empty
        }
    }
}
```

### Intentional issues summary

| Issue Type | Method | Description |
|---|---|---|
| Bug | `divide()` | No check for division by zero |
| Bug | `getName()` | Null pointer if `name` is null |
| Vulnerability | `getUser()` | SQL injection via string concatenation |
| Code Smell | `add()` | Unused variable `unused` |
| Code Smell | `multiply()` + `multiplyAlt()` | Duplicated code blocks |
| Code Smell | `riskyOperation()` | Empty catch block hides errors |

### Create `pom.xml`

Place this in the `sample-java-app` root folder:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>sample-app</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <sonar.projectKey>sample-java-app</sonar.projectKey>
        <sonar.host.url>http://localhost:9000</sonar.host.url>
        <!-- Replace with your actual token after Step 3 -->
        <sonar.login>YOUR_TOKEN_HERE</sonar.login>
    </properties>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.sonarsource.scanner.maven</groupId>
                <artifactId>sonar-maven-plugin</artifactId>
                <version>3.9.1.2184</version>
            </plugin>
        </plugins>
    </build>

</project>
```

---

## 11. Step 3 — Generate a Token (Manual UI Step)

The Scanner needs a token to authenticate with the server. You generate this in the web UI.

### Steps to generate the token

1. Open **http://localhost:9000** and log in as `admin`
2. Click your **user icon** (top-right corner)
3. Click **My Account**
4. Click the **Security** tab
5. Under "Generate Tokens", type a name: `scanner-token`
6. Click **Generate**
7. **Copy the token immediately** — it is shown only once!

The token looks like:
```
sqp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

> ⚠️ **Important:** The token is shown only once. If you close the page without copying it, you must delete it and generate a new one.

### Update pom.xml with the token

Open `pom.xml` and replace `YOUR_TOKEN_HERE`:

```xml
<sonar.login>sqp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</sonar.login>
```

---

## 12. Step 4 — Run the Scanner

There are two options. Choose the one that matches your setup.

### Option A — Maven Plugin (recommended for Java)

```bash
# Make sure you are inside sample-java-app folder
cd sample-java-app

# Run SonarQube analysis via Maven
mvn sonar:sonar -Dsonar.login=YOUR_TOKEN
```

Maven will compile the code, then the sonar plugin will send the analysis report to the server.

---

### Option B — Sonar Scanner CLI via Docker (no Java/Maven needed)

**Step 1:** Create `sonar-project.properties` in the `sample-java-app` folder:

```properties
sonar.projectKey=sample-java-app
sonar.projectName=Sample Java Application
sonar.projectVersion=1.0
sonar.sources=src
sonar.java.binaries=target/classes
sonar.sourceEncoding=UTF-8
```

**Step 2:** Find your Docker network name:

```bash
docker network ls
# Look for a network with "sonarqube" in the name
# e.g. sonar-lab_sonarqube-lab
```

Or inspect the sonarqube container:

```bash
docker inspect sonarqube | grep -i network
```

**Step 3:** Run the scanner container:

```bash
docker run --rm \
  --network sonar-lab_sonarqube-lab \
  -e SONAR_TOKEN="YOUR_TOKEN" \
  -v "$(pwd):/usr/src" \
  sonarsource/sonar-scanner-cli \
  -Dsonar.host.url=http://sonarqube:9000 \
  -Dsonar.projectBaseDir=/usr/src \
  -Dsonar.projectKey=sample-java-app
```

### Docker flag explanation

| Flag | Purpose |
|---|---|
| `--rm` | Auto-delete the scanner container after it finishes |
| `--network` | Connect to the same Docker network as SonarQube |
| `-e SONAR_TOKEN` | Pass the authentication token |
| `-v` | Mount your source code folder inside the container |
| `-Dsonar.host.url` | URL of the SonarQube server |
| `-Dsonar.projectBaseDir` | Project root inside the container |
| `-Dsonar.projectKey` | Project identifier stored on the server |

> **Note:** We use `http://sonarqube:9000` (container name), NOT `localhost`, because the scanner container communicates with the server container over the Docker network.

---

## 13. Step 5 — View Results in the Dashboard

After the scan finishes, open the project dashboard:

```
http://localhost:9000/dashboard?id=sample-java-app
```

### Expected results

```
┌─────────────────────────────────────────────┐
│         sample-java-app — Dashboard         │
├──────────────┬──────────────┬───────────────┤
│  Bugs        │ Vulnerab.    │  Code Smells  │
│    5         │    1         │    8          │
├──────────────┴──────────────┴───────────────┤
│  Coverage: 0%    Duplications: 2 blocks     │
│  Technical Debt: ~1h 30min                  │
│  Quality Gate: ✗ FAILED                     │
└─────────────────────────────────────────────┘
```

Click on any number in the dashboard to see the exact file, line number, and reason for each issue.

### Query results via API (optional)

```bash
# List all bugs found in the project (returns JSON)
curl -u admin:YOUR_TOKEN \
  "http://localhost:9000/api/issues/search?projectKeys=sample-java-app&types=BUG"
```

---

## 14. Step 6 — Integrate with Jenkins (CI/CD)

Once SonarQube is working locally, add it to a Jenkins pipeline so every code commit is automatically scanned.

### Start Jenkins

```bash
docker run -d -p 8080:8080 \
  -v jenkins-data:/var/jenkins_home \
  jenkins/jenkins:lts
```

### Configure SonarQube in Jenkins

1. Go to **Jenkins → Manage Jenkins → Configure System**
2. Add a SonarQube server entry:
   - Name: `SonarQube`
   - URL: `http://sonarqube:9000`
3. Go to **Credentials**, add a Secret Text credential with your token, ID = `sonar-token`

### Create `Jenkinsfile` in your project root

```groovy
// Jenkinsfile
// Pipeline: Checkout → SonarQube Scan → Quality Gate → Build → Deploy

pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://sonarqube:9000'
        // 'sonar-token' is a Jenkins credential — store your token there
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn clean verify sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Wait for SonarQube to finish processing.
                // If Quality Gate fails, pipeline stops here.
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build') {
            steps {
                // Only runs if Quality Gate passed
                sh 'mvn package'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker build -t sample-app .'
                sh 'docker run -d -p 8080:8080 sample-app'
            }
        }
    }
}
```

### Pipeline flow

```
Checkout → SonarQube Scan → Quality Gate check → Build → Deploy
                                    │
                              FAIL? │
                                    ▼
                         Pipeline stops.
                         Code is NOT deployed.
```

---

## 15. Commands Quick Reference

### SonarQube

```bash
# Start server + database
docker-compose up -d

# View logs
docker-compose logs -f sonarqube

# Stop everything
docker-compose down

# Stop and remove all data (volumes)
docker-compose down -v

# Run scan with Maven
mvn sonar:sonar -Dsonar.login=YOUR_TOKEN

# Run scan with Docker CLI
docker run --rm \
  --network sonar-lab_sonarqube-lab \
  -e SONAR_TOKEN="YOUR_TOKEN" \
  -v "$(pwd):/usr/src" \
  sonarsource/sonar-scanner-cli \
  -Dsonar.host.url=http://sonarqube:9000 \
  -Dsonar.projectKey=sample-java-app

# Query API for bugs
curl -u admin:YOUR_TOKEN \
  "http://localhost:9000/api/issues/search?projectKeys=sample-java-app&types=BUG"
```

### Jenkins

```bash
# Start Jenkins container
docker run -d -p 8080:8080 \
  -v jenkins-data:/var/jenkins_home \
  jenkins/jenkins:lts

# Example pipeline snippet
pipeline {
    agent any
    stages {
        stage('Build') { steps { sh 'mvn package' } }
    }
}
```

### Ansible (for reference)

```bash
ansible all -i inventory -m ping
ansible-playbook -i inventory playbook.yml
ansible webservers -m apt -a "name=nginx state=present"
```

### Chef (for reference)

```bash
knife node list
knife cookbook upload webapp
chef-client --local-mode --runlist 'recipe[webapp]'
```

---

## 16. Comparative Analysis

### Tool Comparison Matrix

| Feature | Jenkins | Ansible | Chef | SonarQube |
|---|---|---|---|---|
| Primary Purpose | CI/CD Automation | Config Management | Config Management | Code Quality |
| Architecture | Master-Agent | Agentless | Client-Server | Client-Server |
| Language | Java / Groovy | YAML | Ruby | Java |
| Learning Curve | Moderate | Low | High | Low |
| Setup Complexity | Moderate | Simple | Complex | Simple |
| Use Case | Build, Test, Deploy | Infrastructure as Code | Enterprise CM | Static Analysis |

### Key Differences

**Jenkins vs Configuration Management:**
Use Jenkins to orchestrate pipelines (build → test → deploy). Use Ansible or Chef to maintain server configuration. In practice, Jenkins calls Ansible/Chef as part of the deployment step.

**Ansible vs Chef:**

| Aspect | Ansible | Chef |
|---|---|---|
| Complexity | Simple, YAML | Complex, Ruby DSL |
| Agent required | No (agentless) | Yes (chef-client) |
| Best for | Small–medium teams | Large enterprise |

**SonarQube in CI/CD:**
Scan on every pull request and every nightly build. Use Quality Gates to block deployments when code quality drops below your threshold.

---

## 17. Best Practices

### Security

- Never hardcode tokens or passwords in source files — use environment variables or a secrets manager
- Use HTTPS for all SonarQube server communications in production
- Apply least-privilege: give the scanner token only the permissions it needs

### Code Quality

- Set Quality Gates to block merges when coverage drops below 80%
- Scan on every pull request, not just nightly builds
- Fix issues as they appear — do not let technical debt accumulate

### Infrastructure as Code

- Version all config files in Git
- Use modular Ansible roles and Chef cookbooks
- Test infrastructure code with Molecule (Ansible) or Test Kitchen (Chef)

### CI/CD Optimization

- Cache Maven / npm dependencies between builds
- Run SonarQube analysis in parallel with unit tests where possible
- Configure the pipeline to fail fast on Quality Gate failures

---

## 18. Conclusion

### What We Accomplished

In this experiment, we successfully:

1. **Set up SonarQube** using Docker Compose with a PostgreSQL database backend, exposing the web dashboard at `http://localhost:9000`.

2. **Created a deliberately flawed Java application** that contained real-world examples of bugs (division by zero, null pointer dereference), a security vulnerability (SQL injection), and code smells (unused variables, duplicate methods, empty catch blocks).

3. **Generated a scanner token** from the SonarQube web UI — understanding that this token is the authentication bridge between the Scanner and the Server.

4. **Ran the Sonar Scanner** using both the Maven plugin and Docker CLI approaches, which sent the analysis report to the SonarQube server.

5. **Viewed the analysis results** on the web dashboard, where SonarQube correctly identified all intentionally introduced issues — confirming the full pipeline works end-to-end.

6. **Integrated SonarQube with Jenkins** using a Jenkinsfile that enforces a Quality Gate, which automatically blocks code from being deployed if it fails the quality threshold.

### Key Takeaways

| Concept | Lesson |
|---|---|
| Static analysis | Finds bugs without running the code — fast and reliable |
| Two-component design | Server stores results; Scanner reads code and sends reports |
| Token-based auth | Secure, revocable, and CI/CD friendly |
| Quality Gates | The enforcement mechanism — bad code never reaches production |
| Technical Debt | SonarQube quantifies it, making it visible and actionable |
| CI/CD integration | Scanning every commit is the industry standard practice |

### Why SonarQube Matters

SonarQube transforms code quality from a subjective, manual review process into an **automated, measurable, and enforceable** standard. By integrating it into a CI/CD pipeline with Jenkins, every commit is automatically evaluated — and only code that meets the Quality Gate is allowed to proceed to build and deployment. This dramatically reduces the number of bugs and vulnerabilities that reach production, lowers long-term maintenance costs, and enforces consistent coding standards across the entire team.

---

*Experiment 10 — SonarQube Static Code Analysis | DevOps Lab*
