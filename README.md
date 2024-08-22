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
 
<h2>Integrate Dropbox Business Using SAML</h2> 
 <p align="center">
We will now integrate Dropbox Business with SAML to streamline SSO for users in the ‘Sales’ group. Navigate to ‘Applications > Browse App Catalog,’ search for Dropbox Business, and click ‘Add Integration.’ In the Sign-On Options, select ‘SAML 2.0.’
 <br/>
 <br/>
 <img src="https://i.imgur.com/ySfqIh4.png" alt="Dropbox Integration"/>
    <br/>
 <br/>
To establish a trust relationship between Okta and Dropbox Business, additional configuration on the Dropbox Business side is required. I will follow the documentation available here: https://saml-doc.okta.com/SAML_Docs/How-to-Configure-SAML-2.0-for-Dropbox.html?baseAdminUrl=https://dev-60754792-admin.okta.com&app=dropbox_for_business&instanceId=0oaixkeibeUyc6L6c5d7.
  <br/>
  <br/>
First, create an account in Dropbox Business, then navigate to ‘Admin Console > Settings > Authentication > Single sign-on.’ Select the appropriate Single Sign-On option from the dropdown menu (Off, Optional, or Required).
  <br/>
  <br/>
For the ‘Identity provider sign-in URL,’ click the ‘Add sign-in URL’ link and paste the URL provided in the Okta documentation. In this case, the link is:
https://dev-60754792.okta.com/app/dropbox_for_business/exkixkeibduRi3Dxv5d7/sso/saml
  <br/>
  <br/>
Download the X.509 certificate and upload it to Dropbox, then click ‘Save.’ Back in Okta, you can enable Silent Provisioning as needed and assign Dropbox Business to the ‘Sales’ group.
<br/>
 <br/>
 <img src="https://i.imgur.com/RTuosxX.png" alt="Dropbox Configuration"/>
 <br/>
 <br/>
 <img src="https://i.imgur.com/dJMl5AB.png" alt="Dropbox App Assignment"/>
    <br/>
 <br/>
After signing into Sam’s Okta dashboard, we can see that Dropbox has been added as an app integration.
 <br/>
 <br/>
 <img src="https://i.imgur.com/xagOAMs.png" alt="Okta Dashboard for Sales User"/>
  
<h2>Configure Provisioning for Automated Lifecycle Management</h2> 
 <p align="center">
Although Sam was successfully assigned the app, he does not yet have an account in Dropbox. Manually creating accounts for each onboarded user can be time-consuming for administrators. To streamline this process, Okta offers automated user lifecycle provisioning.
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
