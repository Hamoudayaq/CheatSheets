---
layout: default
---
[RHCSA](./) <br>
[back](./another-page.html) <br>
[Openshift](./Openshift.md) <br>

<head>
  <meta charset="UTF-8">
  <title>Local RHEL Repository & LVM Setup Cheat Sheet</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f9f9f9; color: #333; line-height: 1.6; padding: 20px; }
    h1 { color: #b30000; }
    h2 { margin-top: 25px; color: #444; }
    pre { background: #272822; color: #f8f8f2; padding: 10px; border-radius: 5px; overflow-x: auto; }
    code { color: #f8f8f2; }
    .section { margin-bottom: 30px; }
  </style>
</head>

<h1>Local RHEL Repository & LVM Setup Cheat Sheet</h1>

<div class="section">
  <h2>💽 Mount ISO</h2>
  <pre><code>lsblk
mkdir -p /mnt/rhel
mount /dev/sr0 /mnt/rhel/

# Alternative mount command:
sudo mount -t iso9660 /dev/sr0 /mnt/rhel
</code></pre>
</div>

<div class="section">
  <h2>📝 Create Local Repositories</h2>
  <pre><code>vim /etc/yum.repos.d/local-BaseOS.repo
vim /etc/yum.repos.d/local-AppStream.repo
</code></pre>

  <pre><code>[local-BaseOS]
name=RHEL BaseOS
baseurl=file:///mnt/rhel/BaseOS/
enabled=1
gpgcheck=0

[local-AppStream]
name=RHEL AppStream
baseurl=file:///mnt/rhel/AppStream/
enabled=1
gpgcheck=0
</code></pre>
</div>

<div class="section">
  <h2>⚙️ Configure DNF</h2>
  <pre><code>sudo dnf config-manager --disable \*
sudo dnf config-manager --enable local-BaseOS local-AppStream

sudo dnf clean all
sudo dnf makecache
</code></pre>
</div>

<div class="section">
  <h2>🔐 Register System</h2>
  <pre><code>subscription-manager register --auto-attach
</code></pre>
</div>

<div class="section">
  <h2>🌐 Enable Official Repositories</h2>
  <pre><code>sudo subscription-manager repos --enable=rhel-*-baseos-rpms
sudo subscription-manager repos --enable=rhel-*-appstream-rpms
</code></pre>
</div>

<div class="section">
  <h2>🧹 Disable Local Repositories</h2>
  <pre><code>sudo dnf config-manager --disable local-BaseOS local-AppStream
</code></pre>
</div>

<div class="section">
  <h2>🗄️ Create LVM Storage for Oracle</h2>
  <pre><code>sudo pvcreate /dev/nvme1n1
sudo vgcreate vg_oracle /dev/nvme1n1
sudo lvcreate -L 70G -n lv_oradata vg_oracle

sudo mkfs.xfs /dev/vg_oracle/lv_oradata

sudo mkdir /oradata
sudo mount /dev/vg_oracle/lv_oradata /oradata

blkid
</code></pre>

  <h3>📌 Add to /etc/fstab</h3>
  <pre><code>UUID=&lt;uuid_oradata&gt;  /oradata    xfs   defaults 0 0
</code></pre>
</div>
