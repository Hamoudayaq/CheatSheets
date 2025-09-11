---
layout: default
---
[RHCSA](./).
[back](./another-page.html).
[Openshift](./Openshift.md)

<head>
  <meta charset="UTF-8">
  <title>Helm & OpenShift Commands Cheat Sheet</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f9f9f9; color: #333; line-height: 1.6; padding: 20px; }
    h1 { color: #b30000; }
    h2 { margin-top: 25px; color: #444; }
    pre { background: #272822; color: #f8f8f2; padding: 10px; border-radius: 5px; overflow-x: auto; }
    code { color: #f8f8f2; }
    .section { margin-bottom: 30px; }
  </style>
</head>

  <h1>Helm & OpenShift Commands Cheat Sheet</h1>

  <div class="section">
    <h2>🔧 Setup</h2>
    <pre><code>lab start packaged-charts
helm repo list
helm repo add do280-repo http://helm.ocp4.example.com/charts
helm search repo --versions
helm show values do280-repo/etherpad --version 0.0.6
oc login -u developer -p developer https://api.ocp4.example.com:6443
</code></pre>
  </div>

  <div class="section">
    <h2>📦 Deploy to Development</h2>
    <pre><code>oc new-project packaged-charts-development
helm install example-app do280-repo/etherpad -f values.yaml --version 0.0.6
oc get route
helm list
</code></pre>
  </div>

  <div class="section">
    <h2>⬆️ Upgrade</h2>
    <pre><code>helm upgrade example-app do280-repo/etherpad -f values.yaml --version 0.0.7
helm list
</code></pre>
  </div>

  <div class="section">
    <h2>🏭 Deploy to Production</h2>
    <pre><code>oc new-project packaged-charts-production
helm install production do280-repo/etherpad -f values.yaml --version 0.0.7
oc get pods
helm upgrade production do280-repo/etherpad -f values.yaml
oc get pods
</code></pre>
  </div>

  <div class="section">
    <h2>🧹 Cleanup</h2>
    <pre><code>rm values.yaml
lab finish packaged-charts
</code></pre>
  </div>
