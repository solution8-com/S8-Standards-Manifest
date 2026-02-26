# Security Best Practices

## Overview

Security is not optional—it's a fundamental requirement for modern software development. This guide covers essential security practices to protect your applications, data, and users from evolving threats.

## OWASP Top 10 Prevention

### 1. Broken Access Control

**The Risk**: Users can access resources they shouldn't have permission to view or modify.

**Prevention Strategies**:

```javascript
// ❌ BAD: Direct object reference without authorization
app.get('/api/users/:id/profile', async (req, res) => {
  const profile = await db.getUserProfile(req.params.id);
  res.json(profile);
});

// ✅ GOOD: Verify user owns resource or has permission
app.get('/api/users/:id/profile', authenticateUser, async (req, res) => {
  if (req.user.id !== req.params.id && !req.user.isAdmin) {
    return res.status(403).json({ error: 'Forbidden' });
  }
  const profile = await db.getUserProfile(req.params.id);
  res.json(profile);
});
```

**Best Practices**:
- Deny by default; explicitly grant access
- Implement role-based access control (RBAC)
- Log access control failures
- Disable directory listing on web servers
- Invalidate JWT tokens on logout

### 2. Cryptographic Failures

**The Risk**: Sensitive data exposed due to weak or missing encryption.

**Prevention Strategies**:

```python
# ❌ BAD: Storing passwords in plain text
user.password = request.form['password']
db.save(user)

# ✅ GOOD: Hash passwords with strong algorithm
from werkzeug.security import generate_password_hash

user.password = generate_password_hash(
    request.form['password'],
    method='pbkdf2:sha256',
    salt_length=16
)
db.save(user)
```

**Best Practices**:
- Use TLS 1.3 or 1.2 for data in transit
- Encrypt sensitive data at rest (AES-256)
- Never store sensitive data unnecessarily
- Use secure key management systems (AWS KMS, Azure Key Vault)
- Disable caching for sensitive data

### 3. Injection

**The Risk**: Untrusted data sent to interpreters can execute unintended commands.

**SQL Injection Prevention**:

```java
// ❌ BAD: String concatenation
String query = "SELECT * FROM users WHERE username = '" + username + "'";
Statement stmt = connection.createStatement();
ResultSet rs = stmt.executeQuery(query);

// ✅ GOOD: Parameterized queries
String query = "SELECT * FROM users WHERE username = ?";
PreparedStatement stmt = connection.prepareStatement(query);
stmt.setString(1, username);
ResultSet rs = stmt.executeQuery();
```

**Command Injection Prevention**:

```javascript
// ❌ BAD: Executing user input
const { exec } = require('child_process');
exec(`ping -c 4 ${userInput}`);

// ✅ GOOD: Use libraries or validate input strictly
const { execFile } = require('child_process');
const ipRegex = /^(?:[0-9]{1,3}\.){3}[0-9]{1,3}$/;

if (ipRegex.test(userInput)) {
  execFile('ping', ['-c', '4', userInput]);
} else {
  throw new Error('Invalid IP address');
}
```

**Best Practices**:
- Use parameterized queries/prepared statements
- Employ ORMs with built-in protections
- Validate and sanitize all input
- Use allow-lists over deny-lists
- Implement Content Security Policy (CSP)

### 4. Insecure Design

**The Risk**: Missing or ineffective security controls in the design phase.

**Prevention Strategies**:

```typescript
// ✅ GOOD: Implement rate limiting
import rateLimit from 'express-rate-limit';

const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 5, // Limit each IP to 5 requests per windowMs
  message: 'Too many login attempts, please try again later',
  standardHeaders: true,
  legacyHeaders: false,
});

app.post('/api/login', loginLimiter, async (req, res) => {
  // Login logic
});
```

**Best Practices**:
- Conduct threat modeling during design
- Implement secure development lifecycle (SDL)
- Use security design patterns (least privilege, defense in depth)
- Separate business logic layers
- Plan for security failures gracefully

### 5. Security Misconfiguration

**The Risk**: Default configurations, incomplete setups, or verbose error messages.

**Prevention Strategies**:

