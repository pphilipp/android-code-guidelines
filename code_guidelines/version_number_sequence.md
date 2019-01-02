Version number sequence
=========================

In sequence-based software versioning schemes for Android software release is assigned a unique identifier that consists 
sequences of numbers.

Change significance
-----------------
In some schemes, sequence-based identifiers are used to convey the significance of changes between releases: changes are 
classified by significance level, and the decision of which sequence to change between releases is based on the significance 
of the changes from the previous release, whereby the first sequence is changed for the most significant changes, and changes 
to sequences after the first represent changes of decreasing significance.

This practice permits users to evaluate how much real-world testing a given software release has undergone. 
If changes are made between, say, `1.3rc4` (a release candidate) and the production release of `1.3.0`, then that 
release, which asserts that it has had a production-grade level of testing in the real world, in fact contains changes which 
have not necessarily been tested in the real world at all. This approach commonly permits the third  level of numbering 
("change"), but does not apply this level of rigor to changes in that number: `1.3.1`, `1.3.2` ... `1.4.1`, etc.

In principle, in subsequent releases, the major number is increased when there are significant jumps in functionality such 
as changing the framework which could cause incompatibility with interfacing systems, the minor number is incremented when 
only minor features or significant fixes have been added, and the revision number is incremented when minor bugs are fixed. 
A typical product might use the numbers `0.9.0` (for beta software), `0.9.1`, `0.9.2`, `0.9.3`, `1.0.0`, `1.0.1`, `1.0.2`, 
`2.0.0`, `2.0.1`, `2.0.2`, `2.1.0`, `2.1.1`, etc. Developers may choose to jump multiple minor versions at a time to indicate 
significant features have been added, but are not enough to warrant incrementing a major version number.

Degree of compatibility
-----------------
Android projects use semantic versioning is a formal convention for specifying compatibility using a three-part version 
number: **major** version; **minor** version; and **patch**. 

 * The **patch** number is incremented for minor changes and bug fixes which do not change the software's application programming 
interface (API). 

 * The **minor** version is incremented for releases which add new, but backward-compatible, API features. 

 * The **major** version is incremented for API changes which are not backward-compatible. For example, software which relies on 
version `2.1.5` of an API is compatible with version `2.2.3`, but not necessarily with `3.2.4`.

Gradle configuration
-----------------
1. Add in the `gradle.build` of the `Module:app`-level on root the following lines:
```
def versionMajor = 0
def versionMinor = 1
def versionPatch = 0
def versionBuild = "000"
```
2. Add the version number counter for `versionName` and `versionCode`
```
android {
    ...

    defaultConfig {
        ...

        versionName "${versionMajor}.${versionMinor}.${versionPatch}"
        versionCode versionMajor * 10000 + versionMinor * 1000 + versionPatch * 100 + versionBuild.toInteger()
    }
  }
}
```
