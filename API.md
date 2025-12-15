# API Vulnerability Report

## Issue Description
Provide a description of the API vulnerability class. Common categories include:
- **Broken Object Level Authorization (BOLA/IDOR)** - Accessing other users' resources by manipulating IDs
- **Broken Authentication** - Flaws in authentication mechanisms
- **Broken Object Property Level Authorization** - Mass assignment, excessive data exposure
- **Unrestricted Resource Consumption** - Missing rate limiting, resource exhaustion
- **Broken Function Level Authorization** - Accessing admin functions as regular user
- **Server-Side Request Forgery** - API making requests to attacker-controlled destinations
- **Security Misconfiguration** - Verbose errors, missing security headers, default credentials

## Issue Identified
Describe the specific vulnerability:
- Which API endpoint is affected
- What the expected vs actual behavior is
- What authorization/authentication bypass was achieved

## Risk Rating
- Risk: **Critical / High / Medium / Low**
- Difficulty to Exploit: **Low / Medium / High**
- Authentication Required: **Yes / No**
- User Interaction Required: **Yes / No**
- CVSS 3.1 Score: [X.X](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N)

### API Vulnerability Impact Matrix

| Vulnerability Type | Typical Severity | Key Impact |
|-------------------|------------------|------------|
| BOLA/IDOR (data access) | High-Critical | Access to other users' data |
| Broken Authentication | Critical | Account takeover, session hijacking |
| Mass Assignment | Medium-High | Privilege escalation, data modification |
| Excessive Data Exposure | Medium-High | PII/sensitive data leakage |
| Rate Limiting (auth endpoints) | Medium-High | Brute force, credential stuffing |
| Rate Limiting (other) | Low-Medium | DoS, resource abuse |
| Security Misconfiguration | Low-High | Varies by exposure |

## Affected Endpoint(s)
```
Base URL: https://api.target.com/v1
Affected Endpoints:
- GET  /users/{id}
- POST /users/{id}/update
- DELETE /users/{id}
```

## API Vulnerability Type
- [ ] **BOLA/IDOR** - Broken Object Level Authorization
- [ ] **Broken Authentication** - Auth mechanism bypass
- [ ] **Broken Authorization** - Function level access control
- [ ] **Mass Assignment** - Unexpected property modification
- [ ] **Excessive Data Exposure** - API returns too much data
- [ ] **Rate Limiting** - Missing or bypassable limits
- [ ] **Injection** - SQL, NoSQL, Command injection via API
- [ ] **Other**: ___

## Authentication Context
- **Auth Type**: Bearer Token / API Key / Session Cookie / OAuth / None
- **User Role Tested**: Regular User / Admin / Unauthenticated
- **Token/Key Location**: Header / Query Parameter / Body

## Steps to Reproduce

### Step 1: Authenticate as User A
```http
POST /auth/login HTTP/1.1
Host: api.target.com
Content-Type: application/json

{"email": "usera@example.com", "password": "password123"}
```

Response:
```http
HTTP/1.1 200 OK
Content-Type: application/json

{"token": "eyJhbGciOiJIUzI1NiIs...", "user_id": 1001}
```

### Step 2: Access User B's resource using User A's token
```http
GET /users/1002/profile HTTP/1.1
Host: api.target.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

### Step 3: Observe unauthorized data access
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 1002,
  "email": "userb@example.com",
  "phone": "+1-555-0123",
  "address": "123 Private St",
  "ssn": "XXX-XX-1234"
}
```

## ID/Parameter Enumeration
If IDOR/BOLA, document the enumeration:

| Original Value | Modified Value | Result |
|----------------|----------------|--------|
| user_id=1001 | user_id=1002 | Access granted (VULN) |
| user_id=1001 | user_id=1003 | Access granted (VULN) |
| order_id=ABC123 | order_id=ABC124 | Access granted (VULN) |

### ID Format Analysis
- [ ] Sequential integers (easy to enumerate)
- [ ] UUIDs (harder but may be leaked elsewhere)
- [ ] Encoded values (Base64, etc.)
- [ ] Predictable patterns

## Data Exposed
List sensitive data accessible through this vulnerability:

| Field | Sensitivity | Example |
|-------|-------------|---------|
| Email | PII | userb@example.com |
| Phone | PII | +1-555-0123 |
| Address | PII | 123 Private St |
| Payment Info | Financial | Last 4 digits visible |
| Internal IDs | Technical | database_id, internal refs |

## Affected Demographic
- Number of users potentially affected: [estimate]
- Types of data at risk: PII, financial, authentication
- Scale: Can enumerate [X] records per [time period]
- Business impact: Compliance (GDPR, PCI), reputation, legal

## Recommendation

### Immediate Fixes
1. Implement proper authorization checks on all endpoints
2. Verify user owns/has access to requested resource
3. Add rate limiting to prevent mass enumeration
4. Log and alert on suspicious access patterns

### Authorization Check Example (Pseudocode)
```python
@app.route('/users/<user_id>/profile')
@require_auth
def get_profile(user_id):
    current_user = get_current_user()

    # Authorization check - user can only access own profile
    if str(current_user.id) != user_id:
        if not current_user.is_admin:
            return error(403, "Access denied")

    return get_user_profile(user_id)
```

### Long-term Recommendations
1. Implement attribute-based access control (ABAC)
2. Use non-enumerable identifiers (UUIDs)
3. API gateway with built-in authorization
4. Regular API security testing
5. Implement API versioning and deprecation policy
6. Add response filtering based on user permissions

## Testing Tools Used
- Burp Suite / OWASP ZAP
- Postman / Insomnia
- Custom scripts: [describe]
- Autorize (Burp extension for auth testing)

## References
- [1] [OWASP API Security Top 10](https://owasp.org/www-project-api-security/)
- [2] [OWASP API Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/)
- [3] [CWE-639: Authorization Bypass Through User-Controlled Key](https://cwe.mitre.org/data/definitions/639.html)
- [4] [PortSwigger Access Control](https://portswigger.net/web-security/access-control)
- [5] [API Security Best Practices](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
