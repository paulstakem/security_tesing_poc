# Sensitive Data Exposure and/or Leakage

Identification of the folllowing types of sensitive data:

-  Passwords
-  API Keys
-  Credentials
-  Cryptographic Material
-  etc...

## Implementation

## Local Devlopment Environment

TBC...

## Pre-commit

Aim: **Prevents sensitive data from being introduced at the earliest stage of the git workflow**<br/>
Drawback: **Can be disabled easily by engineer**

### Example: AWS Credentials Exposed in Terraform

```bash
security_tesing_poc on  terraform [+] at ☸️  dev
➜ git commit -m "adding insecure terraform code"
[INFO] Installing environment for https://github.com/pre-commit/pre-commit-hooks.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
Detect hardcoded secrets.................................................Failed
- hook id: gitleaks
- exit code: 1

○
    │╲
    │ ○
    ○ ░
    ░    gitleaks

{
	"Description": "AWS",
	"StartLine": 44,
	"EndLine": 44,
	"StartColumn": 22,
	"EndColumn": 41,
	"Match": "REDACTED",
	"Secret": "REDACT",
	"File": "terraform/lambda.tf",
	"Commit": "",
	"Entropy": 0,
	"Author": "",
	"Email": "",
	"Date": "0001-01-01T00:00:00Z",
	"Message": "",
	"Tags": [],
	"RuleID": "aws-access-token"
}
{
	"Description": "AWS",
	"StartLine": 10,
	"EndLine": 10,
	"StartColumn": 18,
	"EndColumn": 37,
	"Match": "REDACTED",
	"Secret": "REDACT",
	"File": "terraform/providers.tf",
	"Commit": "",
	"Entropy": 0,
	"Author": "",
	"Email": "",
	"Date": "0001-01-01T00:00:00Z",
	"Message": "",
	"Tags": [],
	"RuleID": "aws-access-token"
}
{
	"Description": "AWS",
	"StartLine": 15,
	"EndLine": 15,
	"StartColumn": 27,
	"EndColumn": 46,
	"Match": "REDACTED",
	"Secret": "REDACT",
	"File": "terraform/ec2.tf",
	"Commit": "",
	"Entropy": 0,
	"Author": "",
	"Email": "",
	"Date": "0001-01-01T00:00:00Z",
	"Message": "",
	"Tags": [],
	"RuleID": "aws-access-token"
}
10:24AM WRN leaks found: 3
10:24AM INF scan duration: 121.403089ms
```
