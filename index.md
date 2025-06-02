---
layout: default
---

<!--/*<img src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fwww.pngplay.com%2Fwp-content%2Fuploads%2F6%2FUnder-Construction-Icon-PNG.png" alt="Description of the photo" width="500" />
-->
[back](./another-page.html).

## Monitor process activity
### Fundamental Keystrokes in top Command


```
[student@workstation ~]$ top 
```


<div class="table"><a id="idm45741847401152"></a><p class="title"><strong>Table&nbsp;8.3.&nbsp;Fundamental Keystrokes in <code class="code">top</code> Command</strong></p><div class="table-contents"><table border="1" class="table" summary="Fundamental Keystrokes in top Command"><colgroup><col class="c1"><col class="c2"></colgroup><thead><tr><th align="left" valign="top">Key</th><th align="left" valign="top">Purpose</th></tr></thead><tbody><tr><td align="left" valign="top">
<span class="keycap"><strong>?</strong></span>
<span class="emphasis"><em>or</em></span>
<span class="keycap"><strong>h</strong></span>
</td><td align="left" valign="top">Help for interactive keystrokes.</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>l</strong></span>, <span class="keycap"><strong>t</strong></span>, <span class="keycap"><strong>m</strong></span>
</td><td align="left" valign="top">Toggles for load, threads, and memory header lines.</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>1</strong></span>
</td><td align="left" valign="top">Toggle for individual CPUs or a summary for all CPUs in the header.</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>s</strong></span>
</td><td align="left" valign="top">Change the refresh (screen) rate, in decimal seconds (such as 0.5, 1, 5).</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>b</strong></span>
</td><td align="left" valign="top">Toggle reverse highlighting for <code class="code">Running</code> processes; the default is bold only.</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>Shift</strong></span>+<span class="keycap"><strong>b</strong></span>
</td><td align="left" valign="top">Enables bold use in display, in the header, and for <span class="emphasis"><em>Running</em></span> processes.</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>Shift</strong></span>+<span class="keycap"><strong>h</strong></span>
</td><td align="left" valign="top">Toggle threads; show process summary or individual threads.</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>u</strong></span>, <span class="keycap"><strong>Shift</strong></span>+<span class="keycap"><strong>u</strong></span>
</td><td align="left" valign="top">Filter for any username (effective, real).</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>Shift</strong></span>+<span class="keycap"><strong>m</strong></span>
</td><td align="left" valign="top">Sort process listing by memory usage, in descending order.</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>Shift</strong></span>+<span class="keycap"><strong>p</strong></span>
</td><td align="left" valign="top">Sort process listing by processor use, in descending order.</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>k</strong></span>
</td><td align="left" valign="top">Kill a process. When prompted, enter <code class="code">PID</code>, and then <code class="code">signal</code>.</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>r</strong></span>
</td><td align="left" valign="top">Renice a process. When prompted, enter <code class="code">PID</code>, and then <code class="code">nice_value</code>.</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>Shift</strong></span>+<span class="keycap"><strong>w</strong></span>
</td><td align="left" valign="top">Write (save) the current display configuration for use at the next <code class="code">top</code> restart.</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>q</strong></span>
</td><td align="left" valign="top">Quit.</td></tr><tr><td align="left" valign="top">
<span class="keycap"><strong>f</strong></span>
</td><td align="left" valign="top">Manage the columns by enabling or disabling fields. You can also set the sort field for <code class="code">top</code>.</td></tr></tbody></table></div></div>

### Process Control with Signals

```
[root@host ~]# pkill -SIGKILL -u bob
```

<div class="table"><a id="idm45741847663248"></a><p class="title"><strong>Table&nbsp;8.2.&nbsp;Fundamental process management signals</strong></p><div class="table-contents"><table border="1" class="table" summary="Fundamental process management signals"><colgroup><col class="c1"><col class="c2"><col class="c3"></colgroup><thead><tr><th align="left" valign="top">Signal</th><th align="left" valign="top">Name</th><th align="left" valign="top">Definition</th></tr></thead><tbody><tr><td align="left" valign="top">1</td><td align="left" valign="top">HUP</td><td align="left" valign="top">
<code class="code">Hangup</code> : Reports termination of the controlling process of a terminal. Also requests process re-initialization (configuration reload) without termination.</td></tr><tr><td align="left" valign="top">2</td><td align="left" valign="top">INT</td><td align="left" valign="top">
<code class="code">Keyboard interrupt</code> : Causes program termination. It can be blocked or handled. Sent by pressing the INTR (Interrupt) key sequence (<span class="keycap"><strong>Ctrl</strong></span>+<span class="keycap"><strong>c</strong></span>).</td></tr><tr><td align="left" valign="top">3</td><td align="left" valign="top">QUIT</td><td align="left" valign="top">
<code class="code">Keyboard quit</code> : Similar to SIGINT; adds a process dump at termination. Sent by pressing the QUIT key sequence (<span class="keycap"><strong>Ctrl+\</strong></span>).</td></tr><tr><td align="left" valign="top">9</td><td align="left" valign="top">KILL</td><td align="left" valign="top">
<code class="code">Kill, unblockable</code> : Causes abrupt program termination. It cannot be blocked, ignored, or handled; consistently fatal.</td></tr><tr><td align="left" valign="top">15 <span class="emphasis"><em>default</em></span>
</td><td align="left" valign="top">TERM</td><td align="left" valign="top">
<code class="code">Terminate</code> : Causes program termination. Unlike SIGKILL, it can be blocked, ignored, or handled. The "clean" way to ask a program to terminate; it allows the program to complete essential operations and self-cleanup before termination.</td></tr><tr><td align="left" valign="top">18</td><td align="left" valign="top">CONT</td><td align="left" valign="top">
<code class="code">Continue</code> : Sent to a process to resume if stopped. It cannot be blocked. Even if handled, it always resumes the process.</td></tr><tr><td align="left" valign="top">19</td><td align="left" valign="top">STOP</td><td align="left" valign="top">
<code class="code">Stop, unblockable</code> : Suspends the process. It cannot be blocked or handled.</td></tr><tr><td align="left" valign="top">20</td><td align="left" valign="top">TSTP</td><td align="left" valign="top">
<code class="code">Keyboard stop</code> : Unlike SIGSTOP, it can be blocked, ignored, or handled. Sent by pressing the suspend key sequence (<span class="keycap"><strong>Ctrl</strong></span>+<span class="keycap"><strong>z</strong></span>).</td></tr></tbody></table></div></div>





## Configure and secure SSH
<div class="section"><div class="titlepage"><div><div><h4 class="title"><a id="sshconfigure-_ssh_client_configuration"></a>SSH Client Configuration</h4></div></div></div><p>You can create the <code class="code">~/.ssh/config</code> file to preconfigure SSH connections. Within the configuration file, you can specify connection parameters such as users, keys, and ports for specific hosts. This file eliminates the need to manually specify command parameters each time that you connect to a host. Consider the following <code class="code">~/.ssh/config</code> file, which preconfigures two host connections with different users and keys:</p>

<pre class="screen">[user@host ~]$ <strong class="userinput"><code>cat ~/.ssh/config</code></strong>
host servera
     HostName                      servera.example.com
     User                          usera
     IdentityFile                  ~/.ssh/id_rsa_servera

host serverb
     HostName                      serverb.example.com
     User                          userb
     IdentityFile                  ~/.ssh/id_rsa_serverb
</pre>




## Manage Network

<div class="table"><a id="idm45741845185552"></a><p class="title"><strong>Common IPv6 Addresses and Networks</strong></p><div class="table-contents"><table border="1" class="table" summary="Common IPv6 Addresses and Networks"><colgroup><col class="c1"><col class="c2"><col class="c3"></colgroup><thead><tr><th align="left" valign="top">IPv6 address or network</th><th align="left" valign="top">Purpose</th><th align="left" valign="top">Description</th></tr></thead><tbody><tr><td align="left" valign="top">
<code class="code">::1/128</code>
</td><td align="left" valign="top">localhost</td><td align="left" valign="top">The IPv6 equivalent to the <code class="code">127.0.0.1/8</code> address, which is set on the loopback interface.</td></tr><tr><td align="left" valign="top">
<code class="code">::</code>
</td><td align="left" valign="top">The unspecified address</td><td align="left" valign="top">The IPv6 equivalent to <code class="code">0.0.0.0</code>. For a network service, it might indicate that it is listening on all configured IP addresses.</td></tr><tr><td align="left" valign="top">
<code class="code">::/0</code>
</td><td align="left" valign="top">The default route (the IPv6 internet)</td><td align="left" valign="top">The IPv6 equivalent to the <code class="code">0.0.0.0/0</code> address. The default route in the routing table matches this network; the router for this network is where all traffic is sent in the absence of a better route.</td></tr><tr><td align="left" valign="top">
<code class="code">2000::/3</code>
</td><td align="left" valign="top">Global unicast addresses</td><td align="left" valign="top">The <span class="emphasis"><em>Internet Assigned Numbers Authority (IANA)</em></span> currently allocates "normal" IPv6 addresses from this space. The addresses include all the networks that range from <code class="code">2000::/16</code> through <code class="code">3fff::/16</code>.</td></tr><tr><td align="left" valign="top">
<code class="code">fd00::/8</code>
</td><td align="left" valign="top">Unique local addresses (RFC 4193)</td><td align="left" valign="top">IPv6 has no direct equivalent of the RFC 1918 private address space, although this network range is close. A site can use these networks to self-allocate a private routable IP address space inside the organization. However, these networks cannot be used on the global internet. The site must randomly select a <code class="code">/48</code> from this space, but it can subnet the allocation into <code class="code">/64</code> networks normally.</td></tr><tr><td align="left" valign="top">
<code class="code">fe80::/10</code>
</td><td align="left" valign="top">Link-local addresses</td><td align="left" valign="top">Every IPv6 interface automatically configures a link-local unicast address that works only on the local link on the <code class="code">fe80::/64</code> network.
However, the entire <code class="code">fe80::/10</code> range is reserved for future use by the local link. This topic is discussed in more detail later.</td></tr><tr><td align="left" valign="top">
<code class="code">ff00::/8</code>
</td><td align="left" valign="top">Multicast</td><td align="left" valign="top">The IPv6 equivalent to the <code class="code">224.0.0.0/4</code> address. Multicast is used to transmit to multiple hosts at the same time, and is particularly important in IPv6 because it has no broadcast addresses.</td></tr></tbody></table></div></div>

### Edit Network Configuration Files


<div class="table"><a id="idm45741844597920"></a><p class="title"><strong>Comparison of NetworkManager Settings and Key File Format File</strong></p><div class="table-contents"><table border="1" class="table" summary="Comparison of NetworkManager Settings and Key File Format File"><colgroup><col class="c1"><col class="c2"><col class="c3"></colgroup><thead><tr><th align="left" valign="top">
<code class="code">nmcli con mod</code>
</th><th align="left" valign="top">
<code class="code">*.nmconnection</code> file</th><th align="left" valign="top">Effect</th></tr></thead><tbody><tr><td align="left" valign="top">
<code class="code">ipv4.method manual</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv4]</code>
</p>
<p>
<code class="code">method=manual</code>
</p>
</td><td align="left" valign="top">Configure IPv4 addresses statically.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv4.method auto</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv4]</code>
</p>
<p>
<code class="code">method=auto</code>
</p>
</td><td align="left" valign="top">Look for configuration settings from a DHCPv4 server. It shows static addresses only when it has information from DHCPv4.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv4.addresses 192.0.2.1/24</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv4]</code>
</p>
<p>
<code class="code">address1=192.0.2.1/24</code>
</p>
</td><td align="left" valign="top">Set a static IPv4 address and network prefix.
For more than one connection address, the <code class="code">address2</code> key defines the second address, and the <code class="code">address3</code> key defines the third address.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv4.gateway 192.0.2.254</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv4]</code>
</p>
<p>
<code class="code">gateway=192.0.2.254</code>
</p>
</td><td align="left" valign="top">Set the default gateway.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv4.dns 8.8.8.8</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv4]</code>
</p>
<p>
<code class="code">dns=8.8.8.8</code>
</p>
</td><td align="left" valign="top">Modify <code class="code">/etc/resolv.conf</code> to use this name server.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv4.dns-search example.com</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv4]</code>
</p>
<p>
<code class="code">dns-search=example.com</code>
</p>
</td><td align="left" valign="top">Modify <code class="code">/etc/resolv.conf</code> to use this domain in the <code class="code">search</code> directive.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv4.ignore-auto-dns true</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv4]</code>
</p>
<p>
<code class="code">ignore-auto-dns=true</code>
</p>
</td><td align="left" valign="top">Ignore DNS server information from the DHCP server.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv6.method manual</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv6]</code>
</p>
<p>
<code class="code">method=manual</code>
</p>
</td><td align="left" valign="top">Configure IPv6 addresses statically.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv6.method auto</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv6]</code>
</p>
<p>
<code class="code">method=auto</code>
</p>
</td><td align="left" valign="top">Configure network settings with SLAAC from router advertisements.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv6.method dhcp</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv6]</code>
</p>
<p>
<code class="code">method=dhcp</code>
</p>
</td><td align="left" valign="top">Configure network settings by using DHCPv6, but not by using SLAAC.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv6.addresses 2001:db8::a/64</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv6]</code>
</p>
<p>
<code class="code">address1=2001:db8::a/64</code>
</p>
</td><td align="left" valign="top">Set a static IPv6 address and network prefix.
When using more than one address for a connection, the <code class="code">address2</code> key defines the second address, and the <code class="code">address3</code> key defines the third address.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv6.gateway 2001:db8::1</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv6]</code>
</p>
<p>
<code class="code">gateway=2001:db8::1</code>
</p>
</td><td align="left" valign="top">Set the default gateway.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv6.dns fde2:6494:1e09:2::d</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv6]</code>
</p>
<p>
<code class="code">dns=fde2:6494:1e09:2::d</code>
</p>
</td><td align="left" valign="top">Modify <code class="code">/etc/resolv.conf</code> to use this name server.
The same as IPv4.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv6.dns-search example.com</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv6]</code>
</p>
<p>
<code class="code">dns-search=example.com</code>
</p>
</td><td align="left" valign="top">Modify <code class="code">/etc/resolv.conf</code> to use this domain in the <code class="code">search</code> directive.</td></tr><tr><td align="left" valign="top">
<code class="code">ipv6.ignore-auto-dns true</code>
</td><td align="left" valign="top">
<p>
<code class="code">[ipv6]</code>
</p>
<p>
<code class="code">ignore-auto-dns=true</code>
</p>
</td><td align="left" valign="top">Ignore DNS server information from the DHCP server.</td></tr><tr><td align="left" valign="top">
<code class="code">connection.autoconnect yes</code>
</td><td align="left" valign="top">
<p>
<code class="code">[connection]</code>
</p>
<p>
<code class="code">autoconnect=true</code>
</p>
</td><td align="left" valign="top">Automatically activate this connection at boot.</td></tr><tr><td align="left" valign="top">
<code class="code">connection.id ens3</code>
</td><td align="left" valign="top">
<p>
<code class="code">[connection]</code>
</p>
<p>
<code class="code">id=Main eth0</code>
</p>
</td><td align="left" valign="top">The name of this connection.</td></tr><tr><td align="left" valign="top">
<code class="code">connection.interface-name ens3</code>
</td><td align="left" valign="top">
<p>
<code class="code">[connection]</code>
</p>
<p>
<code class="code">interface-name=ens3</code>
</p>
</td><td align="left" valign="top">The connection is bound to the network interface with this name.</td></tr><tr><td align="left" valign="top">
<code class="code">802-3-ethernet.mac-address …​</code>
</td><td align="left" valign="top">
<p>
<code class="code">[802-3-ethernet]</code>
</p>
<p>
<code class="code">mac-address=</code>
</p>
</td><td align="left" valign="top">The connection is bound to the network interface with this MAC address.</td></tr></tbody></table></div></div>


## Exploring the filesystem Hierarchy

<p>
> https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html
</p>

## Lab and special permissions

<p>Configure the <code class="code">/home/dbadmin1/grading/review2</code> directory to allow members of the <code class="code">database</code> group to create contents in it. All other users should have read and execute permissions on the directory.</p><p>Apply the SetGID special permission on the <code class="code">/home/dbadmin1/grading/review2</code> directory so that the <code class="code">database</code> group owns files that are created in the directory.</p><pre class="screen">[dbadmin1@serverb ~]$ <strong class="userinput"><code>chmod g+s /home/dbadmin1/grading/review2</code></strong></pre><p>Apply the <code class="code">775</code> permission mode on the <code class="code">/home/dbadmin1/grading/review2</code> directory.</p><pre class="screen">[dbadmin1@serverb ~]$ <strong class="userinput"><code>chmod 775 /home/dbadmin1/grading/review2</code></strong></pre>

<p>Apply the sticky bit special permission on the <code class="code">/home/dbadmin1/grading/review2</code> directory.</p><pre class="screen">[dbadmin1@serverb ~]$ <strong class="userinput"><code>chmod o+t /home/dbadmin1/grading/review2</code></strong></pre>


<h1>SSHD Configuration Notes</h1>

<div class="section">
  <h2>Disable Root Login</h2>
  <ol>
    <li>
      On <code>serverb</code>, configure the <code>sshd</code> service to prevent the <code>root</code> user from logging in:
      <ol class="substeps">
        <li>Open the <code>/etc/ssh/sshd_config</code> file with:
          <pre><code>sudo vim /etc/ssh/sshd_config</code></pre>
        </li>
        <li>Set the following parameter:
          <pre><code>PermitRootLogin no</code></pre>
        </li>
        <li>Reload the <code>sshd</code> service to apply changes:
          <pre><code>sudo systemctl reload sshd.service</code></pre>
        </li>
      </ol>
    </li>
  </ol>
</div>

<div class="section">
  <h2>Disable Password Authentication</h2>
  <ol>
    <li>
      On <code>serverb</code>, configure the <code>sshd</code> service to prevent password-based logins:
      <ol class="substeps">
        <li>Open the <code>/etc/ssh/sshd_config</code> file:
          <pre><code>sudo vim /etc/ssh/sshd_config</code></pre>
        </li>
        <li>Set the following parameter:
          <pre><code>PasswordAuthentication no</code></pre>
        </li>
        <li>Reload the <code>sshd</code> service to apply changes:
          <pre><code>sudo systemctl reload sshd.service</code></pre>
        </li>
      </ol>
      Users will now be required to use SSH keys for login.
    </li>
  </ol>
</div>


<div class="table"><a id="idm45741848109936"></a><p class="title"><strong>Linux Process States</strong></p><div class="table-contents"><table border="1" class="table" summary="Linux Process States"><colgroup><col class="c1"><col class="c2"><col class="c3"></colgroup><thead><tr><th align="left" valign="top">Name</th><th align="left" valign="top">Flag</th><th align="left" valign="top">Kernel-defined state name and description</th></tr></thead><tbody><tr><td align="left" valign="top">Running</td><td align="left" valign="top">
<code class="code">R</code>
</td><td align="left" valign="top">TASK_RUNNING: The process is either executing on a CPU or waiting to run.
The process can be executing user routines or kernel routines (system calls), or be queued and ready when in the <span class="emphasis"><em>Running</em></span> (or <span class="emphasis"><em>Runnable</em></span>) state.</td></tr><tr><td align="left" rowspan="4" valign="top">Sleeping</td><td align="left" valign="top">
<code class="code">S</code>
</td><td align="left" valign="top">TASK_INTERRUPTIBLE: The process is waiting for some condition: a hardware request, system resource access, or a signal.
When an event or signal satisfies the condition, the process returns to <span class="emphasis"><em>Running</em></span>.</td></tr><tr><td align="left" valign="top">
<code class="code">D</code>
</td><td align="left" valign="top">TASK_UNINTERRUPTIBLE: This process is also sleeping, but unlike the <code class="code">S</code> state, does not respond to signals.
It is used only when process interruption might cause an unpredictable device state.</td></tr><tr><td align="left" valign="top">
<code class="code">K</code>
</td><td align="left" valign="top">TASK_KILLABLE: Same as the uninterruptible <code class="code">D</code> state, but modified to allow a waiting task to respond to the signal to kill it (exit completely).
Utilities often display <span class="emphasis"><em>Killable</em></span> processes as the <code class="code">D</code> state.</td></tr><tr><td align="left" valign="top">
<code class="code">I</code>
</td><td align="left" valign="top">TASK_REPORT_IDLE: A subset of state <code class="code">D</code>.
The kernel does not count these processes when calculating the load average.
It is used for kernel threads.
The TASK_UNINTERRUPTIBLE and TASK_NOLOAD flags are set.
It is similar to TASK_KILLABLE, and is also a subset of state <code class="code">D</code>.
It accepts fatal signals.</td></tr><tr><td align="left" rowspan="2" valign="top">Stopped</td><td align="left" valign="top">
<code class="code">T</code>
</td><td align="left" valign="top">TASK_STOPPED: The process is stopped (suspended), usually by being signaled by a user or another process. The process can be continued (resumed) by another signal to return to running.</td></tr><tr><td align="left" valign="top">
<code class="code">T</code>
</td><td align="left" valign="top">TASK_TRACED: A process that is being debugged is also temporarily stopped and shares the <code class="code">T</code> state flag.</td></tr><tr><td align="left" rowspan="2" valign="top">Zombie</td><td align="left" valign="top">
<code class="code">Z</code>
</td><td align="left" valign="top">EXIT_ZOMBIE: A child process signals to its parent as it exits. All resources except for the process identity (PID) are released.</td></tr><tr><td align="left" valign="top">
<code class="code">X</code>
</td><td align="left" valign="top">EXIT_DEAD: When the parent cleans up (reaps) the remaining child process structure, the process is now released completely. This state cannot be observed in process-listing utilities.</td></tr></tbody></table></div></div>