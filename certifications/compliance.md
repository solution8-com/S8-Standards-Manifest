# Compliance Frameworks and Regulations

## Overview

Compliance frameworks and regulations establish legal and industry requirements for data protection, privacy, and security. Organizations must understand and adhere to applicable regulations to avoid legal penalties, protect customer data, and maintain trust.

## GDPR (General Data Protection Regulation)

### Overview

**Effective**: May 25, 2018  
**Jurisdiction**: European Union and EEA  
**Scope**: Organizations processing EU residents' personal data  
**Maximum Fine**: €20 million or 4% of annual global turnover (whichever is higher)

### Key Principles

#### 1. Lawfulness, Fairness, and Transparency

**Requirements**:
```
- Clear legal basis for processing
- Transparent privacy notices
- No deceptive practices
- Plain language communications
```

**Legal Bases for Processing**:
1. **Consent**: Freely given, specific, informed
2. **Contract**: Necessary for contract performance
3. **Legal Obligation**: Required by law
4. **Vital Interests**: Protecting life
5. **Public Task**: Official authority or public interest
6. **Legitimate Interests**: Balanced against data subject rights

#### 2. Purpose Limitation

```
- Collect for specified purposes only
- Use only for stated purposes
- Compatible use allowed
- Document purposes clearly
```

#### 3. Data Minimization

```
- Collect only necessary data
- Avoid excessive collection
- Regular data reviews
- Delete unnecessary data
```

#### 4. Accuracy

```
- Keep data accurate and up-to-date
- Correct inaccuracies promptly
- Implement update processes
- Enable user corrections
```

#### 5. Storage Limitation

```
- Define retention periods
- Delete data when no longer needed
- Document retention rationale
- Implement automated deletion
```

#### 6. Integrity and Confidentiality

```
- Appropriate security measures
- Protection against unauthorized access
- Protection against accidental loss
- Regular security assessments
```

#### 7. Accountability

```
- Demonstrate compliance
- Maintain records
- Implement policies
- Conduct DPIAs
```

### Data Subject Rights

#### Right of Access (Article 15)

**Requirements**:
```
- Provide copy of personal data
- Information about processing
- Respond within 1 month
- First copy free
```

**Response Template**:
```
Subject Access Request Response:
1. Confirmation of processing
2. Categories of data
3. Purposes of processing
4. Recipients of data
5. Retention period
6. Rights information
7. Copy of personal data
```

#### Right to Rectification (Article 16)

```
- Correct inaccurate data
- Complete incomplete data
- Notify recipients
- 1-month response time
```

#### Right to Erasure / "Right to be Forgotten" (Article 17)

**Conditions**:
```
- Data no longer necessary
- Consent withdrawn
- Objection to processing
- Unlawfully processed
- Legal obligation to delete
```

**Exceptions**:
```
- Freedom of expression
- Legal compliance
- Public health
- Archiving/research
- Legal claims
```

#### Right to Data Portability (Article 20)

```
- Provide in machine-readable format
- Transmit to another controller
- Applies to automated processing
- Based on consent or contract
```

#### Right to Object (Article 21)

```
- Object to processing
- Particularly for direct marketing
- Stop processing unless compelling grounds
- Absolute right for marketing
```

#### Rights Related to Automated Decision Making (Article 22)

```
- Right not to be subject to automated decisions
- Right to human intervention
- Right to contest decision
- Exception: necessary for contract
```

### GDPR Compliance Requirements

#### 1. Data Protection Officer (DPO)

**When Required**:
```
- Public authorities
- Large-scale systematic monitoring
- Large-scale special category data processing
```

**Responsibilities**:
```
- Advise on GDPR compliance
- Monitor compliance
- Conduct DPIAs
- Cooperate with supervisory authority
- Act as contact point
```

#### 2. Data Protection Impact Assessment (DPIA)

**When Required**:
```
- High risk to data subjects
- Large-scale profiling
- Special category data at scale
- Systematic monitoring of public areas
- New technologies
```

**DPIA Process**:
```
1. Describe processing
2. Assess necessity and proportionality
3. Identify risks to rights and freedoms
4. Measures to address risks
5. Consult DPO
6. Document outcomes
```

**DPIA Template**:
```markdown
## Processing Description
- Nature, scope, context, purposes
- Data types and categories
- Recipients and transfers

## Necessity and Proportionality
- Lawful basis
- Legitimate interests
- Alternatives considered

## Risk Assessment
| Risk | Impact | Likelihood | Severity | Mitigation |
|------|--------|------------|----------|------------|
| ... | ... | ... | ... | ... |

## Consultation
- DPO opinion
- Data subject views
- Expert advice

## Approved By
- Name, role, date
```

