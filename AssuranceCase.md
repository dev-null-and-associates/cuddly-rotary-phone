# Team 1 Assurance Cases

## Assurance Cases

### Case One
#### Top Level Claim
"Requests through the reverse proxy are required to go through the WAF"

![Aaron Assurance Case](/diagrams/assurance_case_diagrams/AaronAssuranceCase.png)

#### AI Prompt from assignment:  
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

#### Reflection on Prompt: 
The first two rebuttals were captured by myself initially before the AI helped out - just in different words. This made
me more confident in making my own rebuttals. The only new one was rebuttal 3. After looking at it for a while, I didn't
think it would be a valid rebuttal, as the claim was that all requests went through the WAF, not necessarily that all 
the requests were being verified/validated by the WAF. Maybe this would be a good rebuttal for a different claim.

### Case Two
#### Top Level Claim
"The WAF is free from known critical CVE's"

![Jmcshannon Assurance Case](/diagrams/assurance_case_diagrams/Assurance%20Case.drawio.png)

#### AI Prompt: 
I am working on an assurance case for Coraza WAF operating in a regional bank environment. My top level claim
is that the WAF is free from critical CVE's and for one of my rebuttals I have "No suitable patch exists" what should my sub-claim be?

GPT responded with a sub-claim to control risk when no patch exists.
"In cases where no suitable patch exists for a known critical CVE affecting the Coraza WAF or its dependencies, compensating controls and temporary mitigations are implemented to ensure the system remains protected and the risk is monitored until remediation is available."

I then asked "describe an undermining detractor for the above sub-claim"

and received "Undermining Detractor: Stale or ineffective Compensating controls
Compensating controls implemented in lieu of a patch may degrade in effectiveness over time, fail to fully mitigate the vulnerability, or become obsolete as the threat landscape evolves — leading to unmitigated exposure despite perceived protection."

#### Reflection on Prompt:
This prompted me to go with the whole weekly risk review process.

### Case Three
#### Top-Level Claim
"Coraza WAF ensures tamper-proof audit logging"

![Aiden Assurance Case](/diagrams/assurance_case_diagrams/AidenAssuranceCase.png)

#### AI Prompt:

You are an expert software security engineer. Your job is to improve the phrasing of assurance claims for an open-source web application firewall (Coraza WAF), focusing specifically on tamper-proof audit logging. Claims should describe outcomes, not mechanisms,for example, “Coraza WAF ensures audit logs cannot be altered without detection” rather than “Coraza uses file permissions.” Each claim must include the entity (Coraza WAF), a security property (log integrity), and the desired assurance value (tamper-proof). Identify clear “unless” rebuttals for each potential weakness, and end every argument branch with measurable, tangible evidence such as file permission settings, auditlog configuration defaults, or structured log format documentation.

#### Reflection on Prompt:

This focused prompt helped refine the tamper-proof logging claim so that it was concise had a clear desired outcome. It ensured that arguments emphasized security properties rather than details about implementation.


### Case Four
#### Top Level Claim
"Only valid people ae able to log in through WAF"

![Matthew Assurance Case](/diagrams/assurance_case_diagrams/vandergriff_AssuranceCase.drawio.png)

#### Reflection on Prompt
  I asked AI, how would a WAF deal with IP blocking.  I knew it could, but would it be covered by a WAF, or a regular FW?  Provided me with the Rules concept.


### Case Five
#### Top Level Claim
"Coraza WAF minimizes injection attacks"

![Mason Wagner Assurance Case](/diagrams/assurance_case_diagrams/MasonAssuranceCase.png)

#### AI Prompt: 
You are a senior-level security engineer at a bank, you are creating an assurance case on the Coraza WAF and your Top-level claim is: Coraza WAF minimizes Injection attacks. What would be four good starting rebuttals?

GPT Responded with: 
1. Coverage Limitation: Not all traffic paths are inspected.
2. Rule Maintenance: Rules must be current and tuned.
3. Operational Bypass: False positives can weaken defense.
4. Dependency on App Controls: WAF is only one layer in defense-in-depth.

