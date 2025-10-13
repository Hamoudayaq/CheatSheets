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


<h2>Openshift Cheat-sheet</h2>


<p>Use a JSONPath filter to determine the capacity and allocatable CPU for the <code class="code">master01</code> node.
The values might differ on your system.</p><pre class="screen">[student@workstation ~]$ <strong class="userinput"><code>oc get node master01 -o jsonpath=\
'Allocatable: {.status.allocatable.cpu}{"\n"}'\
'Capacity: {.status.capacity.cpu}{"\n"}'</code></strong>
Allocatable: 7500m
Capacity: 8</pre>

<p>Display the two most recent logs of the <code class="code">kubelet</code> service on the node.
The logs might differ on your system.</p><pre class="screen">[student@workstation ~]$ <strong class="userinput"><code>oc adm node-logs master01 -u kubelet --tail 2</code></strong>
-- Logs begin at Thu 2023-02-09 21:19:09 UTC, end at Thu 2023-03-09 16:59:16 UTC. --
Mar 09 02:40:57.466711 master01 systemd[1]: Stopped Kubernetes Kubelet.
Mar 09 02:40:57.466835 master01 systemd[1]: kubelet.service: Consumed 1h 27min 8.069s CPU time
-- Logs begin at Thu 2023-02-09 21:19:09 UTC, end at Thu 2023-03-09 16:59:16 UTC. --
Mar 09 16:58:52.133046 master01 kubenswrapper[3195]: I0309 16:58:52.132866    3195 kubelet_getters.go:182] "Pod status updated" pod="openshift-etcd/etcd-master01" status=Running
Mar 09 16:58:52.133046 master01 kubenswrapper[3195]: I0309 16:58:52.132882    3195 kubelet_getters.go:182] "Pod status updated" pod="openshift-kube-apiserver/kube-apiserver-master01" status=Running</pre>


<h3 class="title"><a id="_using_ingress_objects_for_external_connectivity"></a>Using Ingress Objects for External Connectivity</h3>
<p>To create an ingress object, use the <code class="code">oc create ingress ingress-name --rule=URL_route=service-name:port-number</code> command.
Use the <code class="code">--rule</code> option to provide a custom rule in the <code class="code">host/path=service:port[,tls=secretname]</code> format.
If the TLS option is omitted, then an insecure route is created.</p>

<pre class="screen">[user@host ~]$ <strong class="userinput"><code>oc create ingress ingr-sakila \
--rule="ingr-sakila.apps.ocp4.example.com/*=sakila-service:8080"</code></strong></pre>

<h3 class="title"><a id="_sticky_sessions"></a>Sticky Sessions</h3>

<pre class="screen">[user@host ~]$ <strong class="userinput"><code>oc annotate route route-example \
router.openshift.io/cookie_name=myapp</code></strong></pre>

<li class="step"><p>Use the <code class="code">curl</code> command to save the <code class="code">hello</code> cookie to the <code class="code">/tmp/cookie_jar</code> file.
Afterward, confirm that the <code class="code">hello</code> cookie exists in the <code class="code">/tmp/cookie_jar</code> file.</p><pre class="screen">[student@workstation ~]$ <strong class="userinput"><code>curl satir-web-applications.apps.ocp4.example.com \
  -c /tmp/cookie_jar</code></strong>
Welcome to Red Hat Training, from satir-app-787b7d7858-q7bhj</pre><pre class="screen">[student@workstation ~]$ <strong class="userinput"><code>cat /tmp/cookie_jar</code></strong>
<span class="emphasis"><em>...output omitted...</em></span>
#HttpOnly_satir-web-applications.apps.ocp4.example.com	FALSE	/	FALSE	0	hello	b7dd73d32003e513a072e25a32b6c881</pre></li>

<li class="step"><p>The <code class="code">hello</code> cookie provides session stickiness for connections to the <code class="code">satir</code> route.
Use the <code class="code">curl</code> command and the <code class="code">hello</code> cookie in the <code class="code">/tmp/cookie_jar</code> file to connect to the <code class="code">satir</code> route again.
Confirm that you are connected to the same pod that handled the request in the previous step.</p><pre class="screen">[student@workstation ~]$ <strong class="userinput"><code>for i in {1..10}; do \
  curl satir-web-applications.apps.ocp4.example.com -b /tmp/cookie_jar; done</code></strong>
