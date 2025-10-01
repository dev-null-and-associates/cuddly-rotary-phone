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

---
## Use/Misuse Case - Logging, Monitoring and Alerting
#### Aiden Barger

### Use Case: Monitor and Respond to WAF Events
**Actor:** Security Analyst

**Basic Flow:**
1. Coraza inspects HTTP requests using its rule set
2. When a rule includes the `auditlog` action and logging conditions are met, Coraza writes an entry to the audit log
3. A logging agent forwards audit entries to the monitoring platform
4. The monitoring system detects patterns and raises alerts
5. Security analyst reviews alerts, investigates logs, and takes action

### Misuse Cases

| Misuse Case | Misuser/Motive | Attack Vector | Security Requirements |
|-------------|----------------|---------------|----------------------|
| **Tampering with audit logs** | Malicious insider | Gains host access, modifies or deletes audit files, disables audit engine or auditlog actions | • Store audit logs with strict permissions (0600)<br>• Use remote cryptographically signed logs<br>• Limit who can modify Coraza configurations<br>• Monitor that AuditEngine remains On and audit logger is configured |
| **Log flooding / alert fatigue** | Botnet operator | Sends high volumes of malicious requests to trigger excessive log entries and alerts | • Implement rate-limiting upstream<br>• Aggregate duplicate events to suppress redundant alerts<br>• Monitor log volume and set alerts on spikes |
| **Insecure log transmission** | Man-in-the-middle attacker | Intercepts unencrypted audit logs, exfiltrating sensitive data or injecting falsified entries | • Forward logs using TLS and authenticated endpoints<br>• Apply integrity checks on log streams<br>• Avoid sending logs over insecure protocols |

![Observability Use Case](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/diagrams/observabilityusecase.png)

### Existing Features & Gaps

**Tampering with logs:**
- Coraza directives allow administrators to control file and directory modes, defaulting to 0600, which limits access to the owner
- Does not provide cryptographic signing (external implementation required)
- Audit engine only writes logs when certain conditions are met; disabling the engine or removing auditlog actions stops logging

**Log flooding:**
- Coraza logs every matched transaction unless configured otherwise, but lacks built-in rate limiting or log aggregation
- Upstream tools should handle rate limiting and deduplication
- Operators can adjust `SecAuditLogParts` to reduce log verbosity and use `SecAuditLogRelevantStatus` to log only certain status codes

**Insecure transmission:**
- Logging directives specify only the local file path; secure forwarding (TLS, authentication) must be configured in external log shippers
- Coraza does not offer encryption for log outputs

### Reflection
Visualizing the use cases for logging and monitoring data with a diagram was super helpful in considering the potential misuses and gaps in the system. Of coures details matter, but the high level view of the data flow makes identifying the misuses more intuitive.

### Documentation Gap
The current Coraza documentation does a good job of detailing the audit logging directives but provides little guidance on secure log ingestion and monitoring that happens after the fact. It might be helpful to add a short "Secure Logging Setup" section that shows how to forward Coraza’s JSON audit logs to a common log stack. This would make it easier for operators to follow industry best practices for managing logs.
---

## Aaron Use Case
Coraza WAF is able to be run as a module within some common reverse-proxies. This is helpful when a legacy application or an off-the shelf application is needing extra protection. Since the source code is not able to be modified, having Coraza sit in front of the application provides an extra layer of protection against some common attacks.


![Legacy Application](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/aaron_usecase.png)

![Fuzzing of Application](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/aaron_applicationfuzzing.drawio.png)

### Security Requirements
1) WAF should prevent any state changing web requests without a CSRF token
2) IP Tables should be utilized to ensure all requests must come from WAF reverse-proxy server
3) Custom rules can be implemented to further protect from malicious applications - this may require more information about expected input/output
4) Limit over-sized inputs unless expected
5) Verify input is valid within the character set passed in.

### Reflection
At the beginning, the use case diagrams were a little challenging to wrap my head around. I have gone through a few different fake threat assessments at work, and while they had similar sections to these use cases, it was in a different format. Once I understood the layout/formatting of these use cases, it was nice to be able to take one use and work through the attacks and remediations one after the other. I think I still prefer a non-graph based format, but this is a super easy to understand layout that can help visual learners.