#### 3. Records of Processing Activities (Article 30)

**Required Information**:
```
- Controller details
- Processing purposes
- Data subject categories
- Personal data categories
- Recipient categories
- International transfers
- Retention periods
- Security measures
```

**Template**:
```
Processing Activity: Customer Relationship Management
Purpose: Sales and support
Legal Basis: Contract, Consent
Data Categories: Contact info, purchase history
Data Subjects: Customers, prospects
Recipients: CRM provider, email service
Transfers: USA (Standard Contractual Clauses)
Retention: 7 years after last contact
Security: Encryption, access controls
```

#### 4. Breach Notification

**72-Hour Rule** (Article 33):
```
- Notify supervisory authority within 72 hours
- Document all breaches
- Assess breach severity
- Report high-risk breaches to subjects
```

**Breach Notification Template**:
```
TO: Supervisory Authority
RE: Personal Data Breach Notification

1. Breach Description
   - Nature of breach
   - Date/time of breach
   - Categories affected
   - Number of records

2. Data Protection Officer Contact
   - Name, email, phone

3. Likely Consequences
   - Risk assessment
   - Potential impacts

4. Measures Taken
   - Containment actions
   - Mitigation steps
   - Remediation plan

5. Timeline
   - Discovery date
   - Containment date
   - Notification date
```

#### 5. Privacy by Design and Default

**Privacy by Design**:
```
- Integrate privacy from start
- Minimize data collection
- Secure by default
- User-friendly privacy
- End-to-end security
```

**Privacy by Default**:
```
- Strictest privacy settings by default
- No action needed for basic privacy
- Minimal data processing by default
- Limit retention by default
```

### International Data Transfers

#### Transfer Mechanisms

**1. Adequacy Decision**
```
Countries: UK, Switzerland, Japan, etc.
Benefit: No additional safeguards needed
```

**2. Standard Contractual Clauses (SCCs)**
```
- EU Commission approved clauses
- Contractual obligations
- Supplementary measures if needed
- Transfer Impact Assessment required
```

**3. Binding Corporate Rules (BCRs)**
```
- For multinational companies
- Internal data transfers
- Requires authority approval
- Comprehensive commitments
```

**4. Derogations**
```
- Explicit consent
- Contract performance
- Legal claims
- Vital interests
- Public interest
```

#### Transfer Impact Assessment (TIA)

**Process**:
```
1. Map data transfers
2. Identify transfer mechanism
3. Assess recipient country laws
4. Evaluate practical safeguards
5. Document supplementary measures
6. Ongoing monitoring
```

### GDPR Compliance Checklist

```markdown
## Lawful Basis
- [ ] Identified legal basis for each processing activity
- [ ] Documented in privacy policy
- [ ] Obtained valid consent where applicable
- [ ] Consent withdrawal mechanism in place

## Transparency
- [ ] Privacy notice published
- [ ] Privacy policy updated
- [ ] Cookie notice implemented
- [ ] Clear, plain language used

## Data Subject Rights
- [ ] Process for access requests
- [ ] Rectification procedure
- [ ] Erasure process
- [ ] Data portability mechanism
- [ ] Objection handling
- [ ] Automated decision-making controls

## Data Protection Officer
- [ ] DPO appointed (if required)
- [ ] DPO details published
- [ ] DPO involved in decisions
- [ ] DPO resources allocated

## Data Protection Impact Assessments
- [ ] DPIA process established
- [ ] High-risk processing identified
- [ ] DPIAs conducted
- [ ] DPIAs documented

## Records
- [ ] Processing activities documented
- [ ] Records of processing maintained
- [ ] Regular reviews scheduled

## Breach Management
- [ ] Breach response plan
- [ ] Breach register maintained
- [ ] 72-hour notification process
- [ ] Breach investigation procedures

## Processors and Third Parties
- [ ] Processor agreements in place
- [ ] Article 28 requirements met
- [ ] Processor security assessed
- [ ] Sub-processor approval process

## International Transfers
- [ ] Transfers mapped
- [ ] Transfer mechanisms identified
- [ ] SCCs implemented
- [ ] Transfer impact assessments

## Security
- [ ] Appropriate technical measures
- [ ] Appropriate organizational measures
- [ ] Encryption implemented
- [ ] Access controls
- [ ] Regular security testing
```

## SOC 2 (Service Organization Control 2)

### Overview