#### Reflection on Prompt:
The prompt allowed me to see some different ideas of Rebuttals to go against my claim, I had to do some reqording of the prompts in order for it to fit exactly what I needed, but the structured prompt allowed me to get the ball rolling with some good rebuttal ideas.

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

### Case Three
Most of the evidence for the tamper-proof audit logging claim can be found already in the Coraza WAF repo and docs.
Coraza’s audit logging system as defined in the auditlog package supports both serial and concurrent writers, structured formats (JSON and Native), and default secure permissions (0600) for log files and directories. These features are confirmed in the Coraza docs and configuration files like coraza.conf-recommended.

However, some gaps remain. The repo does not necessarily include empirical proof that logs retain correct permissions across deployments or that entries cannot be lost or altered under high load.


### Case Five
For the most part there is a lot of documentation regarding implementation of the Coraza WAF which would be the evidence necessary for a few claims.

They also have a pretty well detail GitHub and website that notes on releases and updates that the Coraza WAF team and the community put out.

However, there are some gaps when it comes to actually having log examples, we would need to do some testing and get logs to actually provide the evidence.

  
### Reflections

#### Aaron Buesing
It was hard to come up with claims that were more than just "[project] does [X]". I thought it was a lot more fun to come
up with the rebuttals than it was the claims. The ability to play devil's advocate is not something I do often, but
it ended up with some good thought processes. I think this would be pretty helpful in the future as an exercise. Being
able to say confidently, "this is what a product does" and then finding ways to throw "what ifs" or doubts into the mix
while trying to refute the doubts. It definitely can expand the thought processes and enumerate the evidence, but it 
can also show where there are gaps (if any) and gives a starting point on how to remediate that gap.

I learned that even the smallest, most obvious rebuttals still need to be addressed. Even if something is obvious, and 
that nobody would ever do it (like having a lightbulb with no power and expecting it to be lit), it still needs to be 
addressed. Some of the most embarrassing flaws I have dealt with in my own programs were because assumptions were made,
but if I had addressed the 'nobody would do this' issues, I wouldn't have had to deal with the mess afterwards.

#### Jmcshannon
I learned that the accepted methods for creating assurance cases are very narrow and much more defined. I was winging it.

I found the rebuttals to be the most useful. I'm somewhat familiar with the assurance case, but the rebuttals felt a lot like
a switch case where I was able to follow a path until it terminated and clearly define what should be present to provide assurance and I liked that because it was a proactive exercise instead of a reactive one.

#### Aiden Barger
In this assignment I of course learned how to build the evidence backed claims that allow us to have trust in our open source software. Actually developing the assurance case helped show me how the claims, rebuttals, and evidence are all connected logically. It’s also much more clear to me now how each must be supported by something measurable so that they can be proved or disproved.

#### Mathew Vandergriff
I thought this exercise was very good, from a logic perspective.  It was interesting to make a claim about functionality, and then come up with arguments as to why it was or was not so.  Much better than the simple argument of "Because it is", or "Because I see it in the code".  Just because you have some logic in the code to address your claim, are you really sure you have it all covered?  Each claim made me take another look, and ask, is that all?  Can I go further, ask more?

### Mason Wagner
For this assignment I learned a lot more about what it actually takes to create an assurance case and a proper diagram. On the previous assignment I wasn't fully aware of what was needed in the diagram, however, after the discussion my team held over the topic I was able to see what was needed.

I think this exercise is really useful for actually putting into perspective what kind of worries/problems could arise with integrating something such as the Coraza WAF. I also think that using AI tools such as Chat GPT can provide other windows of collaboration and rebuttals that you may not think of at the moment.

## Single Team Reflection
Overall the team enjoyed this exercise and we were particularly fond of creating rebuttals. We all see how a process such as this one can add value
to our own work/projects and we are constantly finding new ways to become AI enabled practicioners. Collectively we found that assumptions aren't 
worth very much and coming up with evidence to support our claims was more satisfying than "I told ya, so". We also worked to define a more cohesive 
document and diagramming structure that homogenized our outputs which resulted in a much more polished finished product.


