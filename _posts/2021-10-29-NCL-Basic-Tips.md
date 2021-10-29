---
layout: post
title: "NCL Basic Tips"
permalink: /:categories/:year/:month/:day/:title/
---

The National Cyber League (NCL) is a fun (mostly) Jeopardy style capture the flag (CTF) competition that has two seasons every year: the Spring Season and the Fall Season. NCL is only for college students and is a _great_ learning opportunity. At the time of this writing, I am in my last few weeks of school getting my BAS in Cybersecurity, so this Fall Season is my last time playing. I wrote this post as a way to give newer players some basic tips to help springboard your learning.

First of all, don’t spend too much time on any one challenge. Try it, spend a few minutes on it, then move on. Come back later. Now, for the rest I've broken it down by category.

# Open Source Intelligence (OSINT)

* Google is your friend! Learn how to use the right search terms to find what you want, and quickly

* Reverse image search

# Cryptography

* [Rumkin](http://rumkin.com/tools/cipher/): This has a big list of common ciphers. Sometimes you just have to guess and check several different ones until you get familiar with them. NCL typically uses the following:
    * Atbash
    
    * Base64

    * Caesarian Shift

    * Morse Code

    * Letter Numbers

    * Railfence

* [CryptoCorner Frequency Analysis](https://crypto.interactive-maths.com/frequency-analysis-breaking-the-code.html): This is just one of many tools that CryptoCorner has. It lets you input the ciphered message, and then test different substitutions. It also tells you the most common letters and letter patterns in the English language, then compares them with the most common ciphered letters and patterns.

* [dCode](https://www.dcode.fr/): This website is in French, but Chrome (and I think Firefox) have the ability to translate it for you. It has LOTS of cipher tools, many of which have a brute force option.

* [RapidTables](https://www.dcode.fr/): This tool translates different number systems between each other, such as ASCII, hexadecimal, binary, and Base64. Don’t forget the links listed along the right side that are also very helpful!

# Password Cracking

* Generating hashes

    * [Online tool](https://www.md5hashgenerator.com/): This is one of many tools online that can generate hashes. This one specifically generates MD5 and SHA1 hash values.

    * I recommend learning as much Linux as possible, therefore you can use the terminal in Ubuntu or Kali:

        * `echo -n [string] | md5sum`

        * `echo -n [string] | sha1sum`

        * `echo -n [string] | sha256sum`

* Cracking easy passwords

    The quickest method for the “Easy” challenges is to copy and paste the hash into an online tool such as CrackStation.

* Cracking passwords with the rockyou list

    Use Hashcat or John the Ripper in Kali Linux. My favorite is Hashcat. Be aware, this will take some time depending on your system’s resources. Also be aware that your system can potentially overheat, again depending on your resources. It has never happened to me on an NCL challenge, even running on a VM on my laptop, but it is a possibility. Here’s how I use Hashcat:

    * Create a new text file called to-crack.txt and copy/paste the hashes, one per line.

    * Kali comes with the rockyou list, but it is zipped by default, so if you haven’t unzipped it yet, run this command:
       
        <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo gzip -d /usr/share/wordlists/rockyou.txt.gz</code></pre>

    * With the rockyou list unpackaged, you can run Hashcat:
        
           <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> hashcat -a 0 -m 500 to-crack.txt /usr/share/wordlists/rockyou.txt -o cracked.txt</code></pre>

        * Here’s the explanation:
 
* Cracking passwords with a custom wordlist:

    NCL usually has a challenge that says something like "It appears that the passwords are all in the format: "SKY-BMYS-" followed by 4 digits. Can you crack them?" For this, you have to create your own custom wordlist. There are various tools, like Crunch, that will do this but require some learning to get what you actually want. I just use Excel, here’s how:

    * Open Excel and enter **0** into cell _A1_

    * Highlight A1, then from the _Ribbon_ > _Home_ tab > _Editing_ section, select **Fill** > **Series**

    * Select _Series in_ **Columns**, _Type_ **Linear**, _Step Value:_ **1**, _Stop Value:_ **9999**, then click **OK**

    * In cell B1, enter the following: `=CONCATENATE("SKY-BMYS-",TEXT(A1,"0000"))`

        * Make sure the letters between the dashes match whatever the actual challenge is.

    * Double-click the Fill Handle in the bottom-right corner of cell B1 to auto fill the function all the way down.

    * Copy the whole list of values and paste them into Notepad, then save the file as something meaningful, like `sky.txt`

    * Transfer the file to Kali (how you do this will depend on how you’re using Kali. If you’re using a Kali VM in VirtualBox, you can use the VirtualBox File Manager)

    * This step might not be necessary, but as a precaution it’s a good idea to make sure a text file created in Windows will be properly read in Linux, so assuming you’re current working directory is the one with the sky.txt file, enter this command into the terminal: `dos2linux sky.txt`

    * Now run Hashcat just like above but change the file with hashes (making sure to create such a file with the relevant hashes) and the wordlist. You might also want to change the name of the output file, but you don’t have to.

# Log Analysis

Some log analysis can be done in Excel, by simply opening the files in Excel and applying sorts and filters. A really handy tool in Excel is Tables. On the _Ribbon > Home tab > Styles_ section, click **Format as Table**, then select a color scheme. If your data already has headers, check the box. The dropdown arrows for each column make sorting and filtering very easy and user-friendly.

However, Excel can only get you so far. To really take off with Log Analysis (and be more efficient), you need to learn a few bash commands in Linux. I’m not going to give detailed instruction on these tools, just a quick blurb about what they do. It’s up to you to go learn how to apply them.

* `grep`: this searches files for a specified string, but it gets really handy with Regular Expressions, which is a way of using wildcards and other shorthand to find strings that match a specified pattern.

* `sort`: just like it sounds, this sorts the input. By default, it sorts alphabetically, but you can specify other options.

* `uniq`: this will filter out duplicates, but only if they appear next to each other sequentially. That’s why sort is used before uniq.

* `wc`: Stands for ‘word count’ and counts words, characters, lines, etc. To make it count the number of lines, use wc -l

* Pipes: Notice I didn’t format Pipes the same way as the other tools? That’s intentional. A pipe is not a program like the others, it’s a way of redirecting the output of a command and using it as the input for another command. So you can grep something, then you can sort the output by piping it to the sort command, then eliminate duplicate values by piping it to uniq, then count how many lines you’re left with by piping to wc. That would look something like this:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> grep "GET" access.log | sort | uniq | wc -l</code></pre>

* * *

I have more tips coming soon in the following categories:

* Network Traffic Analysis

* Forensics

* Web Application Exploitation

* Enumeration and Exploitation