**Purpose**: Audit framework for service providers  
**Focus**: Security, availability, processing integrity, confidentiality, privacy  
**Audience**: Customers, prospects, regulators  
**Report Types**: Type I (design) and Type II (operating effectiveness)

### Trust Service Criteria

#### 1. Security (Required)

**Principle**: Information and systems are protected against unauthorized access

**Common Criteria**:
```
CC1: Control Environment
- Integrity and ethical values
- Oversight responsibility
- Organizational structure
- Competence
- Accountability

CC2: Communication and Information
- Quality of information
- Internal communication
- External communication

CC3: Risk Assessment
- Objectives specification
- Risk identification
- Risk assessment
- Fraud consideration

CC4: Monitoring Activities
- Ongoing/separate evaluations
- Deficiency evaluation

CC5: Control Activities
- Control selection and development
- Technology controls
- Policies and procedures deployment

CC6: Logical and Physical Access Controls
- Access authorization
- Access provisioning
- Authentication
- Access removal
- Security monitoring

CC7: System Operations
- Change management
- Incident management
- Configuration management
- System monitoring

CC8: Change Management
- Objectives identification
- Impact assessment
- Implementation control

CC9: Risk Mitigation
- Vendor management
- Business continuity
- Data backup
```

#### 2. Availability (Optional)

**Principle**: System is available for operation and use as committed

**Criteria**:
```
A1: Availability Commitments
- SLA definition
- Performance targets
- Capacity planning

A2: Availability Monitoring
- Performance monitoring
- Incident tracking
- Capacity monitoring

A3: Availability Incidents
- Incident response
- Recovery procedures
- Communication protocols
```

#### 3. Processing Integrity (Optional)

**Principle**: System processing is complete, valid, accurate, timely, authorized

**Criteria**:
```
PI1: Processing Integrity Commitments
- Data accuracy requirements
- Processing completeness
- Timeliness requirements

PI2: Processing Controls
- Input validation
- Processing controls
- Output review
- Error handling
```

#### 4. Confidentiality (Optional)

**Principle**: Confidential information is protected as committed

**Criteria**:
```
C1: Confidentiality Commitments
- Confidentiality agreements
- Data classification
- Protection requirements

C2: Confidentiality Controls
- Encryption
- Access restrictions
- Secure disposal
- DLP controls
```

#### 5. Privacy (Optional)

**Principle**: Personal information is collected, used, retained, disclosed, disposed per commitments

**Criteria**:
```
P1: Notice and Communication
P2: Choice and Consent
P3: Collection
P4: Use, Retention, and Disposal
P5: Access
P6: Disclosure to Third Parties
P7: Security
P8: Data Integrity
P9: Monitoring and Enforcement
```

### SOC 2 Type I vs Type II

**Type I Report**:
```
Scope: Point-in-time assessment
Duration: Single day
Focus: Design of controls
Timeline: 2-4 weeks
Use Case: New services, initial assessment
```

**Type II Report**:
```
Scope: Operating effectiveness
Duration: 6-12 months minimum
Focus: Control operation over time
Timeline: 3-6 months
Use Case: Ongoing assurance, customer requirements
```

### SOC 2 Readiness Process

#### Phase 1: Preparation (Weeks 1-4)

```
- Define scope (systems and criteria)
- Select auditor
- Document system description
- Identify relevant controls
- Gap analysis
```

#### Phase 2: Remediation (Weeks 5-16)

```
- Implement missing controls
- Update policies and procedures
- Configure security tools
- Train personnel
- Document everything
```

#### Phase 3: Readiness Assessment (Weeks 17-20)

```
- Internal audit
- Control testing
- Evidence collection
- Remediate findings
- Final preparation
```

#### Phase 4: Audit (Weeks 21-28)

**Type II**: Observation period (6-12 months)

```
- Planning meeting
- System walkthrough
- Control testing
- Evidence review
- Interviews
- Findings discussion
- Report drafting
```

### SOC 2 Control Examples

**Access Control Example**:
```
Control: User access is reviewed quarterly

Evidence:
- Access review spreadsheet
- Manager approval emails
- Access removal tickets
- Review completion report

Testing:
- Select sample quarter
- Verify review performed
- Check completeness
- Validate approvals
- Confirm removals
```

**Change Management Example**:
```
Control: Production changes require approval and testing

Evidence:
- Change request tickets
- Approval records
- Test results
- Deployment logs
- Rollback procedures

Testing:
- Sample 25 production changes
- Verify approval obtained
- Confirm testing completed
- Check deployment process
- Review exceptions
```

