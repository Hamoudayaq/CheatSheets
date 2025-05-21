---
layout: default
---

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














[back](./another-page.html)
