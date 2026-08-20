# Jenkins Extension lab / presentation
1. Add a code coverage stage
Report how much of the code your tests actually exercise.

Hints: the JaCoCo Maven plugin is the standard tool for this — it needs adding to the <plugins> section of pom.xml and bound to run around the test phase. Once it's producing a report under target/site/jacoco, add a new pipeline stage that runs it and archives the output (archiveArtifacts works fine if you just want the HTML report saved; the HTML Publisher plugin gives you a nicer in-Jenkins view if it's installed on your instance).

2. Add a static analysis stage
Catch style or quality issues automatically instead of relying on code review.

Hints: maven-checkstyle-plugin or spotbugs-maven-plugin are both straightforward to bolt onto this project. Add the plugin to pom.xml, then add a stage that runs its check goal. Think about whether you want the build to fail on violations or just report them — that changes what Maven goal/config you reach for.
