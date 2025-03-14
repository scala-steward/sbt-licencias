# sbt-license-check

[![build](https://github.com/Philippus/sbt-license-check/workflows/build/badge.svg)](https://github.com/Philippus/sbt-license-check/actions/workflows/scala.yml?query=workflow%3Abuild+branch%3Amain)
![Current Version](https://img.shields.io/badge/version-0.0.5-brightgreen.svg?style=flat "0.0.5")
[![Scala Steward badge](https://img.shields.io/badge/Scala_Steward-helping-blue.svg?style=flat&logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAQCAMAAAARSr4IAAAAVFBMVEUAAACHjojlOy5NWlrKzcYRKjGFjIbp293YycuLa3pYY2LSqql4f3pCUFTgSjNodYRmcXUsPD/NTTbjRS+2jomhgnzNc223cGvZS0HaSD0XLjbaSjElhIr+AAAAAXRSTlMAQObYZgAAAHlJREFUCNdNyosOwyAIhWHAQS1Vt7a77/3fcxxdmv0xwmckutAR1nkm4ggbyEcg/wWmlGLDAA3oL50xi6fk5ffZ3E2E3QfZDCcCN2YtbEWZt+Drc6u6rlqv7Uk0LdKqqr5rk2UCRXOk0vmQKGfc94nOJyQjouF9H/wCc9gECEYfONoAAAAASUVORK5CYII=)](https://scala-steward.org)
[![License](https://img.shields.io/badge/License-MPL%202.0-blue.svg?style=flat "MPL 2.0")](LICENSE)

This plugin can check and report the licenses used in your sbt project. You can use it also as part of your build chain
and make the build fail if disallowed licenses are found.

## Installation

sbt-license-check is published for sbt 1.5.8 and above. To start using it add the following to your `plugins.sbt`:

```
addSbtPlugin("nl.gn0s1s" % "sbt-license-check" % "0.0.5")
```

## Usage
### Tasks
| Task         | Description                                                                                                                                                                                                                                                                                                 | Command                  |
|:-------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------|
| licenseCheck | Runs license check. Logs a tree of dependencies along with the licenses found, grouped by organization. If a dependency has no license, or it cannot be found it returns `no license specified`. Setting `useCoursier` to `false` before running the command yields in some cases different/better results. | ```$ sbt licenseCheck``` |

To find out the license(s) of the current project itself, the sbt command `licenses` can be used.

### Configuration
You can configure the configuration in your `build.sbt` file.

| Setting                                  | Description                                                                                                                                                                   | Default Value |
|:-----------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------|
| licenseCheckFailBuildOnDisallowedLicense | Sets whether disallowed licenses fail the build, if `false` disallowed licenses show up as warnings in the log, if `true` they show up as errors.                             | false         |
| licenseCheckDisallowedLicenses           | Sets the disallowed licenses, e.g. `Seq("Apache 2.0")`.                                                                                                                       | Nil           |
| licenseCheckExemptedDependencies         | Sequence of dependency names and revisions whose licenses will be allowed regardless of the `licenseCheckDisallowedLicenses` setting, e.g. `Seq(("scala-xml_2.13", "2.0.1"))` | Nil           |

## Example usage

Below the output for the `scala-isbn`-project is shown:
```
sbt:scala-isbn> set useCoursier := false
[info] Defining useCoursier
[info] The new value will be used by csrCacheDirectory, dependencyResolution and 9 others.
[info]  Run `last` for details.
[info] Reapplying settings...
[info] set current project to scala-isbn (in build ***)
sbt:scala-isbn> licenseCheck
[info] org.scala-lang
[info]   +-scala-library:2.13.8
[info]   | +-Apache-2.0 - https://www.apache.org/licenses/LICENSE-2.0
[info]   +-scala-compiler:2.13.8
[info]   | +-Apache-2.0 - https://www.apache.org/licenses/LICENSE-2.0
[info]   +-scala-reflect:2.13.8
[info]   | +-Apache-2.0 - https://www.apache.org/licenses/LICENSE-2.0
[info] org.scalameta
[info]   +-munit-scalacheck_2.13:0.7.29
[info]   | +-Apache-2.0 - http://www.apache.org/licenses/LICENSE-2.0
[info]   +-munit_2.13:0.7.29
[info]   | +-Apache-2.0 - http://www.apache.org/licenses/LICENSE-2.0
[info]   +-junit-interface:0.7.29
[info]   | +-Apache-2.0 - http://www.apache.org/licenses/LICENSE-2.0
[info] junit
[info]   +-junit:4.13.1
[info]   | +-Eclipse Public License 1.0 - http://www.eclipse.org/legal/epl-v10.html
[info] org.jline
[info]   +-jline:3.21.0
[info]   | +-The BSD License - https://opensource.org/licenses/BSD-3-Clause
[info] org.scala-sbt
[info]   +-test-interface:1.0
[info]   | +-BSD - https://github.com/sbt/test-interface/blob/master/LICENSE
[info] net.java.dev.jna
[info]   +-jna:5.9.0
[info]   | +-LGPL-2.1-or-later - https://www.gnu.org/licenses/old-licenses/lgpl-2.1
[info]   | +-Apache-2.0 - https://www.apache.org/licenses/LICENSE-2.0.txt
[info] org.scalacheck
[info]   +-scalacheck_2.13:1.15.4
[info]   | +-BSD 3-clause - https://opensource.org/licenses/BSD-3-Clause
[info] org.scala-lang.modules
[info]   +-scala-xml_2.13:2.0.1
[info]   | +-Apache-2.0 - https://www.apache.org/licenses/LICENSE-2.0
[info] org.hamcrest
[info]   +-hamcrest-core:1.3
[info]   | +-New BSD License - http://www.opensource.org/licenses/bsd-license.php
```

## License
The code is available under the [Mozilla Public License, version 2.0](LICENSE).
