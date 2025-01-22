## SWRLAPI Parent Project

[![Build Status](https://travis-ci.org/protegeproject/swrlapi-parent.svg?branch=master)](https://travis-ci.org/protegeproject/swrlapi-parent)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/edu.stanford.swrl/swrlapi-parent/badge.svg)](https://maven-badges.herokuapp.com/maven-central/edu.stanford.swrl/swrlapi-parent)

This project contains a Maven POM for [SWRLAPI](https://github.com/protegeproject/swrlapi/wiki)-related components. 
It contains version and configuration information that is used by all SWRLAPI projects.
An associated [SWRLAPI Build Project](https://github.com/protegeproject/swrlapi-project.git) uses this POM
when building all SWRLAPI components.

#### Building

To run the build process in this project you must have the following items installed:

+ [Java 11](https://www.oracle.com/java/technologies/downloads/archive/) or later
+ A tool for checking out a [Git](https://git-scm.com/) repository
+ Apache's [Maven](https://maven.apache.org/index.html)
+ A Protégé (5.6.4 or higher) distribution. Download [here](https://protege.stanford.edu/products.php#desktop-protege).

Create a suitable local directory and then clone the project as follows:

    git clone https://github.com/protegeproject/swrlapi-parent.git

Change into the SWRLAPI parent directory:

    cd swrlapi-parent

And then build the parent project with Maven:

    mvn clean install

#### License

The software is licensed under the [BSD 2-clause License](https://github.com/protegeproject/swrltab-project/blob/master/license.txt).

#### Questions

If you have questions about this project, please go to the main
Protégé website and subscribe to the [Protégé Developer Support
mailing list](http://protege.stanford.edu/support.php#mailingListSupport).
After subscribing, send messages to protege-dev at lists.stanford.edu.
