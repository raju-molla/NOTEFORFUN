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
