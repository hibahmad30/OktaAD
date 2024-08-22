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
To configure Dropbox provisioning, refer to the following documentation: https://help.okta.com/en-us/content/topics/provisioning/dropbox/drbx-main.htm. In Okta, navigate to ‘Applications > Dropbox Business > Provisioning’ and click ‘Configure API Integration.’ Check the ‘Enable API integration’ option and click ‘Authenticate with Dropbox Business.’
 <br/>
 <br/>
 <img src="https://i.imgur.com/nioU0VS.png" alt="Dropbox API Integration"/>
    <br/>
 <br/>
Set up provisioning from App to Okta and from Okta to App, along with attribute mappings, according to your requirements. Once configured, click ‘Save.’
 <br/>
 <br/>
   <img src="https://i.imgur.com/KcTpPut.png" alt="Dropbox Provisioning"/>
    <br/>
 <br/>
   <img src="https://i.imgur.com/4FMstw8.png" alt="Dropbox Attribute Mapping"/>
    <br/>
 <br/>
 In addition, Okta provides the option to schedule imports to run automatically, or select 'never' if you prefer to run imports manually.
     <br/>
 <br/>
   <img src="https://i.imgur.com/IugQrRx.png" alt="Schedule Imports"/>
    <br/>
 <br/>
To provision the accounts, navigate to ‘Import > Import Now.’ After logging into Dropbox, we can see that the ‘Sales’ group containing the user Sam has been automatically provisioned. 
  <br/>
 <br/>
   <img src="https://i.imgur.com/Ze1kXqu.png" alt="Dropbox Account Testing"/>

<h2>Testing and Troubleshooting</h2> 
 <p align="center">
Although Sam was successfully added as a user in Dropbox, an error message stating 'Could not validate SAML assertion' appeared when attempting to SSO from Sam's user dashboard.
 <br/>
 <br/>
Dropbox offers documentation for resolving this specific error, available here: https://help.dropbox.com/security/resolve-SAML-error. 
 <br/>
 <br/>
To address this issue, I returned to Okta, downloaded a new X.509 certificate, and then uploaded the updated certificate to the Dropbox Business Admin Console. As shown in the security logs from Dropbox, after re-uploading the certificate, Sam was able to successfully sign in to his newly provisioned account.
 <br/>
 <br/>
 <img src="https://i.imgur.com/4n0ZzFY.png" alt="Dropbox Logs"/>
 <br/>
 <br/>
 <img src="https://i.imgur.com/E17Gz6Z.png" alt="Dropbox Login"/>

 <h2>Enabling Group Push</h2> 
 <p align="center">
I will now assign the 'Dev' group, which Emma was added to earlier, to Dropbox Business by navigating to the 'Assignments' tab of the Dropbox Business app in Okta. Emma can now use Single Sign-On (SSO) to access Dropbox Business directly from her Okta User Dashboard.
 <br/>
 <br/>
 <img src="https://i.imgur.com/ebzHODK.png" alt="Dropbox Group Assignments"/>
 <br/>
 <br/>
 <img src="https://i.imgur.com/9tOKnoB.png" alt="Okta User Dashboard"/>
 <br/>
 <br/>
With automatic provisioning and user import from Okta to Dropbox, Emma is now a member of Dropbox without needing to manually create an account. Upon accepting the invite link sent from Dropbox Business, her account is automatically set up. As depicted in the security logs, Emma was able to SSO successfully using SAML. 
 <br/>
 <br/> 
 <img src="https://i.imgur.com/9YyylQU.png" alt="Dropbox Logs"/>
 <br/>
 <br/>
While changes—such as adding, editing, or removing members from the Sales or Dev teams—are automatically updated in Dropbox, Group Push ensures that group memberships and access rights are consistently synchronized across platforms for provisioning-enabled applications.
 <br/>
 <br/>
To enable Group Push, navigate to the 'Group Push' tab in Okta and click 'Push Groups.' From there, you can search for and select the desired groups by name to activate Group Push. As shown, both the 'Dev' and 'Sales' groups have been pushed to Dropbox.
 <br/>
 <br/> 
 <img src="https://i.imgur.com/KtDmQCF.png" alt="Configure Group Push"/>
 <br/>
 <br/>
Upon accessing the Dropbox Admin Console, you will see that the 'Dev' and 'Sales' groups have been automatically created in Dropbox. 
 <br/>
 <br/> 
 <img src="https://i.imgur.com/Ww3jHQ1.png" alt="Dropbox Groups"/>

<h2>Key takeaways:</h2>
This project focused on enhancing user management and access control by integrating Okta with Dropbox Business. The primary objective was to streamline user onboarding and automate account provisioning to ensure efficient access management across platforms. Utilizing SAML 2.0 for Dropbox Business addressed the challenge of providing seamless Single Sign-On (SSO) and automated provisioning, thereby reducing administrative overhead and ensuring consistent user access.
 <br/>
 <br/>
The implementation process involved assigning users to the 'Sales' and 'Dev' groups within Dropbox Business via Okta. Provisioning was enabled to ensure that new users created in Okta automatically received Dropbox accounts. Additionally, the Group Push feature was configured to synchronize group memberships between Okta and Dropbox Business.
 <br/>
 <br/>
The project delivered significant benefits, including reduced manual administrative tasks, enhanced security through centralized authentication, and an improved user experience with integrated SSO. Effective group management was achieved through automated provisioning and Group Push, ensuring consistent access controls across platforms. 
 <br/>
 <br/>
During the implementation, a "Could not validate SAML assertion" error was encountered, which was resolved by downloading and uploading a new X.509 certificate from Okta to the Dropbox Business Admin Console. Following this fix, users were able to successfully sign in to their newly provisioned Dropbox accounts.
 <br/>
 <br/>
Overall, the project significantly simplified user management in a multi-application environment, enhancing access management and operational efficiency. Key aspects included user onboarding, admin privileges, SAML integration, and automated lifecycle management. By streamlining these processes, the project reduced administrative overhead and improved consistency across platforms, ensuring a more seamless user experience.

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
