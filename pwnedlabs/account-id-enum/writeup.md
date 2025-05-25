## 🪣 PwnedLabs - AWS S3 Account ID Enumeration

### Scenario
The ability to expose and leverage even the smallest oversights is a coveted skill. A global logistics company has reached out to our cybersecurity team for assistance. They've provided the IP address of their website and asked us to identify their AWS account ID via a public S3 bucket.

**Target IP:** `54.204.171.32`  
**Access Key ID:** `REDACTED_FOR_LAB`  
**Secret Access Key:** `REDACTED_FOR_LAB`

### Step 1: Nmap Scan
Performed an Nmap scan on the target IP to confirm service exposure:

![Screenshot](images/pasted_image_20250518213649.png)

---

### Step 2: Inspect Website Source
Visited the exposed HTTP service and reviewed the page source. The bucket name `mega-big-tech` was discovered:

![Screenshot](images/pasted_image_20250518213811.png)

---

### Step 3: Configure AWS Profile
Configured the provided AWS credentials to use a custom named profile:

```bash
aws configure --profile pwnedidlab
```

---

### Step 4: Use s3-account-search Tool
We used [s3-account-search](https://github.com/WeAreCloudar/s3-account-search) to brute-force the AWS account ID based on the trusted role `LeakyBucket`:

```bash
s3-account-search arn:aws:iam::427648302155:role/LeakyBucket mega-big-tech
```

---

### 🔍 How it Works
- The tool uses the trusted role ARN you supply.
- It replaces the account ID digit by digit, trying new ARNs.
- If the bucket returns a response (instead of `AccessDenied`), the digit is assumed correct.
- This continues until the full 12-digit AWS Account ID is discovered.

---

### ✅ Flag Discovered
The correct AWS account ID was found, revealing the flag:

![Screenshot](images/pasted_image_20250518224459.png)

---
