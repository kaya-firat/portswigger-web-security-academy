# Lab: Unprotected admin functionality (PortSwigger) - Write-up

## 1. Summary
The application in this laboratory contains a Broken Access Control vulnerability due to an unprotected administrative portal. The sensitive administration panel is publicly accessible without any authentication or authorization checks. By exploiting this flaw, an unauthorized attacker can access administrative functions and delete arbitrary user accounts.

* **Vulnerability Type:** Broken Access Control / Missing Function-Level Access Control (CWE-306)
* **Target Endpoint:** `/admin`
* **Severity:** High (Unauthorized Administrative Privilege Escalation / Sensitive Functionality Exposure)

---

## 2. Reconnaissance
During the initial mapping of the application, common directory brute-forcing, file analysis (such as checking `robots.txt`), or manual analysis was performed to locate administrative paths. It was discovered that the application hosts an administrative panel at the predictable `/admin` path. 

This endpoint was accessed directly via the browser without logging in, revealing that the administrative interface and its user management features were fully accessible to unauthenticated users.

> **Screenshot 1:** Accessing `/admin` directly without an active session or credentials showing the Admin Panel.

---

## 3. Exploitation
To exploit this vulnerability and delete the target user `carlos`, the unprotected administrative interface was used to trigger the deletion action.

1. Navigated to the `/admin` URL.
2. Located the "Delete" button next to the user `carlos`.
3. Clicked the button, which triggered the following HTTP request:

```http
GET /admin/delete?username=carlos HTTP/1.1
Host: [your-lab-id].web-security-academy.net
User-Agent: Mozilla/5.0 ...
Connection: close
