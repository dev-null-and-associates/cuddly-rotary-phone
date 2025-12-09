# WAF Code Review

## Strategy
We are going with a hybrid approach where we are performing a checlist and scenario based code review strategy leveraging automated tools to refine what we should look at manually. 

### What Challenges did you expect before starting the code review?
We expected the code review to be challenging because Golang (which Coraza is written in) is not a language any of us use. Due to our unfamiliarity with Golang coding practices we also don't have a strong background in the tool chain used by Golang developers to produce secure software.

### How did your code review strategy attempt to address the anticipated challenges?
In order to address these challenges we did some work to understand Golang a little more and leaned on AI to make sense of small chunks of code when needed.

## Part 1: Code Review Findings

## Jmcshannon Findings

### Code Review
I utilized a hybrid approach of extracting CWEs from the threat model and leveraging the gosec plugin of the golintci linter in order to produce results. While the initial results are quite large, reducing to only the gosec plugin gives us a pretty good idea of different places to look for certain high priority issues.

The initial scan produced 2672 lines of findings, however restricted to just the security plugins of the linter we reduced the scan results to approximately 555 lines of findings, however these findings were across many different types of plugins and not necessarily security issues by themselves, finally, restricting to just the gosec plugin the results are reduced to 23 findings.

Here are the initial findings:

debuglog/default.go:78:46: G115: integer overflow conversion uint -> int (gosec)  
http/e2e/e2e.go:261:5: G104: Errors unhandled (gosec)  
http/e2e/e2e.go:308:3: G104: Errors unhandled (gosec)  
internal/auditlog/formats_ocsf.go:181:22: G115: integer overflow conversion int -> int32 (gosec)  
internal/auditlog/formats_ocsf.go:186:15: G115: integer overflow conversion int -> int32 (gosec)  
internal/auditlog/formats_ocsf.go:190:15: G115: integer overflow conversion int -> int32 (gosec)  
internal/auditlog/formats_ocsf.go:218:45: G115: integer overflow conversion int -> int32 (gosec)  
internal/corazawaf/waf.go:265:9: G302: Expect file permissions to be 0600 or less (gosec)  
internal/environment/default.go:25:3: G104: Errors unhandled (gosec)  
internal/environment/default.go:26:3: G104: Errors unhandled (gosec)  
internal/operators/inspect_file.go:34:9: G204: Subprocess launched with a potential tainted input or cmd arguments (gosec)  
internal/operators/validate_schema.go:10:2: G501: Blocklisted import crypto/md5: weak cryptographic primitive (gosec)  
internal/operators/validate_schema.go:33:12: G401: Use of weak cryptographic primitive (gosec)  
internal/seclang/directives.go:761:56: G115: integer overflow conversion int64 -> uint32 (gosec)  
internal/seclang/directives.go:785:57: G115: integer overflow conversion int64 -> uint32 (gosec)  
internal/seclang/directives.go:933:42: G115: integer overflow conversion int64 -> uint32 (gosec)  
internal/transformations/js_decode.go:75:25: G602: slice index out of range (gosec)  
internal/transformations/md5.go:7:2: G501: Blocklisted import crypto/md5: weak cryptographic primitive (gosec)  
internal/transformations/md5.go:20:7: G401: Use of weak cryptographic primitive (gosec)  
internal/transformations/md5.go:31:9: G401: Use of weak cryptographic primitive (gosec)  
internal/transformations/sha1.go:7:2: G505: Blocklisted import crypto/sha1: weak cryptographic primitive (gosec)  
internal/transformations/sha1.go:19:7: G401: Use of weak cryptographic primitive (gosec)  
internal/transformations/sha1.go:29:9: G401: Use of weak cryptographic primitive (gosec)  


### Manual Analysis

While automated tools are great and drastically reduce the amount of time a code review can take by highlighting problems quickly it is still important to review the findings manually sometimes. 

Because I am not a Golang afficionado I leveraged AI to assist with understanding functions quickly and verifying if the results from the gosec plugin were correct.

# Manual Analysis Results

## CWE-190 Integer Overflow or Wraparound

This bug is located in the internal/auditlog/formats_ocsf.go file which provides a standard logging format that             adheres to the OCSF standard. The code blocks referenced are for HTTP status codes and this wouldn't provide a              feasible risk of CWE-190.


internal/auditlog/formats_ocsf.go:181:22: G115: integer overflow conversion int -> int32 (gosec)  
internal/auditlog/formats_ocsf.go:186:15: G115: integer overflow conversion int -> int32 (gosec)  
internal/auditlog/formats_ocsf.go:190:15: G115: integer overflow conversion int -> int32 (gosec)  
internal/auditlog/formats_ocsf.go:218:45: G115: integer overflow conversion int -> int32 (gosec)  


