# Server-Side Request Forgery (SSRF)

## Issue Description
Server-Side Request Forgery (SSRF) is a vulnerability that allows an attacker to induce the server-side application to make HTTP requests to an arbitrary domain of the attacker's choosing. This can allow attackers to access internal services, read local files, or interact with cloud metadata services.

## Issue Identified
Describe the specific SSRF vulnerability found, including:
- What parameter/functionality is vulnerable
- What type of SSRF (basic, blind, partial)
- What protocols are supported (http, https, file, gopher, dict, etc.)

## Risk Rating
- Risk: **Critical / High / Medium / Low**
- Difficulty to Exploit: **Low / Medium / High**
- Authentication Required: **Yes / No**
- User Interaction Required: **No** (typically)
- CVSS 3.1 Score: [X.X](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:N/A:N)

### SSRF Impact Classification

| Access Level | Severity | Examples |
|--------------|----------|----------|
| Cloud metadata (169.254.169.254) | Critical | AWS/GCP/Azure credentials, IAM roles |
| Internal services (databases, admin panels) | Critical/High | Data exfiltration, internal exploitation |
| Internal network scanning | High | Port scanning, service discovery |
| Local file read (file://) | High | Configuration files, source code |
| Outbound requests only | Medium | Bypass IP restrictions, attack third parties |
| Blind SSRF (no response) | Medium/Low | Limited impact without response data |

## Affected URL/Area
- Vulnerable endpoint: `https://target.com/api/fetch`
- Vulnerable parameter: `url`, `host`, `redirect`, `path`, etc.

## SSRF Type
- [ ] **Basic SSRF** - Full response returned to attacker
- [ ] **Blind SSRF** - No response returned, but request is made
- [ ] **Partial SSRF** - Limited response (status code, headers only)

## Protocols Tested
Document which protocols the SSRF supports:
- [ ] `http://` / `https://`
- [ ] `file://`
- [ ] `gopher://`
- [ ] `dict://`
- [ ] `ftp://`
- [ ] Other: ___

## Steps to Reproduce

### Step 1: Identify the vulnerable parameter
```http
POST /api/fetch HTTP/1.1
Host: target.com
Content-Type: application/json

{"url": "https://attacker.com/callback"}
```

### Step 2: Test internal access
```http
POST /api/fetch HTTP/1.1
Host: target.com
Content-Type: application/json

{"url": "http://127.0.0.1:80"}
```

### Step 3: Access cloud metadata (if applicable)
```http
POST /api/fetch HTTP/1.1
Host: target.com
Content-Type: application/json

{"url": "http://169.254.169.254/latest/meta-data/"}
```

### Response
```http
HTTP/1.1 200 OK
Content-Type: application/json

{"response": "[internal data or metadata here]"}
```

## Bypass Techniques Used
If filters were in place, document bypasses:
- [ ] IP address encoding (decimal, hex, octal)
- [ ] DNS rebinding
- [ ] URL parsing inconsistencies
- [ ] Protocol smuggling
- [ ] Redirect-based bypass
- [ ] IPv6 addressing

Example bypass:
```
# Decimal IP for 127.0.0.1
http://2130706433/

# IPv6 localhost
http://[::1]/

# DNS rebinding
http://your-rebind-domain.com/
```

## Internal Resources Accessed
List what internal resources were accessible:

| Resource | URL | Data Retrieved |
|----------|-----|----------------|
| Cloud Metadata | http://169.254.169.254/latest/meta-data/ | Instance ID, IAM role |
| Internal API | http://internal-api:8080/health | Service status |
| Local files | file:///etc/passwd | User list |

## Affected Demographic
- All users who can access the vulnerable functionality
- Impact extends to internal infrastructure security
- Potential for lateral movement within internal network
- Cloud credential exposure affects entire cloud infrastructure

## Recommendation

### Immediate Mitigations
1. Implement allowlist of permitted domains/IPs
2. Block requests to private IP ranges (10.x.x.x, 172.16-31.x.x, 192.168.x.x, 127.x.x.x)
3. Block cloud metadata IP (169.254.169.254)
4. Disable unnecessary URL schemes (file://, gopher://, dict://)

### Long-term Fixes
1. Use a dedicated service/proxy for external requests
2. Implement network segmentation
3. Apply principle of least privilege to service accounts
4. Use cloud provider metadata service v2 (requires session tokens)
5. Deploy WAF rules for SSRF patterns

### Blocked IP Ranges
```
127.0.0.0/8        # Loopback
10.0.0.0/8         # Private Class A
172.16.0.0/12      # Private Class B
192.168.0.0/16     # Private Class C
169.254.0.0/16     # Link-local (includes metadata)
::1/128            # IPv6 loopback
fc00::/7           # IPv6 private
```

## References
- [1] [OWASP SSRF](https://owasp.org/www-community/attacks/Server_Side_Request_Forgery)
- [2] [PortSwigger SSRF](https://portswigger.net/web-security/ssrf)
- [3] [CWE-918: Server-Side Request Forgery](https://cwe.mitre.org/data/definitions/918.html)
- [4] [AWS IMDSv2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html)
- [5] [Cloud Metadata Endpoints](https://gist.github.com/jhaddix/78cece26c91c6263653f31ba453e273b)
