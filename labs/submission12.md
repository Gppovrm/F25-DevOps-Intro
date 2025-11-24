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

- TinyGo version used: 0.39.0
- WASM binary size (from `ls -lh main.wasm`): 2.4M  
![WASM Binary Size](https://github.com/user-attachments/assets/2a4dfe7f-644e-4ebb-89ab-e0b73cce10d1)
- WASI image size (from `ctr images ls`): 819.9 KiB  
![WASI Image Size](https://github.com/user-attachments/assets/2f93de60-47e4-4991-884a-73bd8ee4373d)
- Average startup time from the `ctr run` benchmark loop (CLI mode): 0.5260 seconds  
![Startup Benchmark](https://github.com/user-attachments/assets/de2098bd-b08e-4e1f-96a2-617c87edf715)
- Explanation of why server mode doesn't work under `ctr` (WASI Preview1 lacks socket support)
  
   The TinyGo net/http library cannot open network sockets in this environment.
  
- Note that server mode **can** be demonstrated via Spin using the same `main.wasm`: Yes, Spin provides HTTP server abstraction via WAGI executor
- Memory usage reporting: N/A - not available via ctr. WASM uses different memory management in wasmtime runtime.
- Note: used **same source code** as traditional build: Yes, same main.go file used for both builds

---
## Task 4 — Performance Comparison & Analysis (2 pts)

### 4.1 Performance Comparison Table

| Metric | Traditional Container | WASM Container | Improvement | Notes |
|--------|----------------------|----------------|-------------|-------|
| **Binary Size** | 4.5 MB | 2.4 MB | 47% smaller | From `ls -lh` |
| **Image Size** | 4.7 MB | 0.8 MB | 83% smaller | From `docker image inspect` and `ctr images ls` |
| **Startup Time (CLI)** | 394 ms | 526 ms | 0.75x slower | Average of 5 runs |
| **Memory Usage** | 1.14 MB | N/A | N/A | From `docker stats` |
| **Base Image** | scratch | scratch | Same | Both minimal |
| **Source Code** | main.go | main.go | Same | ✅ Same file! |
| **Server Mode** | ✅ Works (net/http) | ❌ Not via ctr <br> ✅ Via Spin (WAGI) | N/A | WASI Preview1 lacks sockets; <br> Spin provides HTTP abstraction |

**Improvement Calculations:**
- **Binary Size reduction:** ((4.5 - 2.4) / 4.5) × 100 = 47% smaller
- **Image Size reduction:** ((4.7 - 0.8) / 4.7) × 100 = 83% smaller  
- **Speed improvement:** 394 / 526 = 0.75x (WASM is slower in this test)
  
### 4.2 Analysis Questions

1. **Binary Size Comparison:**
   - Why is the WASM binary so much smaller than the traditional Go binary? 
   The WASM binary is significantly smaller (2.4MB vs 4.5MB) сuz TinyGo strips out the full Go runtime and only includes what's actually needed for WASM
   - What did TinyGo optimize away?
   TinyGo optimized away the garbage collector, unused standard library packages, and platform-specific system code

2. **Startup Performance:**
   - Why does WASM start faster?
   - What initialization overhead exists in traditional containers?
   In our test, WASM actually started slower (526ms vs 394ms) which surprised me. Normally it should be faster since it skips container setup and runs directly in the runtime. Traditional containers have to set up namespaces, networking, and filesystems which adds overhead.

3. **Use Case Decision Matrix:**
   - When would you choose WASM over traditional containers?
   Choose WASM for edge computing, serverless platforms, security-sensitive apps, and multi-platform deployment
   - When would you stick with traditional containers?
   Stick with traditional containers for full system access, complex networking, existing Docker tooling, and Kubernetes orchestration
