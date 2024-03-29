# Quick Start Guide to generating SPDX in you Maven Projects

## Target Audience
This guide is targeted for developers, project managers, and open-source project maintainers using [Apache Maven](https://maven.apache.org/)
who would like to produce accurate SPDX SBOM's which include supplier, dependency, and licensing information.

## Scenario Overview
This guide starts with an existing Maven project with an existing Maven POM file and walks through the steps to generate
SPDX starting with the [Maven in 5 minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) "getting started" exercise results,
using the [SPDX Maven Plugin](https://github.com/spdx/spdx-maven-plugin) to generate an SPDX file then testing the resulting SPDX file for conformance with
the spec.

## Prerequisites
- Complete the [Maven in 5 minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) "getting started" exercise
- Basic understanding of Maven POM file structure and syntax
- Access to the [Maven Central](https://central.sonatype.com/) repository for downloading the SPDX Maven Plugin
- Access to the [SPDX Online Validation tool](https://tools.spdx.org/app/validate/) to validate the resulting SPDX file

## Example
For this quick start guide, we will be using the project resulting from the [Maven in 5 minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) "getting started" exercise.
This is a basic project with just enough information to compile.
You can, of course, follow this guide using one of your own projects, just substitute your POM file for the examples and make the corresponding changes.

## Steps

### 1. (Optional) Maven in 5 Minutes
- Run through all the steps in the [Maven in 5 minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) "getting started"
- Check to make sure your project builds successfully
- You POM file should look like:

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
 
  <properties>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>
 
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

### 2. Add the SPDX Maven Plugin
Edit the pom.xml file for the project.
In the build plugins section, add the plugin with createSPDX goal:

```
<plugin>
  <groupId>org.spdx</groupId>
  <artifactId>spdx-maven-plugin</artifactId>
  <version>0.6.5</version>
  <executions>
    <execution>
      <id>build-spdx</id>
      <phase>verify</phase>
      <goals>
        <goal>createSPDX</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

Note: you may want to replace version with the most recent version of the SPDX maven plugin available on [Maven Central](https://central.sonatype.com/artifact/org.spdx/spdx-maven-plugin/).

You can also put the configuration of spdx-maven-plugin in a `pluginManagement` section of the POM file where it can be
re-used (e.g., defined in a parent POM file).

Below is an example where we start with the [Maven in 5 minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)
output and add the spdx-maven-plugin.

```
...
<build>
  <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
    <plugins>
      <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
      <plugin>
        <artifactId>maven-clean-plugin</artifactId>
        <version>3.1.0</version>
      </plugin>
...
      <plugin>
        <groupId>org.spdx</groupId>
        <artifactId>spdx-maven-plugin</artifactId>
        <version>0.6.5</version>
        <executions>
          <execution>
            <id>build-spdx</id>
            <phase>verify</phase>
            <goals>
              <goal>createSPDX</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </pluginManagement>
  <plugins>
    <plugin>
      <groupId>org.spdx</groupId>
      <artifactId>spdx-maven-plugin</artifactId>
    </plugin>
  </plugins>
</build>
...
```

### 3. Generate the SPDX SBOM
The SPDX document is created during the verify phase.

To test your plugin changes, execute the following:
- `mvn clean`
- `mvn verify`

Your SPDX file will be in the `target/site` directory.

### 4. Test the SPDX Creation

You can verify your SPDX SBOM by running the online [SPDX tool validate](https://tools.spdx.org/app/validate/).

Congratulations!  You can now produce valid SPDX SBOM's for every build.

The SPDX maven plugin will install the SPDX SBOM alongside other build artifacts when you go to install or deploy your project.

## Additional Information
- [Documentation for the createSPDX goal](http://spdx.github.io/spdx-maven-plugin/createSPDX-mojo.html)
- The SPDX Maven Plugin is maintained in the [SPDX Maven Plugin GitHub repo](https://github.com/spdx/spdx-maven-plugin).
- Please see the [README](https://github.com/spdx/spdx-maven-plugin/blob/master/README.md) file for additional documentation many configuration parameters that can be added to your POM file.
- See the [CONTRIBUTING.md](https://github.com/spdx/spdx-maven-plugin/blob/master/CONTRIBUTING.md) file for information on how to report issues and contribute to the SPDX maven plugin project.

## Suggested Next Steps
- Include SPDX generation in your Maven builds today!
- If you are using a project parent POM, consider including the SPDX Maven Plugin in your parent POM file to generate SPDX Documents for all your projects
- Explore additional configuration options for the SPDX plugin
- Improve the quality of your SBOM by adding or reviewing Maven POM file elements used by the SPDX Maven Plugin:
  - name
  - version
  - organization
  - url
  - licenses - with a valid license URL
  - description
- Improve the quality of your SBOM by adding configuration parameters for SBOM details like copyright information, file level information, and package origin
- Get involved - join the SPDX tech mailing list, contribute the SPDX Maven Plugin project
