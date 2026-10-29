# wordpress-security-assessment-wpscan
Security assessment of a local WordPress test environment using WPScan.
# WordPress Security Assessment Using WPScan

## 1. Project Overview

This project involved conducting a security assessment of a WordPress application hosted in a local virtual honeypot environment. The assessment was performed using **WPScan v3.8.28**, an open-source security scanner designed for WordPress websites.

The purpose of this project was to identify potential security weaknesses, examine their possible impact, and recommend appropriate security measures to improve the application's security.

The assessment followed a non-intrusive, black-box approach, examining the application from the perspective of an external user without directly accessing its internal source code.

## 2. Objectives

- Perform a security assessment of a local WordPress environment.
- Identify potential vulnerabilities and security misconfigurations using WPScan.
- Understand the possible impact of identified findings.
- Evaluate security risks using the Confidentiality, Integrity, and Availability (CIA) Triad.
- Recommend security hardening measures.
- Gain practical experience with vulnerability scanning and security reporting.

## 3. Tools and Technologies

| Tool | Purpose |
|---|---|
| WPScan v3.8.28 | WordPress security scanning |
| WordPress | Target web application |
| Apache | Web server in the test environment |
| PHP | Server-side technology used by WordPress |
| Local virtual environment | Controlled testing environment |

## 4. Test Environment

The assessment targeted a WordPress application running in a local virtual honeypot environment.

- **Target:** Local WordPress installation
- **Target address:** `http://192.168.1.71/wordpress-6.5-en_GB/wordpress/`
- **Scanner:** WPScan v3.8.28
- **Assessment type:** Non-intrusive, black-box security assessment
- **Environment:** Local test environment

The target was a private local IP address and was not a public website.

## 5. Assessment Methodology

The assessment involved the following steps:

1. Identified the local WordPress application to be assessed.
2. Ran WPScan against the target application.
3. Examined the HTTP response headers and exposed WordPress endpoints.
4. Checked for security misconfigurations and potential information disclosure.
5. Examined the scan output for exposed usernames.
6. Documented the findings, potential impacts, and recommended mitigations.

### Scan Command

The following command was used during the assessment:

```bash
wpscan --url http://192.168.1.71/wordpress-6.5-en_GB/wordpress/ --enumerate vp,vt,u
```

The command instructed WPScan to enumerate vulnerable plugins, vulnerable themes, and users. The scan also reported other findings, including server headers, XML-RPC availability, directory listing, and WP-Cron exposure.

## 6. Identified Security Findings

The assessment documented five main security findings.

### Finding 1: XML-RPC Enabled

**Evidence:** WPScan confirmed that the `xmlrpc.php` endpoint was accessible.

**Potential impact:**
- May increase exposure to automated password-guessing attacks.
- May allow abuse of XML-RPC functionality, depending on configuration.
- Pingback functionality may contribute to denial-of-service abuse.

**Recommended mitigation:**
Disable XML-RPC if it is not required, or restrict access to the endpoint using suitable WordPress or web-server controls.

### Finding 2: Directory Listing Enabled

**Evidence:** WPScan reported that directory listing was enabled in the WordPress uploads directory.

**Affected path:**

`/wp-content/uploads/`

**Potential impact:**
- Visitors may be able to browse files in the directory.
- Exposed files could reveal information that was not intended to be publicly accessible.

**Recommended mitigation:**
Disable directory indexing in Apache and review uploaded files to ensure that sensitive information is not publicly accessible.

Example Apache configuration:

```apache
Options -Indexes
```

### Finding 3: Server Information Disclosure

**Evidence:** HTTP response headers revealed the following server information:

- Apache/2.4.58 (Win64)
- OpenSSL/3.1.3
- PHP/8.2.12

**Potential impact:**
Exposed software and version details may help an attacker identify technologies running on the server and research potentially relevant vulnerabilities.

**Recommended mitigation:**
Reduce unnecessary server-information disclosure by reviewing Apache and PHP configuration.

Example Apache settings:

```apache
ServerTokens Prod
ServerSignature Off
```

Example PHP setting:

```ini
expose_php = Off
```

### Finding 4: External WP-Cron Enabled

**Evidence:** WPScan reported that the WordPress `wp-cron.php` endpoint appeared to be enabled.

**Potential impact:**
Repeated requests may trigger scheduled tasks and consume server resources under certain conditions.

