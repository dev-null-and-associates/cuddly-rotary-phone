# Coraza WAF Gap Analysis

Our team has leveraged the Microsoft Threat Modeling Tool to create and perform a 
STRIDE per interaction threat model on the Coraza WAF product from OWASP. While 
many of the identified threats had existing mitigations or were external to the 
core Coraza WAF product itself there was one **HUGE** gap in our evaluation. 
It appears that the Coraza WAF does not provide authentication, let alone strong
authentication for access to the administrative interface. This is a serious lack
of capabilities and would make a secure deployment much more challenging and likely
exceed the risk tolerance of most organizations. Additionally, in reviewing the issues
that have been lodged on the GitHub repository for Coraza WAF we see that there have been
reports of uncontrolled resource consumption (CWE-400) which appears to have been addressed,
but still would give a risk adverse organization such as our bank pause before adopting this technology.

Our work is available here: [Coraza WAF Threat Model] (https://github.com/dev-null-and-associates/cuddly-rotary-phone/blob/main/diagrams/data_flow_diagrams/DFD%20-%20Proxy%20User%20Request.htm)
