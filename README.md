To demonstrate: https://github.com/sonatype/ossindex-maven/issues/66
The cache expiry is being set to just 10 seconds to suppress querying the local directory cache.  This is in order to illustrate the issue.

See module1/pom.xml for the oss plugin config.

$ cd module1

$ mvn validate -Dossindex.cache.expiration=PT10S -X | tee log.txt

Relevant bits in debug output:

[DEBUG]   (f) excludeCoordinates = [org.apache.logging.log4j:log4j-core:2.15.0, org.apache.logging.log4j:log4j-api:2.15.0, com.example:module2:0.0.1-SNAPSHOT]

[INFO] Checking for vulnerabilities; 4 artifacts
[DEBUG]   com.example:module2:jar:0.0.1-SNAPSHOT:compile
[DEBUG]   org.yaml:snakeyaml:jar:1.29:compile
[DEBUG]   org.apache.logging.log4j:log4j-api:jar:2.15.0:compile
[DEBUG]   org.apache.logging.log4j:log4j-core:jar:2.15.0:compile
[INFO] Exclude coordinates: [org.apache.logging.log4j:log4j-core:2.15.0, org.apache.logging.log4j:log4j-api:2.15.0, com.example:module2:0.0.1-SNAPSHOT]

**Excluded items are requested in the POST request.**

[DEBUG] Report cache: DirectoryCache{baseDir=/Users/uuu/Library/Application Support/Sonatype/Ossindex/report-cache, expireAfter=PT10S}
[DEBUG] Requesting 4 component-reports
[DEBUG] Requesting 4 un-cached component-reports
[DEBUG] POST https://ossindex.sonatype.org/api/v3/component-report; payload: {"coordinates":["pkg:maven/com.example/module2@0.0.1-SNAPSHOT","pkg:maven/org.yaml/snakeyaml@1.29","pkg:maven/org.apache.logging.log4j/log4j-api@2.15.0","pkg:maven/org.apache.logging.log4j/log4j-core@2.15.0"]} (application/vnd.ossindex.component-report-request.v1+json); accept: application/vnd.ossindex.component-report.v1+json
