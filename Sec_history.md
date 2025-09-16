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