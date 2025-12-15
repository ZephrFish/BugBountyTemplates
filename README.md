# Bug Bounty Templates
A collection of templates for bug bounty reporting, with guides on how to write and fill out. Not the core standard on how to report but certainly a flow I follow personally which has been successful for me. Your mileage may vary.

Feel free to clone down, modify, suggest changes, tweet me ideas [@ZephrFish](https://twitter.com/ZephrFish).

## Templates Included

| Template | Best For | Complexity |
|----------|----------|------------|
| [Blank.md](Blank.md) | Comprehensive reports, learning, complex vulnerabilities | Full |
| [HeadersOnly.md](HeadersOnly.md) | Experienced researchers, quick scaffolding | Minimal |
| [Example.md](Example.md) | Reference for quality standards (XSS demo) | Complete |
| [short.md](short.md) | Simple/low-severity issues, time-sensitive reports | Brief |
| [SSRF.md](SSRF.md) | Server-Side Request Forgery vulnerabilities | Specialized |
| [API.md](API.md) | API-specific vulnerabilities (auth, IDOR, rate limiting) | Specialized |

### When to Use Each Template

- **Blank.md** - Your go-to for most reports. Includes guidance text explaining each section. Best for learning proper report structure or documenting complex vulnerabilities.

- **HeadersOnly.md** - For experienced researchers who know what goes in each section. Just headers, no guidance text. Fast scaffolding.

- **short.md** - When you need to submit quickly or the vulnerability is straightforward. Covers the essentials: what, where, how, fix.

- **Example.md** - Not a template to fill out, but a reference showing what a well-written report looks like. Study this for quality standards.

- **SSRF.md** - Server-Side Request Forgery requires specific context (protocols, internal access, cloud metadata). This template prompts for SSRF-specific details.

- **API.md** - API vulnerabilities have unique characteristics (endpoints, auth tokens, rate limits). Use this for BOLA/IDOR, broken auth, mass assignment, etc.

## Severity Ratings Guide

| Rating | CVSS 3.1 Score | Description | Examples |
|--------|---------------|-------------|----------|
| **Critical** | 9.0 - 10.0 | Immediate, widespread impact with trivial exploitation | RCE, Auth bypass, SQL injection with admin access |
| **High** | 7.0 - 8.9 | Significant impact, may require some conditions | Stored XSS, SSRF to internal services, privilege escalation |
| **Medium** | 4.0 - 6.9 | Moderate impact, requires user interaction or specific conditions | Reflected XSS, CSRF, information disclosure |
| **Low** | 0.1 - 3.9 | Limited impact, difficult to exploit | Self-XSS, verbose errors, minor info leaks |
| **Informational** | 0.0 | No direct security impact | Best practice violations, missing headers |

### CVSS 3.1 Quick Reference
- [FIRST CVSS 3.1 Calculator](https://www.first.org/cvss/calculator/3.1)
- [NVD CVSS Calculator](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator)

## Evidence Collection Checklist

Before submitting, ensure you have:

- [ ] **Screenshots** - Clear, annotated images showing each step
- [ ] **HTTP Requests/Responses** - Full request/response pairs from Burp/proxy
- [ ] **Timestamps** - When you discovered and tested the vulnerability
- [ ] **Affected URLs** - Complete list of vulnerable endpoints
- [ ] **Payload Used** - Exact payload/input that triggers the vulnerability
- [ ] **Impact Demonstration** - Concrete proof of what an attacker gains
- [ ] **Environment Details** - Browser, OS, tools used (if relevant)
- [ ] **Video PoC** (optional) - For complex multi-step exploits

### Screenshot Best Practices
1. Highlight/annotate the relevant parts
2. Remove or redact sensitive personal data
3. Include browser URL bar when relevant
4. Use sequential naming (step1.png, step2.png)

## Common Mistakes to Avoid

### Report Quality Issues
- **Vague descriptions** - "I found XSS" without specifics. Always include exact location and payload.
- **Missing reproduction steps** - Triagers must reproduce your finding. Be explicit.
- **No impact explanation** - "XSS is bad" is not enough. Explain what an attacker could actually do.
- **Duplicate without checking** - Search disclosed reports first.
- **Screenshots without context** - Images should have descriptions and fit into the narrative.

### Technical Mistakes
- **Self-XSS reported as XSS** - If only you can trigger it on yourself, it's usually not valid.
- **Logout CSRF** - Generally not accepted as a valid vulnerability.
- **Missing security headers only** - Without demonstrable impact, usually informational at best.
- **Rate limiting on non-sensitive endpoints** - Focus on auth, password reset, OTP endpoints.
- **Theoretical attacks** - Prove exploitation, don't just theorize.

### Communication Issues
- **Demanding bounties** - Let the program assess severity.
- **Threatening disclosure** - Follow responsible disclosure timelines.
- **Poor formatting** - Use markdown, structure your report, make it easy to read.
- **Not responding to questions** - Engage with triagers promptly.

## Writing Markdown
Sometimes manipulating markdown can be an alien task. Luckily there are several tools out there for writing it and helping out.

VS Code now supports markdown by default via plugins, and so do many other text editors. VSCode is cross platform and a good option for writing.

### Online
- [StackEdit](https://stackedit.io/)
- [Dillinger](https://dillinger.io/)

### Desktop Apps
- [Obsidian](https://obsidian.md/) (cross-platform)
- [Typora](https://typora.io/) (cross-platform)
- [MacDown](http://macdown.uranusjr.com/) (macOS)

## Further Reading
- [HackerOne Disclosed Reports](https://hackerone.com/hacktivity?sort_type=latest_disclosable_activity_at&filter=type%3Apublic&page=1) - Learn from real reports
- [Writing Better Bug Bounty Reports](https://blog.zsec.uk/stay-beautiful-stay-verbose/) - My blog post on report writing
- [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/) - Comprehensive testing methodology
- [PortSwigger Web Security Academy](https://portswigger.net/web-security) - Learn web security fundamentals
