# Docker

Building an intentionally insecure dockerfile

## Security Defects

Dockerfile contains the following Defects
1\. Poorly sourced `FROM` container for Build<br/>
2\. Container runs as `root`<br/>
3\. Container exposes `port 22` aka SSH</br>
4\. Container has hardcoded `ENVIRONMENT_VARIABLE` which contains sensitive data

## Detection of the Defects with SAST tools

### Trivy: to scan the `Dockerfile`
```bash
➜ make scan/dockerfile
trivy config ./
2022-03-01T11:40:34.573Z	INFO	Detected config files: 1

Dockerfile (dockerfile)
=======================
Tests: 23 (SUCCESSES: 20, FAILURES: 3, EXCEPTIONS: 0)
Failures: 3 (UNKNOWN: 0, LOW: 0, MEDIUM: 2, HIGH: 1, CRITICAL: 0)

+---------------------------+------------+--------------------+----------+------------------------------------------+
|           TYPE            | MISCONF ID |       CHECK        | SEVERITY |                 MESSAGE                  |
+---------------------------+------------+--------------------+----------+------------------------------------------+
| Dockerfile Security Check |   DS001    | ':latest' tag used |  MEDIUM  | Specify a tag in the 'FROM' statement    |
|                           |            |                    |          | for image 'gvenzl/oracle-xe'             |
|                           |            |                    |          | -->avd.aquasec.com/appshield/ds001       |
+                           +------------+--------------------+----------+------------------------------------------+
|                           |   DS002    | root user          |   HIGH   | Specify at least 1 USER                  |
|                           |            |                    |          | command in Dockerfile with               |
|                           |            |                    |          | non-root user as argument                |
|                           |            |                    |          | -->avd.aquasec.com/appshield/ds002       |
+                           +------------+--------------------+----------+------------------------------------------+
|                           |   DS004    | Port 22 exposed    |  MEDIUM  | Port 22 should not be                    |
|                           |            |                    |          | exposed in Dockerfile                    |
|                           |            |                    |          | -->avd.aquasec.com/appshield/ds004       |
+---------------------------+------------+--------------------+----------+------------------------------------------+
```

### Trivy: to scan the `Container Image`
```bash
➜ make scan/container
trivy image --severity HIGH,CRITICAL --ignore-unfixed --ignore-policy example-policy.rego quay.io/acme/dummy-container:1.1.unknown
2022-03-01T11:50:21.807Z	INFO	Detected OS: oracle
2022-03-01T11:50:21.812Z	INFO	Detecting Oracle Linux vulnerabilities...
2022-03-01T11:50:21.851Z	INFO	Number of language-specific files: 0

quay.io/acme/dummy-container:1.1.unknown (oracle 8.5)
=====================================================
Total: 1 (HIGH: 0, CRITICAL: 1)

+----------------+------------------+----------+-------------------+----------------+---------------------------------------+
|    LIBRARY     | VULNERABILITY ID | SEVERITY | INSTALLED VERSION | FIXED VERSION  |                 TITLE                 |
+----------------+------------------+----------+-------------------+----------------+---------------------------------------+
| cyrus-sasl-lib | CVE-2022-24407   | CRITICAL | 2.1.27-5.el8      | 2.1.27-6.el8_5 | cyrus-sasl: failure to properly       |
|                |                  |          |                   |                | escape SQL input allows               |
|                |                  |          |                   |                | an attacker to execute...             |
|                |                  |          |                   |                | -->avd.aquasec.com/nvd/cve-2022-24407 |
+----------------+------------------+----------+-------------------+----------------+---------------------------------------+
```

### Gitleaks: detect secrets

```bash
➜ gitleaks protect -v

    ○
    │╲
    │ ○
    ○ ░
    ░    gitleaks

{
	"Description": "AWS",
	"StartLine": 5,
	"EndLine": 5,
	"StartColumn": 13,
	"EndColumn": 32,
	"Match": "AKIALALEMEL33243OLIA",
	"Secret": "AKIALALEMEL33243OLIA",
	"File": "docker/Dockerfile",
	"Commit": "",
	"Entropy": 0,
	"Author": "",
	"Email": "",
	"Date": "0001-01-01T00:00:00Z",
	"Message": "",
	"Tags": [],
	"RuleID": "aws-access-token"
}
12:30PM WRN leaks found: 1
12:30PM INF scan duration: 57.810054ms
```
