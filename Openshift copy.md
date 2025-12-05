<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>RHACM Observability Cheatsheet</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background: #ffffff;
        margin: 40px auto;
        max-width: 900px;
        line-height: 1.6;
    }
    h1 {
        text-align: center;
        color: #009879;
        margin-bottom: 40px;
    }
    h2 {
        color: #009879;
        margin-top: 40px;
    }
    pre {
        background: #f4f4f4;
        padding: 12px;
        border-radius: 5px;
        overflow-x: auto;
        border-left: 4px solid #009879;
    }
    code {
        font-family: monospace;
    }
    .section {
        margin-bottom: 40px;
    }
    hr {
        border: none;
        height: 1px;
        background: #ddd;
        margin: 40px 0;
    }
</style>
</head>
<body>

<h1>RHACM Observability – Complete Cheatsheet</h1>

<div class="section">
<h2>1. Create Observability Namespace</h2>
<pre><code>oc login -u admin -p redhatocp https://api.ocp4.example.com:6443
oc create namespace open-cluster-management-observability</code></pre>
</div>

<hr>

<div class="section">
<h2>2. Create Pull Secret</h2>

<b>Extract the global pull-secret</b>
<pre><code>DOCKER_CONFIG_JSON=$(oc extract secret/pull-secret -n openshift-config --to=-)
echo "$DOCKER_CONFIG_JSON"</code></pre>

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

<b>Move to lab directory</b>
<pre><code>cd ~/DO0014L/labs/observability-enable/</code></pre>

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

<b>mcobs.yaml example</b>
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

</body>
</html>
