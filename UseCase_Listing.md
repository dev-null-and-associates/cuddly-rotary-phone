# Team 1 UseCase / MisUseCase

## Vandergriff UseCase - Employee access through WAF
![UseCase 1](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/vandergriff_UseCase1.png)
![UseCase 2](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/vandergriff_UseCase2.drawio.png)
![UseCase 3](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/vandergriff_UseCase3.drawio.png)

## Use of AI
### Vandergriff
  I asked CoPilot this question "CoPilot Question - given a WAF, what usecases could be created for an employee to access through the WAF", and got suggestions about internal concerns and attacks.
  
## Security Requirements

### Vandergriff	

1) WAF should be setup to require strong passwords for access
2) WAF should sanitize any information coming in before using it.
3) WAF should have IP blocking on, to prevent DDOS attacks.
4) Anti Virus software, and maintained.
5) Spam Blockers
6) Phishing awareness education
7) Login credentials Security training
8) Logging with Admin review

## Reflections
### Vandergriff
I found the UseCase diagrams very helpful in thinking about misuse cases.  Having the intended action visable, helped me to think about it specifically, and I started thinking up how I would try to harm that part of the system.  Without the diagram, some attacks were easy to think up, but others would not have been considered.  Especially the secondary attack ideas.

## Assessment
### Vandergriff
Current documentation of the project states: 🔥 Security - Coraza runs the OWASP CRS v4 (Formerly known as Core Rule Set) to protect your web applications from a wide range of attacks, including the OWASP Top Ten, with a minimum of false alerts. CRS protects from many common attack categories including: SQL Injection (SQLi), Cross Site Scripting (XSS), PHP & Java Code Injection, HTTPoxy, Shellshock, Scripting/Scanner/Bot Detection & Metadata & Error Leakages. Note that older versions of the CRS are not compatible.
Looking through the code, I found a AuditLog section, that should be used for review of access and transactions.  I did not see anything connecting to an antivirus software, so that may need to be addressed.

## Assignment Part 2
### Vandergriff
1. The main gap I have found thus far is related to emails passing through this project.  I do not see anything related to attachment security, anti virus, spam blocking

---

## Jmcshannon UseCase - WAF Platform Integrity
![UC1: Deploy WAF to protect banking applications](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/uc1_jmcshannon.drawio%20(2).png)
![UC2: Monitor and Maintain WAF Operations](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/uc2_jmcshannon.drawio.png)
![UC3: Rule Updates and Tuning](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/uc3_jmcshannon.drawio.png)
![UC4: Ensure WAF Integrity](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/uc4_jmcshannon.drawio.png)
![UC5: Automate Updates and Deployment (CI/CD)](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/uc5_jmcshannon.drawio.png)

## Use of AI
### Jmcshannon
I asked GPT the following: "Given an opensource WAF, what use cases could be created for systems administrators at a regional bank deploying the WAF in their environment. What are the misuse cases?" 
I followed up with additional questions and conversation to restrict the use and misuse cases to sys admins/security engineers and operations CI/CD. I then asked the AI to generate UML for each of the use cases presented and adapted the UML diagrams to draw.io. Additionally I asked the AI to derive security requirements given the bounds of the use cases and actor persona.

## Security Requirements

### Jmcshannon

1. **SR1: Hardened Deployment Configurations**
2. **SR2: Separation of Environments**
3. **SR3: Rule Change Control**
4. **SR4: False Positive Management**
5. **SR5: Secure Log Handling**
6. **SR6: Access Control for Logs**
7. **SR7: Continuous Monitoring**
8. **SR8: Provenance Verification**
9. **SR9: Dependency Security**
10. **SR10: Third-Party Component Verification**
11. **SR11: Automated Patch Management**
12. **SR12: Version Pinning**
13. **SR13: Update Validation**
14. **SR14: Least Privilege Tokens**
15. **SR15: Ephemeral Tokens**
16. **SR16: Pipeline Activity Monitoring**
17. **SR17: Segregation of Duties in CI/CD**

## Assessment
### Jmcshannon
The documentation for the Coraza WAF project mainly focuses on the features of the WAF (Surprise, surprise), and doesn't include a lot of information about deployment/maintenance or assurance of the project. While there are some pages available on the instantion of the WAF and a quickstart for adding the WAF to a project there is really nothing about operations. In the code base I found some issues with the current handling of certain aspects that could lead to problems down the line for the maintainers and anyone who is dependent on this product.

## Reflections
### Jmcshannon
I enjoy me a good diagram. I have learned that it helps to get a birds eye view of the interactions and putting user stories into diagrams helps to envision all of the ways that a software would be used as well as some likely ways it could be misused. I think the most useful takeaway for me is the concept of anti-requirements. I like that and I will use those moving forward.

## Assignment Part 2
### Jmcshannon
The documentation is basically devoid of security-related configuration and installation information. There isn't much in the way of integration regarding things like MFA/SSO and there is a gap in the documentation with regards to properly vetting the ruleset. It basically relies on the core rule set documentation provided by the OWASP CRS maintainers, which while we could assume the CRS provides rules that are safe to use, a lot of work would need to be done to prevent oopsies with rules that block necessary traffic. 

### How to improve the documentation
As previously mentioned the documentation focuses on installation and some of the core features of the WAF, but provides near zero guidance on creating an enterprise or even SMB worthy deployment of the product so adding some of that documentation would be useful as well as documentation regarding the steps the developers and project team themselves take to provide integrity and provenance of their product.


### Leave this until all team members have submitted their changes and then condense
## Team Reflection