### Assessment
From the documentation: 🔌 Extensible - Coraza is a library at its core, with many integrations to deploy on-premise Web Application Firewall instances. Audit Loggers, persistence engines, operators, actions, create your own functionalities to extend Coraza as much as you want.

While the default code base does not support CSRF out of the bag, but as it is an extensible library, you are free to write your own functionality. This can be useful here to extend the ruleset to drop any POST, PUT, or DELETE request without a corresponding CSRF token in the header.

### Assignment Part 2
Looking through some of the issues, there are a few documentation improvements needed. For example, you can install plugins for Coraza, but there isn't a README documentation about how to configure the plugin when it is included in a proxy server ![Issue Link](https://github.com/corazawaf/coraza-caddy/issues/160)

The issues section of Coraza seem to be pretty well maintained. A lot of the issues with documentation have a 'Documentation' tag on them, so it is easy to spot. For example: ![documenting performance tweaks](https://github.com/corazawaf/coraza-caddy/issues). This makes for an easy start into documentation improvements.

=====

## Mason Wagner UseCase - WAF Integration
![UC: Reverse Proxy Integration](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/Wagner_Use_Case.drawio.png)

## Use of AI
### Mason Wagner
I gave the following prompt to Chat GPT: "Suppose you are a security engineer at a company implementing a WAF, what are some possibly Use-misuse cases that you would look at?".
I followed up with more specific questions about certain integration methods for WAF, until I determined what would be best suited.

## Security Requirements

### Mason Wagner

1. Anomaly scoring and custom rules in order to reduce false positives
2. High Availibility is necessary for eliminating a single point of failure.
3. High Availbility is needed for scaling to handle additional traffic.
4. Fault tolerance is necessary for providing protection even if a component of the system fails.
5. All inbound TLS traffic must be terminated at the WAF to allow for full inspection. Immediately after inspection, the WAF must re-establish TLS and forward traffic to backend application servers over a secure channel.

## Assessment
### Mason Wagner
Coraza WAF has documentation on the Reverse Proxy integration - ![Caddy Reverse Proxy and Webserver Plugin](https://github.com/corazawaf/coraza-caddy/tree/main). Based on my misuse case analysis I would say my findings on the Caddy Reverse Proxy and Webserver Plugin provide sufficient security for the integration of the WAF. Albeit is up to the Security professional on the strategy they want to use for integration, based on my analysis I think a gradual enforcement would be the most efficient way.

## Reflections
### Mason Wagner
I think that visualizing the good and bad parts of the integration aspect helps put into perspective what is actually needed when it comes to security requirements. In my personal opinion it allows my mind to actually see the direct causes and effects of the integration rather than looking at a jumble of words. Misuse case diagrams should always be used to find the best possible solution.

## Assignment Part 2
### Mason Wagner
After looking through the github I did find some integration instructions for plug-ins regarding the reverse proxy. There is some information regarding the Core Rule Set provided by the Coraza WAF: [Ruleset](https://github.com/corazawaf/coraza-caddy/issues/143). There are some, albeit limited, integration instructions for the WAF: [Caddy Reverse Proxy and Webserver Plugin](https://github.com/corazawaf/coraza-caddy/tree/main).


### How to improve the documentation
I think they could improve the documentation by adding in documentation regarding to testing the Coraza WAF and configuring the WAF to be implemented in different situations from a big enterprise all the way to a small business. The security related documentation is also lacking and could use some updating on the proper ways to integrate and maintain the WAF for the highest ability to stay secure.


## Team Reflection

We all found diagrams helpful, however laying the diagrams out with a table would add a dimension and likely be better for documentation purposes (Diagrams and Spreadsheets). We found that being able to view the use cases and misuse cases through the lens of requirements helped us establish the potential misuse cases and determine the appropriate set of security requirements for our product. Finally, breaking the team out into domains was extremely helpful for providing a robust set of requirements and potential use/abuse cases.
