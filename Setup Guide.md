Big thanks to Jim Shultz for his [amazing guide](https://www.youtube.com/watch?v=aqA6bktFHoY&ab_channel=JimSchultz). Great channel with not enough recognition!

As per Microsoft, "A directory is a hierarchical structure that stores information about objects on the network. A directory service, such as Active Directory Domain Services (AD DS), provides the methods for storing directory data and making this data 
available to network users and administrators. For example, AD DS stores information about user accounts, such as names, passwords, phone numbers, and so on, and enables other authorized users on the same network to access this information."

This infrastructure is commonly used in businesses and companies that have offices and computers which need to be accessed by employees. It is necessary where users will need to created and managed with permissions and abilities as to what 
they can do on the company network. It also can allow Help Desk members to assist users with password resets and more. 

For this home lab we will be using [Windows Server 2022](https://info.microsoft.com/ww-landing-windows-server-2022.html) and a [Windows 11](https://www.microsoft.com/software-download/windows11) machine. Both of these will be done through Virtualbox.

First off both iso files will need to be downloaded through the links above. 
Once downloaded you should verify the files integrity. 

In Virtualbox, we will then click on File, hover Tools, and click Network Manager. Click on the NAT Networks tab and then create while naming it something you will remember, default settings are fine for home lab purposes but may need to be 
tailored to specific company requirements in a production setting. Creating this network will allow the server to connect with the Windows 11 client.

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/7af95e6f-37cb-423f-a474-cfac9590ce43)



Now, go to the Machine tab and click on New, to create a new Virtual Machine. 
The name of the VM should relate to the function it is performing. Here we will start with the Windows Server 2022. 
Name the VM something along the lines of Server-AD. Save to an appropriate folder, default is fine.  

In the ISO Image section, click the dropdown and go to Other and navigate to where the Windows Server 2022 and add it. 
The Edition dropdown is now selectable and choose either the Standard or Datacenter Evaluation but be sure to select one that is a Desktop Experience.
Then click Skip Unattended Installation, and Next.

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/85ec714e-17b3-4faf-a777-eef6de4d20b6)



Depending on the host machine hardware the settings here will vary.
I recommend at least 2-4GB of RAM (Base Memory) and 1-2 Processors, anything less will increase wait times and can lead to crashing issues. But for home lab purposes we are just looking to get the machines up and running. 
Press next and Create a Virtual Hard Disk Now, lower this to around 20GB or so, click next and Finish.

Once finished, right click on the newly created VM and go to settings. Here you will go to Network and in the Attched to drop down, select NAT Network and the Name dropdown should select the newly created one. 

To create the Windows 11 machine we will follow the same process as above with the only major differences being which iso is selected in the initial configuration screen, and adding additional processors(4 should be good)/RAM(the more RAM, the smoother the process). 

We will begin the setup process with  Windows Server VM first, so right click that machine and press start.
Once the VM has booted you will see a screen titled Microsoft Server Operating System Setup. And if you are okay with the languages being English press next, otherwise change to desired language and continue. 
Click Install Now and again select one of the two Desktop Experience options, I will go with Datacenter Evaluation.
Next, accept the terms and press next. We will be doing a custom install and installing the server to the premade drive that should be allocated about 20GB. 
From here loading it up is dependant on the specs given but should take about 5-10 minutes. 

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/b9571877-80ec-4585-802c-291717761a4e)

Once that machine has rebooted, you will create the Admin account. For home labs purposes the password will still need to follow the guidelines but it can be something easily remembered. 
Click next and from there you may need to press the Input tab, hover Keyboard and press the CRTL + ALT + DELETE option. Enter the Admin password you just made.

Now, the first that should be done to increase functionality is to install Guest Additionals to the machine. If you do not have Guest Additions, please follow this [link](https://www.linkedin.com/advice/0/how-do-you-use-virtualbox-extensions-guest-additions#:~:text=To%20install%2C%20start%20your%20virtual,and%20accept%20the%20default%20options.) to a Linkedin post explaining where to get and add to Virtualbox.
Once you have it downloaded we then need to add it to the VM. This is done by pressing the Devices tab and pressing Insert Guest Additions CD. 
Then go to the File Explorer and click on CD Drive. In here we will select the -amd64 Application and run it. Continue through by clicking next. If you would like the larger screen immediately you can reboot the system now, 
otherwise it will be rebooted shortly.

The next change we will make is to change the PC Name by typing name in the search bar and selecting View Your PC name.
Click Rename this PC and change it to something easily remembered like AD-PC. It will take a reboot for this to take effect.

Once changed, type Control Panel in the search bar and then go to Network and Internet, and then the Network and Sharing Center. 
In this main screen, click on Ethernet. The next menu we will click Properties, and then find Internet Protocol Version 4. 
Select it and hit properties once again. 

Leaving that tab open, in the search bar below, type cmd to open the command prompt.
In the command prompt, type ```ipconfig```.

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/c4144f05-cfb2-42f5-a17f-1e36dab88e53)


