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