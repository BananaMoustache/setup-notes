# Switch from Java 25 to Java 21

This guide explains how to remove **Java 25**, install **Java 21**, configure the default Java version, and verify the Java environment on Kali Linux.

Java 21 is useful when an application does not support newer Java releases such as Java 25.

---

## 1. Check the Current Java Version

Check the currently active Java runtime:

```bash
java -version
```

Check the Java compiler:

```bash
javac -version
```

Example:

```text
openjdk version "25.x.x"
OpenJDK Runtime Environment
OpenJDK 64-Bit Server VM
```

You can also check which Java binary is currently being used:

```bash
which java
```

---

## 2. Check Installed Java Packages

Before removing Java 25, check how it was installed.

```bash
dpkg -l | grep -E 'openjdk|jdk|jre'
```

To specifically search for Java 25:

```bash
dpkg -l | grep openjdk-25
```

If Java 25 appears in the package list, it was installed through the Debian/Kali package management system.

---

## 3. Remove Java 25

If Java 25 was installed using APT, remove its packages:

```bash
sudo apt purge 'openjdk-25-*' -y
```

Then remove unused dependencies:

```bash
sudo apt autoremove -y
```

Clean the local package cache if desired:

```bash
sudo apt autoclean
```

Check again:

```bash
dpkg -l | grep openjdk-25
```

If nothing is returned, the Java 25 packages have been removed.

---

## 4. Java 25 Installed Manually

If Java 25 does not appear in `dpkg`, it may have been installed manually.

Check available Java installations:

```bash
ls -lah /usr/lib/jvm/
```

Example:

```text
java-21-openjdk-amd64
java-25-openjdk-amd64
jdk-25
```

If the manual Java 25 installation is located at:

```text
/usr/lib/jvm/jdk-25
```

remove it with:

```bash
sudo rm -rf /usr/lib/jvm/jdk-25
```

> Make sure the directory actually belongs to Java 25 before deleting it.

Do not blindly delete directories inside `/usr/lib/jvm`.

---

## 5. Check JAVA_HOME

Check whether `JAVA_HOME` is configured:

```bash
echo $JAVA_HOME
```

Search your shell configuration for Java-related variables:

```bash
grep -nE 'JAVA_HOME|jdk-25|java-25' ~/.zshrc ~/.bashrc 2>/dev/null
```

For example, if you find:

```bash
export JAVA_HOME=/usr/lib/jvm/jdk-25
export PATH=$JAVA_HOME/bin:$PATH
```

remove or update those lines before configuring Java 21.

---

## 6. Update Package Information

Update the package list:

```bash
sudo apt update
```

Check whether Java 21 is available from the currently configured repositories:

```bash
apt policy openjdk-21-jdk
```

You can also search for it:

```bash
apt search openjdk-21
```

---

## 7. Install Java 21 Using APT

If `openjdk-21-jdk` is available:

```bash
sudo apt install openjdk-21-jdk -y
```

The JDK includes the Java runtime and development tools such as:

```text
java
javac
jar
javadoc
```

After installation, check:

```bash
java -version
```

and:

```bash
javac -version
```

Expected major version:

```text
21
```

---

## 8. If `openjdk-21-jdk` Is Not Available

Do not add random Debian repositories to Kali just to obtain Java 21.

Instead, install a trusted Java 21 distribution manually.

The general directory structure should look like:

```text
/usr/lib/jvm/
└── jdk-21/
```

After downloading and extracting a Java 21 JDK, move it to:

```bash
sudo mv jdk-21* /usr/lib/jvm/jdk-21
```

Verify the binary directly:

```bash
/usr/lib/jvm/jdk-21/bin/java -version
```

and:

```bash
/usr/lib/jvm/jdk-21/bin/javac -version
```

---

## 9. Configure Java 21 with `update-alternatives`

If Java 21 was installed manually and is not registered with the system, register the Java binary:

```bash
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-21/bin/java 2100
```

Register the compiler:

```bash
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk-21/bin/javac 2100
```

Then select the Java version:

```bash
sudo update-alternatives --config java
```

Example:

```text
Selection    Path
------------------------------------------------------------
0            /usr/lib/jvm/java-21-openjdk-amd64/bin/java
1            /usr/lib/jvm/java-21-openjdk-amd64/bin/java
2            /usr/lib/jvm/java-25-openjdk-amd64/bin/java
```

