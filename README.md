# STRIDE vs ASVS

The purpose of this repository is to act as a bridge between the Threat Modeling and the security controls by providing an [equivalence table](#equivalence-table) between the [STRIDE](#stride-2) model and the [Application Security Verification Standard (ASVS)](#asvs-1) chapters.

## Pre-requisites and context

In the Security Requirement Engineering process, we commonly use techniques like Threat Modeling (along with frameworks such as STRIDE, LINDUNN or PASTA) to identify threats to which the system is exposed.
Once we have identified those threats, we need to mitigate them by defining security controls using industry standards like ASVS.

### Application Security Verification Standard (ASVS) [^1]

The [OWASP Application Security Verification Standard (ASVS)](https://owasp.org/www-project-application-security-verification-standard) Project provides a basis for testing web application technical security controls and also provides developers with a list of requirements for secure development.

The primary aim of the project is to normalize the range in the coverage and level of rigour available in the market when it comes to performing Web application security verification using a commercially-workable open standard. The standard provides a basis for testing application technical security controls, as well as any technical security controls in the environment, that are relied on to protect against vulnerabilities such as Cross-Site Scripting (XSS) and SQL injection. This standard can be used to establish a level of confidence in the security of Web applications. The requirements were developed with the following objectives in mind:

* **Use as a metric** - Provide application developers and application owners with a yardstick with which to assess the degree of trust that can be placed in their Web applications,
* **Use as guidance** - Provide guidance to security control developers as to what to build into security controls in order to satisfy application security requirements, and
* **Use during procurement** - Provide a basis for specifying application security verification requirements in contracts.

### STRIDE [^2]

STRIDE is a model for identifying computer security threats developed by Praerit Garg and Loren Kohnfelder at Microsoft. It provides a mnemonic for security threats in six categories.
It was initially created as part of the process of threat modelling. It is used in conjunction with a model of the target system that can be constructed in parallel. This includes a full breakdown of processes, data stores, data flows, and trust boundaries.
Today it is often used by security experts to help answer the question *"what can go wrong in this system we're working on?"*. 

Each threat is a violation of a desirable property for a system:

| Threat                   | Emoji | Description                                                                                                                                                                           | Desired property  |
|--------------------------|:-----:|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|
| **S**poofing                 | 🎭   | Accessing and use of another user’s credentials, such as username and password.                                                                                                       | Authenticity
| **T**ampering                | 🤡   | Maliciously change or modify persistent data, such as records in a database, and the alteration of data in transit between two computers over an open network, such as the Internet.  | Integrity
| **R**epudiation              | 📝   | Performing prohibited operations in a system that cannot trace the operations.                                                                                          | Trazability [^3]
| **I**nformation disclosure   | 🔓   | Read a file that one was not granted access to, or to read data in transit.                                                                                                           | Confidentiality
| **D**enial of Service        | 💥   | Deny access to valid users, such as by making a web server temporarily unavailable or unusable.                                                                                       | Availability
| **E**levation of Privilege   | 👑   | Gain privileged access to resources to gain unauthorized access to information or to compromise a system.                                                                    | Authorization

## Equivalence table

The following table has an equivalence between ASVS chapters and STRIDE threats (check [Table format](#table-format) and [References and Inspiration](#references-and-inspiration) for more context):

| ASVS Chapter                                   | Teams | STRIDE                                                                                       | References                                                       
|------------------------------------------------|----|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------------| 
| V1 - Architecture & Design                 | Design |                                                                      | - Mario Platt → Scoping and Design                              
| V2 - Authentication                        | Development | `🎭 S`                                                                         | - RTMP - A2 Broken Authentication → SRI<br>- Mario Platt → S  
| V3 - Session Management                    | Development | `🎭 S`                                                                         | - RTMP - A2 Broken Authentication → SRI<br>- Mario Platt → S  
| V4 - Access Control                        | Development | `🤡 T`<br>`👑 E`                                        | - RTMP - A5 Access Control → ERT<br>- Mario Platt → E<br>- ASVS pag. 36 → TE (mentions "tampering" explicitly)  
| V5 - Validation, Sanitization and Encoding | Development | `🤡 T`                                                                        | - RTMP - A1 Injection → TED<br>- Mario Platt → T  
| V6 - Stored Cryptography                   | Development, Infrastructure | `🔓 I`                                           | - ASVS Pag 42 → I |                                                                  
| V7 - Error Handling and Logging            | Development | `📝 R`<br>`🔓 I`                                      | - RTMP - A10 Insufficient Logging & Monitoring → TRI<br>- Mario Platt → RID<br>- ASVS pag. 44 → RI                                                                   
| V8 - Data Protection                       | Development | `🔓 I`                                                           | - Mario Platt → I |                                                                   
| V9 - Communication                         | Development, Infrastructure | `🔓 I`                                           | - Mario Platt → Scoping and Design | Related to *V1 - Architecture & Design*                                                                   
| V10 - Malicious Code                       | Development, Infrastructure |                                                 |                                                                      
| V11 - Business Logic [^4]                      | Development, Design |                                                          |                                                                     
| V12 - Files and Resources [^5]                  | Development |                                                               | - ASVS pag. 56 → DI (LFI might be considered as I)<br>- Mario Platt → Scoping and Design                                                               
| V13 - API and Web Service [^5]                 | Development |                                                                                                                               
| V14 - Configuration                        | Development, Infrastructure | `🔓 I`<br>`💥 D`<br>`👑 E` | - RTMP - A6 Security Misconfiguration → ESI<br>- RTMP - A9 Using Components with know Vuln→ ESD<br>- Mario Platt → IDE                                            

### Table format

* **ASVS Chapter**: the chapter of the ASVS standard to be mapped.
    - Check ASVS high-level structure at [How To Reference ASVS Requirements](https://owasp.org/www-project-application-security-verification-standard/).
* **Teams**: the teams that are usually involved in the implementation of the control. 
    - Bear in mind that ASVS aims for an holistic approach to promote cross-team collaboration. "Security is eveyone job!"
* **STRIDE**: threats applicable to that ASVS chapter (for more info check the [STRIDE](#stride) section).
    - Threats names are compressed by their first letter and corresponding emoji (i.e. `🤡 T` for Tampering).
    - There are some blank cells because some chapters that depend on the specific context of the project (i.e. *V12 - Files and Resources*) or are addressed in the design phase (i.e. *V1 - Architecture & Design*).
* **References**: the short code of the reference (more info at [References and inspiration](#references-and-inspiration) section).
* **Notes**: additional context or comments if applicable.

### References and inspiration

| Short name | Reference | Author | Notes |
|-----|-----|-----|-----|
| RTMP | [**HOWTO-Rapid Threat Model Prototyping** book ](https://github.com/geoffrey-hill-tutamantic/rapid-threat-model-prototyping-docs/blob/master/18x26.Tutamen%20HOWTO-Rapid%20Threat%20Model%20Prototyping.pdf) | Geoffrey Hill, Tunamatic | Buit on OWASP Top 10 2017
| Martio Platt | [**STRIDE-OT10-CWE-OPC-ASVS** excel](https://github.com/geoffrey-hill-tutamantic/rapid-threat-model-prototyping-docs/blob/master/19h20.mar.mapping%20table%20-%20STRIDE-OT10-CWE-OPC-ASVS.xlsx) | Mario Platt | Buit on OWASP Top 10 2017 and ASVS 3.x
| ASVS | [**Application Security Verification Standard v4.0.3**](https://github.com/OWASP/ASVS/tree/v4.0.3/4.0) | OWASP | N/A

## How to use

1. Understand the functional and technical requirements and their business context.
2. Use Threat Modeling with STRIDE to identify the threats.
    - My personal choice is [RTMP methodology](https://github.com/geoffrey-hill-tutamantic/rapid-threat-model-prototyping-docs), which is *agile-friendly*.
3. Use the [STRIDE vs ASVS table](#equivalence-table) to detect which ASVS chapter aims to mitigate each kind of threat.
4. Accommodate the requirements to the context of your problem (maybe some controls need modifications or not all of them are applicable).
5. Provide extra context using the [User Stories](https://en.wikipedia.org/wiki/User_story) format.
    - Check [project ASVS User Stories](https://github.com/OpenSecuritySummit/project-ASVS-User-Stories) for practical examples.
6. Try to automate security controls as much as possible. Level 1 is usually easy to automate.
    - Check [OWASP ASVS 4.0 testing guide](https://github.com/BlazingWind/OWASP-ASVS-4.0-testing-guide) for practical examples.
7. Track the completion of the security requirements to handle [residual risk](https://en.wikipedia.org/wiki/Residual_risk).

## TO-DO

 - [ ] Find aditional references to support the categorization.
 - [ ] Decompose the ASVS chapters into sections to be more specific.
 - [ ] Create a [SecurityRAT](https://owasp.org/www-project-securityrat/) requirement set using this format.


[^1]: Extracted from https://owasp.org/www-project-application-security-verification-standard/
[^2]: Extracted from https://en.wikipedia.org/wiki/STRIDE_(security)
[^3]: Commonly referenced as "non-repudiation", but IMHO it's self-referential and not very descriptive.
[^4]: Related to V1 - Architecture & Design
[^5]: Depends if the service has this feature.
