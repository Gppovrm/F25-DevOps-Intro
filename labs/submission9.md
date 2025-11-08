# Lab 9 — Introduction to DevSecOps Tools

## Tasks

### Task 1 — Web Application Scanning with OWASP ZAP (5 pts)

#### 1.1: Start the Vulnerable Target Application

1. **Deploy Juice Shop (Intentionally Vulnerable Application):**

   ```bash
   docker run -d --name juice-shop -p 3000:3000 bkimminich/juice-shop
   ```

2. **Verify It's Running:**

   `http://localhost:3000`

   <img width="1851" height="887" alt="image" src="https://github.com/user-attachments/assets/9de7dd7a-5b7d-46b3-beae-c2caec83991d" />

#### 1.2: Scan with OWASP ZAP

1. **Run ZAP Baseline Scan:**

   Get your Docker bridge IP:

   ```bash
   ip -f inet -o addr show docker0 | awk '{print $4}' | cut -d '/' -f 1
   ```

   IP found: **172.17.0.1**

   ```bash
   docker run --rm -u zap -v $(pwd):/zap/wrk:rw \
   -t ghcr.io/zaproxy/zaproxy:stable zap-baseline.py \
   -t http://172.17.0.1:3000 \
   -g gen.conf \
   -r zap-report.html
   ```

#### 1.3: Analyze Results

1. **Open the Report:**

- **Screenshot of ZAP HTML report overview:**

  <img width="1402" height="876" alt="image" src="https://github.com/user-attachments/assets/eb19717e-13d6-443a-af67-eda3e9b585dd" />

2. **Identify Vulnerabilities:**

- **Number of Medium Risk Vulnerabilities Found: 2**

- **Description of the 2 most interesting vulnerabilities:**
  
   **Content Security Policy (CSP) Header Not Set** (Medium Risk, 11 Instances)
  
   - **Description**: `Content Security Policy (CSP)` is an added layer of security that helps to detect and mitigate certain types of attacks, including `Cross Site Scripting (XSS)` and data injection attacks. These attacks are used for everything from data theft to site defacement or distribution of malware. `CSP` provides a set of standard HTTP headers that allow website owners to declare approved sources of content that browsers should be allowed to load on that page — covered types are JavaScript, CSS, HTML frames, fonts, images and embeddable objects such as Java applets, ActiveX, audio and video files.

   **Cross-Domain Misconfiguration** (Medium Risk, 11 Instances)
  
   - **Description**: Web browser data loading may be possible, due to a `Cross Origin Resource Sharing (CORS)` misconfiguration on the web server..

- **Security headers status (which are present/missing and why they matter):**

   **Present Headers:**
   - **`Access-Control-Allow-Origin: *`** - Present but misconfigured (overly permissive)
   - **`X-Content-Type-Options: nosniff`** - Prevents MIME sniffing attacks
   - **`X-Frame-Options: SAMEORIGIN`** - Prevents clickjacking
   - **`Feature-Policy`** - Present but should be updated to modern Permissions-Policy
   
   **Missing Critical Headers:**
   - **`Content-Security-Policy (CSP)`** -  (Medium risk vulnerability) - Critical for preventing XSS and data injection attacks
   - **`Strict-Transport-Security (HSTS)`** - Header (HSTS) lets a website tell browsers that it should only be accessed using HTTPS instead of HTTP. vulnerable to MITM attacks
   - **`Cross-Origin-Embedder-Policy (COEP)`** - (Low risk) - Enables cross-origin isolation and protects against Spectre attacks
   - **`Cross-Origin-Opener-Policy (COOP) header`** - (Low risk) - Prevents cross-site attacks by isolating top-level documents

  **Analysis**: HTTP Security Headers are important for protecting websites and their users. They help prevent various types of code injection attacks such as Cross-Site Scripting (XSS), clickjacking, and other vulnerabilities. By setting these headers on a website, they instruct the browser on how to handle certain security aspects, reducing the risk of data breaches and enhancing user trust.

- **Analysis: What type of vulnerabilities are most common in web applications?**
  
  Based on the ZAP scan results, the most prevalent vulnerabilities in web applications stem from security misconfigurations—specifically missing or improperly configured security headers. Issues like absent Content Security Policy and overly permissive CORS settings create openings for cross-site scripting and data exposure.
  
---

### Task 2 — Container Vulnerability Scanning with Trivy (5 pts)

#### 2.1: Scan Container Image

1. **Screenshot of Run Trivy Scan terminal output showing critical findings**
   
   <img width="1188" height="820" alt="image" src="https://github.com/user-attachments/assets/8740efc0-7a87-46fe-9987-d81744f25984" />

#### 2.2: Analyze Results

1. **Identify Key Findings from the scan output:**

   - Total number of CRITICAL vulnerabilities: 8 CRITICAL
     
   - Total number of HIGH vulnerabilities: 22 Node.js package highs, 1 debian high, 2 secrets
     
   - At least 2 vulnerable package names with their CVE IDs:

      `crypto-js (package.json)`            │ CVE-2023-46233
     
      `jsonwebtoken (package.json)`         │ CVE-2015-9235      │ CVE-2015-9235
        
      `lodash (package.json)`               │ CVE-2019-10744
     
      `marsdb (package.json)`               │ GHSA-5mrr-rgp6-x4gr
     
      `vm2 (package.json)`                  │ CVE-2023-32314     │ CVE-2023-37466 │ CVE-2023-37903

      <img width="1573" height="420" alt="image" src="https://github.com/user-attachments/assets/8013092e-2641-4634-b5a0-8df9d17098e5" />
      <img width="1570" height="258" alt="image" src="https://github.com/user-attachments/assets/3e7314ea-ec66-457c-bd38-004141aca60b" />
      <img width="1572" height="82" alt="image" src="https://github.com/user-attachments/assets/4a626b92-3e94-48e6-a538-4eb32dd5f3ce" />
      <img width="1573" height="61" alt="image" src="https://github.com/user-attachments/assets/4b4470b8-64e6-43f8-aced-57a5a866890c" />
      <img width="1573" height="193" alt="image" src="https://github.com/user-attachments/assets/be571123-8937-4cc3-afde-f8c7163c2ff4" />

   - The most common vulnerability type found(CVE category):
      
      The most common vulnerability types are **prototype pollution** and **authentication bypass** in Node.js libraries like `lodash` and `jsonwebtoken`
   
   - **Analysis: Why is container image scanning important before deploying to production?**
     
     Container image scanning is essential before production because it detects known vulnerabilities (CVEs) in dependencies and system libraries, allowing teams to patch security issues before attackers can exploit them.
     
   - **Reflection: How would you integrate these scans into a CI/CD pipeline?**

     I would integrate security scanning into the CI/CD pipeline by adding automated quality gates that block deployments on CRITICAL vulnerabilities, require manual approval for HIGH severity issues, run ZAP scans in staging environments, and implement secret detection to prevent hardcoded credentials from reaching production.
