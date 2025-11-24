---
layout: default
---
[RHCSA](./) <br>
[back](./another-page.html) <br>
[Openshift](./Openshift.md) <br>

<head>
  <meta charset="UTF-8">
  <title>CKA Labs Provisioning Architecture Cheat Sheet</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f9f9f9; color: #333; line-height: 1.6; padding: 20px; }
    h1 { color: #b30000; }
    h2 { margin-top: 25px; color: #444; }
    h3 { color: #555; }
    pre { background: #272822; color: #f8f8f2; padding: 10px; border-radius: 5px; overflow-x: auto; }
    code { color: #f8f8f2; }
    .section { margin-bottom: 30px; }
  </style>
</head>

<h1>CKA Labs Provisioning Architecture</h1>

<div class="section">
  <h2>🏗️ Architecture Overview</h2>
  <h3>On Public Cloud (External)</h3>
  <pre><code>External infrastructure used for CKA/CKAD lab environments.</code></pre>

  <h3>On Prem (Internal)</h3>
  <pre><code>Internal OpenShift Virtualization setup for lab deployments.</code></pre>

  <h3>User-Defined Network (UDN) & OpenShift Virtualization</h3>
  <pre><code>NAD (Network Attachment Definition)
Prefix: CKA/CKAD-UserID

→ Helm chart creates:
   - Required Kubernetes resources
   - User entry inside Helm for UDN
</code></pre>

  <h3>Lab Modes</h3>
  <pre><code>1. Without Kubernetes → Ubuntu VM with DNS pre-configuration
2. With Kubernetes → Full CKA environment</code></pre>
</div>

<div class="section">
  <h2>🧩 Step 1 — Foundation Server (f0-server)</h2>
  <pre><code>Machine: Ubuntu VM latest version
IP: 172.16.7.4

Tasks:
- Install Ubuntu latest
- Configure network: IP = 172.16.7.4
</code></pre>
</div>

<div class="section">
  <h2>🧱 Step 2 — OpenShift VM Preparation</h2>

  <h3>VM Disk Configuration (Quicko 2 Image)</h3>
  <pre><code>- name: ubuntu-harlequin-goldfish-50-cdrom-iso
  persistentVolumeClaim:
    claimName: ubuntu-harlequin-goldfish-50-volume

- name: rootdisk
  persistentVolumeClaim:
    claimName: ubuntu-harlequin-goldfish-50-volume-blank
</code></pre>
</div>

<div class="section">
  <h2>⬇️ VM Export & Disk Conversion</h2>

  <h3>📥 Download with Retry</h3>
  <pre><code>virtctl vmexport create v2-export --pvc=f0-server
virtctl vmexport download v2-export --volume=f0-server \
  --output=v2.img.gz --retry 3
</code></pre>

  <h3>🔄 Convert RAW → QCOW2</h3>
  <pre><code>qemu-img info disk.img.gz
qemu-img convert -f raw -O qcow2 disk.img.gz disk.qcow2
</code></pre>

  <h3>▶️ Run QCOW2 Locally</h3>
  <pre><code>qemu-system-x86_64.exe \
  -m 4096 -smp 2 \
  -drive file=yourimage.qcow2,format=qcow2 \
  -nic user -boot c
</code></pre>
</div>

<div class="section">
  <h2>📤 Upload Image as PVC (OpenShift)</h2>
  <pre><code>virtctl image-upload pvc v2-lab \
  --namespace yhamouda \
  --image-path=v2.img/v2.img \
  --size=35Gi \
  --insecure \
  --retry 3 \
  --storage-class ocs-storagecluster-ceph-rbd-virtualization \
  --volume-mode block \
  --access-mode ReadWriteMany
</code></pre>
</div>
