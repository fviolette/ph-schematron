#ph-schematron

ph-schematron is a Java library that validates XML documents via [ISO Schematron](http://www.schematron.com).
It is licensed under Apache 2.0 license.

It offers several different possibilities to perform this task where each solution offers its own advantages and disadvantages that are outlined below in more detail. ph-schematron only supports ISO Schematron and no other Schematron version.
The most common way is to convert the source Schematron file to an XSLT script and apply this XSLT on the XML document to be validated. Alternatively ph-schematron offers a native implementation for the Schematron XPath binding which offers superior performance over the XSLT approach but has some other minor limitations.

Continue reading the **full documentation** at http://phax.github.io/ph-schematron/.

##News and noteworthy

  * v4.2.2 - 2017-02-22
    * Updated to Saxon-HE 9.7.0-15
    * Fixed usage of `<let>` in `<extend>`-based rules for the pure implementation (#36)
  * v4.2.1 - 2017-01-20
    * Added WrappedCollectingPSErrorHandler
  * v4.2.0 - 2017-01-09
    * Binds to ph-commons 8.6.0
    * Updated to Saxon-HE 9.7.0-14
    * Added a new Schematron validation Maven plugin
  * v4.1.1 - 2016-11-03
    * Added possibility to use XML EntityResolver (#30)
    * Updated to Saxon-HE 9.7.0-10
  * v4.1.0 - 2016-09-09
    * Binding to ph-commons 8.5.x
  * v4.0.2 - 2016-07-22
  * v4.0.1 - 2016-07-05
    * better integration of sch2xslt Maven plugin into m2e - thanks to @baerrach
  * v4.0.0 - 2016-06-15
    * updated to JDK8
    * updated to Saxon-HE 9.7
  * v3.0.1 - 2015-10-14
    * keep diagnostics in Pure version; resource resolving emits to error handler
  * v3.0.0 - 2015-07-29
    * because of update to ph-commons 6.0.0; extended XSLT based API 
  * v2.9.2 - 2015-03-12
    * because of update to ph-commons 5.6.0 
  * v2.9.1 - 2015-02-03
    * fixes a classloader issue added in 2.9.0
  * v2.9.0 - 2015-01-30
    * introduced new APIs in several places
    * updated to Saxon-HE 9.6
  * v2.8.4 - 2014-10-30    
  * v2.8.3 - 2014-09-16
    * An easy way to use XQuery functions (like funcx library) as custom XPath functions was added
  * v2.8.2 - 2014-09-02
  * v2.8.1 - 2014-08-29
  * v2.8.0 - 2014-08-28

## Usage with Maven
The dependency for ph-schematron looks like this:
```
<dependency>
  <groupId>com.helger</groupId>
  <artifactId>ph-schematron</artifactId>
  <version>4.2.2</version>
</dependency>
```
It transitively contains [ph-commons](https://github.com/phax/ph-commons), [SLF4J](http://www.slf4j.org/) and [Saxon HE](http://saxon.sourceforge.net/).

#ph-sch2xslt-maven-plugin

Maven plugin to convert Schematron (SCH) to XSLT at compile time using [ph-schematron](https://github.com/phax/ph-schematron) as the converter.

The conversion of Schematron to XSLT is quite costly. That’s why this Maven plugin that does the conversion at build time. 

By default the plugin is run in the Maven lifecycle phase *generate-resources*. The basic configuration of the plugin in the `pom.xml` looks like this (inside the `<build>/<plugins>` element):
```xml
<plugin>
  <groupId>com.helger.maven</groupId>
  <artifactId>ph-sch2xslt-maven-plugin</artifactId>
  <version>4.2.2</version>
  <executions>
    <execution>
      <goals>
        <goal>convert</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```
The possible configuration parameters are:
  * `schematronDirectory` - The directory where the Schematron files reside. Defaults to `${basedir}/src/main/schematron`.
  * `schematronPattern` - A pattern for the Schematron files. Can contain Ant-style wildcards and double wildcards. All files that match the pattern will be converted. Files in the `schematronDirectory` and its subdirectories will be considered. Default is `**/*.sch`.
  * `xsltDirectory` - The directory where the XSLT files will be saved. Default is `${basedir}/src/main/xslt`.
  * `xsltExtension` - The file extension of the created XSLT files. Default is `.xslt`.
  * `overwriteWithoutQuestion` - Overwrite existing Schematron files without notice? If this is set to `false` than existing XSLT files are not overwritten. Default is `true`.
  * `phaseName` - Define the phase to be used for XSLT creation. By default the `defaultPhase` attribute of the Schematron file is used.
  * `languageCode` - Define the language code for the XSLT creation. Default is `null` which means English. Supported language codes are: cs, de, en, fr, nl.

#ph-schematron-maven-plugin

Maven plugin to validate XML files against convert Schematron (SCH) at compile time using [ph-schematron](https://github.com/phax/ph-schematron) as the validator.

This plugin was introduced in version 4.2.0.

By default the plugin is run in the Maven lifecycle phase *process-resources*. The basic configuration of the plugin in the `pom.xml` looks like this (inside the `<build>/<plugins>` element):
```xml
<plugin>
  <groupId>com.helger.maven</groupId>
  <artifactId>ph-schematron-maven-plugin</artifactId>
  <version>4.2.2</version>
  <executions>
    <execution>
      <goals>
        <goal>validate</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```
The possible configuration parameters are:
  * `schematronFile` - The Schematron file to be applied. This parameter is mandatory.
  * `schematronProcessingEngine` - The Schematron processing engine to be used. Can be one of `pure` (pure implementation), `schematron` (Schematron to XSLT engine) and `xslt` (pre-compiled XSLT files available). Default is `pure`.
  * `xmlDirectory` - The directory where the XML files to be validated are contained. This parameter is mandatory.
  * `xmlIncludes` - A pattern for the XML files to be included. Can contain Ant-style wildcards and double wildcards. All files that match the pattern will be converted. Files in the `xmlDirectory` and its subdirectories will be considered. Default is `**/*.xml`.
  * `xmlExcludes` - A pattern for the XML files to be excluded. Can contain Ant-style wildcards and double wildcards. All files that match the pattern will **NOT** be converted. Files in the `xmlDirectory` and its subdirectories will be considered.
  * `phaseName` - Define the phase to be used for XSLT creation. By default the `defaultPhase` attribute of the Schematron file is used.
  * `languageCode` - Define the language code for the XSLT creation. Default is `null` which means English. Supported language codes are: cs, de, en, fr, nl.
  * `svrlDirectory` - The directory where the SVRL files will be saved. If this property is not specified, no SVRL files will be written. By default the name of the SVRL file corresponds to the XML file that is validated with the suffix `.svrl`.

#ph-schematron-validator

A validator for Schematron definitions based on RelaxNG definition.

#Maven usage
Add the following to your pom.xml to use this artifact:
```
<dependency>
  <groupId>com.helger</groupId>
  <artifactId>ph-schematron-validator</artifactId>
  <version>4.2.2</version>
</dependency>
```

---

My personal [Coding Styleguide](https://github.com/phax/meta/blob/master/CodeingStyleguide.md) |
On Twitter: <a href="https://twitter.com/philiphelger">@philiphelger</a>