```yaml
# ✅ GOOD: Secure HTTP headers configuration
# helmet.js middleware for Express
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      scriptSrc: ["'self'"],
      imgSrc: ["'self'", "data:", "https:"],
    },
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true,
    preload: true
  },
  frameguard: { action: 'deny' },
  noSniff: true,
  xssFilter: true
}));
```

**Best Practices**:
- Remove default accounts and passwords
- Disable unnecessary features and services
- Keep all software updated and patched
- Use security headers (CSP, HSTS, X-Frame-Options)
- Implement proper error handling (no stack traces in production)

### 6. Vulnerable and Outdated Components

**Prevention Strategies**:

```json
// package.json - Use npm audit
{
  "scripts": {
    "audit": "npm audit --audit-level=moderate",
    "audit:fix": "npm audit fix"
  }
}
```

**Best Practices**:
- Maintain software inventory (SBOMs)
- Monitor CVE databases and security advisories
- Only use official sources for components
- Prefer actively maintained libraries
- Automate dependency scanning in CI/CD

### 7. Identification and Authentication Failures

**Prevention Strategies**:

```python
# ✅ GOOD: Multi-factor authentication
from pyotp import TOTP

def verify_mfa(user, token):
    totp = TOTP(user.mfa_secret)
    if not totp.verify(token, valid_window=1):
        raise AuthenticationError('Invalid MFA token')
    return True

def login(username, password, mfa_token):
    user = authenticate_credentials(username, password)
    if user.mfa_enabled:
        verify_mfa(user, mfa_token)
    return create_session(user)
```

**Best Practices**:
- Implement multi-factor authentication (MFA)
- Use secure session management
- Implement account lockout after failed attempts
- Enforce strong password policies
- Never expose session identifiers in URLs
- Rotate session IDs after login

### 8. Software and Data Integrity Failures

**Prevention Strategies**:

```yaml
# ✅ GOOD: Verify package integrity
# Use package-lock.json or yarn.lock
# Verify checksums and signatures

# .npmrc
audit=true
audit-level=moderate
package-lock=true
```

**Best Practices**:
- Use code signing for releases
- Verify checksums and signatures
- Implement CI/CD pipeline security
- Review third-party code before integration
- Use Subresource Integrity (SRI) for CDN resources

### 9. Security Logging and Monitoring Failures

**Prevention Strategies**:

```javascript
// ✅ GOOD: Comprehensive security logging
const winston = require('winston');

const securityLogger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'security.log' })
  ]
});

function logSecurityEvent(event, user, details) {
  securityLogger.info({
    timestamp: new Date().toISOString(),
    event,
    user: user?.id,
    ip: details.ip,
    userAgent: details.userAgent,
    success: details.success
  });
}

// Usage
app.post('/api/login', (req, res) => {
  logSecurityEvent('LOGIN_ATTEMPT', null, {
    ip: req.ip,
    userAgent: req.get('user-agent'),
    success: false
  });
});
```

**Best Practices**:
- Log all authentication events
- Log access control failures
- Protect logs from tampering
- Implement real-time monitoring and alerts
- Establish incident response procedures

### 10. Server-Side Request Forgery (SSRF)

**Prevention Strategies**:

```javascript
// ✅ GOOD: Validate and sanitize URLs
const { URL } = require('url');

function isAllowedUrl(urlString) {
  try {
    const url = new URL(urlString);
    
    // Block private IP ranges
    const hostname = url.hostname;
    if (
      hostname === 'localhost' ||
      hostname.startsWith('127.') ||
      hostname.startsWith('192.168.') ||
      hostname.startsWith('10.') ||
      /^172\.(1[6-9]|2\d|3[0-1])\./.test(hostname)
    ) {
      return false;
    }
    
    // Allow-list approach
    const allowedDomains = ['api.example.com', 'cdn.example.com'];
    return allowedDomains.includes(hostname);
  } catch {
    return false;
  }
}

app.post('/api/fetch', async (req, res) => {
  if (!isAllowedUrl(req.body.url)) {
    return res.status(400).json({ error: 'Invalid URL' });
  }
  // Proceed with request
});
```

## Secure Coding Practices

### Input Validation

