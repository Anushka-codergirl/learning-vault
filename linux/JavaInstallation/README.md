# Java Installation

## ⚙️ Prerequisites
- For this I'll use JDK14. You can download any version according to your need. [Java Downloads - Oracle](https://oracle.com/java/technologies/downloads/) 
- [Link to Download JDK 14 Debian File](https://www.oracle.com/java/technologies/javase/jdk14-archive-downloads.html)


## Things to do on Terminal

- `java --version` - to check whether JDK is available or not in your system.
    - If jdk isn't present you'll get this output:
    ```bash
    Command 'java' not found, but can be installed with:
    sudo apt install default-jre              # version 2:1.11-72, or
    sudo apt install openjdk-11-jre-headless  # version 11.0.16+8-0ubuntu1~20.04
    sudo apt install openjdk-13-jre-headless  # version 13.0.7+5-0ubuntu1~20.04
    sudo apt install openjdk-16-jre-headless  # version 16.0.1+9-1~20.04
    sudo apt install openjdk-17-jre-headless  # version 17.0.4+8-1~20.04
    sudo apt install openjdk-8-jre-headless   # version 8u342-b07-0ubuntu1~20.04
    ```
    - If jdk is available you'll get this output:
    ```bash
        java 14.0.2 2020-07-14
        Java(TM) SE Runtime Environment (build 14.0.2+12-46)
        Java HotSpot(TM) 64-Bit Server VM (build 14.0.2+12-46, mixed mode, sharing)
    ```


## Let's get started

- Let's navigate to Downloads folder
```bash
anushcodergirl@anushcodergirl:~$ cd Downloads/
```
- ls command will show list of files or directories
```bash
anushcodergirl@anushcodergirl:~/Downloads$ ls
google-chrome-stable_current_amd64.deb  jdk-14.0.2_linux-x64_bin.deb
```
- Enter the following command in your terminal to run the file
```bash
anushcodergirl@anushcodergirl:~/Downloads$ sudo dpkg -i jdk-14.0.2_linux-x64_bin.deb
```
- Enter your UBUNTU passoword
```bash
[sudo] password for anushcodergirl: 
Selecting previously unselected package jdk-14.0.2.
(Reading database ... 185871 files and directories currently installed.)
Preparing to unpack jdk-14.0.2_linux-x64_bin.deb ...
Unpacking jdk-14.0.2 (14.0.2-1) ...
Setting up jdk-14.0.2 (14.0.2-1) ...
```
- Let's run the following alternative command which will setup the path to the Oracle: **sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-14.0.2/bin/java 1**
- Your terminal should look like this: 
```bash
anushcodergirl@anushcodergirl:~/Downloads$ sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-14.0.2/bin/java 1 
update-alternatives: using /usr/lib/jvm/jdk-14.0.2/bin/java to provide /usr/bin/java (java) in auto mode
```
- Next alternative to javac: **update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk-14.0.2/bin/javac 1**
- Your terminal will look similar to this: 
```bash
anushcodergirl@anushcodergirl:~/Downloads$ sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk-14.0.2/bin/javac 1 
update-alternatives: using /usr/lib/jvm/jdk-14.0.2/bin/javac to provide /usr/bin/javac (javac) in auto mode
```
- Let's verify installation done by running this command:   
    - **java -version** - To see java version
    - **javac -version** - To see javac version

---

## Let's set Java Environment Variable

- Running **sudo update-alternatives --config java** command will give you the location to your java installation
```bash
anushcodergirl@anushcodergirl:~/Downloads$ sudo update-alternatives --config java
There is only one alternative in link group java (providing /usr/bin/java): /usr/lib/jvm/jdk-14.0.2/bin/java
Nothing to configure.
```
- Now edit the environment variable file using the command:
**sudo gedit /etc/environment**
- This will open a text editor
- Write following command at the end of the file: 
```java
JAVA_HOME="/usr/lib/jvm/jdk-14.0.2/"
```
- Save and close the file
- Refresh all the paths by running this command: **source /etc/environment**
- Now echo the Java Home: ***echo $JAVA_HOME**
```bash
anushcodergirl@anushcodergirl:~/Downloads$ echo $JAVA_HOME
/usr/lib/jvm/jdk-14.0.2
```

---

<blockquote>Hurray!🙌🥳🎉 You just finished Java Installation. Now you can write Java Code and run in your terminal.</blockquote>
