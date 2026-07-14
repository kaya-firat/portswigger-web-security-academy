# Security Assessment Report: Unprotected Admin Functionality

## 1. Executive Summary
During the security assessment of the target web application, a **Broken Access Control (Missing Function-Level Access Control)** vulnerability was identified. The application exposes an administrative portal that lacks authentication and authorization checks. 

An unauthenticated, external attacker can directly access the administration panel and perform high-privilege actions, such as deleting arbitrary user accounts, leading to a complete compromise of user management.

* **Vulnerability Name:** Unprotected Administrative Functionality
* **Vulnerability Type:** Broken Access Control / Missing Function-Level Access Control ([CWE-306](https://cwe.mit.org/data/definitions/306.html))
* **Severity:** Critical (High Impact / Low Complexity)
* **Target Endpoint:** `/admin`
* **Impact:** Unauthorized Privilege Escalation & Denial of Service (Account Deletion)

---

## 2. Reconnaissance & Discovery
During the initial mapping phase, standard directory discovery or manual analysis was performed to identify potential administrative interfaces. A predictable administrative path was identified at `/admin`.

By attempting to access this endpoint directly without providing any credentials or session identifiers, it was observed that the application does not enforce any access controls. The server returned the administrative panel dashboard, exposing user management operations to public view.

> **[Screenshot 1 Placeholder]** *Accessing `/admin` directly without an active session or credentials, revealing the user management interface.*

---

## 3. Exploitation
To demonstrate the impact of this vulnerability, the target user account `carlos` was deleted by directly invoking the administrative endpoint.

1. The attacker navigates to the `/admin` URL without logging in.
2. The attacker locates the "Delete" link/button corresponding to the user `carlos`.
3. Clicking the button initiates the following HTTP request, executing the deletion without validating the requester's identity:

```http
GET /admin/delete?username=carlos HTTP/1.1
Host: <your-lab-id>.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Connection: close
