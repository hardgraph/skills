
## Requiring `--add-opens=java.base/java.lang=ALL-UNNAMED` JVM option

As part of the refactoring of our HTML reports generation, we use some advanced technics that require the `--add-opens=java.base/java.lang=ALL-UNNAMED` JVM option.

This option is set by default in the following updates of our plugins:
* maven: 4.11.0
* gradle: 3.13.1
* sbt: 4.10.2