**Backup Example**:
```
Control: Daily automated backups with weekly restoration testing

Evidence:
- Backup logs
- Restoration test results
- Monitoring alerts
- Backup policy
- Storage verification

Testing:
- Review backup logs for period
- Verify daily execution
- Check weekly restore tests
- Validate monitoring
- Assess backup success rate
```

## Industry-Specific Regulations

### HIPAA (Health Insurance Portability and Accountability Act)

**Scope**: Healthcare providers, health plans, healthcare clearinghouses  
**Protected Data**: Protected Health Information (PHI)

**Key Requirements**:
```
Privacy Rule:
- Individual rights to PHI
- Minimum necessary disclosure
- Notice of privacy practices
- Business associate agreements

Security Rule:
- Administrative safeguards
- Physical safeguards
- Technical safeguards
- Organizational requirements

Breach Notification Rule:
- 60-day notification to individuals
- Notify HHS
- Media notification (500+ individuals)
```

**Technical Safeguards**:
```
- Access control (unique user IDs, emergency access)
- Audit controls (log access to ePHI)
- Integrity controls (protect from alteration)
- Transmission security (encryption)
```

### PCI DSS (Payment Card Industry Data Security Standard)

**Scope**: Organizations handling credit card data  
**Versions**: PCI DSS 4.0 (current)

**12 Requirements**:
```
Build and Maintain Secure Network:
1. Install and maintain firewall configuration
2. Don't use vendor-supplied defaults

Protect Cardholder Data:
3. Protect stored cardholder data
4. Encrypt transmission of cardholder data

Maintain Vulnerability Management:
5. Protect systems against malware
6. Develop and maintain secure systems

Implement Strong Access Control:
7. Restrict access to cardholder data
8. Identify and authenticate access
9. Restrict physical access

Monitor and Test Networks:
10. Track and monitor all access
11. Regularly test security systems

Maintain Information Security Policy:
12. Maintain a policy addressing information security
```

**Validation Levels**:
```
Level 1: 6M+ transactions/year - Annual onsite audit
Level 2: 1-6M transactions/year - Annual SAQ
Level 3: 20K-1M e-commerce/year - Annual SAQ
Level 4: <20K e-commerce or <1M total - Annual SAQ
```

### FedRAMP (Federal Risk and Authorization Management Program)

**Scope**: Cloud services for US federal agencies  
**Purpose**: Standardized security assessment for cloud services

**Impact Levels**:
```
Low: Limited impact to operations, assets, individuals
Moderate: Serious impact
High: Severe or catastrophic impact
```

**Authorization Process**:
```
1. Package preparation (3-6 months)
2. Assessment by 3PAO (2-4 months)
3. Authorization review (3-6 months)
4. Ongoing monitoring
```

### CCPA (California Consumer Privacy Act)

**Effective**: January 1, 2020  
**Scope**: Businesses serving California residents

**Thresholds** (any one):
```
- $25M+ annual revenue
- 50K+ consumers/households/devices
- 50%+ revenue from selling personal information
```

**Consumer Rights**:
```
- Right to know what data is collected
- Right to know if data is sold/disclosed
- Right to opt-out of data sale
- Right to deletion
- Right to non-discrimination
- Right to correct inaccurate information (CPRA)
```

**Compliance Requirements**:
```
- Privacy policy disclosure
- "Do Not Sell My Personal Information" link
- 45-day response to requests
- Reasonable security measures
- Service provider agreements
- Data inventory and mapping
```

## Data Protection Practices

### Data Classification

**Classification Levels**:

```
Public:
- No harm if disclosed
- Marketing materials
- Public website content
- Press releases

Internal:
- Intended for internal use
- Business plans
- Organizational charts
- Internal communications

Confidential:
- Competitive harm if disclosed
- Customer data
- Financial information
- Strategic plans

Restricted:
- Severe harm if disclosed
- Trade secrets
- Personal data
- Payment card data
- Health information
```

**Handling Requirements by Classification**:

| Classification | Storage | Transmission | Access | Retention | Disposal |
|---------------|---------|--------------|--------|-----------|----------|
| Public | Standard | Any | Open | As needed | Standard delete |
| Internal | Secured | Encrypted | Authenticated | Per policy | Secure delete |
| Confidential | Encrypted | Encrypted & logged | MFA + authorization | Minimal | Certified destruction |
| Restricted | Encrypted + DLP | Encrypted + approval | MFA + strict authorization | Minimal | Certified destruction + audit |

### Data Lifecycle Management

**Lifecycle Stages**:

