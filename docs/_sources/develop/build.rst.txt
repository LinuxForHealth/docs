Building Your Code
******************

Overview
========
Once you have created a new LinuxForHealth component or route (see `Developing for LinuxForHealth <./extend.html>`_), your next step is to run a build.  To build LinuxForHealth (LFH) with your code changes, you will first need to add any new dependencies to LFH.

Dependencies
============
To add new dependencies to LinuxForHealth, first look at any new classes that you use. Determine the Java packages those classes are in and make sure those packages are in the connect/build.gradle file.  If the dependencies do not exist yet in build.gradle, follow the example below to learn how to find the dependency information required by LFH.

Example:

A new feature requires the class org.apache.commons.codec.binary.Base64 for base64 encoding.

1. Use your browser to find the javadoc for this class (Search: "org.apache.commons.codec.binary.Base64").  From that search, we find the `javadoc <https://www.javadoc.io/doc/commons-codec/commons-codec/1.13/org/apache/commons/codec/binary/Base64.html>`_.

2. On that page, we see that the package path for the class is org.apache.commons.codec.binary.

3. Use your browser to search the maven repo for that package (Search: "maven org.apache.commons.codec.binary"). From that search, we find the package information in `Maven Central <https://mvnrepository.com/artifact/commons-codec/commons-codec>`_.

4. On that page, click on the latest version, then click on the Gradle tab and you will see::

    // https://mvnrepository.com/artifact/commons-codec/commons-codec
    compile group: 'commons-codec', name: 'commons-codec', version: '1.14'

5. Add a new line to the LFH build.gradle for this dependency. Since this was the first general utility we added, we added a line to the dependencies section.::

    // Java utils (base64 encoding, OS type)
    implementation group: 'commons-codec', name: 'commons-codec', version: project.commonsVersion

6. Add a property in gradle.properties for the package version, which we called "commonsVersion" in this case.::

    # java util versions
    commonsVersion=1.14

Build
=====
Once all your dependencies are added, run::

    ./gradlew build

in the top-level connect directory of the LinuxForHealth repo.
