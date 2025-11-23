## **Hvad er Secure by Design?**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#hvad-er-secure-by-design)

**Secure by Design** er en tilgang hvor **sikkerhed** er integreret fra **starten** af udviklings-processen, ikke tilføjet bagefter som et ekstra lag. Sikkerhed er en kernedel af design, arkitektur og implementation.

**Filosofi:** "Sikkerhed er ikke en feature - det er et fundament"

---

## **Kerneprincipper**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#kerneprincipper)

### **1. Security First**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#1-security-first)

- Sikkerhed prioriteres fra dag 1
- Ikke en eftertanke eller "nice-to-have"
- Integreret i alle faser af udvikling

### **2. Defense in Depth (Lag-på-lag forsvar)**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#2-defense-in-depth-lag-p%C3%A5-lag-forsvar)

- Multiple sikkerhedslag
- Hvis ét lag fejler, beskytter andre stadig
- Ingen single point of failure

### **3. Least Privilege (Mindste privilegium)**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#3-least-privilege-mindste-privilegium)

- Brugere/systemer får kun de rettigheder de **skal** bruge
- Default er ingen adgang
- Explicit grant af rettigheder

### **4. Fail Secure**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#4-fail-secure)

- Ved fejl/nedbrud: Systemet lukker sikkert ned
- Ikke "fail open" hvor adgang gives ved fejl
- Graceful degradation med sikkerhed intakt

### **5. Zero Trust**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#5-zero-trust)

- Stol aldrig på noget automatisk
- Verificér altid: "Never trust, always verify"
- Gælder både eksternt og internt netværk

---

## **Secure by Design Principper**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#secure-by-design-principper)

### **1. Minimize Attack Surface (Minimer angrebsoverflade)**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#1-minimize-attack-surface-minimer-angrebsoverflade)

**Hvad:**

- Reducer antallet af mulige angrebsvektorer
- Fjern unødvendige features og services
- Eksponér kun hvad der er nødvendigt

**Eksempler:**

```
❌ Åben admin-panel på internet
✅ Admin kun tilgængeligt via VPN

❌ Alle API endpoints offentlige
✅ Kun nødvendige endpoints eksponeret

❌ Debug-information i produktion
✅ Debug kun i development
```

---

### **2. Secure Defaults**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#2-secure-defaults)

**Hvad:**

- Default konfiguration skal være sikker
- Brugere skal aktivt vælge mindre sikre options
- "Opt-in" til risikable features

**Eksempler:**

```
✅ HTTPS som default (ikke HTTP)
✅ Kryptering aktiveret by default
✅ Strenge password-krav by default
✅ Sessions expire automatisk
✅ Audit logging aktiveret
```

---

### **3. Input Validation**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#3-input-validation)

**Hvad:**

- **Aldrig** stol på bruger-input
- Validér alt input på server-side
- Sanitize data før brug

**Teknikker:**

```cs
// Whitelist - kun tilladte værdier
if (!allowedValues.Contains(input)) 
    throw new ValidationException();

// Type checking
if (!int.TryParse(input, out int value))
    return BadRequest();

// Length limits
if (input.Length > MAX_LENGTH)
    return BadRequest();

// Encoding
var safe = HtmlEncoder.Default.Encode(userInput);

// Parameterized queries (SQL Injection prevention)
cmd.Parameters.AddWithValue("@username", username);
```

**Beskyt mod:**

- SQL Injection
- XSS (Cross-Site Scripting)
- Command Injection
- Path Traversal

---

### **4. Output Encoding**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#4-output-encoding)

**Hvad:**

- Encode data før visning
- Forhindrer XSS-angreb
- Context-aware encoding

**Eksempler:**

```js
// HTML Context
const safe = escapeHtml(userInput);

// JavaScript Context
const safe = JSON.stringify(userInput);

// URL Context
const safe = encodeURIComponent(userInput);
```

---

### **5. Authentication & Authorization**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#5-authentication--authorization)

**Authentication (Hvem er du?)**

```
✅ Multi-factor authentication (MFA)
✅ Strong password policies
✅ Account lockout efter X forsøg
✅ Secure password storage (bcrypt, Argon2)
✅ Session management
```

**Authorization (Hvad må du?)**

```
✅ Role-Based Access Control (RBAC)
✅ Attribute-Based Access Control (ABAC)
✅ Principle of least privilege
✅ Verify authorization på hver request
```

**Eksempel:**

```cs
[Authorize(Roles = "Admin")]
public IActionResult DeleteUser(int userId) 
{
    // Double-check authorization
    if (!User.IsInRole("Admin"))
        return Forbid();
    
    // Business logic
}
```

---

### **6. Encryption**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#6-encryption)

**Data at Rest (gemt data)**

