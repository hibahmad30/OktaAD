<h1>Active Directory Integration with Okta</h1>

<h2>Description</h2>
The integration of Active Directory (AD) with Okta centralizes identity management and enhances security across various applications. This project leverages Okta’s directory integration capabilities to synchronize user attributes, streamline onboarding processes, and ensure consistent identity management. By mapping AD attributes to Okta, the integration guarantees that user and group information is accurately reflected across downstream applications. This setup reduces manual administrative tasks, ensures consistent access control, and enhances the overall user experience by providing seamless and secure access to necessary resources.

<p align="center">

<h2>Environments Used </h2>

- <b>Okta</b>
- <b>Oracle VM VirtualBox</b>
- <b>Microsoft Windows Server</b>
- <b>Active Directory (AD)</b>
- <b>Okta AD Agent</b>

<h2>Configure Okta AD Agent on the Host Server</h2> 

<p align="center">
To begin, we will install the Okta Active Directory Agent on the host server. I started my Windows Server instance, which was previously configured for the <a href="https://github.com/hibahmad30/ActiveDirectoryMigration/">AD to Entra ID migration</a> project. Next, I created a new Organizational Unit (OU) named ‘Okta Users’ and added two users, ‘Jim’ and ‘Mary,’ who will be imported into Okta.
<br/>
<br/>
<img src="https://i.imgur.com/eFhmtDA.png" alt="Virtual Box Instance"/>
 <br/>
 <br/>
<img src="https://i.imgur.com/6K9nZEL.png" alt="Okta Users OU"/>
  <br/>
 <br/>
Once logged into the server, login to Okta and navigate to the Admin Console. Go to ‘Directory > Directory Integration,’ click ‘Add Directory,’ and then select ‘Add Active Directory.’ Next, click ‘Set Up Active Directory’ and proceed to click ‘Download Agent.’
<br/>
<br/>
<img src="https://i.imgur.com/7KOici1.png" alt="Add Active Directory"/>
 <br/>
 <br/>
<img src="https://i.imgur.com/pO9SRO0.png" alt="Download Agent"/>
 <br/>
 <br/>
 Once downloaded, open the installer (.exe) file and complete the installation. Enter the domain name that you want the agent to manage. The Okta AD Agent will run as a domain user, and I’ll be using the OktaService account for this purpose. If needed, you can specify a proxy server through which the AD agent will connect, but I will be using the default proxy settings.
 <br/>
 <br/>
Next, enter your Okta domain, and navigate to provide the registration link and code. This will register your domain with Okta, allowing you to proceed with the directory integration configuration directly in Okta. 
  <br/>
 <br/>
<img src="https://i.imgur.com/edk8krA.png" alt="Register Domain"/>
 <br/>
 <br/> 
<img src="https://i.imgur.com/oBJVlZR.png" alt="Okta Registration"/>
 <br/>
 <br/> 
The Okta AD Management Utility should now be running: 
   <br/>
 <br/>
<img src="https://i.imgur.com/UiPeriU.png" alt="Okta AD Management Utility"/>
 <br/>
 <br/>
 Navigate back to Okta and select the Organizational Unit(s) from which you want to import users and groups. Next, choose the attributes you wish to import from Active Directory into Okta. Once these configurations are set, the integration will be ready: 
  <br/>
 <br/>
<img src="https://i.imgur.com/aUUxhJO.png" alt="Integration Ready"/>

<h2>Import AD Users into Okta</h2> 
 <p align="center">
We will now test the integration by importing the two Active Directory-sourced users, ‘Jim’ and ‘Mary,’ into Okta. Navigate to the ‘Import’ tab of the directory integration and click ‘Import Now.’
 <br/>
 <br/>
 <img src="https://i.imgur.com/aCryPxj.png" alt="Okta Import"/>
    <br/>
 <br/>
