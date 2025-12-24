<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>PostgreSQL & Redis Lab Exercise</title>

  <style>
    body {
      font-family: Inter, system-ui, sans-serif;
      line-height: 1.5;
      color: #0f172a;
      background: #f8fafc;
      padding: 24px;
    }

    .container {
      max-width: 900px;
      margin: auto;
      background: #ffffff;
      padding: 28px;
      border-radius: 12px;
      box-shadow: 0 6px 20px rgba(2, 6, 23, 0.08);
    }

    h1 { font-size: 1.6rem; margin-bottom: 8px; }
    h2 { font-size: 1.2rem; margin-top: 24px; }
    h3 { font-size: 1rem; margin-top: 16px; }

    p, ul, ol {
      margin: 8px 0;
    }

    pre {
      background: #0b1220;
      color: #e6eef8;
      padding: 12px;
      border-radius: 8px;
      overflow-x: auto;
    }

    code {
      font-family: monospace;
    }

    .actions {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      margin-bottom: 16px;
    }

    .btn {
      padding: 10px 18px;
      background: #2563eb;
      color: #fff;
      text-decoration: none;
      border-radius: 6px;
    }

    .badge {
      display: inline-block;
      background: #eef2ff;
      color: #3730a3;
      padding: 6px 10px;
      border-radius: 999px;
      font-size: 0.85rem;
      font-weight: 600;
      margin-bottom: 12px;
    }

    .key {
      background: #fffbeb;
      border-left: 4px solid #f59e0b;
      padding: 12px;
      border-radius: 6px;
      margin-top: 16px;
    }

    .tip {
      background: #ecfeff;
      border-left: 4px solid #06b6d4;
      padding: 12px;
      border-radius: 6px;
      margin-top: 16px;
    }

    table {
      border-collapse: collapse;
      margin: 12px 0;
      width: 100%;
    }

    table, th, td {
      border: 1px solid #cbd5e1;
      padding: 6px 12px;
    }

    th {
      background: #f1f5f9;
    }

    footer {
      margin-top: 32px;
      text-align: center;
      font-size: 0.9rem;
      color: #475569;
    }
  </style>
</head>

<body>
  <div class="container">

    <!-- Navigation -->
    <div class="actions">
      <a href="Openshift.html" class="btn">Openshift</a>
      <a href="." class="btn">Back</a>
      <a href="./quay.html" class="btn">Quay</a>
    </div>

    <!-- Title -->
    <h1>PostgreSQL & Redis Lab Exercise</h1>
    <span class="badge">Task: Install and configure PostgreSQL and Redis containers as user services</span>

    <!-- Outcomes -->
    <section>
      <h2>Learning Outcomes</h2>
      <ul>
        <li>Configure a PostgreSQL database container.</li>
        <li>Configure a Redis container.</li>
        <li>Run both dependencies as user services with systemd.</li>
      </ul>
    </section>

    <!-- Instructions -->
    <section>
      <h2>Instructions</h2>
      <p>All steps are performed as the <code>lab</code> user on the <code>registry</code> machine.</p>

      <h3>1. Edit Quadlet container definitions</h3>
      <ol>
        <li>Log in to the <code>registry</code> machine via SSH:
          <pre><code>[student@workstation ~]$ ssh lab@registry.ocp4.example.com
...output omitted...</code></pre>
        </li>
        <li>Inspect your home directory. Two Quadlet files should exist:
          <pre><code>[lab@registry ~]$ ls -l
