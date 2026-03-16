---
title: "Setting up your PopOS"
date: 2021-10-30T05:17:12+05:30
tags:
  - os
---

<p>So I used to love ElementaryOS (still do :p) even as a programmer, since its UI was intuitive and clean, and nothing else needed to be done on top of it. During one of my assignments though, I realised the issue I was having was my OS was in Ubuntu 18.04 instead of 20.04, which was a problem. Simple enough, I tried to update it as I had tried to update by Ubuntu (you could do that simply by a command on terminal). Apparantly, not for Elementary, since it required me to
do a fresh install.</p>
<h2 id="why-popos">Why PopOS<a href="#why-popos" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Considering the reasons why I was using ElementaryOS in the first place, and the reason I moved, I felt it was the best choice. It is one of the most intuitive OS out there, meant to be user friendly (not more than Elementary though). Also, it is essentially built on Ubuntu (kinda), so for updating systems, all you need to do is write a line on terminal and boom!
It also more supportive to programmers, since it has a lots of default keybindings and window tilings, which makes it easier for me to open multiple terminals at once and get my work done without lifting my hand to use my mouse.</p>
<h2 id="get-started">Get Started<a href="#get-started" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>All you need is a empty pendrive of about 8-16GB. Go to <a href="https://pop.system76.com/%5D">https://pop.system76.com/]</a>  and click on Download. As of me posting this, the last LTS (stable) version of popOS out is 20.04, so go for it (although you can go for 21.04 as well if you want to). Just install whichever you think is a better choice for your laptop&rsquo;s specs (whether you want popOS with NVIDEA driver or not).
Once you have installed it, you need to burn the iso file onto your pendrive. You can do this by using <a href="https://www.balena.io/etcher/,">https://www.balena.io/etcher/,</a> a lightweight program which etches your drive (warning, this will delete all existing data on your pendrive, so ensure that your pendrive doesnt have any important data in it). Once etched and verified, you are good to go!</p>
<h2 id="partitions">Partitions<a href="#partitions" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Well, sorry, you are almost good to go. Last thing you need to do is create partitions on your disk. On Windows 10, its very easy to do so, just go to Disk Management (Google how to go to Disk Management if you dont know :p, its easy enough. Now, there are two cases. First, you might just have one drive (ideally SSD, but even if its HDD thats chill) or second, you might have 2 drives, one being SSD and other HDD. In general, its better if you use SSD, which is what I did, but ultimately
thats your choice.
Select the unpartitioned space and click on shrink. Allocate some memory here (dont worry, you are just preparing it for partitions, if you mess up its easy to correct at this stage). Ideally, you should allocate close to 100 GB to popOS (and ideally in SSD). Now, you have to partition that 100 GB as well, to create a smaller partition of 512 MB as well.
Once you are done with this, lets go and boot this bad boi up</p>
<h2 id="booting">Booting<a href="#booting" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>With your pendrive connected to the laptop, restart. During bootup, press F4 (you might have a different key depending, google it). Here, you will get a menu to choose whether you want to boot from a device or enter bios among other things. Choose the option to boot from your pendrive to boot into PopOS.
Once you are here, select your languages, keyboard layouts etc and install <strong>custom</strong> install. It is extremely important you choose custom, since clean install would remove your windows as well and install just popOS on your system. Click on modify partitions and then select the partition for /root (bigger partition) and /home (512MB partition). To allocate, right click on unallocated partition, click on new and keep free space as 0. Then, left click on the partition and turn the
toggle for use partition on and keep file system as ext4.</p>
<figure>
    <img src="https://bln364.com/wp-content/uploads/2020/06/23.png"/> 
</figure>

<p>So, after this, you are done with the basic installation. Now lets go on to make it customized to a programmer</p>
<h2 id="boot-option-menu">Boot option menu<a href="#boot-option-menu" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>After using your new OS for an hour or two you might have realised that you really dont get an option to shift between Windows and PopOS easily (you could just BIOS always, but thats a dumb solution). The reason is that popOS does not use <code>grub</code>, the thing which displays code on boot menu and stuff, it uses <code>systemd-boot</code> which, frankly, is kind of a pain in the ass.
There are three solutions available for this, none of them are discovered by me, its just a bit of googling and experimenting:</p>
<ol>
<li>
<p>Using <code>systemd-boot</code>: This one did not work for me, but I also did not really understand it well. But, this is a more &lsquo;proper&rsquo; method, per se: <a href="https://www.reddit.com/r/pop_os/comments/gjsr6r/psa_how_to_dual_boot_pop_os_with_windows_with_a/">https://www.reddit.com/r/pop_os/comments/gjsr6r/psa_how_to_dual_boot_pop_os_with_windows_with_a/</a></p>
</li>
<li>
<p>Using rEFInd: Honestly, the best method, makes life so so easy. rEFInd is a library which you need to install which helps in finding EFI folders (used for booting n stuff): <a href="https://www.reddit.com/r/pop_os/comments/ip1vt6/windows_doesnt_show_in_systemdboot_menu_but_its/">https://www.reddit.com/r/pop_os/comments/ip1vt6/windows_doesnt_show_in_systemdboot_menu_but_its/</a></p>
</li>
<li>
<p>Works only for multi-drive system: You manually find the windows EFI partition (dont worry, there is <code>lsblk</code> to make life easier). Once you do that, you move windows EFI to popOS EFI and add a timer to the bootloader. <a href="https://unix.stackexchange.com/questions/610779/pop-os-systemd-boot-cant-detect-windows">https://unix.stackexchange.com/questions/610779/pop-os-systemd-boot-cant-detect-windows</a></p>
</li>
</ol>
<h2 id="regolith">Regolith<a href="#regolith" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Generally speaking, popOS has a good equilibrium between having good GUI for normal users and good options like tiling, shortcuts for programmers. But, I am a programmer that prefers to use keyboard most of the time, hence I wanted to customize a bit, but since it was middle of my semester I really did not want to spend 2-3 days + just googling around different available customizations. Hence, regolith: <a href="https://regolith-linux.org/">https://regolith-linux.org/</a>. It is a desktop environment built on top of GNOME
and i3, and has amazing user interface (for programmers ofcourse).</p>
<figure>
    <img src="https://i.imgur.com/GPpZekT.png"/> 
</figure>

<p>Installing it is super easy, just type in</p>
{{< highlight bash >}}
sudo add-apt-repository ppa:regolith:linux/release
sudo apt install regolith-desktop-standard # or regolith-desktop-mobile for laptops
{{< /highlight >}}

<p>Here is the official installation guide (which is pretty much the same): <a href="https://regolith-linux.org/docs/getting-started/install/">https://regolith-linux.org/docs/getting-started/install/</a></p>
<h2 id="vim">Vim<a href="#vim" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>So, shifting to popOS and regolith is quite a new journey for me, and the important thing that I need to learn is to use keyboards for <strong>everything</strong>. There are keybindings for everything in regolith which make my life easier, but it would be wasted if I ended up just using <code>vscode</code> with my mouse for coding. So, I am actively trying to learn vim keybindings as well. For this, I looked into multiple vimrcs, tried writing my own and had to give up. A easy solution is to take the best
vimrc available on github and use it (though I would suggest you experiment a bit before so you atleast know a bit about whats happening in your vimrc). The one I chose was <a href="https://github.com/amix/vimrc">https://github.com/amix/vimrc</a> which is, honestly, just so very well done.</p>