Welcome to Red Hat Training, from satir-app-787b7d7858-q7bhj
Welcome to Red Hat Training, from satir-app-787b7d7858-q7bhj
Welcome to Red Hat Training, from satir-app-787b7d7858-q7bhj
<span class="emphasis"><em>...output omitted...</em></span></pre></li>




  <div class="container">
    <header>
      <h1>OpenShift Templates </h1>
      <p class="badge">Objective: Deploy/update apps from OpenShift templates</p>
    </header>

    <section>
      <div class="key">
        <strong>Use cases:</strong> quick app deployment, parameterized configs for environments, shareable/reusable deployment definitions.
      </div>
    </section>

    <section>
      <h2>Discovering Templates</h2>
      <ul>
        <li>List templates in the <code>openshift</code> namespace:
          <pre><code>oc get templates -n openshift</code></pre>
        </li>
        <li>Show details of a template:
          <pre><code>oc describe template &lt;name&gt; -n openshift</code></pre>
        </li>
        <li>Show only parameters:
          <pre><code>oc process --parameters &lt;template&gt; -n openshift</code></pre>
        </li>
        <li>Export full template YAML:
          <pre><code>oc get template &lt;name&gt; -o yaml -n openshift</code></pre>
        </li>
      </ul>
    </section>

    <section>
      <h2>Using Templates</h2>
      <p>Templates can be used interactively or to generate manifests for automated workflows.</p>
      <ul>
        <li><strong>Quick create (one-shot):</strong>
          <pre><code>oc new-app --template=cache-service -p APPLICATION_USER=my-user</code></pre>
          <small>Creates new resources only — convenient for development.</small>
        </li>
        <li><strong>Generate manifest with parameters:</strong>
          <pre><code>oc process my-template -p KEY=VALUE -o yaml &gt; manifest.yaml</code></pre>
        </li>
        <li><strong>Apply directly (no file):</strong>
          <pre><code>oc process my-template --param-file=params.env | oc apply -f -</code></pre>
        </li>
      </ul>
    </section>

    <section>
      <h2>Updating Apps from Templates</h2>
      <p>Re-process the template with new parameters and pipe to <code>oc apply</code> to update live resources. Good for simple parameter changes.</p>
      <p><strong>Preview changes before applying:</strong></p>
      <pre><code>oc process my-template --param-file=params2.env | oc diff -f -</code></pre>
      <div class="tip">Tip: For complex application updates, consider using Helm charts instead of templates.</div>
    </section>

    <section>
      <h2>Managing Templates (best practices)</h2>
      <ol>
        <li>Copy and customize an existing template to your project:
          <pre><code>oc get template cache-service -o yaml -n openshift &gt; my-template.yaml</code></pre>
        </li>
        <li>Recommended edits to the YAML before uploading:
          <ul>
            <li>Give the template a project-specific name</li>
            <li>Adjust default parameter values</li>
            <li>Remove <code>metadata.namespace</code> to avoid namespace errors</li>
          </ul>
        </li>
        <li>Upload template to your project or shared project:
          <pre><code>oc create -f my-template.yaml
              oc create -f my-template.yaml -n shared-templates</code></pre>
        </li>
      </ol>
    </section>

    <section>
      <h2>Important Commands (quick reference)</h2>
      <div class="actions">
        <div class="pill">oc get templates -n openshift</div>
        <div class="pill">oc describe template &lt;name&gt; -n openshift</div>
        <div class="pill">oc process --parameters &lt;template&gt;</div>
        <div class="pill">oc process &lt;template&gt; -p KEY=VALUE -o yaml</div>
        <div class="pill">oc process --param-file=params.env | oc apply -f -</div>
        <div class="pill">oc process ... | oc diff -f -</div>
      </div>
    </section>
  </div>

<div class="container">
  <header>
    <h1>Generate and Test a CA-Signed Certificate</h1>
    <p class="badge">Objective: Create a private key, CSR, and CA-signed cert for OpenShift apps</p>
  </header>

  <section>
    <h2>Step 1: Generate Private Key</h2>
    <pre><code>[student@workstation certs]$ openssl genrsa -out training.key 4096</code></pre>
  </section>

  <section>
    <h2>Step 2: Generate CSR</h2>
    <p>For the hostname <code>todo-https.apps.ocp4.example.com</code>:</p>
    <pre><code>[student@workstation certs]$ openssl req -new \
    -key training.key -out training.csr \
    -subj "/C=US/ST=North Carolina/L=Raleigh/O=Red Hat/\
    CN=todo-https.apps.ocp4.example.com"</code></pre>
  </section>

  <section>
    <h2>Step 3: Sign the Certificate</h2>
    <p>Use your CA key, cert, and password. Add SANs with an <code>extfile</code>.</p>
    <pre><code>[student@workstation certs]$ openssl x509 -req -in training.csr \
    -passin file:passphrase.txt \
    -CA training-CA.pem -CAkey training-CA.key -CAcreateserial \
    -out training.crt -days 1825 -sha256 -extfile training.ext</code></pre>
    <div class="tip">Tip: The <code>-extfile</code> allows you to add Subject Alternative Names (SANs).</div>
  </section>

  <section>
    <h2>Step 4: Test the Certificate</h2>
    <ul>
      <li>Verify using the CA certificate:
        <pre><code>curl -vv -I \
    --cacert certs/training-CA.pem \
    https://todo-https.apps.ocp4.example.com</code></pre>
      </li>
      <li>Test directly against pod/service IP with <code>-k</code> (insecure, skip cert validation):
        <pre><code>curl -s -k https://172.30.121.154:8443 | head -n5</code></pre>
      </li>
    </ul>
  </section>
</div>
