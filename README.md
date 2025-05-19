# munitRecorder
A different way to create and run munits in Mule4, without leaving the Mule configuration code.

## parts
### mr-core.xml
The main logic components that don't need to be adjusted unless there is a bug or you would like to add a custom feature.
### mr-all.xml
Contains subflows that allow for triggering of all spies, mocks and verifies through simple subflow references. Needs to be expanded for every supported connector component.


