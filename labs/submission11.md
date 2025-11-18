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

IPFS uses content-based hashes (CIDs) to find, verify, and share files across a decentralized network, where all content is referenced through a unique value known as the content identifier or CID. In comparison to location addressing, content addressing refers to what the stored data contains, versus where the content is located on the web server. Traditional URLs suffer from 'link rot' - if the URL's slug changes or is moved, the link no longer works and returns a 404 error, requiring entire web pages to be renamed and accessed through new URLs.

**What are the advantages and disadvantages of decentralized storage?**

**Advantages:** Content addressing **isn't affected by link rot** and provides **immutability** - data files stored on IPFS are immutable since any change to file contents results in a new, unique CID.

**Disadvantages:**
- **Temporary cache dependency** - If files are not "pinned", they are stored in temporary cache and periodically removed by garbage collection
- **Unreliable availabilitye** - Files are only available as long as at least one node stores them; if all nodes remove your content, it disappears forever
- **No guaranteed availability** - Unlike traditional hosting where you pay and files are guaranteed to be stored, IPFS relies on voluntary participation
- **Variable performance** - If a file is only on one node in another country, loading will be slow; popular content loads fast, rare content loads slowly
- **Update complexity** - Any file change creates a new CID and requires distributing new links throughout the network

---

### Task 2 — Static Site Deployment with 4EVERLAND (5 pts)

### 4EVERLAND Project URL
`https://f25-devops-intro-gye79pd3-gppovrm.ipfs.4everland.app/`

### IPFS CID from 4EVERLAND Dashboard
`bafybeiffhpbj7u3zpsa2ywhnvh5ckn2gsdaniygytew6jock3nvkig51x3e`

## Screenshots

### 4EVERLAND Deployment Dashboard:

<img width="1866" height="1014" alt="Снимок экрана 2025-11-18 003820" src="https://github.com/user-attachments/assets/60f08702-da74-4a33-adeb-34273d2230c4" />

### Site Accessed Through 4EVERLAND Domain:

<img width="1917" height="983" alt="image" src="https://github.com/user-attachments/assets/e0683966-48f2-4c92-a6cd-409d8643015c" />

### Site Accessed Through Public IPFS Gateway:

https://bafybeifhpby7u3zpsa2ywhwh5ckn2gsdsniygytew6jxok3nvkiq5t3v3e.ipfs.dweb.link/

<img width="1900" height="950" alt="image" src="https://github.com/user-attachments/assets/2540ab27-f2c7-4714-8820-3e67b20e5e05" />

## Test Continuous Deployment (Optional)

I pushed 2 new commits to the GitHub repository and 4EVERLAND automatically detected the changes: 

<img width="1898" height="614" alt="image" src="https://github.com/user-attachments/assets/dda76563-9aea-42b2-801d-d2a1ebd97c16" />

After clicking the Deploy button, the site was **successfully updated**, demonstrating the **continuous deployment** workflow:

<img width="1680" height="894" alt="image" src="https://github.com/user-attachments/assets/0dc7adc4-06bf-4dca-9697-478c50a1a001" />

<img width="1366" height="564" alt="image" src="https://github.com/user-attachments/assets/2de9a4a9-86cc-4fa3-a932-30f0e0495eea" />

## Analysis

**How does 4EVERLAND simplify IPFS deployment compared to manual methods?**

4EVERLAND makes IPFS deployment as easy as traditional web hosting. Instead of dealing with complex peer-to-peer networks and command-line tools, you simply connect your GitHub repository and the platform handles everything automatically. It generates IPFS hashes, provides free CDN and SSL certificates, and sets up continuous deployment so your site updates whenever you push changes to GitHub. All the technical complexities of decentralized storage are handled behind the scenes.

**What are the trade-offs between traditional web hosting and IPFS hosting?**

Traditional hosting is simpler and more predictable but relies on central servers that can fail or be censored. IPFS offers better reliability through decentralization - your content stays available even if some nodes go offline - but can be slower for rarely accessed files since they might be stored on distant nodes. Traditional hosting gives you clear performance guarantees for a price, while IPFS depends on network participation where no single entity guarantees your files will always be available unless you actively ensure they're pinned across multiple nodes.
