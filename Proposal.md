# Team 1 Project Proposal 🚀🚀🚀

## Name of opensource software 🔮
For our project proposal we have selected Coraza WAF

## Link to selected projects repository 💻
https://github.com/corazawaf/coraza

## Hypothetical operational environment 🖥️
We will be using a medium-sized corporate enterprise as our operational environment. In 
this enterprise, there is an on-site technology team that creates the majority of the 
company's websites / web applications. Everything else is sourced as COTS systems. All 
of the web applications can be grouped into three different audiences: employee-only, 
customer-facing, and public access. This is important because each audience should only 
get access to the information relevant to their needs: business sensitive information 
(employee information, business data, etc), customer data (including orders and bank 
account information), or only the generic marketing and customer sign-up portals. 

## Systems Engineering view in the hypothetical environment context 🌌
![Systems Engineering View](https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/systemEngineering.jpeg)

## Perceived threats in the operational environment 🌵
Running a web infrastructure always comes with vulnerability risks. This can be from poorly 
configured applications or from vulnerabilities within the application itself. Things like 
 * SQL Injections 
 * PHP/CSS Attacks 
 * Cross-Site Request Forgery
   
In order to protect customer and business information, protections need to be put into place 
to prevent the exploitation of these vulnerabilities. With Coraza WAF, these vulnerabilities 
can be caught and the requests can be rejected before they even make it to the web 
application. 

When vulnerabilities are discovered, there can be varying levels of fixes for them. If the 
application was developed internally, the source code can be updated to fix it. On the other 
hand, if the vulnerability is in an purchased product, the business will need to wait for a 
fix to come from a vendor. With Coraza WAF installed as a reverse-proxy, the business can 
use Coraza without any integration into a purchased product and can then add or update a rule 
within the firewall to reject requests that seek to exploit the found vulnerability. 

## Current security features ✈️

## Team Motivation 🐉

## Selected Project Description 💂‍♂️

For our team project we selected Coraza Web Application Firewall.

### What is Coraza WAF?
Coraza WAF is an enterprise grade Web Application Firewall written in Go
that provides support for both the Apache Modsecurity SecLang rules and 
OWASP CRSv4. Coraza is an OWASP Production level project and
while at it's core it is a library it is highly extendable
and can be implemented as an appliance with a GUI. The Coraza project
maintains a number of plugins for additional features such as the 
aforementioned GUI, Nginx support, HA, and WASM.

### Who Are The Contributors to Coraza WAF?

Coraza WAF has 44 contributors according to GitHub, however these numbers
include bots such as renovate and dependabot. At it's core there are two developers
whose contributions easily eclipise the the contributions of the third listed developer
by commit metrics.

### Coraza WAF Activity

Coraza WAF has recent pushes from the current week and has been under development
for a number of years. There are currently 30 open PR's, 68 issues, and a number
of threads in the discussion section as well as alternative community channels.
In the past week there have been 7 merged PR's and 1 closed issue. In the past two
weeks the repository has ran over 3K GitHub actions workflows at over 6k minutes.

### Who Uses Coraza WAF?

