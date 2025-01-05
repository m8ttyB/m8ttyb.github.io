---
title: "Jython and unzip drama"
date: 2010-10-12
draft: false
tags: ["Java", "Python", "testing", "IBM"]
---
This post is really an effort to comfort myself after experiencing
something of a late night conniption fit while on a business trip. The
goals of the trip were many but my role was simple, assist our Solution
Architect in *un-buggering* a few problems at a client site in North
Carolina. The *buggering* I'm referring to was frustrating bad
performance of our product.

Our product is an enterprise web-based solution for solving complex
print workflows. Due to the sensitivity of the data, our product is run
 on-prem by our customers, and air-gapped from other networks.
Ever wonder how your bank statements, cell phone bills, or even your IRS
statements are printed, stuffed, mailed, and tracked? We'll those are the 
sorts of strangely exciting problems we work with. I'm not being facetious here... 
this can be some seriously honest-fun.

I was sitting in my hotel room bed with my laptop propped open on my
lap, it was around 2 am, and I had a bad horror movie playing on the TV
set, a Colorado beer near at hand, and my in-room jacuzzi was slowly
filling with hot water... don't ask, I tend to get into the zone when
there's background distraction (I've always blamed it on my ADD).

At 9am my colleague wanted to begin a series of load tests to begin zeroing
in on the problem areas. The script I was working on would provide the
means to throw seriously large amounts of data at the customers systems,
we wanted to observe the systems when they were churning hard.

```python
import os... do a bunch of fairly nifty stuff...
os.popen("unzip large_zip_file.zip")
... do a whole lot more nifty stuff, before being mean to the server...
```

The script was meant to unzip an archive, modify several of the unzipped
files, and then do nasty things to the servers by injecting the files
into parts of our workflow. What was perplexing me was that the script
seemed to work fine most of the time. Large zip files (2+ gb) seemed to
illicit perplexing behavior sometimes. After being confused for awhile,
it appeared that the unzipping wouldn't quite finish before the
remainder of the script would start to run. Before I go much further I
should add some constraints to this exercise, I am stuck with Java 1.4
and Jython 2.5.0.

It made sense to try for a solution confined to Python's api instead of
reaching out to the OS.  A solution that still still didn't work for my
needs (code snippet [credit goes to Corey Goldberg](https://coreygoldberg.blogspot.com/). 
Jython (at least2.5.1) cannot handle large files, http://bugs.jython.org/issue1253. A
Java OutOfMemory Error is thrown.

```python
import zipfile
file_handler = open('foo.zip', 'rb')
    zip_files = zipfile.ZipFile(file_handler)
    for name in zip_files.namelist():
        outfile = open(name, 'wb')
outfile.write(zip_files.read(name))
outfile.close()
file_handler.close()
```

Time to try hacking something together in Java since I can harness the
power of Java in Jython (code snippet credit goes to [java_geek on
StackOverflow](https://stackoverflow.com/questions/2257620/outofmemory-error-while-trying-to-extract-a-large-jar-using-zipfileset)).
Again I was faced with an out of memory error alongdue to the limitation 
of the runtime environment... aargh!

```java
import java.io.*;
import java.util.zip.*;

public class UnZip {
       final int BUFFER = 4096;
       public static void main (String argv[]) {
          try {
             BufferedOutputStream dest = null;
             FileInputStream fis = new FileInputStream(argv[0]);
             ZipInputStream zis = new ZipInputStream(new BufferedInputStream(fis));
             ZipEntry entry;
             while((entry = zis.getNextEntry()) != null) {
                System.out.println("Extracting: " +entry);
                int count;
                byte data[] = new byte[BUFFER];
                // write the files to the disk
                FileOutputStream fos = new FileOutputStream(entry.getName());
                dest = new BufferedOutputStream(fos, BUFFER);
                while ((count = zis.read(data, 0, BUFFER)) != -1) {
                   dest.write(data, 0, count);
                }
                dest.flush();
                dest.close();
             }
             zis.close();
          } catch(Exception e) {
             e.printStackTrace();
          }
       }
    }
```

The quick fix that I ended up implementing was to explicitly shell out a
*subprocess* to ensure the command finished running. This is was
suboptimal but I was tired.

```python
import subprocess
unzip_file = subprocess.Popen("unzip " + "large_zip_file.zip", shell=True)
unzip_file.wait()
```

After getting home from the trip I came across this solution (credit
goes to [S.Lott on StackOverflow](https://stackoverflow.com/questions/339053/how-do-you-unzip-very-large-files-in-python). Much cleaner and OS agnostic.

```python
import zipfile
import zlib
import ossrc = open( doc, "rb" )
zf = zipfile.ZipFile( src )
for m in  zf.infolist():    # Examine the header
        print m.filename, m.header_offset, m.compress_size, repr(m.extra), repr(m.comment)
        src.seek( m.header_offset )
        src.read( 30 ) # Good to use struct to unpack this.
        nm= src.read( len(m.filename) )
        if len(m.extra) > 0: ex= src.read( len(m.extra) )
        if len(m.comment) > 0: cm= src.read( len(m.comment) )    # Build a decompression object
        decomp= zlib.decompressobj(-15)    # This can be done with a loop reading blocks
        out= open( m.filename, "wb" )
        result= decomp.decompress( src.read( m.compress_size ) )
        out.write( result )
        result = decomp.flush()
        out.write( result )
        # end of the loop
        out.close()zf.close()
    src.close()
```
