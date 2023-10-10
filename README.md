# Swift Testing

[![license](https://img.shields.io/github/license/interlok-testing/testing_swift.svg)](https://github.com/interlok-testing/testing_swift/blob/develop/LICENSE)
[![Actions Status](https://github.com/interlok-testing/testing_swift/actions/workflows/gradle-build.yml/badge.svg)](https://github.com/interlok-testing/testing_swift/actions/workflows/gradle-build.yml)

Project tests interlok-swift features

## What it does

This project is very simple and contains only one single channel with one workflow.

The workflow has a polling trigger that produces an XML message with a serviceId metadata every 10 seconds.
Then a service convert the XML message to a Swift message, another one transform the Swift message back to XML, then a Xpath service extract the service id from the XML and add it to a metadata.
Finally a If service check if the new service id equals the original one and throws an exception if it doesn't.

```mermaid
graph LR
  subgraph Swift
    PT_C[Polling Trigger] --> XS(XML To Swift)
    XS --> SX(Swift To XML)
    SX --> XP(Extract Service ID)
    XP --> IF(Check New Service ID)
  end

  style PT_C fill:#FF6C6C
  style XS fill: #F89347
  style SX fill: #F89347
  style XP fill: #F89347
  style IF fill: #F89347
```

The polling trigger has a message provider that uses an Embedded Scripting Service to create the XML message and add the serviceId metadata.
The serviceId is a random number bewteen 1 and 10 and is not added as a metadata when its value is 9 to simulate some sporadic failures.

## Getting started

* `./gradlew clean build`
* `(cd ./build/distribution && java -jar lib/interlok-boot.jar)`
