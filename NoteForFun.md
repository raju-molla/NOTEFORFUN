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