```typescript
// ✅ Comprehensive input validation
import { z } from 'zod';

const userSchema = z.object({
  email: z.string().email().max(255),
  age: z.number().int().min(13).max(120),
  username: z.string().min(3).max(30).regex(/^[a-zA-Z0-9_]+$/),
  bio: z.string().max(500).optional()
});

app.post('/api/users', (req, res) => {
  try {
    const validatedData = userSchema.parse(req.body);
    // Process validated data
  } catch (error) {
    return res.status(400).json({ error: error.errors });
  }
});
```

### Output Encoding

```javascript
// ✅ Prevent XSS with proper encoding
const escapeHtml = (unsafe) => {
  return unsafe
    .replace(/&/g, "&amp;")
    .replace(/</g, "&lt;")
    .replace(/>/g, "&gt;")
    .replace(/"/g, "&quot;")
    .replace(/'/g, "&#039;");
};

// Use templating engines with auto-escaping
// React, Vue, Angular automatically escape by default
```

### Secure Random Generation

```python
# ✅ GOOD: Use cryptographically secure random
import secrets

# Generate secure token
token = secrets.token_urlsafe(32)

# Generate secure random number
secure_number = secrets.randbelow(1000)

# ❌ BAD: Don't use for security
import random
token = str(random.randint(1000000, 9999999))  # Predictable!
```

## Authentication and Authorization

### Password Management

```javascript
// ✅ GOOD: Secure password handling
const bcrypt = require('bcrypt');
const SALT_ROUNDS = 12;

async function hashPassword(password) {
  // Validate password strength first
  if (password.length < 12) {
    throw new Error('Password must be at least 12 characters');
  }
  return await bcrypt.hash(password, SALT_ROUNDS);
}

async function verifyPassword(password, hash) {
  return await bcrypt.compare(password, hash);
}
```

### JWT Security

```javascript
// ✅ GOOD: Secure JWT implementation
const jwt = require('jsonwebtoken');

function createToken(user) {
  return jwt.sign(
    { 
      id: user.id,
      role: user.role 
    },
    process.env.JWT_SECRET,
    { 
      expiresIn: '15m',
      issuer: 'yourapp.com',
      audience: 'yourapp.com'
    }
  );
}

function verifyToken(token) {
  try {
    return jwt.verify(token, process.env.JWT_SECRET, {
      issuer: 'yourapp.com',
      audience: 'yourapp.com'
    });
  } catch (error) {
    throw new Error('Invalid token');
  }
}

// Implement refresh tokens for longer sessions
```

### OAuth 2.0 / OpenID Connect

```typescript
// ✅ GOOD: Use state parameter to prevent CSRF
import crypto from 'crypto';

app.get('/auth/oauth/login', (req, res) => {
  const state = crypto.randomBytes(32).toString('hex');
  req.session.oauthState = state;
  
  const authUrl = `https://oauth.provider.com/authorize?` +
    `client_id=${CLIENT_ID}` +
    `&redirect_uri=${REDIRECT_URI}` +
    `&state=${state}` +
    `&response_type=code` +
    `&scope=openid profile email`;
    
  res.redirect(authUrl);
});

app.get('/auth/oauth/callback', (req, res) => {
  if (req.query.state !== req.session.oauthState) {
    return res.status(403).send('Invalid state parameter');
  }
  // Exchange code for token
});
```

## Data Encryption

### Encryption at Rest

```python
# ✅ GOOD: Encrypt sensitive data
from cryptography.fernet import Fernet

class EncryptionService:
    def __init__(self, key):
        self.cipher = Fernet(key)
    
    def encrypt(self, data: str) -> bytes:
        return self.cipher.encrypt(data.encode())
    
    def decrypt(self, encrypted_data: bytes) -> str:
        return self.cipher.decrypt(encrypted_data).decode()

