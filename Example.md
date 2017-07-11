## Issue Description
Cross-Site Scripting (XSS) attacks are a type of injection, in which malicious scripts are injected into otherwise benign and trusted web sites. XSS attacks occur when an attacker uses a web application to send malicious code, generally in the form of a browser side script, to a different end user. 

## Issue Identified
The consultant identified that the update profile picture is vulnerable to cross site scripting, it is possible to upload an image with a MIME type of `text/html` this is then stored on the user's profile as an XSS payload, the outline below demonstrates the steps taken to exploit and reproduce.

## Risk Breakdown
- Risk: **High**
 
- Difficulty to Exploit: **Medium**
 
- CVSS2 Score: **7.9** [(AV:N/AC:M/Au:S/C:C/I:C/A:N)](https://nvd.nist.gov/cvss.cfm?calculator&version=2&vector= (AV:N/AC:M/Au:S/C:C/I:C/A:N))

## Affected URLs
 - https://example.com/update-profile
 - https://example.com/file/upload

## Steps to Reproduce
The following steps indicate a proof of concept outlined in three(3) steps to reproduce and execute the issue.

**Step 1:**
Navigate to https://example.com/update-profile and select edit as shown in screenshot attached labelled step1.jpg.

**Step 2:**
Modify the profile image request with a local proxy, in this case the consultant is using Burp Suite. Change the Content type from image to text/html as shown in the post request:

    POST /file/upload/ HTTP/1.1
    Host: example.com
    ---snip----
    
    -----------------------------900627130554
    Content-Disposition: form-data; name="stored_XSS.jpg"; filename="stored_XSS.jpg"
    Content-Type: text/html
    
    <script>alert('ZephrFish')</script>
    -----------------------------900627130554

When this is sent, the following response is shown:

    HTTP/1.1 200 OK
    Date: Sat, 13 Aug 2016 14:31:44 GMT
    ---snip---
    
    {"url": "https://example.com/56fc3b92159006271305543ef45a04452e8e45ce4/stored_XSS.jpg?Expires=1465669904&Signature=dNtl1PzWV&Key-Pair-Id=APKAJQWLJPIV25LBZGAQ", "pk": "56fc3b92159006271305543ef45a04452e8e45ce4/stored_XSS.jpg", "success": true}

**Step 3:**
The file has been uploaded to Application X and is hyperlinked to from the profile page as shown in step 3.jpg. By simply following the link to the image which in this case is:

    https://example.com/56fc3b92159006271305543ef45a04452e8e45ce4/stored_XSS.jpg

The payload is executed as shown in attached screenshot labelled step3.jpg, thus this demonstrates the issue is stored cross site scripting.

## Affected Demographic
This issue will affect all users on the site who view the profile of the attacker, when the image is rendered the payload is executed instead of a profile image. Additionally when the malicious user posts anything on the forums the payload will execute.

## Recommendation
Insure that file upload checks the MIME type of content being uploaded, for additional security implement server side content checking to ensure file headers match that of the file extension. Additionally make sure that all user input is treated as dangerous do not render any HTML tags.

## References
For more information on remediation steps check out reference [[2]](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)).

 - [1] [OWASP XSS Explained](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS))
 - [2] [OWASP XSS Prevention Cheat Sheet](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet)
