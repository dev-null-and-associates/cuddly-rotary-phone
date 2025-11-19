# Mitigations 1 - 10
    1. Current standard rules are provided by OWASP CRS and signed.  Any further rules need to have authentication added to the project.
    2. Project has timeout logic in place to prevent runaway processes
    3. Keep a default copy of rules with in the project, refreshed frequently
    4. Keep a default copy of rules with in the project, refreshed frequently
    5. Project is currently using HTTPS to connect to all outside sources.  There is no specific encryption process currently available.
    6. Mitigated
    7. Project is using HTTPS to connect to Data Store, and rules are provided via OWASP CRS and signed.
    8. No authentication mechanism currently available.
    9. Default, safe refusal is returned to the user.
    10. Audit logging is currently in place.