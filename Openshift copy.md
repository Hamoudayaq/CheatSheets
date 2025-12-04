<html lang="en">
<head>
<meta charset="UTF-8">
<title>RHACM Observability Cheatsheet</title>
<style>
    body {
        font-family: Arial, sans-serif;
        line-height: 1.6;
        margin: 30px;
        background: #f9f9f9;
    }
    h1, h2 {
        color: #d03232;
    }
    code {
        background: #eee;
        padding: 3px 5px;
        border-radius: 4px;
    }
    pre {
        background: #272822;
        color: #fafafa;
        padding: 12px;
        border-radius: 6px;
        overflow-x: auto;
    }
    .section {
        margin-bottom: 40px;
    }
</style>
</head>
<body>

<h1>RHACM Observability — Workflow Cheatsheet</h1>

<div class="section">
<h2>1. Create Observability Namespace</h2>
 <code>oc login -u admin -p redhatocp https://api.ocp4.example.com:6443
oc create namespace open-cluster-management-observability</code> 
</div>

<div class="section">
<h2>2. Create Pull Secret</h2>
<h3>Extract global pull-secret:</h3>
 <code>DOCKER_CONFIG_JSON=$(oc extract secret/pull-secret -n openshift-config --to=-)
echo $DOCKER_CONFIG_JSON</code> 

<h3>Create secret in the observability namespace:</h3>
 <code>oc create secret generic multiclusterhub-operator-pull-secret \
  -n open-cluster-management-observability \
  --from-literal=.dockerconfigjson="$DOCKER_CONFIG_JSON" \
  --type=kubernetes.io/dockerconfigjson</code> 
</div>

<div class="section">
<h2>3. Verify ODF Readiness</h2>
 <code>oc get storagecluster -n openshift-storage
oc get noobaa -n openshift-storage
oc get storageclasses -o custom-columns='NAME:metadata.name,PROVISIONER:provisioner'</code> 
</div>

<div class="section">
<h2>4. Create Object Bucket Claim (OBC)</h2>

<h3>Move to lab directory:</h3>
 <code>cd ~/DO0014L/labs/observability-enable/</code> 

<h3>Example <em>obc.yaml</em>:</h3>
 <code>apiVersion: objectbucket.io/v1alpha1
kind: ObjectBucketClaim
metadata:
  name: thanos-obc
  namespace: open-cluster-management-observability
spec:
  storageClassName: openshift-storage.noobaa.io
  generateBucketName: observability-bucket</code> 

<h3>Apply manifest:</h3>
 <code>oc apply -f obc.yaml
oc get objectbucketclaim -n open-cluster-management-observability</code> 
</div>

<div class="section">
<h2>5. Retrieve S3 Bucket Info</h2>

<h3>From ConfigMap:</h3>
 <code>oc extract --to=- cm/thanos-obc -n open-cluster-management-observability</code> 

<h3>From Secret:</h3>
 <code>oc extract --to=- secret/thanos-obc -n open-cluster-management-observability</code> 

<h3>Get S3 public route:</h3>
 <code>oc get route s3 -n openshift-storage</code> 
</div>

<div class="section">
<h2>6. Create Thanos Storage Secret</h2>

 <code>apiVersion: v1
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
</code> 

 <code>oc apply -f secret.yaml</code> 
</div>

<div class="section">
<h2>7. Deploy the MultiClusterObservability Resource</h2>

<h3>Example <em>mcobs.yaml</em>:</h3>
 <code>apiVersion: observability.open-cluster-management.io/v1beta2
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
</code> 

<h3>Apply:</h3>
 <code>oc apply -f mcobs.yaml</code> 

<h3>Verify operator:</h3>
 <code>oc get deploy multicluster-observability-operator -n open-cluster-management</code> 

<h3>Verify observability pods:</h3>
 <code>oc get pods -n open-cluster-management-observability</code> 
</div>

<div class="section">
<h2>8. Access Grafana Dashboards</h2>
<p>
1. Go to: <strong>OpenShift Console</strong><br>
2. Login as <code>admin / redhatocp</code><br>
3. Switch to <strong>RHACM Console</strong><br>
4. Navigate to <strong>Infrastructure → Clusters</strong><br>
5. Click <strong>Grafana</strong> (top right)
</p>
</div>

</body>