# Usage
encryption_service = EncryptionService(os.environ['ENCRYPTION_KEY'])
encrypted_ssn = encryption_service.encrypt(user.ssn)
```

### Encryption in Transit

```nginx
# ✅ GOOD: Nginx TLS configuration
server {
    listen 443 ssl http2;
    server_name example.com;

    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;
    
    # Use strong protocols only
    ssl_protocols TLSv1.2 TLSv1.3;
    
    # Strong cipher suites
    ssl_ciphers 'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256';
    ssl_prefer_server_ciphers off;
    
    # Enable HSTS
    add_header Strict-Transport-Security "max-age=63072000" always;
}
```

## Security Testing

### Static Application Security Testing (SAST)

```yaml
# .github/workflows/security.yml
name: Security Scan

on: [push, pull_request]

jobs:
  sast:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Run Semgrep
        uses: returntocorp/semgrep-action@v1
        with:
          config: >-
            p/security-audit
            p/owasp-top-ten
      
      - name: Run npm audit
        run: npm audit --audit-level=moderate
      
      - name: Run Snyk
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
```

### Dynamic Application Security Testing (DAST)

```bash
# ZAP baseline scan
docker run -v $(pwd):/zap/wrk/:rw \
  -t owasp/zap2docker-stable \
  zap-baseline.py \
  -t https://example.com \
  -r security-report.html
```

### Penetration Testing

**Regular Testing Schedule**:
- Automated scans: Daily/Weekly
- Manual penetration tests: Quarterly
- Third-party audits: Annually

**Testing Checklist**:
- [ ] Authentication and session management
- [ ] Authorization and access controls
- [ ] Input validation
- [ ] Cryptography implementation
- [ ] Business logic flaws
- [ ] API security

## Incident Response

### Incident Response Plan

```markdown
1. **Preparation**
   - Maintain security contacts list
   - Document escalation procedures
   - Prepare communication templates

2. **Detection and Analysis**
   - Monitor security alerts
   - Investigate anomalies
   - Determine severity and scope

3. **Containment**
   - Isolate affected systems
   - Preserve evidence
   - Implement temporary fixes

4. **Eradication**
   - Remove threat from environment
   - Patch vulnerabilities
   - Update security controls

5. **Recovery**
   - Restore systems from clean backups
   - Verify system integrity
   - Monitor for reinfection

6. **Post-Incident**
   - Document lessons learned
   - Update security measures
   - Conduct team retrospective
```

### Security Incident Template

```yaml
Incident ID: INC-2024-001
Severity: [Critical/High/Medium/Low]
Status: [Detected/Contained/Resolved]

Timeline:
  - 2024-01-15 14:30 UTC: Detected unusual database access
  - 2024-01-15 14:35 UTC: Isolated affected server
  - 2024-01-15 15:00 UTC: Confirmed unauthorized access

Impact:
  - Affected Systems: Production database server
  - Data Exposure: Customer email addresses (10,000 records)
  - Downtime: 2 hours

Actions Taken:
  - Revoked compromised credentials
  - Applied security patches
  - Enhanced monitoring

Preventive Measures:
  - Implement MFA for database access
  - Add rate limiting to admin endpoints
  - Schedule security training
```

## Security Checklist

### Development
- [ ] Input validation on all user inputs
- [ ] Output encoding to prevent XSS
- [ ] Parameterized queries to prevent SQL injection
- [ ] Secure password hashing (bcrypt, Argon2)
- [ ] Proper error handling (no sensitive data in errors)
- [ ] Security headers configured
- [ ] CSRF protection enabled
- [ ] Rate limiting implemented

### Deployment
- [ ] Secrets managed securely (not in code)
- [ ] TLS/SSL certificates valid and up-to-date
- [ ] Security scanning in CI/CD pipeline
- [ ] Dependency vulnerabilities checked
- [ ] Least privilege access configured
- [ ] Logging and monitoring enabled
- [ ] Backup and recovery tested

### Ongoing
- [ ] Regular security updates
- [ ] Periodic security audits
- [ ] Incident response plan tested
- [ ] Team security training conducted
- [ ] Compliance requirements met

## Additional Resources

- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [CWE Top 25 Most Dangerous Software Weaknesses](https://cwe.mitre.org/top25/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [SANS Security Resources](https://www.sans.org/security-resources/)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)

---

**Remember**: Security is an ongoing process, not a one-time task. Stay informed about emerging threats and continuously improve your security posture.