While Coraza WAF doesn't have the deep market penetration that other OWASP projects
may enjoy the software does has a fairly large user base according to the dependency graph
available on GitHub. The dependency graph notes 140+ dependents, with some fairly popular repositories
such as: 
- [Perforator 3.246 Stars and 146 Forks] (https://github.com/yandex/perforator)
- [Easegress 5,851 Stars and 493 Forks] (https://github.com/easegress-io/easegress)
- [CrowdSec 11,155 Stars and 536 Forks] (https://github.com/crowdsecurity/crowdsec)

### Coraza WAF Popularity

Coraza WAF has 2.9k Stars, 280 Forks, with 37 people watching the repository.
Coraza WAF has 152 dependent repositories and 105 dependent packages.
So while the WAF may not seem popular directly it is a key component for several
other inputs with a few notably popular projects in the dependencies.

### What Languages Does Coraza WAF Use?

The WAF itself is written in pure Golang.

### Documentation Sources

Coraza WAF documentation is mainly found on the Coraza.io website, however
OWASP also maintains a page dedicated to Coraza and the discussion and Issues
sections of GitHub also contain a wealth of information in addition to the example
code found on the main GitHub.

## Project Licensing, contribution procedures and contributor agreements ✈️

## Security related history of the project 🙈🙊🙉

## Team Reflections 🏁

### Jmcshannon 

**What did you learn from this assignment**

For me the biggest takeaway from this proposal was the amount of work involved in all 
of the additional overhead needed to run a moderately successful open source project.
While the team has OWASP Production Product status and have already written the web
application firewall in Golang, there are still a myriad of tasks for the team to work on
to drive community engagement and further adoption.

**What did you find the most useful**

I found the dependency graph to be extremely useful and something I hadn't really 
spent a lot of time looking at before. The dependency graph also sports an Export
as SBOM feature which is super useful.

### Aaron Buesing

**What did you learn from this assignment**

I learned how big and how much variety there is in the Open Source world. Something small 
like a library all the way up to fully-fledged applications with thousands or millions of 
users. While knowing about Linux and Android (AOSP) being open source, it is still surprising 
to see the vast number of other successful projects out there. It was also interesting to see 
the level of maturity for different projects. Some projects didn't have formal documentation 
or security/merge requirements listed within their projects so it would be a harder to start 
contributing to the project, while some of the more mature projects had full documentation 
and a single point to start learning how to contribute.

**What did you find the most useful**

I think having the websites that list open source projects was very helpful. Having them 
broken down by application type/domain made it very easy to figure out a handful of projects 
within a domain that a user is interested in. It is very intimidating to see thousands of 
projects, so being able to narrow it down is very helpful.

### Matthew Vandergriff

**What did you learn from this assignment**

I was amazed at how big the open source community has gotten.  How many different projects are available
and how hit and miss they are.  Some projects are fully fleshed out, some are just getting started, and
some have been abandoned.  And also, how hit and miss the development style is.  Given all of the discussion
in the world about security, and how many projects over the years have failed due to overhead and the cost
of making changes, how some projects look to be totally locked down, and some are just happy path.

**What did you find most useful**

Working on the system engineering view, reviewing the slides to better understand the pieces.  Looking at the
way projects are tagged in GitHub to help narrow down the search for suitable projects.  

## Task
Do research into the Coraza Waf in order to figure out the security history.

## Common Vulnerabilities and Exposures
Back in August of 2023, The application using Coraza Waf crashed due to a misuse of 'log.Fatalf', attackers were able to craft specific requests which forced the application to crash. The malicious request triggered an error in 'mime.ParseMediaType'. The vulnerability was patched in version 3.0.1.

The attack was not very complex and heavily impacted the availability. There was no need for elevated permissions or user interaction, which contribute to the ease of the attack.

https://app.opencve.io/cve/CVE-2023-40586

Prior to 3.3.3, if a request was made on an URI starting with //, coraza would set a wrong value in REQUEST_FILENAME. For example, if the URI //bar/uploads/foo.php?a=b is passed to coraza: , REQUEST_FILENAME will be set to /uploads/foo.php. This can lead to a rules bypass. This vulnerability is fixed in 3.3.3

https://github.com/corazawaf/coraza/commit/4722c9ad0d502abd56b8d6733c6b47eb4111742d
https://github.com/corazawaf/coraza/security/advisories/GHSA-q9f5-625g-xm39

## Security-related Engineering Decisions
Within Coraza you can enforce policies using the OWASP Core Ruleset, or create your own policies. Implementation of the Core Ruleset is fairly simple, as shown here 'https://coraza.io/docs/tutorials/coreruleset/'.

Additionally, Coraza follows the "Guide to coordinated vulnerability disclosure for open source software projects" where possible > https://github.com/ossf/oss-vulnerability-guide

Versions currently being supported with security updates.
Version 	Supported 	EOL
v1.2.x 	❌ 	Jun 1st 2022
v2.x 	✅ 	TBD
v3.x 	✅ 	Not defined

https://github.com/corazawaf/coraza/blob/main/SECURITY.md

## Security Feature Additions/Removals
Coraza utilizes the OWASP CRS (Core Ruleset).

The CRS uses Anomaly Scoring, Paranoia Levels, False Positives and Tuning, and Sampling Mode to ensure for the best possible protection.

Anomaly Scoring is a scoring mechanism utilized in CRS. It assigns a numeric score to HTTP transactions (requests and responses), representing how ‘anomalous’ they appear to be. Anomaly scores can then be used to make blocking decisions. The default CRS blocking policy, for example, is to block any transaction that meets or exceeds a defined anomaly score threshold.

Paranoia Levels make it possible to define how aggressive CRS is. Paranoia level 1 provides a set of rules that hardly ever trigger a false alarm (ideally never, but it can happen, depending on the local setup). PL 2 provides additional rules that detect more attacks (these rules operate in addition to the PL 1 rules), but there’s a chance that the additional rules will also trigger new false alarms over perfectly legitimate HTTP requests.

This continues at PL 3, where more rules are added, namely for certain specialized attacks. This leads to even more false alarms. Then at PL 4, the rules are so aggressive that they detect almost every possible attack, yet they also flag a lot of legitimate traffic as malicious.

False Positives and Tuning, CRS provides generic attack detection capabilities. A fresh CRS deployment has no awareness of the web services that may be running behind it, or the quirks of how those services work. It is possible that genuine transactions may cause some CRS rules to match in error, if the transactions happen to match one of the generic attack behaviors or patterns that are being detected. Such a match is referred to as a false positive, or false alarm.

Sampling mode makes it possible to run CRS on a limited percentage of traffic. The remaining traffic will bypass the rule set.