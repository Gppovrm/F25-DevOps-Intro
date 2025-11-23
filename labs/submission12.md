# Lab 12 — WebAssembly Containers vs Traditional Containers

## Task 1 — Create the Moscow Time Application (2 pts)

### CLI Mode Test
**Command:** `MODE=once go run main.go`  
![CLI Output](https://github.com/user-attachments/assets/a51f38bc-d3ad-404d-8592-7e3295d6a211)

### Server Mode Test  
**Command:** `go run main.go`  
![Server Browser](https://github.com/user-attachments/assets/86253e19-19f5-49ed-8fa6-9907d25aeca4)

### Explanation of how the single `main.go` works in three different contexts

The main.go file handles three different scenarios through environment detection. It first checks for the `MODE=once` environment variable - if present, it prints the current Moscow time as JSON and exits.

If not in CLI mode, it checks for CGI-style environment variables like `REQUEST_METHOD` that Spin sets up. When detected, it switches to WAGI mode and handles web requests by printing HTTP responses directly to stdout.

If neither special case applies, it falls back to being a normal Go web server using the standard `net/http` package, setting up routes and listening on port 8080.

The same binary handles all these situations without maintaining separate versions - the code automatically detects its environment and behaves accordingly.

---

## Task 2 — Build Traditional Docker Container (3 pts)

4. **Test CLI Mode:**

<img width="1008" height="117" alt="image" src="https://github.com/user-attachments/assets/9b18422f-b296-49d0-aa45-bc9f6c68fc08" />

5. **Test Server Mode (Optional):**
   
<img width="1564" height="717" alt="image" src="https://github.com/user-attachments/assets/59542924-8b72-45ca-a3c0-ab3edac5785c" />

#### 2.3: Measure Performance

1. **Check Binary Size:**

<img width="968" height="91" alt="image" src="https://github.com/user-attachments/assets/d8d9b024-42a8-43fd-81ed-3289fb348867" />

**Result:** 4.5M

2. **Check Image Size:**

<img width="741" height="82" alt="image" src="https://github.com/user-attachments/assets/49fdd22a-d838-465f-a851-466fbba4a9eb" />

**Results:**
- `docker images`: 4.7MB
- `docker image inspect`: 4.48047 MB

3. **Startup Time Benchmark (CLI Mode):**

**Result:** Average: 0.394 seconds

4. **Memory Usage (Server Mode):**

<img width="1022" height="78" alt="image" src="https://github.com/user-attachments/assets/c3107d68-e2da-4dd1-8416-101cf423219b" />

**Result:** 1.148MiB

---

## Task 3 — Build WASM Container (ctr-based) (3 pts)

### TinyGo Version
**Version:** 0.39.0

### WASM Binary Size
**Command:** `ls -lh main.wasm`  
**Result:** 2.4M  
![WASM Binary Size](https://github.com/user-attachments/assets/2a4dfe7f-644e-4ebb-89ab-e0b73cce10d1)

- WASI image size (from `ctr images ls`)
- Average startup time from the `ctr run` benchmark loop (CLI mode)
- Explanation of why server mode doesn't work under `ctr` (WASI Preview1 lacks socket support)
- Note that server mode **can** be demonstrated via Spin using the same `main.wasm`
- Memory usage reporting (likely "N/A" with explanation)
- Note: used **same source code** as traditional build
- Confirmation that you used `ctr` (containerd CLI) for WASM execution

---

## Task 4 — Performance Comparison & Analysis (2 pts)

In `labs/submission12.md`, document:
- Complete comparison table with all metrics
- Calculated improvement percentages
- Detailed answers to all questions
- Recommendations for when to use each approach

---

## Bonus Task — Deploy to Fermyon Spin Cloud (Extra Credit)
In your **bonus section** of `labs/submission12.md`, document:

- Public URL of your deployed application (`$SPIN_URL`)
- Deployment time from `spin deploy` command output
- **Cold start measurements:**
  - Calculated average cold start time
- **Warm measurements:**
  - Calculated average warm time
  - Comparison with cold start times
- **Local Spin measurements:**
  - Calculated average local time
  - Comparison with cloud deployment
- **Reflection:**
  - Would you use Spin for production workloads? Why or why not?
  - How does this compare to traditional serverless (AWS Lambda, Cloud Functions)?

---
