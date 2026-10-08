## What is Checkmarx?
- Checkmarx is an **Application Security Testing (AppSec)** platform used
- Checkmarx One is used for several types of security scanning:

	- **SAST (Static Application Security Testing)**: analyzes source code for security flaws without running the application.
	- **SCA (Software Composition Analysis)**: scans open source dependencies and libraries for known vulnerabilities and license risks.
	- **IaC Security**: scans Infrastructure as Code files such as Terraform and other configuration definitions for security misconfigurations
	- **Container Security**: analyzes Dockerfiles and container images for security risks. 
	- **API Security**: discovers APIs and identifies API-related vulnerabilities and risks.
- Development and Production are tracked differently in CheckMarx and remediation timelines differ as well
-  In summary all GM financial code must be scanned for vulnerabilities regardless if code is in production or development

# Why Should I Care?

Think of Checkmarx as:

- SonarQube → Code Quality

- Checkmarx → Security

A pull request can pass functional tests and code review but still be blocked by security findings.

---

# Vulnerability Remediation Periods (GM Financial)

## High

### Development Projects

- Must be remediated before the application is promoted to production.

### Production Projects

- 30 days to remediate.

### Notes

- Applications cannot be released to production with unresolved High vulnerabilities.

- Risk Exceptions may be requested but remediation is preferred. 【1-6cc8f4】

---

## Medium

### Production Projects

- 60 days to remediate. 【1-6cc8f4】

---

## Low

### Production Projects

- 90 days to remediate. 【1-6cc8f4】

---

# Scan Types

## SAST (Static Application Security Testing)

Analyzes source code without executing it.

Typical findings:

- SQL Injection

- Cross-Site Scripting (XSS)

- Hardcoded credentials

- Unsafe input handling

- Authentication flaws

Most relevant for:

- .NET APIs

- React applications

- React Native applications


---

## SCA (Software Composition Analysis)

Scans open-source dependencies.

Typical findings:

- Vulnerable npm packages

- Vulnerable NuGet packages

- License compliance issues

Most relevant for:

- React

- Next.js

- React Native

- .NET


---

## IaC Security

Scans Infrastructure as Code.

Typical findings:

- Public cloud resources

- Insecure Terraform configurations

- Weak security policies

Most relevant for:

- Terraform

- Cloud infrastructure


---

## Container Security

Scans:

- Dockerfiles

- Container images

Typical findings:

- Vulnerable base images

- Outdated packages

- Misconfigurations

【1-817f44】

---

## API Security

Focuses on API inventory and API vulnerabilities.

Typical findings:

- Exposed APIs

- Vulnerable endpoints

- API security gaps

【1-817f44】

---

# Typical Developer Workflow

```text

Write Code

    ↓

Open Pull Request

    ↓

Pipeline Executes

    ↓

Checkmarx Scan Runs

    ↓

Review Findings

    ↓

Fix Vulnerabilities

    ↓

Re-run Pipeline

    ↓

Merge

```

---

# Understanding Findings

For each vulnerability Checkmarx provides:

- File name

- Line number

- Severity

- Vulnerability category

- Remediation guidance

The tool can navigate directly to the affected code location.

---

# False Positives

Sometimes findings are incorrect.

Recommended workflow:

1. Add comments explaining why it is a false positive.

2. Change status to "Proposed Not Exploitable".

3. Submit for AppSec review.

4. Wait for approval.

Once confirmed, the finding is marked as "Not Exploitable".

---

# Full Scan vs Incremental Scan

## Full Scan

Pros:

- Most comprehensive

Cons:

- Slower

Typical usage:

- New projects

- Periodic validation

---

## Incremental Scan

Pros:

- Faster

Cons:

- Scans only changes

Typical usage:

- Pull Requests

- CI/CD pipelines


---

# Graph View (High Value Feature)

Graph View helps identify the root cause of multiple findings.

Instead of fixing hundreds of detections individually, fixing a parent node may eliminate many related findings.

Always check Graph View before starting large remediation efforts. 【3-82d7f8】

---

# Vulnerabilities Every Developer Should Know

## Critical Knowledge Areas

### SQL Injection

Never concatenate user input into SQL queries.

Preferred:

- Parameterized queries

- ORM protections

---

### Cross-Site Scripting (XSS)

Never trust browser input.

Preferred:

- Output encoding

- Input validation

---

### Hardcoded Secrets

Never commit:

- Passwords

- API Keys

- Tokens

- Connection Strings with credentials

Preferred:

- Azure Key Vault

- Secret Managers

- Environment Variables

---

### Dependency Vulnerabilities

Keep dependencies updated.

Monitor:

- npm packages

- NuGet packages

---

### Authentication Issues

Common problems:

- Weak authentication logic

- Missing token validation

- Improper session management

---

### Authorization Issues

Always verify:

- User permissions

- Resource ownership

- Role validation

---

# Priority Order for My Stack

As a Software Development Engineer working with .NET, React, React Native, Terraform, and Azure DevOps:

## Highest Priority

- SAST findings in .NET APIs

- SCA findings in npm dependencies

- SCA findings in NuGet dependencies

- IaC findings in Terraform

## Medium Priority

- Container Security

- API Security

## Lowest Priority

- Manual reporting

- Team administration

- User onboarding

---

# Quick Mental Model

Before merging code, ask:

- Did Checkmarx run successfully?

- Do I have High vulnerabilities?

- Are dependencies secure?

- Am I exposing secrets?

- Is user input validated?

- Is authentication/authorization enforced?

If all answers are "yes", security risk is usually significantly lower.

---

# Key Takeaway

* As a developer, the most important skill is not learning Checkmarx itself, but understanding the vulnerabilities that Checkmarx reports.