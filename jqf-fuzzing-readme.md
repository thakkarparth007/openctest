# jqf-fuzzing

This document describes how to use the JQF fuzzer with CTests.
It's a WIP document, as I play around with JQF, I'll keep updating this doc.

- Currently I'm only playing with the hadoop-common project for JQF fuzzing
- The pom.xml in hadoop-common and hadoop-common-project have been updated to add dependences to JQF
- If your `~.m2/settings.xml` is empty, then create it with the following contents, else ensure pluginGroups contains edu.berkeley.cs.jqf.

    ```
    <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 https://maven.apache.org/xsd/settings-1.0.0.xsd">

        <pluginGroups>
                <pluginGroup>edu.berkeley.cs.jqf</pluginGroup>
        </pluginGroups>
    </settings>
    ```

- The above ensures you can easily invoke the JQF maven plugin
- `cd` to the hadoop-common folder (use `core/app` and not `core/identify_param` for this)
- Here run `mvn jqf:fuzz -Dclass=org.apache.hadoop.ha.TestHAAdmin -Dmethod=testHelpFuzz` to run fuzzing on the `testHelpFuzz` method. Currently only this method is annotated with @Fuzz
- You can run `mvn jqf:repro -Dclass=org.apache.hadoop.ha.TestHAAdmin -Dmethod=testHelpFuzz -Dinput=target/fuzz-results/org.apache.hadoop.ha.TestHAAdmin/testHelpFuzz/failures/id_000000` to get the test result of the first input that caused a failure of the test.