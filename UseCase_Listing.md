# Team 1 UseCase / MisUseCase

## Vandergriff UseCase - Employee access through WAF
![UseCase 1](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/Vandergriff_usecase1.png)
![UseCase 2](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/Vandergriff_usecase2.drawio.png)
![UseCase 3](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/Vandergriff_usecase3.drawio.png)

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