**Recommended mitigation:**
Where appropriate, disable WordPress's request-triggered cron mechanism and configure an operating-system scheduled task to execute the required WordPress jobs.

Example setting in `wp-config.php`:

```php
define('DISABLE_WP_CRON', true);
```

This setting should only be applied when an alternative scheduled-task mechanism has been configured.

### Finding 5: User Enumeration Through the REST API

**Evidence:** WPScan identified the username `cyber_admin` through the WordPress REST API.

**Affected endpoint:**

`/wp-json/wp/v2/users/`

**Potential impact:**
Exposed usernames may make targeted password-guessing attempts easier. If an account is subsequently compromised, an attacker may gain unauthorised access depending on the account's privileges.

**Recommended mitigation:**
Review public REST API user information, restrict unnecessary exposure where appropriate, and use strong passwords and multi-factor authentication for privileged accounts.

## 7. Summary of Findings

| Finding | Evidence | Recommended action |
|---|---|---|
| XML-RPC enabled | Accessible `xmlrpc.php` endpoint | Disable or restrict if unnecessary |
| Directory listing | Upload directory listing enabled | Disable directory indexing |
| Server information disclosure | Apache and PHP details in headers | Reduce unnecessary version disclosure |
| External WP-Cron | WP-Cron endpoint reported as enabled | Consider system-scheduled execution |
| User enumeration | Username `cyber_admin` exposed | Restrict unnecessary user information exposure |

## 8. Security Impact: CIA Triad

The findings were considered in relation to the CIA Triad.

- **Confidentiality:** Directory listing, server information disclosure, and username enumeration may expose information that could assist further attacks.
- **Integrity:** User enumeration may contribute to account-compromise risks if combined with successful authentication attacks.
- **Availability:** XML-RPC abuse and excessive requests to scheduled-task endpoints may affect service availability under certain conditions.

These are potential impacts. The assessment did not establish that an account was compromised or that a successful denial-of-service attack occurred.

## 9. Recommended Security Improvements

Based on the findings, the following improvements were recommended:

1. Disable or restrict XML-RPC if the functionality is not required.
2. Disable directory indexing on the web server.
3. Reduce unnecessary server and PHP version disclosure.
4. Configure a suitable system-level scheduler if replacing WordPress's request-triggered cron.
5. Restrict unnecessary public exposure of WordPress user information.
6. Review the security configuration periodically and repeat authorised assessments when appropriate.

**Important:** These were recommendations documented in the assessment report. The fixes were not applied because the environment was being used as a honeypot.

## 10. Limitations

- The assessment used a black-box approach and did not include a complete source-code review.
- WPScan reported potential weaknesses and configuration issues; not every finding proves that a successful attack is possible.
- No WPScan API token was provided, so vulnerability database information was not included in the scan output.
- The recommended security fixes were not applied or verified as part of this assessment.
- The results represent the state of the local test environment at the time of scanning.

## 11. Key Learnings

Through this project, I gained practical experience in:

- Running WPScan against a local WordPress application.
- Understanding WordPress endpoints and common security misconfigurations.
- Reading and interpreting vulnerability scanner output.
- Identifying information disclosure and user enumeration risks.
- Understanding the potential security impact of exposed services and configurations.
- Mapping security findings to the CIA Triad.
- Documenting findings and recommending appropriate remediation measures.

## 12. Conclusion

This project demonstrated how a WordPress security assessment can identify potentially risky configurations and information exposure. Using WPScan, I documented five main findings and recommended security improvements for each.

The assessment helped me develop a better understanding of vulnerability scanning, web application security, risk analysis, and security reporting. These skills provide a useful foundation for further learning in cybersecurity and security analysis.

## 13. Ethical Considerations

This assessment was conducted against a local test environment for educational purposes. Security scanning should only be performed on systems that you own or have explicit permission to assess.

The findings and recommendations in this project are intended for defensive security learning and improving the security of authorised environments.

## 14. Project Evidence

Screenshots and the assessment report can be added to the repository to demonstrate the work performed.

Suggested repository structure:

```text
wordpress-security-assessment-wpscan/
├── README.md
├── reports/
│   └── wordpress-security-audit.pdf
└── screenshots/
    ├── wpscan-initial-scan.png
    └── wpscan-user-enumeration.png
```

The screenshot filenames above are examples. Use your actual uploaded filenames when adding evidence to this README.

