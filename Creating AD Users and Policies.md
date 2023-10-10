Update: There are many Powershell scripts online that will create a user based on a given formatted list. ie. Firstname Lastname. It will use each name and create an account based on the given attributes. I chose to do it manually as 1. I don't know any Powershell
scripting, 2. Since I can't make it myself and it would be directly copied I would rather not include it in my tutorial. A script would most likely be used in a production environment unless only one or so user 
were to be created but for this tutorial it wasn't necessary. 

Now we can begin creating and managing users, groups and more within Active Directory. 

Before we begin, it is worth noting that LDAP is the protocol that is used to "read" the AD information and authorize/authenticate users. Since AD is simply a database of rules, users and groups its main function is 
to just be a host of information and without a protocol like LDAP it is just a database. 

To start we will begin with adding a basic user with default permissions. To do this, in the Server Manager, click on Tools and select Active Directory Users and Computers. 
Lets notice the different folders that appear, such as the domain we created during the setup. This parent folder will contain all of the users and groups that we will create.

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/ea96f190-c21f-4ddf-9b17-2db7da27d46d)

- Builtin is a collection of groups that come preconfigured in AD, these are usually a bit more broad and we can further define groups later on. 
- Computers is a collection of the different machines that have logged in and connected.
- Domain controllers is where the various AD servers that are set up are stored. (in a production setting there would be at least a few here for redundancy)
- ForeignSecurityPrincipals is where groups/users from other domain that are trusted will be stored. (Personally haven't used this feature, so I can't speak on it)
- Managed Service Accounts is where accounts that are used to run specific scheduled tasks are stored, these use Kerberos for authentication. (I also have not dealt with these accounts yet)
- Users is the main tab we will be using and this contains every user that is authorized in the AD domain. You will notice many premade security groups that can be local, global or universal.

Before we start creating and managing, lets check out the various properties that are associated with various objects.
Click on computers and right click and hit properties on the Windows 11 machine we created previously. 
- General tab, we can change the name if needed, DNS name, DC Type, add a site or add a description. 
- System tab, we see the name of the OS along with the version.
- Member Of will show the groups the computer is apart of.
- Delegation as stated allows servers to act on behalf of another user. (I am not familiar with Delegation)
- Location is more if there are multiple locations and you would need the location of each computer for organizational purposes (Also not familiar with Location)
- Managed By is a tab where you can set a group or user to be the specific ones to manage this computer. This is again helpful for large companies with multiple locations.
- Dial-in is no longer necessary as it is not the year 2000 anymore. Joking aside this tab is virtually useless nowadays as most internet speed is up to date.

Now, lets create a user.
First, right click on the domain and hover New and select Organizational Unit. This will allow us to easily sort and organize new users into specific roles.
Name the object something like Sales Managers, IT Support, etc. 
Right click on the newly created object and hover New, select User.

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/ff7755f6-76ec-45af-a60c-6c34b8c231b9)

Again, you can personalize this as you choose and notice that their username will point to the AD domain. Press Next and notice the various options for passwords.
- User must change password at next logon. This is a good option to select when creating new users at it means a default password can be set and then the user will have to create their own upon sign-in.
- User cannot change their password. Self-explanatory and could be applied to various situations.
- Password never expires. Almost always leave this unchecked as it is a good policy to have new passwords on a semi-frequent basis.
- Account is disabled. Also a good option for new users or just a user that does not need immediate access to an account.

Create the password, verify and press Next. Now press Finish.
You should now see the newly created user in the folder we just previously created. 
Lets right click on that user and check out the properties associated with them. 
You will immediately notice many more fields than the ones we previously saw, we won't go over all of them but a few.

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/f5fa29eb-37bc-4b9e-b1c8-86ca6eae4c08)

- General, Telephones and Address is where you can update any common personal information and is very helpful with queries to add users in groups later on.
- Account is a very important tab. Not only can you update account details, but the Logon Hours and Log On To fields are extremely important.
- Logon Hours can restrict access to the account during certain times of the day. If the field is blue, the user is able to log on at that time.
- Log On To sets the computers to which the account is able to access. This is again, very helpful to limit who uses what workstation if applicable.
- You can also unlock/lock accounts, setup encryption, and set a time when the account should expire, say if you are working with a contractor.
- Organization can be very helpful for, you guessed it, organzational purposes. You can even set a manager who this account should be reporting to.
- Member Of is again the groups the account is apart of.

Again, lets right click on the user we created and check out the various other options we can select.
- Copy, creates a copy of the user. Not extrememly useful but good to have.
- Add to a group.
- Disable Account, which is very good is a user does not need access to their account currently for various reasons.
- Reset Password, very helpful for Support/Help Desk positions.
- Send Mail, if the user has an email address attached to the account you can even send an email.

Now, we will open a new tool called Group Policy Management by click Tools in the top right and selecting it.
We will drop down the forest we made, then domains, and finally our domain we made. 
You should have multiple options here but the one we are interested in is the folder we made earlier called Sales Managers or however you named it.
Click on this folder and notice the different options that open up on the right. Linked Group Policy Objects, Group Policy Inheritence and Delegation.

