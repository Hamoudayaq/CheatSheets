<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>OpenShift Templates — Study Sheet</title>
  <style>
    body{font-family:Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;line-height:1.5;color:#0f172a;background:#f8fafc;padding:24px}
    .container{max-width:900px;margin:0 auto;background:#fff;padding:28px;border-radius:12px;box-shadow:0 6px 20px rgba(2,6,23,0.08)}
    h1{font-size:1.6rem;margin-bottom:6px}
    h2{font-size:1.1rem;margin-top:18px}
    p{margin:8px 0}
    .badge{display:inline-block;background:#eef2ff;color:#3730a3;padding:6px 10px;border-radius:999px;font-weight:600;font-size:0.85rem}
    pre{background:#0b1220;color:#e6eef8;padding:12px;border-radius:8px;overflow:auto}
    code{font-family:SFMono-Regular, Menlo, Monaco, Consolas, 'Liberation Mono', monospace}
    ul{margin:8px 0 8px 20px}
    .key{background:#fffbeb;border-left:4px solid #f59e0b;padding:10px;border-radius:6px}
    .tip{background:#ecfeff;border-left:4px solid #06b6d4;padding:10px;border-radius:6px}
    .actions{display:flex;gap:8px;flex-wrap:wrap;margin-top:12px}
    .pill{background:#eef2ff;color:#3730a3;padding:6px 10px;border-radius:999px;font-weight:600}
    footer{margin-top:18px;font-size:0.9rem;color:#475569}
    .btn {
      display: inline-block;
      margin: 8px 0;
      padding: 10px 18px;
      background-color: #007bff;
      color: #fff;
      text-decoration: none;
      border-radius: 6px;
      font-size: 16px;
      transition: background-color 0.3s;
    }
    .btn:hover {
      background-color: #0056b3;
    }
  </style>
</head>
---
layout: default
---
  <a href="./" class="btn">RHCSA</a><br>
  <a href="./another-page.html" class="btn">Back</a><br>
  <a href="./helm.html" class="btn">Helm</a><br>


<h1>RHACM Observability – Complete Cheatsheet</h1>

<!-- RHACM Observability Rules -->
<section id="rhacm-observability-rules">
  <h2>RHACM Observability: Prometheus, Thanos, Default & Custom Rules</h2>
  <p>
    In RHACM observability, Prometheus and Thanos use the same PromQL rule language, but Thanos evaluates rules centrally with a global, multi-cluster view, while each cluster Prometheus instance only sees its own cluster. There are default rules, shipped by Red Hat in a Thanos Ruler default rules ConfigMap, which give you ready-made alerts, and custom rules, which you add in a Thanos Ruler custom rules ConfigMap to define your own fleet-wide alerts or recordings. 
  </p>

  <h3>Super Simple Mental Model</h3>
  <ul>
    <li><strong>Local Prometheus rules</strong>: Per-cluster only; good for local alerts and SLOs.</li>
    <li><strong>Thanos/RHACM rules</strong>: Same PromQL syntax, but run on Thanos with metrics from all clusters; good for global/fleet alerts and long-term data.</li>
    <li><strong>Default rules</strong>: Pre-made Thanos rules in the default rules ConfigMap; you get useful alerts without writing anything.</li>
    <li><strong>Custom rules</strong>: Your own Thanos rules in the custom rules ConfigMap when you want extra or different alerts/recordings.</li>
  </ul>
</section>

<!-- Configure RHACM Observability-->


<div class="section">
<h2>1. Create Observability Namespace</h2>
<pre><code>oc login -u admin -p redhatocp https://api.ocp4.example.com:6443
oc create namespace open-cluster-management-observability</code></pre>
</div>

<hr>

<div class="section">
<h2>2. Create Pull Secret</h2>

<b>Extract the global  pull-secret from openshift-config or open-cluster-management</b>
<pre><code>DOCKER_CONFIG_JSON=$(oc extract secret/multiclusterhub-observability-pull-secret -n open-cluster-management --to=-)</code></pre>
<b> Or </b>
<pre><code>DOCKER_CONFIG_JSON=$(oc extract secret/pull-secret -n openshift-config --to=-)</code></pre>
<pre><code>echo "$DOCKER_CONFIG_JSON"</code></pre>

<b>Create pull-secret in the observability namespace</b>
<pre><code>oc create secret generic multiclusterhub-operator-pull-secret \
  -n open-cluster-management-observability \
  --from-literal=.dockerconfigjson="$DOCKER_CONFIG_JSON" \
  --type=kubernetes.io/dockerconfigjson</code></pre>
</div>

<hr>

<div class="section">
<h2>3. Verify ODF Readiness</h2>
<pre><code>oc get storagecluster -n openshift-storage
oc get noobaa -n openshift-storage
oc get storageclasses -o custom-columns='NAME:metadata.name,PROVISIONER:provisioner'</code></pre>
</div>

<hr>

<div class="section">
<h2>4. Create Object Bucket Claim (OBC)</h2>


<b>Example obc.yaml</b>
<pre><code>apiVersion: objectbucket.io/v1alpha1
kind: ObjectBucketClaim
metadata:
  name: thanos-obc
  namespace: open-cluster-management-observability
spec:
  storageClassName: openshift-storage.noobaa.io
  generateBucketName: observability-bucket</code></pre>

<b>Apply</b>
<pre><code>oc apply -f obc.yaml
oc get objectbucketclaim -n open-cluster-management-observability</code></pre>
</div>

<hr>

<div class="section">
<h2>5. Retrieve S3 Bucket Information</h2>

<b>From ConfigMap</b>
<pre><code>oc extract --to=- cm/thanos-obc -n open-cluster-management-observability</code></pre>

<b>From Secret</b>
<pre><code>oc extract --to=- secret/thanos-obc -n open-cluster-management-observability</code></pre>

<b>S3 route</b>
<pre><code>oc get route s3 -n openshift-storage</code></pre>
</div>

<hr>

<div class="section">
<h2>6. Create Thanos Storage Secret</h2>
<pre><code>apiVersion: v1
kind: Secret
metadata:
  name: thanos-object-storage
  namespace: open-cluster-management-observability
type: Opaque
stringData:
  thanos.yaml: |
    type: s3
    config:
      bucket: &lt;YOUR_BUCKET_NAME&gt;
      endpoint: &lt;YOUR_S3_ROUTE&gt;
      insecure: true
      access_key: &lt;YOUR_ACCESS_KEY&gt;
      secret_key: &lt;YOUR_SECRET_KEY&gt;
</code></pre>

<pre><code>oc apply -f secret.yaml</code></pre>
</div>

<hr>

<div class="section">
<h2>7. Deploy MultiClusterObservability Resource</h2>

<b>Example</b>
<pre><code>apiVersion: observability.open-cluster-management.io/v1beta2
kind: MultiClusterObservability
metadata:
  name: observability
spec:
  observabilityAddonSpec:
    enableMetrics: true
  storageConfig:
    metricObjectStorage:
      key: thanos.yaml
      name: thanos-object-storage
    storageClass: nfs-storage
    alertmanagerStorageSize: 1Gi
    compactStorageSize: 1Gi
    receiveStorageSize: 1Gi
    ruleStorageSize: 1Gi
    storeStorageSize: 1Gi
</code></pre>

<b>Apply</b>
<pre><code>oc apply -f mcobs.yaml</code></pre>

<b>Check operator</b>
<pre><code>oc get deploy multicluster-observability-operator -n open-cluster-management</code></pre>

<b>Check observability pods</b>
<pre><code>oc get pods -n open-cluster-management-observability</code></pre>
</div>

<hr>

<div class="section">
<h2>8. Access Grafana Dashboards</h2>
<p>
1. OpenShift Console → Administrator<br>
2. Login (<code>admin/redhatocp</code>)<br>
3. Switch to <b>RHACM Console</b><br>
4. Navigate: <b>Infrastructure → Clusters</b><br>
5. Click <b>Grafana</b> (top-right)
</p>
</div>