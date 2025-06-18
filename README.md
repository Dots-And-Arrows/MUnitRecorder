# munitRecorder
A different way to create and run munits in Mule4, without leaving the Mule configuration code.

## installation
Move the munit-recorder folder to src/test/munits
All components used are standard components, but you may have to drag in a compression and validation module to get them to be loaded into the project dependencies properly.
Or add this to pom.xml
```xml
<dependency>
    <groupId>org.mule.modules</groupId>
    <artifactId>mule-compression-module</artifactId>
    <version>2.2.3</version>
    <classifier>mule-plugin</classifier>
</dependency>

<dependency>
    <groupId>org.mule.modules</groupId>
    <artifactId>mule-validation-module</artifactId>
    <version>2.0.6</version>
    <classifier>mule-plugin</classifier>
</dependency>
```
## parts
### mr-core.xml
The main logic components that don't need to be adjusted unless there is a bug or you would like to add a custom feature.
### mr-all.xml
Contains subflows that allow for triggering of all spies, mocks and verifies through simple subflow references. Needs to be expanded for every supported connector component.
### connectors
In here are individual XML files that represent all the flows needed to mock a specific tag. 

To make a new one, copy the one from templates, replace TAG with your new tag, example db:select, and move the components in the move-me subflow to mr-all.xml

## design requirements
The main requirement of your project that the munit recorder has is that you set all parameters relevant to an outbound connector (host, path, etc) in the payload or in vars.requestParams right before the connector component is triggered. This variable will be used to save the outbound parameters as well as fetch the mock payload during replay. Same outbound params means same payload, unkown outbound params will result in a test failure.

## usage
- set up a munit test to be able to connect to your testing environment
- add a `record` component to the flow.
- add a variable setter that sets the `testName`, recommended is `flow.name` to make it dynamic
- run the test
- the test files appear in src/test/resources/`testName`/
- replace the `record` component with a `replay` component
- add a `verify` component at the end
- done 