After confirming the import assignments, I navigated to ‘Directory > People’. As depicted, ‘Jim’ and ‘Mary’ have been added as users in our Okta Universal Directory. 
  <br/>
  <br/>
 <img src="https://i.imgur.com/lt73ERg.png" alt="Okta Users"/>
 <br/>
 <br/>
 <img src="https://i.imgur.com/Ne4W82g.png" alt="AD Integration Users"/>
    <br/>
 <br/>
To test this further, I navigated to ‘Active Directory Users and Computers’ on my Windows Server instance and created a new user in the ‘Okta Users’ OU named ‘Hannah Lee.’ I then returned to Okta and tested the import again to bring the newly created user into Okta.
 <br/>
 <br/>
 <img src="https://i.imgur.com/iQl4Tjk.png" alt="New AD User"/>
   <br/>
 <br/>
 <img src="https://i.imgur.com/RPun4cX.png" alt="Okta Users OU Update"/>
    <br/>
 <br/>
  <img src="https://i.imgur.com/48h0BB9.png" alt="Okta Users Import"/>
    <br/>
 <br/>
   <img src="https://i.imgur.com/urL4Dho.png" alt="Okta Directory"/>
    <br/>
 <br/> 
  <img src="https://i.imgur.com/iL4lJ8q.png" alt="AD Users"/>
    <br/>
 <br/> 
  
<h2>Custom Attribute Mappings for User Data Integration</h2> 
 <p align="center">
Custom attribute mappings are crucial for ensuring that specific user data from Active Directory is accurately represented in Okta, enabling consistent identity management across integrated downstream applications. This process ensures that user attributes are correctly synchronized. 
 <br/>
 <br/>
For this use case, we will map the ‘samAccountName’ from Active Directory into Okta. Since ‘samAccountName’ is not a default attribute in Okta, we need to create it as a custom attribute. In Okta, navigate to ‘Profile Editor,’ then to the ‘Okta User Profile,’ click ‘Add Attribute,’ and add ‘samAccountName’ as a string custom attribute.
 <br/>
 <br/>
 <img src="https://i.imgur.com/9KpnuPj.png" alt="Create Custom Attribute"/>
    <br/>
 <br/>
Next, in ‘Profile Editor,’ click the ‘Mappings’ button next to the Active Directory user profile. Map the ‘appuser.samAccountName’ attribute from Active Directory to the custom ‘samAccountName’ attribute in Okta. I used Okta’s preview feature to test the mapping with the ‘Jim’ user before saving the configuration.
 <br/>
 <br/>
   <img src="https://i.imgur.com/JqjF9La.png" alt="Attribute Mapping"/>
    <br/>
 <br/>
   <img src="https://i.imgur.com/fJHfupT.png" alt="Attribute Testing"/>

<h2>Key takeaways:</h2>
This project focused on the integration of Active Directory (AD) with Okta to streamline user management and enhance security. The primary goal was to synchronize user attributes and import users from AD into Okta, ensuring that identity management was centralized and consistent across various applications.
 <br/>
 <br/>
The integration was crucial for reducing administrative overhead and improving user experience by ensuring that user data was consistently reflected across systems. By mapping attributes from AD to Okta, the project facilitated seamless access to downstream applications and simplified the management of user identities.
 <br/>
 <br/>
The implementation process involved several key steps. First, the Okta AD Agent was installed on the host server, and a new Organizational Unit (OU) was created in AD for testing purposes. Users were then created in this OU and imported into Okta to test the integration.
 <br/>
 <br/>
Custom attribute mappings were a significant part of the setup. Since ‘samAccountName’ is not a default attribute in Okta, it was necessary to create it as a custom attribute. This step involved configuring the attribute in Okta’s Profile Editor and mapping it to the corresponding AD attribute.
 <br/>
 <br/>
Finally, the integration was tested by importing users and using Okta’s preview feature to verify the configuration. This testing ensured that user data was accurately synced and that the integration was functioning as expected before finalizing the setup. Overall, the project successfully achieved its objectives by providing a streamlined solution for identity management and user access.

<p align="center">
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
