# SFTP Only OpenShift Quickstart
This is a quickstart to allow for users who aren't particularly fond of the command line and/or git and would prefer to use SFTP to manage their application data. This quickstart will still require the installation of the [rhc client tools](https://developers.openshift.com/managing-your-applications/client-tools.html) on the end user's computer.

## Configuration Process
1. Install the Red Hat tools required to work in OpenShift. The following documents will walk you through the tool installation process: [https://developers.openshift.com/managing-your-applications/client-tools.html](https://developers.openshift.com/managing-your-applications/client-tools.html) This document will set the "broker server" to the RedHat OpenShift Online environment. If you would prefer to use an OpenShift Enterprise installation (such as [Carolina CloudApps](http://cloudapps.unc.edu)) you must enter that during setup.  
  
  During the setup process, the rhc client tools will ask to create a pair of keys for you (or use your existing keys). If you create new keys, the output of the client tools setup will include something along the lines of `Created: /Users/myusername/.ssh/id_rsa.pub` (the path will be different on Windows but the output should be similar.) You should take note of the path to this file before proceeding. 

2. Create a new gear using the following command.  
  `rhc app-create gearname php-5.4 --from-code https://github.com/michaelryanmcneill/sftp-only-openshift.git`
  
  You’ll receive output that looks something like this:
  ```
Application Options
-------------------
Domain:     namespace
Cartridges: php-5.4
Source Code: https://github.com/michaelryanmcneill/sftp-only-openshift.git
Gear Size:  default
Scaling:    no

Creating application 'gearname' ... done


Waiting for your DNS name to be available ... done

Cloning into 'gearname'...
Warning: Permanently added the RSA host key for IP address '127.0.0.1' to the list of known hosts.

Your application 'gearname' is now available.

  URL:        http://gearname-namespace.rhcloud.com/ 
  SSH to:     XXXXXXXXXXXXXXXXXXXXXXXX@gearname-namespace.rhcloud.com
  Git remote: ssh://XXXXXXXXXXXXXXXXXXXXXXXX@gearname-namespace.rhcloud.com/~/git/gearname.git/
  Cloned to:  /Users/myusername/Git/gearname 

Run 'rhc show-app gearname' for more details about your app.
```
3. Copy the username portion of the SSH information (the information before `@gearname-namespace.rhcloud.com`) and use it as the username in your SFTP client. 

5. Set the host to connect to `gearname-namespace.rhcloud.com` over port `22`.

6. Leave the password field blank, and select “Public Key Authentication” (the setting will be located in different locations depending on the tool you use to connect.) When prompted, select the location of the id_rsa file (not id_rsa.pub that is mentioned by rhc) using the path from step 1 above.

7. Once complete, you should be able to successfully login to your gear. You should only write to one directory, and that is the html directory located at `~/app-root/data/html/`. You can set this in you SFTP client as a path, or you can just navigate to it. Any content you place in this folder will be what gets served by your application. The deploy action hook puts a temporary index.php page in place, but you are free to modify or remove that page as necessary.