Select the entry pointing to Java 21.

Do the same for the compiler:

```bash
sudo update-alternatives --config javac
```

---

## 10. Verify the Default Java Version

Run:

```bash
java -version
```

Then:

```bash
javac -version
```

Check the selected binary:

```bash
readlink -f $(which java)
```

Example:

```text
/usr/lib/jvm/java-21-openjdk-amd64/bin/java
```

For a manually installed JDK, it may look like:

```text
/usr/lib/jvm/jdk-21/bin/java
```

---

## 11. Configure JAVA_HOME

Find the active Java installation:

```bash
dirname $(dirname $(readlink -f $(which java)))
```

Example:

```text
/usr/lib/jvm/java-21-openjdk-amd64
```

For a manually installed JDK:

```text
/usr/lib/jvm/jdk-21
```

Open the Zsh configuration:

```bash
nano ~/.zshrc
```

Add the appropriate configuration.

For Kali/OpenJDK packages:

```bash
# Java 21
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
```

For a manually installed JDK:

```bash
# Java 21
export JAVA_HOME=/usr/lib/jvm/jdk-21
export PATH=$JAVA_HOME/bin:$PATH
```

Reload the configuration:

```bash
source ~/.zshrc
```

---

## 12. Verify JAVA_HOME

Check:

```bash
echo $JAVA_HOME
```

Then:

```bash
$JAVA_HOME/bin/java -version
```

Also verify:

```bash
which java
readlink -f $(which java)
```

All paths should point to Java 21.

---

## 13. Test Java

Create a simple Java file:

```bash
nano Hello.java
```

Add:

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Java 21 is working!");
    }
}
```

Compile it:

```bash
javac Hello.java
```

Run it:

```bash
java Hello
```

Expected output:

```text
Java 21 is working!
```

Clean up:

```bash
rm Hello.java Hello.class
```

---

## 14. Running `.jar` Files

Once Java 21 is configured, a `.jar` application can be executed with:

```bash
java -jar application.jar
```

Example:

```bash
java -jar app.jar
```

For files with spaces in their names:

```bash
java -jar "My Application.jar"
```

---

## 15. Troubleshooting

### Java Still Shows Version 25

Check:

```bash
java -version
```

Then inspect the active binary:

```bash
readlink -f $(which java)
```

Select Java 21 again:

```bash
sudo update-alternatives --config java
```

Also check `JAVA_HOME`:

```bash
echo $JAVA_HOME
```

Search for old Java 25 configurations:

```bash
grep -nE 'jdk-25|java-25|JAVA_HOME' ~/.zshrc ~/.bashrc 2>/dev/null
```

Remove or replace any Java 25 configuration.

---

### `java: command not found`

Check installed JDK directories:

```bash
ls /usr/lib/jvm/
```

Then check alternatives:

```bash
update-alternatives --list java
```

If Java 21 is installed manually but missing from alternatives:

```bash
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-21/bin/java 2100
```

---

### `javac: command not found`

The Java Runtime Environment alone is not enough for compiling Java source code.

Verify:

```bash
which javac
```

If Java 21 was installed using APT:

```bash
sudo apt install openjdk-21-jdk
```

If Java 21 was installed manually, check:

```bash
ls $JAVA_HOME/bin/javac
```

Then register it if necessary:

```bash
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk-21/bin/javac 2100
```

---

## 16. Final Verification

Run:

```bash
java -version
javac -version
echo $JAVA_HOME
which java
which javac
readlink -f $(which java)
```

The environment should now consistently point to Java 21.

Example:

```text
Java Version : 21.x.x
JAVA_HOME    : /usr/lib/jvm/java-21-openjdk-amd64
Java Binary  : /usr/lib/jvm/java-21-openjdk-amd64/bin/java
Javac Binary : /usr/lib/jvm/java-21-openjdk-amd64/bin/javac
```

---

## Recommended Zsh Configuration

For an OpenJDK 21 package installation:

```bash
# Java 21
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
```

Reload:

```bash
source ~/.zshrc
```

Final check:

```bash
java -version
javac -version
echo $JAVA_HOME
```

Java 25 has now been removed and Java 21 is configured as the primary Java environment.
