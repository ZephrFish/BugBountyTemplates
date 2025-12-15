# Title
## Issue Description
A generic overview of the issue, I usually use the default text from OWASP as it explains the issue well. Include a more specific description of the issue identified within the application.

## Affected URL/Area
- The affected urls or area of the application where the issue exists.

## Risk Rating
- Risk: **Critical / High / Medium / Low / Informational**
- Difficulty to Exploit: **Low / Medium / High**
- Authentication Required: **Yes / No**
- User Interaction Required: **Yes / No**
- CVSS 3.1 Score: [X.X](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:N)

### Impact
- What kind of attacker could exploit this? (external, authenticated user, admin)
- What access/privileges do they need?
- What can they achieve? (data theft, privilege escalation, service disruption)
- Who else does it affect? (other users, administrators, the organization)

### Attack Scenario
Describe a realistic attack scenario showing how this vulnerability could be exploited in a real-world situation.

## Steps to Reproduce/PoC
A clear outline of the steps required to execute the payload as an attacker, this can include how to setup the payload and launch it.

1. Step one...
2. Step two...
3. Step three...

### Request
```http
POST /endpoint HTTP/1.1
Host: target.com
Content-Type: application/json

{"example": "payload"}
```

### Response
```http
HTTP/1.1 200 OK
Content-Type: application/json

{"result": "response showing vulnerability"}
```

### Screenshots
- screenshot1.png - Description of what it shows
- screenshot2.png - Description of what it shows

## Affected Demographic/User Base
- Explain who this issue affects?
- Is it everyone or just a select amount of users?
- How can this occur in normal usage?
- What is the potential scale of impact?

## Recommended Fix
- How do you fix the issue?
- What is the recommended remediation actions required to successfully fix issue x?
- Are there any quick mitigations available while a full fix is developed?

## References
Include additional reading for the client to further backup the issues explained or elaborate more on other potential issues chained to the one identified.
- [1] [Reference 1](https://example.com)
- [2] [Reference 2](https://example.com)
