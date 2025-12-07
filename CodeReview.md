# Cuddly-Rotary-Phone Coraza WAF Code Review

## Strategy
We are going with a hybrid approach where we are performing a checlist and scenario based code review strategy leveraging automated tools to refine what we should look at manually. 

### What Challenges did you expect before starting the code review?
We expected the code review to be challenging because Golang (which Coraza is written in) is not a language any of us use. Due to our unfamiliarity with Golang coding practices we also don't have a strong background in the tool chain used by Golang developers to produce secure software.

### How did your code review strategy attempt to address the anticipated challenges?
In order to address these challenges we did some work to understand Golang a little more and leaned on AI to make sense of small chunks of code when needed.

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

### Manual Analysis Results

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

internal/operators/validate_schema.go:10:2: G501: Blocklisted import crypto/md5: weak cryptographic primitive (gosec)
internal/operators/validate_schema.go:33:12: G401: Use of weak cryptographic primitive (gosec)
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

### Summary of findings

While some false positives were uncovered it does appear that there are several real issues in the code including use of weak/outdated cryptographic libraries as well as real issues with file permissions being overly permissive on created log files. The log file permission creation issue is a really good find because that could have had some very negative outcomes if this application were to be deployed into our hypothetical environment.

### Planned Contributions

Upon further review of the code I would like to update the file permission code blocks as that would have the most impact with the smallest change. Additionally taking on the update of crypto libs would be a good idea, I fear that those values are intertwined throughout the project and a much bigger undertaking that appears at first glance.

### Jmcshannon Reflection

What did you learn from this assignment?
I learned coding in a language that has support for tooling is really important and that even if you use the same scanner as the development team there are still likely issues that the team has either muted or ignored that you can find and possibly help with.

What did you find most useful?
I enjoyed the gosec plugin and I thought it was exteremely useful and easy to use once I figured out how to only look at those results.


