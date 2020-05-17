# CI/CD 
## 1. What is CI/CD? 
CI/CD is famous in software engineering. One may know that CI stand for Common Interface and CD is the good old Compact Disk. But this is not our case. 
> **CI/CD is the acronym for Continuous Integration/Continuous Delivery or, in some document, Continuous Integration/Continuous Development.** 

So, CI/CD is like two brothers that usually go together. 

### 1.1 The first one - CI

>**_CI is the practice of merging all developers' working copy of code into a shared mainline several times a day_**. 

To ensure code quality and minimize chance of failure, **automation testing** is employed during the process, and there may also be **code review** to improve code quality. C* requires a **culture** of the code team, especially a **coding standard** to reduce conflicting coding style. 

**Automated tests** play an essential role in CI, and they should be carefully designed, frequently update and reviewed. Whenever there's a failure occuring in production, we should add a test case for it to our test suite, and bug fixing should be prioritized over new builds. These tests include uniting tests, integration testing and system testing. _Unit testing_ is the most important and should be focused - it that make sure one's changes to a current working function do not affect others. **Unit tests should be frequently updated whenever new changes are made.**. _Integration test_ and _system testing_ is important as well. However, we should keep in mind the difference between testing environment and production environment - these two environments should be as close to each other as possible. Also, **_since we merge code serveral times a day, testing should not take too long_.**