With that command prompt still open, go back to the IPv4 tab and click Use the following IP address. 
In here we will enter the information found in the IP config command. Copy over the IPv4 address, subnet mask, and default gateway. 

Now, in the perferred DNS we will use the loopback address of 127.0.0.1 and then a public DNS such as Google's 8.8.8.8.
Remember these configurations are different for each machine so do not copy the IP addressing from the picture below but the information given in your ipconfig. 

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/fa674493-3cfd-4057-a736-96741b313670)


That is all set now and the beginner of the server setup has been finished. So we will go and reboot this machine. 

Once it is back online, log in the same way we did previously. 
You can also now go to View, and click Virtual Screen 1 to adjust the screen size. If this does not work please consult this [link](https://forums.virtualbox.org/viewtopic.php?t=68966) in the Virtualbox forums.

Continuing on, in the Service Manager app, click on the Add roles and features option.
Click next, choose role-based, next again and then click Active Directory Domain Services and then Add Features. 
Using all of the defaults until Install is able to be clicked in which we will click it. 

Now, once that is finished, click the flag with the yellow warning sign and click Promote this server to a domain controller.
Since this is just a home lab configuration we will select Add a new forest and create a Root domain name. For me I used addomain.test.
Continue on, and leave the defaults for domain controller capabilities. Create a password, it can be the same as configured as this is just for testing. 

Click next, on the DNS options. Addional options are fine as the default location as well. 
In paths, the defaults are ok. Click next on Review Options again. And in Installation click Install as long as there are no errors, warnings are ok.
Once finished the server will then restart.

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/118c0e68-6e9d-44a2-90cc-af59dd3ec6b0)

Notice the new username as it is now attached to the domain we previously created. Log in as normal. Reopen a command prompt and check the ipconfig but this time with a /all suffix.
```ipconfig /all```
As long as it is the same as previously created with the 127.0.0.1 and 8.8.8.8 DNS servers we can continue. You can see in the screenshot below, the public DNS I added was removed.
We will just re-add it the same was as done originally.

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/ece7d949-c99a-41c2-a59c-b7b5f61d8fe6)


The domain controller is now functional and we are ready to set up the Windows 11 VM.

Depending on the system resources, you may want to power down the server before starting this machine.
Just like the server, we will power on this machine by rightclicking and pressing Start.
In the first pop up, again select the correct languages and press next. Then Install now.
Click I don't have a product key, and then be sure to select a Windows Pro version while avoiding any "N" versions as well.

For mine I will choose Windows Pro Education but Windows Pro works fine as well. 
And we will also be doing a Custom Install for this. Select the drive and press next.
Again, this installation depends on the resources allocated and host system. It should take about 5-10 minutes.

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/63c81b53-5371-45f3-9deb-22cdbae70901)


Once the Windows 11 machine has been restarted, go through the basic configuration settings on a personalized basis.
The VM will automatically restart after it has checked for updates. We can now rename the device, for home lab purposes its best if it is something simple such as PC-1 or name it the same as the VM name in Virtualbox.
The next screen we will choose, set up for work or school so this PC is able to managed centrally. Click sign-in options, and then domain-join instead. 
Enter a username, password and then set up 3 security questions. Since it is just for testing the tracking and such options can be accepted. 

Now the VM will restart once again, but take a bit longer to come back online as it is personalizing the settings. You may want to restart the Active Directory server at this point
so it is ready to go when needed.

In the Windows 11 VM we should install the guest additions CD like done in the Server, if you would like to have a better sized screen.
We will then confiure the DNS settings like we did with the server. Open Control Panel, Network and Internet, and then Network and Sharing Center.
Click on Ethernet, Properties, select Internet Protocol Version 4 and click Properties once again.

For this we will be using the IP address of the server we created earlier.
Below, on the left is the Server and the right is the Windows 11 machine with updated DNS addresses. One resolves to the Server we created and the other to a public DNS.
We should now attempt to ping the server from the Windows machine to ensure the DNS is set up properly. This is used with the ```ping``` command and the domain name that was set up on the server.

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/b16c84e7-d2f6-4966-bac6-0c3cf4c9d124)


If you get a response, this means it is properly set up and you can continue. Otherwise recheck the dns settings you have set up.
On the Windows machine, in the search bar, type domain and click on Access work or school.
In the Add a work or school account click the blue Connect button. Then click Join this device to a local Active Directory domain.
Type in the domain you created earlier.

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/47869016-d3c9-4565-8398-9460f38c4d58)


Press next, and then sign in with Administrator as the username and then the password for the admin account. 
The next screen that is displayed is to create a new account to be used on the domain, select administrator under Account type, and restart the PC.
Once restarted, note that you are now using an account attached to the domain and let the initial setup take place.

You have now completed the basic setup of an Active Directory home lab.

If you want, you may go on the server pc and search for Active Directory Users and Computers. In here you can create and manage new users, manage connected PCs and more.
In the next write up this is what we will go over. 

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/73d21f61-b0bd-4676-aca0-1af5c7d38ed4)