debuglog/default.go:78:46: G115: integer overflow conversion uint -> int (gosec)

This instance of potential CWE-190 appears to be real, while AI suggests this is a type safety bug so long as the code isn't reachable or attacker controlled, that still feels wrong and this is probably just something that should be fixed.


These results are false positives as the parseInt portion of the code prevents the integer overflow from returning something problematic.

internal/seclang/directives.go:761:56: G115: integer overflow conversion int64 -> uint32 (gosec)  
internal/seclang/directives.go:785:57: G115: integer overflow conversion int64 -> uint32 (gosec)  
internal/seclang/directives.go:933:42: G115: integer overflow conversion int64 -> uint32 (gosec)  

## CWE-252 Unchecked Return Value


http/e2e/e2e.go:261:5: G104: Errors unhandled (gosec)  
http/e2e/e2e.go:308:3: G104: Errors unhandled (gosec)

internal/environment/default.go:25:3: G104: Errors unhandled (gosec)  
internal/environment/default.go:26:3: G104: Errors unhandled (gosec)

While the first two instances of this CWE are thought to be false positives the last two instances are true positives that could impact the utility of the WAF as the function referenced tests whether or not the directory is writable and it not processing errors correctly.


## CWE-732 Incorrect Permission Assignment for Critical Resource


internal/corazawaf/waf.go:265:9: G302: Expect file permissions to be 0600 or less (gosec)

This is a valid issue as the file gets created with world read and world write. At a minimum this file likely should only be read/write for the user the WAF is running as any perhaps a group. So 660 at most, but this is even worse at 0666

return os.OpenFile(path, os.O_RDWR|os.O_CREATE|os.O_APPEND, 0666)


## CWE-78 OS Command Injection

After discussing this code block with AI I have determined that this is likely not a real issue, while it could be considered Command Injection, Golang exec does work to prevent executing shell commands and this path is not likely to be reachable by an attacker, but if it was reachable by an attacker and they could control the value being passed then theoreitcally it would be a valid CWE-78 finding because then the attacker would be able to control what programs are getting executed. 

internal/operators/inspect_file.go:34:9: G204: Subprocess launched with a potential tainted input or cmd arguments          (gosec)


## CWE-327 Use of a Broken or Risky Crypto Algo 

All of these code blocks are exposed to this CWE as they are importing the older/weak libraries on purpose and after looking at the comments in various parts of the code it appears that the developers felt the risk of collision was low enough to allow the use of these weak primitives, however the last change to one of these files was years ago so it may be time to update the rationale behind the exception and explore updating these libraries.

internal/operators/validate_schema.go:10:2: G501: Blocklisted import crypto/md5: weak cryptographic primitive (gosec)internal/operators/validate_schema.go:33:12: G401: Use of weak cryptographic primitive (gosec)  
internal/transformations/md5.go:7:2: G501: Blocklisted import crypto/md5: weak cryptographic primitive (gosec)  
internal/transformations/sha1.go:7:2: G505: Blocklisted import crypto/sha1: weak cryptographic primitive (gosec)  
internal/transformations/md5.go:7:2: G501: Blocklisted import crypto/md5: weak cryptographic primitive (gosec)  
internal/transformations/md5.go:20:7: G401: Use of weak cryptographic primitive (gosec)  
internal/transformations/md5.go:31:9: G401: Use of weak cryptographic primitive (gosec)  
internal/transformations/sha1.go:7:2: G505: Blocklisted import crypto/sha1: weak cryptographic primitive (gosec)  
internal/transformations/sha1.go:19:7: G401: Use of weak cryptographic primitive (gosec)  
internal/transformations/sha1.go:29:9: G401: Use of weak cryptographic primitive (gosec)  

## CWE-125 Out of Bounds Read

This could be a true positive findind as there appears to be an off-by-one error on the final case statement for the referenced file that would lead to an index out of range error.

internal/transformations/js_decode.go:75:25: G602: slice index out of range (gosec)

## MWvandergriff Findings

### Code Review

I took the automated code review approach.  Looking through the provided list, I hooked up SemGrep, to my fork of the Coraza/CorazaWAF project and ran the code review process.  The process came back with 10 issues, that correlated to 5 CWEs.  the URl is https://semgrep.dev/orgs/mvandergriff_unomaha_edu/findings?repo_ref=686559747.  The 5 CWEs that were found are
1. CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')
2. CWE-319: Cleartext Transmission of Sensitive Information
3. CWE-328: Use of Weak Hash
4. CWE-94: Improper Control of Generation of Code ('Code Injection')
5. CWE-338: Use of Cryptographically Weak Pseudo-Random Number Generator (PRNG)