```
✅ Database encryption
✅ Encrypted backups
✅ Encrypted file storage
✅ Full disk encryption
```

**Data in Transit (data under transport)**

```
✅ TLS/SSL (HTTPS)
✅ Certificate pinning
✅ Strong cipher suites
✅ Disable old protocols (SSLv3, TLS 1.0)
```

**Data in Use (data i hukommelsen)**

```
✅ Secure memory handling
✅ Clear sensitive data efter brug
✅ Avoid logging sensitive data
```

---

### **7. Error Handling**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#7-error-handling)

**Hvad:**

- Håndter fejl uden at lække information
- Log detaljeret internt
- Vis generisk fejl til brugere

**Dårligt:**

```
❌ Error: User 'admin' not found in database 'production_db'
❌ SQL Error: Invalid syntax near 'SELECT * FROM users'
❌ File not found: /var/www/secret/passwords.txt
```

**Godt:**

```
✅ Login failed. Please check your credentials.
✅ An error occurred. Please try again later.
✅ Invalid request.
```

**Logging (internt):**

```cs
try {
    // Logic
} catch (Exception ex) {
    // Log detailed error internt
    logger.LogError(ex, "Failed to process order {OrderId}", orderId);
    
    // Return generic error til bruger
    return StatusCode(500, "An error occurred");
}
```

---

### **8. Logging & Monitoring**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#8-logging--monitoring)

**Hvad skal logges:**

```
✅ Authentication attempts (success/failure)
✅ Authorization failures
✅ Input validation failures
✅ Changes to critical data
✅ Administrative actions
✅ System errors
```

**Hvad skal IKKE logges:**

```
❌ Passwords
❌ Credit card numbers
❌ Personal identifiable information (PII)
❌ Session tokens
❌ API keys
```

**Monitoring:**

```
⚠️ Unusual login patterns
⚠️ Multiple failed logins
⚠️ Privilege escalation attempts
⚠️ Suspicious API usage
⚠️ Data exfiltration patterns
```

---

### **9. Secure Dependencies**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#9-secure-dependencies)

**Dependency Management:**

```
✅ Keep dependencies updated
✅ Scan for known vulnerabilities
✅ Use dependency lock files
✅ Remove unused dependencies
✅ Vet third-party libraries
```

**Værktøjer:**

- **OWASP Dependency-Check**
- **Snyk**
- **GitHub Dependabot**
- **npm audit** / **dotnet list package --vulnerable**

---

### **10. Secrets Management**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#10-secrets-management)

**ALDRIG:**

```
❌ Hardcode passwords i kode
❌ Commit secrets til Git
❌ Store API keys i config files
❌ Share credentials i plain text
```

**I stedet:**

```
✅ Environment variables
✅ Key vaults (Azure Key Vault, AWS Secrets Manager)
✅ Encrypted configuration
✅ Secrets rotation
```

**Eksempel:**

```cs
// ❌ FORKERT
string apiKey = "sk_live_123456789";

// ✅ KORREKT
string apiKey = Environment.GetEnvironmentVariable("API_KEY");
// eller
string apiKey = keyVault.GetSecret("ApiKey");
```

---

## **Secure Development Lifecycle (SDL)**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#secure-development-lifecycle-sdl)

### **1. Requirements Phase**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#1-requirements-phase)

- Identificér security requirements
- Threat modeling
- Privacy requirements
- Compliance requirements (GDPR, PCI-DSS)

### **2. Design Phase**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#2-design-phase)

- Security architecture review
- Threat modeling (STRIDE, DREAD)
- Design review med security fokus
- Choose secure frameworks/libraries

### **3. Implementation Phase**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#3-implementation-phase)

- Secure coding guidelines
- Code reviews med security fokus
- Static Application Security Testing (SAST)
- Peer review

### **4. Testing Phase**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#4-testing-phase)

- Dynamic Application Security Testing (DAST)
- Penetration testing
- Security scanning
- Vulnerability assessment

### **5. Deployment Phase**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#5-deployment-phase)

- Secure configuration
- Hardening af servers
- Network segmentation
- Security monitoring setup

### **6. Maintenance Phase**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#6-maintenance-phase)

- Security patches
- Vulnerability management
- Incident response
- Security audits

---

## **Threat Modeling - STRIDE**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#threat-modeling---stride)

Framework til at identificere trusler:

|**Trussel**|**Beskrivelse**|**Modtræk**|
|---|---|---|
|**S**poofing|Udgive sig for nogen anden|Strong authentication, MFA|
|**T**ampering|Uautoriseret ændring af data|Input validation, checksums|
|**R**epudiation|Benægte handlinger|Logging, digital signatures|
|**I**nformation Disclosure|Uautoriseret informationsadgang|Encryption, access control|
|**D**enial of Service|Gøre system utilgængeligt|Rate limiting, redundancy|
|**E**levation of Privilege|Få højere rettigheder|Least privilege, authorization|

