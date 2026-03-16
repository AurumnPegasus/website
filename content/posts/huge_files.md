---
title: "Reversing Huge Files"
date: 2021-09-13T12:00:06+09:00
tags:
  - system
---

<p>This blog contains an explanation and tutorial of how to reverse very large files in C</p>
<h2 id="introduction">Introduction<a href="#introduction" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>To understand requiring a different method to read and reverse huge files, one must first understand the standard process. Your system loads the program you want to execute into the RAM, and since you are reading the whole file at once, it loads it into the RAM. Now, in standard systems, RAM is limited to about 4-8 GB; hence, it cannot load the whole file into the RAM at once.
A simple work-around to this is not to load the file into the RAM at all. If you read everything directly from the disk storage, your program would execute, but it will become excruciatingly slow.</p>
<h2 id="problem">Problem<a href="#problem" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>The problem here is that we are given three values: path to the file, number of parts we want to divide the text into and the block we want to reverse. Just for a simple explanation, we will assume that the text length is perfectly divisible by the number of parts given.</p>
<h2 id="solution">Solution<a href="#solution" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>To solve this, instead of trying to either load the whole text into the RAM or not load it at all, we will load it block by block.  The text file will remain in the disk, but we will move the cursor so that it loads just X amount of characters into the RAM each time.</p>
<h4 id="setup">Setup<a href="#setup" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>First, we read the given arguments and get our source and destination files ready. It is important to know what each of the flags here do. For a better understanding, refer to its <a href="https://man7.org/linux/man-pages/man2/open.2.html">man page.</a></p>
{{< highlight c >}}

// O_RDONLY: Read Only  
int source = open(value[1], O_RDONLY);

// O_WRONLY: Write Only
// O_CREAT: Create the file if it does not exist
// O_TRUNC: Overwrite the file
int destination = open("output.txt", O_WRONLY | O_CREAT | O_TRUNC, 0600);

int total_parts = *value[2] - 48;
int index = *value[3] - 48;
{{< /highlight >}}

<h4 id="main-code">Main Code<a href="#main-code" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>The star of our show is <a href="https://man7.org/linux/man-pages/man2/lseek.2.html">lseek.</a> It is a system call which repositions the cursor as we want (without loading it into the RAM). Before going into the main logic, we need to define some variables.</p>
{{< highlight c >}}
  // in the source file, go the end of file and offset by 0
  off_t source_size = lseek(source, 0, SEEK_END);

  // block size
  long limit = 1024;

  // Length of each part of source file
  long each_part = source_size/total_parts

  // Start and ending position of part we need to reverse
  long start = (index - 1)*each_part;
  long end = (index -1)*each_part + each_part

  // Arrays for storing read and reversed part
  char *r = (char *) malloc(limit);
  char *rev = (char *) malloc(limit);
{{< /highlight >}}

<p>Now, onto the main logic (which is surprisingly small)</p>
{{< highlight c >}}
  for(long i=start+limit; i<end; i+=limit){

      // SET the cursor at the position given, which is end - i + start 
      lseek(source, end -i + start, SEEK_SET);

      // Read limit number of characters from source and store it into r
      // It is important to note that the cursor moves ahead limit amount of times due to reading
      read(source, r, limit);

      // Reverse the read array
      for(int j=0; j<limit; j++){
          rev[j] = r[limit -1 -j];
      }

      // SET the cursor at the position given, which is i - limit - start
      lseek(destination, i -limit -start, SEEK_SET);

      // Write the number of characters from rev into destination
      // It is important to note that the cursor moves ahead limit amount of times due to writing
      write(destination, rev, limit);

      if(i +limit >= each_part){
          // This part is left as an excercise, think why this is needed and what will come here
          // Also think about better ways to structure the loop so that you do not require this :p
      }
  }
{{< /highlight >}}