#### CWE - 94
This issue was found in internal/operators/inspect_file.go, line 33.  The application is executing a Command Prompt with out input validation.  This opens up the code to potential Code Injection.

#### CWE - 328
This issue was found internal/operations/validate_schema.go line 32, internal/transformations/md5.go line 20, and line 31.  MD5 is an out of date encryption process, and should no longer be used.
Also, internal/transformations/sha1.go line 19, and line 29.  SHA -1 is an out of date encryption process and should not be used.

#### CWE - 338
This issue is in internal/strings/strings.go line 7.  This is using Math/Rand as part of the encryption process, when it should be using crypto/rand.

#### CWE - 319
This issue is found in examples/http-server/main.go line 39.  The application is writing text using HTTP instead of using HTTPS for all communication.

#### CWE - 79
This issue was found in internal/variable/generator/main.go line 19 and examples/http-server/main.go line 29.  The application is using test/template to render the pages, including user input.  This does not escape any HTML content and can lead to Cross Site Scripting.  The application should use html/template

## Part 2: Key Findings and Contributions

### Summary of Findings

While some false positives were uncovered, it does appear that there are several real issues in the code including use of weak/outdated cryptographic libraries as well as real issues with file permissions being overly permissive on created log files. The log file permission creation issue is a really good find because that could have had some very negative outcomes if this application were to be deployed into our hypothetical environment.

The automated code review did an excellent job at finding some potential serious issues. Use of MD5 or SHA-1 should have been discontinued years ago. And not sanitizing any kind of input leaves the project wide open for issues.

### Planned Contributions

Upon further review of the code, we would like to update the file permission code blocks as that would have the most impact with the smallest change. Additionally, taking on the update of crypto libs would be a good idea, though we fear that those values are intertwined throughout the project and a much bigger undertaking than appears at first glance.

We would also like to improve the sanitization of input data, and to look at the scope of the encryption process, and what that would take to update to a valid and secure encryption level.

## Team Reflection

### What did you learn from this assignment?

**Jmcshannon:** I learned coding in a language that has support for tooling is really important and that even if you use the same scanner as the development team there are still likely issues that the team has either muted or ignored that you can find and possibly help with.

**MWvandergriff:** The automated code review process was awesome. Having worked with a lot of legacy code, finding issues that are not throwing errors and demanding attention can be extremely difficult. And that is if you are familiar with the code. Working on an OSS project, in an unfamiliar language, increased the difficulty by a large margin. Being able to use the automated scan, really helped to focus, and identify major red flags.

**Aiden Barger:** This assignment showed the importance of using automated tools with manual analysis. Scanners can quickly find potential issues, but they might necessitate diving deep into context with manual review. I also learned how necessary it is to map findings back to very specific CWEs.

**Aaron Buesing:** We use a SAST tool for work to scan all of our code before deploying to production, 
so I was familiar with using the results from the scans to help improve code security posture. As this
was a programming language we don't use at work, looking at all the tools (or lack of tools) was a surprise 
to me.

**Mason Wagner:** I think it is important for different people to do code review rather than just one person. Adding different types of code review increases the likelihood that you will find CWEs. Different approaches with different tools will lead to different results, just as different people looking at the same code, with different experience levels will find different vulnerabilities.

### What did you find most useful?

**Jmcshannon:** I enjoyed the gosec plugin and I thought it was extremely useful and easy to use once I figured out how to only look at those results.

**MWvandergriff:** I used the Semgrep automated scanning tool. It identified several issues, where in the code the issue was, and mapped the issues back to CWEs.

**Aiden Barger:** Seeing the difference in results of the hybrid approach of automated or manual review was cool.

**Aaron Buesing:** I thought that having Semgrep list the CWE with its findings was helpful in order
to have a place to start looking for ways to fix it. You don't have to go to Google or Stack Overflow
looking for random fixes, you can start with the CWE document itself.

**Mason Wagner:** The varying ways to scan for CWEs whether manual or automated helps properly ensure the security of the WAF. Only using one tool, or only code reviewing manually, may allow for vulnerabilities to go by unnoticed.

### Combined Reflection
Overall, the team agreed that in a code review, an automated scan is a great starting point. It pin-points
places that a reviewer needs manually review, adding additional context into the scanner's output. We agree that
having multiple people (and tools) looking at the code provides a different lens that vulnerabilites can be found,
so having a single person review code will not catch everything. Finally, mapping the findings to a specific CWE
can help start the process to remediate it, so it is important that the tool assists with that.