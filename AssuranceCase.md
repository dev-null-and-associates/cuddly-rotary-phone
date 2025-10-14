# Team 1 Assurance Cases

## Assurance Cases

### Case One
![Aaron Assurance Case](/diagrams/assurance_case_diagrams/AaronAssuranceCase.png)

AI Prompt from assignment:  
You are an expert software security engineer. Your job is to suggest rebutting defeater for a given assurance claim 
using the concept of eliminative induction where support/assurance increases as reasons for doubt are eliminated.  
Here is an example claim for which rebutting defeaters are identified.   
Claim: The bulb will glow when switched on  
Rebuttal 1: Unless switch not connected to light  
Rebuttal 2: Unless no power  
Rebuttal 3: Unless dead light bulb  
Use this statement as a potential claim for analysis. Keep the rebuttals short and intuitive for including in a readable
diagram.   
"""   
All requests through the reverse proxy are required to go through the Web Application Firewall (WAF)  
"""  

Response:  
Rebuttal 1: Unless backend server is directly exposed to the internet  
Rebuttal 2: Unless WAF fails to initialize  
Rebuttal 3: Unless the WAF is in detection-only mode

The first two rebuttals were captured by myself initially before the AI helped out - just in different words. This made
me more confident in making my own rebuttals. The only new one was rebuttal 3. After looking at it for a while, I didn't
think it would be a valid rebuttal, as the claim was that all requests went through the WAF, not necessarily that all 
the requests were being verified/validated by the WAF. Maybe this would be a good rebuttal for a different claim.

### Case Two
![Jmcshannon Assurance Case](/diagrams/assurance_case_diagrams/Assurance%20Case.drawio.png)

AI Prompt: I am working on an assurance case for Coraza WAF operating in a regional bank environment. My top level claim
is that the WAF is free from critical CVE's and for one of my rebuttals I have "No suitable patch exists" what should my sub-claim be?

GPT responded with a sub-claim to control risk when no patch exists.
"In cases where no suitable patch exists for a known critical CVE affecting the Coraza WAF or its dependencies, compensating controls and temporary mitigations are implemented to ensure the system remains protected and the risk is monitored until remediation is available."

I then asked "describe an undermining detractor for the above sub-claim"

and received "Undermining Detractor: Stale or ineffective Compensating controls
Compensating controls implemented in lieu of a patch may degrade in effectiveness over time, fail to fully mitigate the vulnerability, or become obsolete as the threat landscape evolves — leading to unmitigated exposure despite perceived protection."

This prompted me to go with the whole weekly risk review process.

## Part Two

### Case One
The evidence for case one would be pretty easy to collect. They are all configuration files and logs that would be
external to the Coraza project itself. The four files that would need to be examined are:
1) Reverse proxy configuration file
2) Reverse proxy logs
3) Firewall logs
4) Firewall configuration file

### Case Two
The building blocks of the evidence I would need for my assurance cases are already there. Coraza WAF already has
a go.mod and a go.sum and a pretty nice collection of GitHub actions and workflows that could easily be extended to 
show that regular vulnerability scanning is occurring and that the WAF has no critical CVE's. The only evidence the
operational environment would really need to get for themselves is in the case of two rebuttals where a critical CVE is
present in which case we would need
1) The manual patching logs showing that a patch was performed and that the critical cve is no longer listed
2) The risk register/tickets referencing the case where there is a critical vulnerability and a patch does not exist or cannot be implemented.


### Reflections

#### Aaron Buesing
It was hard to come up with claims that were more than just "[project] does [X]". I thought it was a log more fun to come
up with the rebuttals than it was the claims. The ability to play devil's advocate was not something I do often, but
it ended up with some good thought processes. I think this would be pretty helpful in the future as an exercise. Being
able to say confidently, "this is what a product does" and then finding ways to throw "what ifs" or doubts into the mix
while trying to refute the doubts. It definitely can expand the thought processes and enumerate the evidence, but it 
can also show where there are gaps (if any) and gives a starting point on how to remediate that gap.

I learned that even the smallest, most obvious rebuttals still need to be addressed. Even if something is obvious, and 
that nobody would ever do it (like having a lightbulb with no power and expecting it to be lit), it still needs to be 
addressed. Some of the most embarrassing flaws I have dealt with in my own programs were because assumptions were made,
but if I had addressed the 'nobody would do this' issues, I wouldn't have had to deal with the mess afterwards.

### Jmcshannon
I learned that the accepted methods for creating assurance cases are very narrow and much more defined. I was winging it.

I found the rebuttals to be the most useful. I'm somewhat familiar with the assurance case, but the rebuttals felt a lot like
a case switch where I was able to follow a path until it terminated and clearly define what should be present to provide assurane and I liked that because it was a proactive exercise instead of a reactive one.

## Single Team Reflection