```
1. Creation/Collection
   - Data classification
   - Legal basis check
   - Privacy notice
   - Consent capture

2. Storage
   - Encryption at rest
   - Access controls
   - Geographic controls
   - Backup procedures

3. Use/Processing
   - Purpose limitation
   - Access logging
   - Data minimization
   - Quality controls

4. Sharing/Disclosure
   - Recipient validation
   - Transfer mechanisms
   - Data sharing agreements
   - Purpose alignment

5. Archival
   - Retention policy compliance
   - Reduced access
   - Integrity preservation
   - Regular review

6. Destruction
   - Secure deletion
   - Certificate of destruction
   - Deletion verification
   - Documentation
```

### Privacy Requirements

#### Privacy Notice Elements

```markdown
# Privacy Notice

## What We Collect
- Contact information (name, email, phone)
- Account information (username, password)
- Usage data (pages visited, features used)
- Device information (IP address, browser type)

## Why We Collect
- Provide services
- Improve user experience
- Communicate updates
- Comply with legal obligations

## Legal Basis
- Contract performance
- Legitimate interests
- Consent (where required)
- Legal compliance

## Who We Share With
- Service providers (hosting, analytics)
- Legal authorities (when required)
- Business partners (with consent)

## How Long We Keep Data
- Active accounts: Duration of relationship + 7 years
- Inactive accounts: 3 years, then deleted
- Marketing data: Until consent withdrawn

## Your Rights
- Access your data
- Correct inaccuracies
- Request deletion
- Object to processing
- Data portability
- Withdraw consent

## Contact
- Privacy Officer: privacy@example.com
- DPO: dpo@example.com (if applicable)

## Updates
Last updated: [Date]
We will notify you of material changes
```

#### Consent Management

**Valid Consent Requirements**:
```
- Freely given (no coercion)
- Specific (clear purpose)
- Informed (understand what agreeing to)
- Unambiguous (clear affirmative action)
- Withdrawable (easy to revoke)
- Documented (proof maintained)
```

**Consent Form Template**:
```
☐ I agree to receive marketing emails about products and services

You can withdraw consent at any time by:
- Clicking unsubscribe in emails
- Updating preferences at [URL]
- Contacting privacy@example.com

We will process your data according to our Privacy Notice.
Last updated: [Date]

[Accept] [Decline]
```

#### Privacy-Preserving Techniques

**Data Anonymization**:
```
- Remove direct identifiers
- Remove quasi-identifiers
- Add noise to data
- Aggregate data
- Verify re-identification risk
```

**Pseudonymization**:
```
- Replace identifiers with pseudonyms
- Store mapping separately
- Additional security for mapping
- Enable data utility
- Reversible for authorized purposes
```

**Data Masking**:
```
Production → Development:
- Email: user@example.com → ***@example.com
- Phone: +1-555-0123 → +1-555-XXXX
- SSN: 123-45-6789 → ***-**-****
- Credit Card: 4111-1111-1111-1111 → 4111-****-****-1111
```

## Compliance Program Structure

### Governance Model

```
Board of Directors
      ↓
Chief Privacy Officer / Data Protection Officer
      ↓
├── Privacy Team
│   ├── Privacy Engineers
│   ├── Privacy Analysts
│   └── Compliance Coordinators
├── Legal Team
│   └── Privacy Counsel
├── Security Team
│   └── Security Engineers
└── Business Units
    └── Privacy Champions
```

### Compliance Metrics

**KPIs to Track**:
```
Privacy Requests:
- Number of access requests
- Average response time
- Requests completed on time (%)
- Deletion requests fulfilled

Consent:
- Consent rate
- Withdrawal rate
- Consent refresh rate

Incidents:
- Data breaches
- Privacy incidents
- Time to detection
- Time to notification

Training:
- Completion rate
- Quiz scores
- Awareness level

Vendor Management:
- Processors with agreements (%)
- Vendor assessments completed
- High-risk vendors identified
```

## Resources

### Official Resources

- GDPR Official Text: https://gdpr.eu
- AICPA SOC 2: https://www.aicpa.org/soc
- PCI DSS: https://www.pcisecuritystandards.org
- HIPAA: https://www.hhs.gov/hipaa
- FedRAMP: https://www.fedramp.gov

### Tools

- Consent management platforms
- Privacy management software
- Data mapping tools
- Cookie consent tools
- DPIA templates

### Training

- IAPP Certifications (CIPP, CIPM, CIPT)
- SOC 2 training programs
- Industry-specific compliance training

---

*For related information, see [Security Standards](security.md), [ISO Certifications](iso.md), and [Audit Preparation](audit.md).*
