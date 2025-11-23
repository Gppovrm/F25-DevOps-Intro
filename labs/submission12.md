# Lab 12 — WebAssembly Containers vs Traditional Containers

---

## Tasks

### Task 1 — Create the Moscow Time Application (2 pts)

In `labs/submission12.md`, document:
- Screenshot of CLI mode output (`MODE=once`)
- Screenshot of server mode running in browser (if tested)
- Confirmation that you're working directly in `labs/lab12/` directory
- Explanation of how the single `main.go` works in three different contexts

---

### Task 2 — Build Traditional Docker Container (3 pts)

In `labs/submission12.md`, document:
- Binary size from `ls -lh moscow-time-traditional`
- Image size from both `docker images` and `docker image inspect`
- Average startup time across 5 CLI mode runs
- Memory usage from `docker stats` (MEM USAGE column)
- Screenshot of application running in browser (server mode)

---

### Task 3 — Build WASM Container (ctr-based) (3 pts)

In `labs/submission12.md`, document:
- TinyGo version used
- WASM binary size (from `ls -lh main.wasm`)
- WASI image size (from `ctr images ls`)
- Average startup time from the `ctr run` benchmark loop (CLI mode)
- Explanation of why server mode doesn't work under `ctr` (WASI Preview1 lacks socket support)
- Note that server mode **can** be demonstrated via Spin using the same `main.wasm`
- Memory usage reporting (likely "N/A" with explanation)
- Note: used **same source code** as traditional build
- Confirmation that you used `ctr` (containerd CLI) for WASM execution

---

### Task 4 — Performance Comparison & Analysis (2 pts)

In `labs/submission12.md`, document:
- Complete comparison table with all metrics
- Calculated improvement percentages
- Detailed answers to all questions
- Recommendations for when to use each approach

---

### Bonus Task — Deploy to Fermyon Spin Cloud (Extra Credit)
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
