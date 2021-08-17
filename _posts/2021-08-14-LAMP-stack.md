---
layout: post
title: "LAMP Stack with Customized DVWA"
permalink: /:categories/:year/:month/:day/:title/
---

Currently, I’m working for both Pacific Northwest National Laboratory and Marcraft, an Educational Technologies Group company. For both jobs recently, I had the opportunity to set up multiple web servers in Linux. For one of these projects I also set up a whole LAMP stack and the Damn Vulnerable Web Application (DVWA), but I wanted to customize it with a new look. I also wanted to have a fake front end website that has a link for “employees” to login with, which would take them to the DVWA login page.

For starters, I’m most familiar with Ubuntu and CentOS 7, so I started this project in Ubuntu (not Ubuntu Server, which was probably a mistake). I tried and failed probably 5 times to get MySQL or MariaDB set up! So then I used CentOS 7 and it worked. Soon I’ll do it all again either with CentOS Stream or with Rocky, but for now it’s CentOS 7.

A few things to note:
* Somebody else created the web files for the front end for me, I just added them to the right directories.
* I’m working on a Windows 10 machine running VirtualBox and building the server on a VM
* I’m writing this as a step-by-step tutorial
* Turns out the boss wanted the VM on Hyper-V, so there’s instructions for how to convert it for Hyper-V

Alright, here we go!

* * *

## Set up the DVWA

