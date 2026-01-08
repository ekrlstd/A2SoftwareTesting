Testing Documentation: Checkstyle

Ebbe Karlstad (ek224ev), Aakrish Lama (al226na) Course: Software Testing - 1DV609

## 1. Project Overview

Checkstyle is a static code analysis tool which checks if a projects Java source code complies with certain code standards, which essentially are rules for how code should be written and formatted. It's highly customizable and has a lot of templates like Googles and Suns Java styles. It's used in many big projects due to the fact that it makes these proejcts easier to handle for developers, and gets everyone on the same page when it comes to code style and standards. Checkstyle itself even uses checkstyle.
[https://github.com/checkstyle/checkstyle](https://github.com/checkstyle/checkstyle)

## 2. Requirements & Specifications

Core functional requirements:

- Checks Java code against a defined set of coding standards/rules.
- Allows for users to define their own rules.
- If checkstyle fails and finds an error, it reports that to the user with file paths, line numbers, etc. in the CLI.
- Can generate reports (in XML for example) so users can integrate those with other tools.
- Provides a standalone UI (CLI/GUI interface), so users can run checks without a building tool like Maven or Gradle.

Non-functional requirements:

- Must have good performance for large codebases, be compatible with many Java versions, be usable and reliable. we've chosen not to go too in depth in this question as NF requirements are often similar across projects unless you have specific details, which I couldn't find.

Stakeholders:

- Java developers: This is the main stakeholder, and it is the people this project is built for.
- Bigger development teams: Checkstyle enforces a coding standard across developers in teams for a project, so everyone has to follow it.
- CI/CD systems: Automated environments like GitHub Actions can run Checkstyle as a "gate", to make sure that all code that gets pushed passes said gate.

Key risks:

- Flagging correct code as a violation is probably the biggest one. It's frustrating, and when a user knows what they're doing, it can lead to removing or rewriting code in a way that makes it work, just to pass the Checkstyle checks.
- When Java introduces new features in new updates, Checkstyle may lag behind as it doesn't know about these yet. Since Checkstyle is a highly active project, this usually gets fixed fast however.

## 3. Project Development History

Checkstyle began as an open-source Java static analysis project in 2001, with the earliest public release (version 1.1) appearing in June 2001 by the creator Oliver Burn. Over time it has evolved through many versions to support newer Java language features and API changes; recent development includes a 13.0.0 release in early January 2026. Recent activity includes ongoing commits and published releases within the past few months, which tells us there's maintenance and updates being done on the project actively.

- Initial Release: 2001
- Current Version: 13.0.0
- Maintenance Status: Active
- Community: 500+ contributors, ~8.8K stars on GitHub

## 4. Current Testing Strategy

Checkstyle uses various different methods when it comes to testing. The one we saw used the most is granular unit testing. Every single check is granular unit testing. Now that we know what checkstyle is, we can get in to how this works. There exists many different "checks" that checkstyle ideally should catch. These are just badly written Java files which gets fed into checkstyle. If checkstyle catches a check, the test for that check passes, if not, it fails. We can see why the developers of checkstyle chose to go for a TDD workflow, as they can have tests for each check and essentially always test that every part of checkstyle works with these tests. The nature of checkstyle fits TDD very well.

Checkstyle also runs checkstyle on its own source code during the building process. This obviously checks so that all code is properly formatted and styled.

They also use regression testing with a huge library of files in src/tests/resources which contains input. When they fix a bug, they add a new input file in this directory to ensure that that bug is reproducible. Then the code is fixed until that test passes.

Checkstyle uses JUnit 5, which is the primary test writing framework. They use Maven for managing building and running tests. They also use SpotBugs to catch bugs Checkstyle and unit tests might miss, like potential null pointers etc.

Checkstyle also has a good CI/CD system with GitHub Actions. These are automated pipelines that run all tests on every pull request to ensure that no broken code is merged.

## 5. Existing Test Analysis

We've executed the projects full JUnit testing suite using Maven (command ./mvnw clean test). Here is the result:

Tests run: 5672, Failures: 0, Errors: 0, Skipped: 16
Time taken: 2m 48s.

Note that the skipped ones are because I ran testing commands in a terminal, so some tests are skipped because the GUI needs a head.

The tests themselves are obviously at an excellent level, they all pass and are generally good tests from what we've discovered. As far as tests we've ran, we've performed exploratory testing on Checkstyle, and noticed that a few tests fail when we run it on Java version 25 (bleeding edge), and we had to roll back to Java 21 in order for these test to pass. This wasn't a Checkstyle issue nor an issue with the tests themselves, but a dependency called bytebuddy which doesn't yet have support for Java 25, so if all tests are to pass, the Java version needs to be rolled back to 21. Note that Checkstyle will still work on Java 25 code, just not the tests specifically depending on bytebuddy (about 2% of all tests). Furthermore, we've conducted exploratory tests on the rest of the project, as mentioned earlier, and on its GUI. When testing the GUI, it crashes when in a headless environment, like a terminal or SSH. This though is something that's supposed to happen, and not something we can pin on the developers.

## 6. Improvements

The coverage and test quality is excellent. In the pom.xml file, it's configuration enforces a 100% coverage on many of the modules (coverageThreshold > 100) which ensures almost every line is being gone through.

The process is also rigorous and good. The building pipeline uses self verification, which we mentioned earlier, where Checkstyle runs on itself, along with other analysis tools like SpotBugs.

Something we noticed which could be improved is the integration of testing against multiple JDK versions (LTS vs bleeding edge) to catch those incompatibility issues we saw with Java version 25. The GUI crash in a headless environment is also something we figure can be improved, where the user gets a better error message instead of just a crash.

## Appendencies
Checsktyle GUI:
![alt text](https://github.com/ekrlstd/A2SoftwareTesting/blob/main/2026-01-08-223912_hyprshot.png?raw=true)

