## OWASP Top 10

### 1. Broken Access Control

#### a. Cookie Manipulation

Changing the value of a cookie to access data or features that the user should not be allowed to access.

**Example:** A normal user changes a cookie from `role=user` to `role=admin` and gains access to the admin page.

#### b. Accessing Private User Data

Create two users and test whether one user can access the other user's data.

#### c. IDOR (Insecure Direct Object Reference)

Objects are accessed directly based on user input.

Examples include:

- Documents
- Images
- Database records

#### d. Privilege Escalation

Attempting to update another user or change account permissions without authorization.

### 2. Path / Directory Traversal

#### a. Path Traversal

A vulnerability where an attacker changes a file path to access files or directories they should not be able to access.

#### b. Basic Example

```text
../../../etc/passwd
```

#### c. Null-Byte Variant

```text
../../../etc/passwd%00.jpg
```

#### d. Alternate-Dot Variant

```text
….//….//….//etc/passwd
```

#### e. URL-Encoded Variant

```text
..%2F..%2F..%2Fetc%2Fpasswd
```

After URL decoding:

```text
../../../etc/passwd
```

### 3. Directory Traversal Cheatsheet

See [directory-traversal-cheatsheet.txt](directory-traversal-cheatsheet.txt) for the complete payload list. Use it as a automated in burp intruder 

### 4. CSRF (Cross-Site Request Forgery)

CSRF tricks a logged-in user's browser into performing an unwanted action using the user's existing session.

#### How to Test

Find a sensitive request, capture it, remove or change the CSRF token, resend it, and check whether the action still succeeds.

#### Example

Remove the CSRF token from a **change-email** request. If the email still changes and there is no other effective cross-site protection, investigate for CSRF.

#### Remember

A missing CSRF token alone does not prove a vulnerability. The request must be possible cross-site using the victim's authenticated session.

### 5. OAuth Redirect URI Manipulation

OAuth applications must strictly validate the `redirect_uri` against an allowlist of exact, pre-registered URLs. If validation is weak, an attacker may replace it with a URL they control.

#### Example

The victim signs in with Google and approves access. Because the application accepts the attacker-controlled `redirect_uri`, Google sends the authorization code to the attacker's site. The attacker may then try to exchange the code for tokens and access the victim's account.

#### How to Test

Modify the `redirect_uri` and test whether the authorization request is accepted when using an unregistered host, subdomain, path, scheme, port, URL encoding, or an open redirect.

#### Remember

Authorization codes are generally short-lived and bound to the client and `redirect_uri`; code interception alone may not be exploitable. Check whether PKCE is enforced and whether the token exchange requires the exact registered redirect URI.

### 6. SQL Injection and OS Command Injection

#### a. SQL Injection

SQL injection occurs when user input is concatenated into a database query instead of being handled with parameterized queries. For example, review parameters such as `productId=3` or `storeId=2` for unexpected changes in database behavior, errors, or response differences.

Test only systems you are authorized to assess, and use non-destructive requests. A database error or a changed response is an indicator to investigate, not proof by itself.

#### b. OS Command Injection

OS command injection is separate from SQL injection. Appending a shell command such as `uname` to `productId` or `storeId` will not normally run it. It is relevant only if the application passes that input to an operating-system command, or if a SQL injection can reach a database feature that is explicitly capable of executing OS commands.

#### c. Blind OS Command Injection

Blind command injection occurs when a command may execute but its output is not returned in the HTTP response. Test for it with a measurable, non-destructive delay or an out-of-band DNS callback to an interaction server you control.

Examples:

- Linux/Unix timing check: `; sleep 5`
- Windows timing check: `& timeout /t 5 /nobreak`
- OOB check: `; nslookup blind-command-ID.example.test`

Replace `blind-command-ID.example.test` with a unique domain from an authorized interaction service. Compare timing against a baseline and send OOB payloads only to infrastructure you control. A delay or callback is evidence to investigate, not proof without controls for network latency, retries, and application timeouts.

See [blind-command-cheatsheet.txt](blind-command-cheatsheet.txt) for safe, non-destructive payload templates for Burp Intruder.

#### Remember

Do not assume that a successful SQL injection provides operating-system access. Confirm the affected layer, avoid destructive commands, and report the minimum evidence needed to demonstrate impact.