1.	Download the VirtualBox OVA file from [Linux Images](https://www.linuxvmimages.com/images/centos-7)

    Note: 
    
    > I used the Minimal Install to save on storage and memory resources, but you can use a desktop environment if you want

2.	Import the VM from the OVA file
    * Open VirtualBox and click **File > Import Appliance**
    * Click the **Browse** icon and locate the OVA file, then click **Import**
3.	Change the **Network Adapter**
    * Open the VM **Settings**
    * Click **Network**, then select **NAT** from the **Attached to:** dropdown menu
    * Click the right-pointing arrow next to _Advanced_, then click **Port Forwarding**
    * Click the icon to add two new rules, then use the variables in the table below:

    | Name | Protocol | Host IP   | Host Port | Guest IP  | Guest Port |
    |:-----|:---------|:----------|:----------|:----------|:-----------|
    | HTTP | TCP      | 127.0.0.1 | 80        | 10.0.2.15 | 80         |
    | SSH  | TCP      | 127.0.0.1 | 22        | 10.0.2.15 | 22         |

    * Click **OK**, then click **OK** again

    ![VM Network Configuration](/assets/images/dvwa/vm-network-settings.png)

4.	Take a snapshot
    * With the VM highlighted, click the icon on the right side with 3 dots and horizontal lines, then click Snapshots
    * On the new screen, click **Take**, then give it a name such as _Fresh Install_, then any comments if you wish

    ![VM Snaptshot](/assets/images/dvwa/vm-snapshot.png)

5.	Start the VM and login with the following credentials:
    * User: centos
    * Password: centos
6.	Update the repositories, then install the needed packages:

    <pre>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo yum update</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo yum install nano git httpd mariadb-server mariadb php php-mysql php-gd</code>
    </pre>

10.	Start the Apache service and allow it to start automatically on reboot:

    <pre>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo systemctl start httpd.service</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo systemctl enable httpd.service</code>
    </pre>

13.	Start the database service:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo systemctl start mariadb</code></pre>

14.	Run the MySQL security script:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo mysql_secure_installation</code></pre>

    * When prompted for root password, simply press `ENTER`
    * When asked to if you want to create a root password, enter `y`, then enter a password
    * Press `y` at each prompt to remove some sample users and databases and disable remote root logins

15.	Enable MariaDB to start on boot:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo systemctl enable mariadb.service</code></pre>

17.	Restart the Apache service:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo systemctl restart httpd.service</code></pre>

19.	Open MariaDB to create the DVWA database:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> mysql -u root -p</code></pre>

    * Enter the password you entered in step 12b above

20.	Create the database:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">>></span> create database dvwadb;</code></pre>

    > This can be any name, but whatever you use, take note – it must match a configuration file later on.

21.	Create DVWA database user with all the privileges assigned on the DVWA database:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">>></span> grant all on dvwadb.* to dvwamgr@localhost identified by 'mypassword';</code></pre>

    > Again, the username (dvwamgr) and password (mypassword) can be whatever, as long as you take note of it and match it to the config file

22.	Reload the privileges table and exit:

    <pre>
    <code><span style="color:rgba(255, 255, 255, 0.5)">>></span> flush privileges;</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">>></span> exit</code>
    </pre>

24.	Open the _php.ini_ file, then edit the specified lines as necessary:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo nano /etc/php.ini</code></pre>

    ```
    allow_url_fopen = On
    allow_url_include = On
    ```

26.	Press `ctrl + x`, then `y`, then `ENTER` to exit and save changes

28.	Download the DVWA files:

    <pre>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> cd /var/www/html</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo git clone https://github.com/ethicalhack3r/DVWA</code>
    </pre>

30.	Rename the sample configuration file, then open it and edit the following variables to match the database name, username, and password you used above:

    <pre>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> cd /var/www/html/DVWA/config</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo cp config.inc.php.dist config.inc.php</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo nano config.inc.php</code>
    </pre>
    
    ```
    $_DVWA[ 'db_database' ] = 'dvwadb';
    $_DVWA[ 'db_user' ]     = 'dvwamgr';
    $_DVWA[ 'db_password' ] = 'mypassword';
    ```

33.	Restart the database and web services:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo systemctl restart mariadb httpd</code></pre>

34.	Configure SELinux with the following commands:

    <pre>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo setsebool -P httpd_unified 1</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo setsebool -P httpd_can_network_connect 1</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo setsebool -P httpd_can_network_connect_db 1</code>
    </pre>

35.	Give Apache ownership of all web server files:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo chown -R apache:apache /var/www/html</code></pre>

36.	On the host, navigate to **127.0.0.1/DVWA** in a web browser

37.	On the _Server Check_ screen, make sure the only variables in red are the same as the ones displayed in the screenshot (reCAPTCHA key, allow_url_fopen = On, and allow_url_include = On)

    ![DVWA Server Check Screen](/assets/images/dvwa/DVWA-server-check-screen.png)
 
38.	Click the **Create / Reset Database** button. When the page refreshes, it will take you to the DVWA login page

At this point, the DVWA has been set up and is ready for use. However, I mentioned in the beginning that I wanted to customize it with a new theme and new logo.

* * *

## Customize the DVWA

For my style, I wanted to change the colors to a blood red and change the logo to one for the traditional fake company: ACME.

39.	On the VM, backup the original logos:

    <pre>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> cd /var/www/html/DVWA/dvwa/images</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo mv login_logo.png login_logo.png.bak</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo mv logo.png logo.png.bak</code>
    </pre>

41.	On the host, open **WinSCP** and login to the VM

42.	Copy the logo.png file from the host machine to the home directory of the VM

43.	On the VM, you should still be in the _/var/www/html/DVWA/dvwa/images_ directory. Move the logo.png file to the directory, then copy it as login_logo.png:

    <pre>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo mv ~/logo.png ./</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo cp logo.png login_logo.png</code>
    </pre>

46.	Change the display size of the logo.png image:

    <pre>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> cd /var/www/html/DVWA/dvwa/includes</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo nano dvwaPage.inc.php</code>
    </pre>

    * On line 302 (press `alt + c` to display line numbers), add **width=\"150\"**  to the line so that it reads:
    ```
    <img src=\"" . DVWA_WEB_PAGE_TO_ROOT . "dvwa/images/logo.png\" alt=\"Damn Vulnerable Web Application\” width=\"150\" />
    ```

48.	Change the display size of the login_logo.png image:

    <pre>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> cd /var/www/html/DVWA</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo nano login.php</code>
    </pre>

    * On line 83, add width=\"150\"  to the line so that it reads:

    ```
    <p><img src=\"" . DVWA_WEB_PAGE_TO_ROOT . "dvwa/images/login_logo.png\" width=\"300\" /></p>
    ```

50.	Edit the colors in the CSS:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo nano /var/www/DVWA/dvwa/css/main.css</code></pre>

    * Line 19: color: #ac2424;
    * Line 121: background: #ac2424;
    * Line 122: border-bottom: #2f1912;
    * Line 171: background-color: #ac2424;
    * Line 195: border-top: 5px solid #ac2424;

Sweet! That was easy. Now we have a new ACME logo and a red color theme. You might need to restart the Apache service, then you can reload the browser to see the new look.

* * *

## Adding a fake front end

The idea with this is to provide some OSINT practice and multiple layers of attack. Like I said in my intro, somebody else made the web files for me, which I'll just refer to as ACME web files. To maintain the theme that the DVWA is actually the inside portion of an employee portal, I'm going to change the names of the buttons in the main DVWA menu.

52.	Use WinSCP from the host to copy the folder containing the ACME web files to the home directory of the VM

53.	Copy the ACME web files to the Apache directory:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo cp -R ~/ACME/* /var/www/html/</code></pre>

54.	Change directory:
55.	Open the _dvwaPage.inc.php_ config file to change the button names:

    <pre>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> cd /var/www/html/DVWA/dvwa/includes</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo nano dvwaPage.inc.php</code>
    </pre>

56.	Edit the button “names” according to the table below, modifying only the parts highlighted in the example.

    Note:
    
    > The first button, Brute Force, is on Line 199

    Original line:

    <pre style="color:#d0d0d0">
    $menuBlocks[ 'vulnerabilities' ][] = array( 'id' => 'brute', 'name' => '<span style="color:#ac2424">Brute Force</span>', 'url' => 'vulnerabilities/brute/' );
    </pre>

    Modified line:

    <pre style="color:#d0d0d0">
    $menuBlocks[ 'vulnerabilities' ][] = array( 'id' => 'brute', 'name' => '<span style="color:#ac2424">Admin Login</span>', 'url' => 'vulnerabilities/brute/' );
    </pre>

    | Orginal Button Name   | Modified Button Name |
    |:----------------------|:---------------------|
    | Brute Force           | Administrator Login  |
    | Command Injection     | Network Tool         |
    | CSRF                  |                      |
    | File Inclusion        |                      |
    | File Upload           |                      |
    | Insecure CAPTCHA      |                      |
    | SQL Injection	ACME    | Catalog              |
    | SQL Injection (Blind)	| ---                  |
    | Weak Session IDs      |                      |
    | XSS (DOM)             |                      |
    | XSS (Reflected)       |                      |
    | XSS (Stored)          |                      |
    | CSP Bypass            |                      |
    | Javascript            |                      |

    Note:

    > The Modified values in this table are only suggestions, and is incomplete. You will need to come up with ideas for the rest.

57.	On the host, type up the text for a replacement message of the DVWA homepage, which you will insert into the _index.php_ file on the server.

59.	On the VM, make a backup copy of the _index.php_ file:

    <pre>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> cd  cd /var/www/html/DVWA</code>
    <code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo cp index.php index.php.bak</code>
    </pre>

60.	On the host, use _WinSCP_ to pull a copy of the _index.php_ file to the host.

61.	On the VM, delete the original _index.php_ file:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> sudo rm index.php</code></pre>

62.	On the host, open the _index.php_ file in a text editor such as Notepad, Notepad+, etc, then edit it with the replacement message, being careful to use the appropriate html tags and to not delete necessary portions.

63.	Using _WinSCP_, copy the modified _index.php_ file back to the _/var/www/html/DVWA_ directory on the VM

64.	Shutdown the VM:

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">$</span> shutdown now</code></pre>

65.	Take another VM snapshot, giving it a relevant name

All done!