# Supported Attendance and Bonus System — Environment Setup Guide

> This is a web-based attendance and bonus management application built with **Java 17, Spring Boot 4.1.1, Thymeleaf, Spring Security, JDBC and SQLite**.
> Key facts: **no MySQL and no separate Maven installation are required**. The database is a single file (`data/attendance.db`), and the project ships with its own `mvnw` wrapper.
> The Windows and macOS setup flows below are independent — read only the section for your operating system.

---

## Contents

- [0. Key Facts (Both Systems)](#0-key-facts-both-systems)
- [1. Quick Start Summary](#1-quick-start-summary)
- [2. Windows Setup](#2-windows-setup)
  - [2.1 Install JDK 17](#21-install-jdk-17)
  - [2.2 Network Check and Extraction](#22-network-check-and-extraction)
  - [2.3 Start the Server](#23-start-the-server)
  - [2.4 Stop the Server](#24-stop-the-server)
- [3. macOS Setup](#3-macos-setup)
  - [3.1 Install JDK 17](#31-install-jdk-17)
  - [3.2 Quarantine and Hidden Folders](#32-quarantine-and-hidden-folders)
  - [3.3 Start the Server](#33-start-the-server)
  - [3.4 Stop the Server](#34-stop-the-server)
- [4. Verify It Is Running](#4-verify-it-is-running)
- [5. Open Pages & Log In](#5-open-pages--log-in)
- [6. Language Switching (Bilingual)](#6-language-switching-bilingual)
- [7. Run the Automated Tests](#7-run-the-automated-tests)
- [Appendix: Project Structure](#appendix-project-structure)

---

## 0. Key Facts (Both Systems)

| Item | Description |
|---|---|
| **JDK version** | Must be **17** (Eclipse Temurin 17 or another compatible distribution) |
| **Database** | **SQLite**, a single file `data/attendance.db`; no installation or setup required |
| **Build tool** | The project ships with its own **Maven Wrapper** (`mvnw` / `mvnw.cmd`); no separate Maven install needed |
| **Port** | `8080` |
| **Default account** | username `admin`, password `123456` (created automatically on first start) |
| **Default language** | English; switch via `English \| 中文` on any page |

> ⚠️ Installing **JDK 17** is the only prerequisite on either system; everything else is included.

---

## 1. Quick Start Summary

| Step | Windows | macOS |
|---|---|---|
| ① Install JDK | Install Temurin 17; set `JAVA_HOME` and `Path` | `brew install --cask temurin17` |
| ② Open terminal | Right-click the project folder → Open in Terminal (CMD) | `cd ~/Desktop/csIA` (replace with your actual path) |
| ③ Start | `mvnw.cmd spring-boot:run` | `chmod +x mvnw` → `./mvnw spring-boot:run` |
| ④ Verify | Browser: `http://localhost:8080` | Same as above |

---

## 2. Windows Setup

### 2.1 Install JDK 17

**Option 1: Eclipse Temurin (recommended, free, no sign-up)**
1. Open https://adoptium.net/temurin/releases/ .
2. Set the options to: Operating System = Windows, Architecture = x64, Package Type = JDK, Version = 17.
3. Download the `.msi` and double-click to install.
4. During setup, **make sure to tick "Set JAVA_HOME variable"** — this configures the environment variable automatically.

**Option 2: Oracle JDK 17**
- Go to https://www.oracle.com/java/technologies/downloads/ and download the `x64 MSI Installer`, then double-click to install.
- The environment variables must be configured manually (see below).

**Manual environment setup (only needed for Option 2 or if the checkbox was missed)**
1. Press `Win + S`, search for "Edit the system environment variables" → open it → click "Environment Variables" at the bottom right.
2. Under **System variables**, click **New**:
   - Variable name: `JAVA_HOME`
   - Variable value: `C:\Program Files\Eclipse Adoptium\jdk-17.0.x-hotspot` (replace with your actual install path, **without `bin`**)
3. Find `Path` in the system variables, double-click it → **New** → enter `%JAVA_HOME%\bin`.
4. Click **OK** all the way through to save.

**Verify the installation (open a new CMD window):**

```cmd
java -version
```

The output should show `17.0.x`. If you see `'java' is not recognized...`, the environment variable did not take effect.

### 2.2 Network Check and Extraction

On first start the build downloads Maven and project dependencies, so an internet connection is required (defaults to the Maven Central repository, which may be slow in some regions).

**Recommended: configure the Aliyun mirror (optional, but makes the first download much faster)**

1. Open or create `C:\Users\<your-username>\.m2\settings.xml`.
2. Add:

```xml
<settings>
  <mirrors>
    <mirror>
      <id>aliyun</id>
      <mirrorOf>central</mirrorOf>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
  </mirrors>
</settings>
```

**Extract the project**
- Extract `supported-attendance.zip` to a folder whose path **contains no Chinese characters or spaces**, e.g. `D:\projects\csIA`.
- Paths with Chinese characters or spaces can occasionally break the `mvnw` script, so avoid them.

### 2.3 Start the Server

Open CMD in the project root (the folder containing `pom.xml` and `mvnw.cmd`):

**Method A: address bar**
1. Click the address bar of the folder window, type `cmd` and press Enter to open CMD in that folder.

**Method B: right-click menu**
- Hold `Shift` and right-click in an empty area of the folder → "Open in Terminal" / "Open command window here".

Then run:

```cmd
mvnw.cmd spring-boot:run
```

> If you are using **PowerShell**, write it as:
> ```powershell
> .\mvnw.cmd spring-boot:run
> ```
> Do **not** copy the `PS C:\...>` prompt or the leading `>` character.

The first start downloads dependencies (a few minutes depending on speed). Success looks like this:

```
Tomcat started on port 8080 (http) with context path '/'
Started SupportedAttendanceApplication in x.xxx seconds
```

### 2.4 Stop the Server

In that CMD window, press **`Ctrl + C`** and then type `Y` and press Enter to confirm.

---

## 3. macOS Setup

### 3.1 Install JDK 17

**Option 1: Homebrew (recommended, one command)**

First make sure Homebrew is installed (if not, install it from https://brew.sh ), then:

```bash
brew install --cask temurin17
```

Verify:

```bash
java -version
```

The output should be `17.0.x`. Also check the environment variable:

```bash
echo $JAVA_HOME
```

If it is empty, add the following line to `~/.zshrc` (Apple Silicon) or `~/.bash_profile` (Intel):

```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 17)
```

Then run `source ~/.zshrc` to apply it.

**Option 2: official pkg (no command line needed)**
1. Open https://adoptium.net/temurin/releases/ .
2. Choose Operating System = macOS, Architecture = aarch64 (M1/M2/M3/M4 chips) or `x64` (Intel chips), Package Type = JDK, Version = 17.
3. Download the `.pkg`, double-click to install; the installer configures everything automatically.

> 💡 Not sure which chip? Click  → "About This Mac": "Apple M?" means aarch64, "Intel" means x64.

### 3.2 Quarantine and Hidden Folders

**① Remove the download quarantine (the most common issue)**

macOS marks scripts downloaded from the internet as being "from an unidentified developer", which prevents `mvnw` from running. Before the first run, in the project root:

```bash
xattr -d com.apple.quarantine mvnw
```

If the whole extracted project was quarantined, handle it recursively:

```bash
xattr -dr com.apple.quarantine .
```

**② Make mvnw executable**

```bash
chmod +x mvnw
```

**③ Do not omit the hidden `.mvn` folder**

When copying the project (e.g. via USB drive or re-compressing), **be sure to include the hidden `.mvn` folder** — it contains `maven-wrapper.properties`, without which `mvnw` cannot download Maven. In Finder, press `Cmd + Shift + .` to toggle hidden files.

### 3.3 Start the Server

Open Terminal (Launchpad → Other → Terminal) and enter the project directory. For example, if the project is on the Desktop:

```bash
cd ~/Desktop/csIA
```

> 💡 Tip: type `cd` followed by a space, then drag the folder from Finder into the Terminal window — the path will be filled in automatically, avoiding typos.

Then run:

```bash
./mvnw spring-boot:run
```

The first start downloads Maven and dependencies, so an internet connection is required; please wait a few minutes. Success looks like this:

```
Tomcat started on port 8080 (http) with context path '/'
Started SupportedAttendanceApplication in x.xxx seconds
```

### 3.4 Stop the Server

In the terminal window, press **`Ctrl + C`**.

---

## 4. Verify It Is Running

The same on both systems: when the startup log shows **`Tomcat started on port 8080`** and **`Started ... in x seconds`**, the server is running.

You can now open the following URLs in a browser:

| URL | Purpose |
|---|---|
| `http://localhost:8080/` | Main entry |
| `http://localhost:8080/login` | Administrator login |
| `http://localhost:8080/attendance` | Attendance terminal |
| `http://localhost:8080/health` | System status |

---

## 5. Open Pages & Log In

**Default demonstration account** (created automatically on first start; the password is stored as a BCrypt hash, so later restarts do not overwrite it):

```
username：admin
password：  123456
```

> These credentials are configured in `src/main/resources/application.properties`. To change the password later, do so through the page; editing the properties file has no effect on an account that already exists.

**Login steps**: open `http://localhost:8080/login` in a browser → enter `admin` / `123456` → log in.

---

## 6. Language Switching (Bilingual)

- The `English | 中文` control on any page switches the interface language; the selection is stored in the browser for **one year**.
- The two sets of text correspond to: English — `src/main/resources/messages.properties`; Chinese — `src/main/resources/messages_zh_CN.properties`.
- There is **one codebase and one database**; table names, column names and persisted status values (`OPEN`, `COMPLETED`, `QUALIFIED`, `NOT_QUALIFIED`) are language-neutral English codes and do not change with the interface language.

---

## 7. Run the Automated Tests

The commands differ slightly between systems:

**macOS / Linux:**

```bash
./mvnw test
```

**Windows (CMD):**

```cmd
mvnw.cmd test
```

The tests cover authentication, security routes, database schema, worker management, attendance, bonus thresholds, monthly calculation, page rendering, and English/Chinese language switching. A green **BUILD SUCCESS** means everything passed.

---

## Appendix: Project Structure

```
csIA/
├── pom.xml                          # Maven config: Spring Boot 4.1.1, JDK 17, SQLite, etc.
├── mvnw / mvnw.cmd                  # Maven wrapper (mvnw for Mac/Linux, mvnw.cmd for Windows)
├── .mvn/                            # ⚠️ Hidden folder, keep it
├── data/
│   └── attendance.db                # ⚠️ SQLite database file; all data lives here
├── src/
│   └── main/
│       ├── java/                    # Backend Java code
│       └── resources/
│           ├── application.properties    # Config (includes initial credentials)
│           ├── messages.properties        # English interface text
│           └── messages_zh_CN.properties # Chinese interface text
├── target/                          # Build output (generated automatically; safe to delete)
├── README.md
├── HELP.md
└── supported-attendance.zip         # Project archive
```

**A few things you should not touch or delete:**

- `data/attendance.db`: the database; deleting it wipes your data.
- `.mvn/`: the hidden folder; without it `mvnw` cannot run.
- `pom.xml`: dependency configuration; leave it alone unless you know exactly what you are doing.

---

> If you have **multiple JDK versions (8 / 11 / 17)** installed, make sure `java -version` in the terminal shows 17; otherwise point `JAVA_HOME` at the JDK 17 path.
>
> To repackage as an executable jar, run `mvnw.cmd clean package -DskipTests` (Win) or `./mvnw clean package -DskipTests` (Mac); the output is in `target/`.
