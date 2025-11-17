# Lab 11 — Decentralized Web Hosting with IPFS & 4EVERLAND

## Tasks

### Task 1 — Local IPFS Node Setup and File Publishing (5 pts)

### 1.1: Start IPFS Container

**Deploy IPFS Node:**
```bash
docker run -d --name ipfs_node \
  -v ipfs_staging:/export \
  -v ipfs_data:/data/ipfs \
  -p 4001:4001 -p 8080:8080 -p 5001:5001 \
  ipfs/kubo:latest
```

**Wait for Initialization:**
```bash
sleep 60
docker ps
```

**Output:**
```
ONTAINER ID   IMAGE              COMMAND                  CREATED          STATUS                    PORTS                                                                                                                                                       NAMES
a3b183e1351c   ipfs/kubo:latest   "/sbin/tini -- /usr/…"   22 seconds ago   Up 22 seconds (healthy)   0.0.0.0:4001->4001/tcp, [::]:4001->4001/tcp, 0.0.0.0:5001->5001/tcp, [::]:5001->5001/tcp, 4001/udp, 0.0.0.0:8080->8080/tcp, [::]:8080->8080/tcp, 8081/tcp   ipfs_node
```

### 1.2: Verify Node Operation

**Check Connected Peers:**
```bash
docker exec ipfs_node ipfs swarm peers
```

**Output:**
```
/ip4/102.207.250.124/udp/4001/quic-v1/p2p/12D3KooWLM9d11uRxHGJgujF5hTAmfzeHEebrCvS9DQx83z558Nv
/ip4/104.248.143.24/tcp/40299/p2p/QmXKoh9psm6bZViAvYxwfPDa2UH9woHTcbP4M1Q8NQv5Ht
... [multiple peers listed]
```

**Access IPFS Web UI:**

Web UI Status page showing peer count and bandwidth statistics:

<img width="909" height="880" alt="image" src="https://github.com/user-attachments/assets/4952de9e-024f-45d9-b864-8cb2d55ad094" />

**Web UI Data:**
- Connected peers: 387 peers
- Network bandwidth: Hosting 2 MiB of data
- Agent: kubov0.38.2 9fd105a/docker
- Peer ID: 12D3KooWEzbfNjpHW8eKu42raStRmohnJgrxmqe7EJ4Fv14wGpk3

### 1.3: Add File to IPFS

**Create Test File:**
```bash
echo "Hello IPFS Lab" > testfile.txt
docker cp testfile.txt ipfs_node:/export/
docker exec ipfs_node ls /export/
```

**Output:**
```
testfile.txt
```

**Add File to IPFS:**
```bash
docker exec ipfs_node ipfs add /export/testfile.txt
```

**Output:**
```
added QmUFJmQRosK4Amzcjwbip8kV3gkJ8jqCURjCNxuv3bWYS1 testfile.txt
 15 B / 15 B  100.00%
```

### 1.4: Access Content

**Via Local Gateway:**
URL: `http://localhost:8080/ipfs/QmUFJmQRosK4Amzcjwbip8kV3gkJ8jqCURjCNxuv3bWYS1`

*Browser showing "Hello IPFS Lab"*:

<img width="1638" height="875" alt="Снимок экрана 2025-11-17 235636" src="https://github.com/user-attachments/assets/29ce21f5-3c2a-4596-811d-2dc9d036c8cd" />

**Via Public Gateways:**
- `https://ipfs.io/ipfs/QmUFJmQRosK4Amzcjwbip8kV3gkJ8jqCURjCNxuv3bWYS1`
- `https://cloudflare-ipfs.com/ipfs/QmUFJmQRosK4Amzcjwbip8kV3gkJ8jqCURjCNxuv3bWYS1`

## Analysis

**How does IPFS's content addressing differ from traditional URLs?**


**What are the advantages and disadvantages of decentralized storage?**

---

### Task 2 — Static Site Deployment with 4EVERLAND (5 pts)

### 4EVERLAND Project URL
`https://f25-devops-intro-gye79pd3-gppovrm.ipfs.4everland.app/`

### IPFS CID from 4EVERLAND Dashboard
`bafybeiffhpbj7u3zpsa2ywhnvh5ckn2gsdaniygytew6jock3nvkig51x3e`

## Screenshots

**4EVERLAND Deployment Dashboard:**

<img width="1866" height="1014" alt="Снимок экрана 2025-11-18 003820" src="https://github.com/user-attachments/assets/60f08702-da74-4a33-adeb-34273d2230c4" />

**Site Accessed Through 4EVERLAND Domain:**

<img width="1917" height="983" alt="image" src="https://github.com/user-attachments/assets/e0683966-48f2-4c92-a6cd-409d8643015c" />

**Site Accessed Through Public IPFS Gateway:**
https://bafybeifhpby7u3zpsa2ywhwh5ckn2gsdsniygytew6jxok3nvkiq5t3v3e.ipfs.dweb.link/

<img width="1900" height="950" alt="image" src="https://github.com/user-attachments/assets/2540ab27-f2c7-4714-8820-3e67b20e5e05" />

## Analysis

**How does 4EVERLAND simplify IPFS deployment compared to manual methods?**

**What are the trade-offs between traditional web hosting and IPFS hosting?**
