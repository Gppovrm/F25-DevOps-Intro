# Lab 9 — Introduction to DevSecOps Tools
## Tasks

### Task 1 — Web Application Scanning with OWASP ZAP (5 pts)

#### 1.1: Start the Vulnerable Target Application

1. **Deploy Juice Shop (Intentionally Vulnerable Application):**

   ```bash
   docker run -d --name juice-shop -p 3000:3000 bkimminich/juice-shop
   ```

2. **Verify It's Running:**

   Open your browser and navigate to `http://localhost:3000`

#### 1.2: Scan with OWASP ZAP

1. **Run ZAP Baseline Scan:**

   <details>
   <summary>🐧 Linux Users - Network Configuration</summary>

   On Linux, Docker containers can't use `host.docker.internal`. Get your Docker bridge IP:

   ```bash
   ip -f inet -o addr show docker0 | awk '{print $4}' | cut -d '/' -f 1
   ```

   Then use that IP in the ZAP command below instead of `host.docker.internal`.

   </details>

   ```bash
   docker run --rm -u zap -v $(pwd):/zap/wrk:rw \
   -t ghcr.io/zaproxy/zaproxy:stable zap-baseline.py \
   -t http://host.docker.internal:3000 \
   -g gen.conf \
   -r zap-report.html
   ```

   > Mac/Windows users: Use `host.docker.internal` as shown above

#### 1.3: Analyze Results

1. **Open the Report:**

   - Find `zap-report.html` in your current directory
   - Open it in a browser

2. **Identify Vulnerabilities:**

   - Find at least 2 Medium risk vulnerabilities
   - Check security headers status (which headers are present/missing?)
   - Note the most interesting vulnerability found

#### 1.4: Clean Up

   ```bash
   docker stop juice-shop && docker rm juice-shop
   ```

In `labs/submission9.md`, document:
- Number of Medium risk vulnerabilities found
- Description of the 2 most interesting vulnerabilities
- Security headers status (which are present/missing and why they matter)
- Screenshot of ZAP HTML report overview
- Analysis: What type of vulnerabilities are most common in web applications?

---

### Task 2 — Container Vulnerability Scanning with Trivy (5 pts)

#### 2.1: Scan Container Image

1. **Run Trivy Scan:**

   ```bash
   docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
   aquasec/trivy:latest image \
   --severity HIGH,CRITICAL \
   bkimminich/juice-shop
   ```

   <details>
   <summary>🔍 Understanding Trivy flags</summary>

   - `--severity HIGH,CRITICAL`: Only show high and critical vulnerabilities
   - `-v /var/run/docker.sock`: Allows Trivy to access Docker images on your host
   - `image`: Scan a container image

   </details>

#### 2.2: Analyze Results

1. **Identify Key Findings:**

   From the scan output, identify:
   - Total number of CRITICAL vulnerabilities
   - Total number of HIGH vulnerabilities
   - At least 2 vulnerable package names
   - The most common vulnerability type (CVE category)

#### 2.3: Clean Up

   ```bash
   docker rmi bkimminich/juice-shop
   ```

In `labs/submission9.md`, document:
- Total count of CRITICAL and HIGH vulnerabilities
- List of 2 vulnerable packages with their CVE IDs
- Most common vulnerability type found
- Screenshot of Trivy terminal output showing critical findings
- Analysis: Why is container image scanning important before deploying to production?
- Reflection: How would you integrate these scans into a CI/CD pipeline?

---