- Linked Group Policy Objects is where any policies that directly effect that group are stored which should currently be empty.
- Group Policy Inheritence is any policies that are meant to effect the domain are spread to all of the child object as well. This can be disabled.
- Delegation is the groups and users who have permission for the folder.
  
Notice when you click on it there is nothing in it, meaning no policies are being enforced.
Right click the folder and select Create a GPO in this domain, and Link it here. 
Name it something along the lines of "Folder Name" Policy". Then click OK.
Notice the policy appeared under the folder.

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/58b7e570-0b6e-477f-bd8b-e067d20d6ab8)

Right click on the new policy and press Edit. You will see 2 main drop downs, Computer Configuration and User Configuration.
Computer configuration policies will affect every user who logs on, while User configurations are simply user based.
Preferences are more about customization so we won't really go over that here. Drop down the Policies object under User configuration.

We now see 3 different folders, Software Settings, Windows Settings and Administrative Templates.
- Software Settings will be used if a user is meant to have certain software applied to them.
- Windows Settings contains some security and Qos policies which we will not go over much. Although an interesting one is Folder Redirection as you can redirect a folder to a path on the network in a centralized location.
- Administrative Templates are where the core of the policies will be applied and we will go over each in detail in the next section. 

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/564b1d54-79fd-4d36-a367-252c8ce08c6d)

Click on Administrative Templates and see the various features that can be changed. You can poke around and see the different policies that are able to be put in place and it is encouraged to do so. 
Things like hiding the desktop icons, removing items from the start menu, Windows Automatic Updates, etc.
Lets change some policies so they will take effect on the user we created earlier. First, use Microsoft Edge and search the internet for a background of whatever you choose, and save it to Downloads.
- In Desktop, lets Hide and Disable all items on the desktop. In the dropdown click Desktop again. Choose Desktop Wallpaper, enable and then copy the file path from the previously downloaded wallpaper.
- In Control Panel, lets choose Prohibit access to Control Panel and PC settings and enable it. 
- Still in Control Panel, select Programs and Hide "Windows Marketplace"
- In Start Menu and Taskbar, find Remove links and access to Windows Update and enable it, along with Remove user name from Start Menu. 
- In System, Ctrl+Alt+Del enable all options besides Remove Logoff.
- Still in System, go to Power Management and enable Prompt for password on resume from hibernate/suspend.

There are many more options that would normally be enabled/disabled but we just want to choose the ones that can be seen to understand how policies are enforced.

Now open up a command prompt as an administrator, and type ```gpupdate /force```. This forces the registry to update with the new policies that we put in place.
Make sure to remember the user we have just created so we are able to log on to the Windows machine, which is what we will do now. 

Start up the Windows VM while leaving the Active Directory Server still open and log into the newly created user account.
If you enabled that the user must change their password you will be prompted to right after entering your credentials.

![image](https://github.com/JMacPort/Active-Directory/assets/145376972/e876f2d5-9032-4c6c-ac8e-a69c60e6a45c)

It will again, take a minute or 2 to set up the personalized account and once done we will make sure the features we disabled/enabled are working properly. 
The first thing we can notice is the background and that there is no items on the desktop. When pressing Crtl+Alt+Del we will also notice the only options are to switch users and sign out.
![image](https://github.com/JMacPort/Active-Directory/assets/145376972/7f331550-4285-4dad-a51f-eaddc397bf6a)

Now type Control Panel in the search bar and press Enter. You should be greeted by the image below
![image](https://github.com/JMacPort/Active-Directory/assets/145376972/94830afa-8dee-4723-b51b-41513a6eae73)

Also, try to search for Windows Update. This should open and close the Settings menu which shows we are not allowed to access that update.
The final test if you would like to, is to leave the VM alone until it hibernates and then check if you are required to enter the password again. 
Also, if you would like to, you can reset the password of the user via the Users and Computers on the server. Then, logout on the user account and when logging back in it will require the new password.

This is now a completed tutorial of Active Directory. There is a much deeper dive that can be done but this should lay the groundwork of the various features and abilities you have within the administrative settings. In a production setting
there would be a lot more restrictive features and most likely many more groups and users. I actively encourage you to try and mess around with the options, make more groups and see what works and what doesn't.

Recap of what was learned:
- Set up a Windows 11 and Windows Server 2022 VM
- Connecting the 2 machines on a NAT Network
- Adjusting DNS and IP settings to ensure connectivity
- Enabling Active Directory on the server as a Domain Controller
- Basic commands such as ```ipconfig``` and ```gpupdate /force```
- Provisioning Users, Groups and more
- Setting up Group Policies

What's Next:
- Entra ID!! Previously Azure AD - cloud based administration.
- Continue working towards my Sec+
- Possibly some Python Scripting
- Possibly Packet Tracer work to set up basic Networking


   
















