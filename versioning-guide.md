# API Versioning Guide

## Rules
- First declare a public API. This may consist of documentation or be enforced by the code itself. Regardless, it is important that this API be clear and precise.
- Once the API is public, communicate changes with specific increments to the version number. 
- Consider a version format of X.Y.Z (Major.Minor.Patch).
  -  When bug fixes are backward compatible or bug fixes will not affect the pubic API, increment the __patch__ version.
  -  When you add functionality in a backward compatible manner,  increment the __minor__ version.
  -  When changes are not backward compatible, increment the __major__ version.
- Major version zero (0.y.z) is for initial development. Anything MAY change at any time. The public API SHOULD NOT be considered stable.

## References
[semantic-versioning](https://semver.org/)
[semver-github](https://github.com/semver/semver)