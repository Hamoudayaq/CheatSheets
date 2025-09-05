---
layout: default
---
[RHCSA](./).
[back](./another-page.html).

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