---

## **OWASP Top 10 (2021)**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#owasp-top-10-2021)

De mest kritiske web application sikkerhedsrisici:

1. **Broken Access Control** - Manglende adgangskontrol
2. **Cryptographic Failures** - Svag/manglende kryptering
3. **Injection** - SQL, XSS, Command injection
4. **Insecure Design** - Fundamentale design-fejl
5. **Security Misconfiguration** - Forkert konfiguration
6. **Vulnerable Components** - Sårbare dependencies
7. **Authentication Failures** - Svag authentication
8. **Software and Data Integrity Failures** - CI/CD, updates
9. **Security Logging Failures** - Manglende logging/monitoring
10. **Server-Side Request Forgery (SSRF)** - Uautoriserede requests

---

## **Security Testing Værktøjer**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#security-testing-v%C3%A6rkt%C3%B8jer)

### **SAST (Static Application Security Testing)**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#sast-static-application-security-testing)

- **SonarQube** - Code quality & security
- **Checkmarx** - Static analysis
- **Fortify** - Static code analyzer
- **Semgrep** - Open source static analysis

### **DAST (Dynamic Application Security Testing)**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#dast-dynamic-application-security-testing)

- **OWASP ZAP** - Web app scanner
- **Burp Suite** - Security testing
- **Acunetix** - Vulnerability scanner

### **Dependency Scanning**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#dependency-scanning)

- **OWASP Dependency-Check**
- **Snyk**
- **WhiteSource**

### **Container Security**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#container-security)

- **Trivy** - Container vulnerability scanner
- **Clair** - Container static analysis
- **Aqua Security**

---

## **Security Best Practices - Checkliste**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#security-best-practices---checkliste)

### **Authentication & Authorization**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#authentication--authorization)

- [ ]  Multi-factor authentication implementeret
- [ ]  Strong password policy (min. 12 tegn, kompleksitet)
- [ ]  Account lockout efter failed attempts
- [ ]  Passwords hashed med bcrypt/Argon2
- [ ]  Session timeout implementeret
- [ ]  Authorization check på alle endpoints

### **Data Protection**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#data-protection)

- [ ]  HTTPS everywhere (TLS 1.2+)
- [ ]  Database encryption
- [ ]  Sensitive data encrypted at rest
- [ ]  PII håndteret korrekt (GDPR)
- [ ]  Secure backup procedures

### **Input Validation**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#input-validation)

- [ ]  All user input validated server-side
- [ ]  Parameterized queries (SQL Injection prevention)
- [ ]  Output encoding (XSS prevention)
- [ ]  File upload restrictions
- [ ]  Request size limits

### **Infrastructure**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#infrastructure)

- [ ]  Firewalls konfigureret
- [ ]  Unnecessary services disabled
- [ ]  Regular security updates
- [ ]  Network segmentation
- [ ]  Intrusion detection system

### **Monitoring**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#monitoring)

- [ ]  Security event logging
- [ ]  Anomaly detection
- [ ]  Incident response plan
- [ ]  Regular security audits
- [ ]  Vulnerability scanning

### **Development**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#development)

- [ ]  Secure coding guidelines
- [ ]  Code reviews
- [ ]  Dependency scanning
- [ ]  SAST/DAST i CI/CD
- [ ]  Secrets ikke i kode

---

## **Compliance & Standards**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#compliance--standards)

### **GDPR (General Data Protection Regulation)**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#gdpr-general-data-protection-regulation)

- Privacy by design
- Data minimization
- Right to be forgotten
- Data breach notification

### **PCI-DSS (Payment Card Industry)**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#pci-dss-payment-card-industry)

- Kreditkort data beskyttelse
- Network security
- Access control
- Regular testing

### **ISO 27001**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#iso-27001)

- Information Security Management System
- Risk assessment
- Security controls

### **NIST Cybersecurity Framework**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#nist-cybersecurity-framework)

- Identify
- Protect
- Detect
- Respond
- Recover

---

## **Konklusion**

[](https://github.com/bofl2002/Obsidian/blob/main/Secure%20by%20design.md#konklusion)

**Secure by Design** handler om at:

- 🔒 Bygge sikkerhed ind fra starten
- 🛡️ Tænke som en angriber (threat modeling)
- 🔍 Validere alt input, aldrig stole på brugere
- 📊 Logge og monitorere kontinuerligt
- 🔄 Opdatere og patche regelmæssigt
- 👥 Træne udviklere i secure coding

**Husk:** Det er billigere og nemmere at bygge sikkert fra starten end at rette sikkerhedsproblemer senere!