-rw-r--r--. 1 lab lab 451 Sep 13 09:22 quay-pgsql.container
-rw-r--r--. 1 lab lab 261 Sep 13 09:22 quay-redis.container</code></pre>
        </li>
        <li>Edit <code>quay-pgsql.container</code> and replace <code>CHANGEME</code> values:

          <table>
            <thead>
              <tr>
                <th>Environment variable</th>
                <th>Value</th>
              </tr>
            </thead>
            <tbody>
              <tr><td>POSTGRESQL_USER</td><td>quay</td></tr>
              <tr><td>POSTGRESQL_PASSWORD</td><td>secret</td></tr>
              <tr><td>POSTGRESQL_DATABASE</td><td>quay</td></tr>
              <tr><td>POSTGRESQL_ADMIN_PASSWORD</td><td>verysecret</td></tr>
            </tbody>
          </table>

          <pre><code>[Unit]
Description=PostgreSQL container for Quay
After=network-online.target

[Container]
ContainerName=postgresql
Image=registry.redhat.io/rhel9/postgresql-15:latest
Volume=/local/quay-db:/var/lib/pgsql/data:Z
Network=quay
Environment=POSTGRESQL_USER=quay
Environment=POSTGRESQL_PASSWORD=secret
Environment=POSTGRESQL_DATABASE=quay
Environment=POSTGRESQL_ADMIN_PASSWORD=verysecret

[Service]
Restart=always

[Install]
WantedBy=default.target</code></pre>

          <div class="tip">
            <strong>Note:</strong> An example file is available in <code>~/DO0031L/solutions/quay-depend/quay-pgsql.container</code>
          </div>
        </li>
        <li>Edit <code>quay-redis.container</code> and set <code>REDIS_PASSWORD=verysecret</code>:

          <pre><code>[Unit]
Description=Redis container for Quay
After=network-online.target

[Container]
ContainerName=redis
Image=registry.redhat.io/rhel9/redis-7:latest
Network=quay
Environment=REDIS_PASSWORD=verysecret

[Service]
Restart=always

[Install]
WantedBy=default.target</code></pre>

          <div class="tip">
            <strong>Note:</strong> Example file available in <code>~/DO0031L/solutions/quay-depend/quay-redis.container</code>
          </div>
        </li>
      </ol>

      <h3>2. Publish Quadlet files</h3>
      <ol>
        <li>Verify <code>quay</code> user's Quadlet directory:
          <pre><code>[lab@registry ~]$ sudo -u quay ls -la ~quay/.config/containers/systemd/</code></pre>
        </li>
        <li>Copy files:
          <pre><code>[lab@registry ~]$ sudo cp quay-*.container ~quay/.config/containers/systemd/</code></pre>
        </li>
        <li>Ensure correct ownership:
          <pre><code>[lab@registry ~]$ sudo chown -R quay:quay ~quay/.config/containers/systemd/</code></pre>
        </li>
      </ol>

      <h3>3. Correct PostgreSQL data volume ownership</h3>
      <ol>
        <li>Check directory ownership:
          <pre><code>[lab@registry ~]$ ls -ld /local/quay-db</code></pre>
        </li>
        <li>Move to a readable directory for container commands:
          <pre><code>[lab@registry ~]$ cd /tmp</code></pre>
        </li>
        <li>Correct ownership with podman unshare:
          <pre><code>[lab@registry tmp]$ sudo -u quay podman unshare chown -R 26 /local/quay-db/</code></pre>
        </li>
      </ol>

      <h3>4. Start services and verify</h3>
      <ol>
        <li>Reload systemd daemon:
          <pre><code>[lab@registry tmp]$ sudo systemctl -M quay@ --user daemon-reload</code></pre>
        </li>
        <li>Check services:
          <pre><code>[lab@registry tmp]$ sudo systemctl -M quay@ --user status quay-pgsql.service quay-redis.service</code></pre>
        </li>
        <li>Start services:
          <pre><code>[lab@registry tmp]$ sudo systemctl -M quay@ --user start quay-pgsql.service quay-redis.service</code></pre>
        </li>
        <li>Inspect containers:
          <pre><code>[lab@registry tmp]$ sudo -u quay podman ps</code></pre>
        </li>
      </ol>
    </section>

    <footer>
      © Study Sheet – Yassine
    </footer>
  </div>
</body>
