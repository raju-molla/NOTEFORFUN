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

### 6. Injection Vulnerability

An injection vulnerability occurs when an application sends untrusted user input to an interpreter or parser as instructions instead of treating it only as data. The affected component may be a database, operating-system shell, application interpreter, template engine, or another parser.

#### How to Test

Identify parameters that influence a query, command, template, expression, or file operation. Use harmless, controlled input and compare the response, error messages, output, or timing with a normal request.

Examples include:

- SQL injection in parameters such as `productId=3` or `storeId=2`
- OS command injection when input is passed to a system shell
- Template or expression injection when input is evaluated by an application engine
- LDAP, XPath, or NoSQL injection when input changes a directory or data query

#### Blind Injection

Blind injection occurs when the injected result is not visible in the response. Depending on the affected interpreter, test with a measurable, non-destructive delay or an out-of-band callback to an interaction server you control.

For blind OS command injection, use the payloads in [blind-command-cheatsheet.txt](blind-command-cheatsheet.txt) with Burp Intruder. Replace the callback placeholder with a unique domain from your authorized interaction service. Establish a baseline and account for latency, retries, and application timeouts.

#### Remember

An unusual response, error, delay, or callback is an indicator to investigate, not proof by itself. Confirm which interpreter or parser is affected, avoid destructive actions, and test only systems you own or are authorized to assess.
