# safelog4j

SafeLog4j can identify and resolve the log4j2 [CVE-2021-45046](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-45046). It can work with custom and third party applications that run on Java and does not require source code.

The patch works by connecting to a Java process, looking for affected versions of Log4j2, and neutralizing the vulnerability. Other application functionality is unaffected and applications proceed as normal with a reduced risk profile.
- If the application was previously vulnerable, this will patch it.
- If the application was not previously vulnerable, this will simply do nothing and can remain in place.

## Usage

SafeLog4j is a Java agent and can be used on any Java application from any OpenJDK vendor: Oracle, AdoptOpenJDK, Azul, Corretto, Liberica, etc.

1. Download the latest SafeLog4j2
1. Place the download on the server you wish to defend
1. Create an environment variable for the user, either in .bashrc or any other location that can affect the user:
  ```shell
  JAVA_TOOL_OPTIONS=-javaagent:/path/to/safelog4j-0.9.1.jar
  ```
1. Restart the application.

Once in place, your application will be defended and you will see output like the picture below. Logs by the defense begin with [safelog4j].

PICTURE

## Comparison to other Scanners and Structures

| Defense | How it compares |
| ------- | ----- |
| File scanning | Logging frameworks can missed when buried inside [fat jars](https://www.baeldung.com/gradle-fat-jar) that are commonly used. A single file does not exist and therefore will not be found. SafeLog4j will find those. |
| Shaded JARs | [Shading](https://maven.apache.org/plugins/maven-shade-plugin/) is a process of moving classes from one package name to another. Logging frameworks are often moved for compatibility reasons to avoid conflicts between libraries. SafeLog4j will find shaded copies of log4j2. |
| Application Servers | Scanning tools will miss vulnerable versions of log4j2 when they scan an application that goes inside the application server but do not scan the application server itself. SafeLog4j analyzes the entire stack to locate vulnerable copies of log4j2. |
| Multiple copies of log4j and other logging frameworks | Some applications, especially large enterprise applications, may contain multiple copies of logging frameworks. While many detection techniques will stop at the first, SafeLog4j will identify and neutralize each vulnerable Log4j2 as it is loaded. |


