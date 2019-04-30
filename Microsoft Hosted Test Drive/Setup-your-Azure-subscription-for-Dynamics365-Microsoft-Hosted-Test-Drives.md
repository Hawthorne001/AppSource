# Setup your Azure subscription for Dynamics 365 for Customer Engagement Microsoft Hosted Test Drives

1.	Login to Azure Portal with Admin account- https://portal.azure.com
2. Verify you are in the tenant associated with your Dynamics 365 Test Drive instance by hovering your mouse over your account icon in the upper right corner. If you are not in the correct tenant, click on the account icon to switch into the correct tenant.
 
![](https://github.com/Azure/AzureTestDrive/blob/master/AzureTestDriveImages/SetupSub4.jpg)

3. 	Create an Azure AD App in Azure. AppSource will use this App to provision and deprovision the Test Drive user in your tenant.
      *    Select Azure Active Directory in the filter pane
      *    Select “App registrations” <br /> ![](https://github.com/Azure/AzureTestDrive/blob/master/AzureTestDriveImages/SetupSub5.jpg)
      *    Click on the 'New registration' button
      *    Provide an application name <br /> ![](https://github.com/Microsoft/AppSource/blob/master/Images/App%20Registration.png)
      *    Select 3rd option (Account in any organization directory and personal microsoft account) under Supported account types.
      *    Click Create and wait for your app to be created.
      *    Once App created successfully. Take note of your 'Application ID' displayed in overview. You will need this value later when configuring your Test Drive. 
      *    Click on API permission under Manage Application.
      *    Select Microsoft Graph API and choose the Application Permission type. 
      *    Select Directory.Read.All and Directory.ReadWrite.All permissions. <br /> ![](https://github.com/Microsoft/AppSource/blob/master/Images/Add%20Permission.png)
      *    Click on Add permission button.
      *    Once the permission is added successfully, click on Grant Admin consent for microsoft button under Grant consent. <br /> ![](https://github.com/Microsoft/AppSource/blob/master/Images/Grant%20Permission.png)
      *    Generate a secret for the Azure AD App. Navigate to 'Certificate and secrets' under Manage Application. 
      *    Click on New client secret button under Client secrets.
      *    Provide a valid description (example - "Test Drive") and select an appropriate duration. Be aware that your Test Drive will break once this Key expires and you will need to generate and provide to AppSource a new key when this happens. 
      *    Click Add. This should generate the Azure App Secret. Copy this value as it will be hidden as soon as you navigate away from this blade. You will need this value later when configuring your Test Drive. <br /> ![](https://github.com/Microsoft/AppSource/blob/master/Images/Add%20Secret%20Key.png)

4. Add Service Principal role to application to allow the Azure AD App to remove users from your Azure tenant. 
    * Open an Administrative-level PowerShell command promt.
    * Install-Module MSOnline  (run this command if MSOnline is not installed)
    * Connect-MsolService (Will show a popup window to login. Login with newly created org tenant)
    * $applicationId = "<YOUR_APPLICATION_ID>"
    * $sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
    * Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal <br /> ![](https://github.com/Microsoft/AppSource/blob/patch-1/Images/Connect_MsolService.PNG)

5. Add the above created Azure App as an application user to your Test Drive CRM instance. 
     * Add a new user in Azure Active Directory. Only Name and Username (belong to same tenant) values are required to create this user, leave the other fields as default. Copy the username value.
     * Login to CRM instance and navigate to Setting -> Security -> Users
     * Change the view to Application Users <br /> ![](https://github.com/Microsoft/AppSource/blob/patch-1/Images/ApplicationUser_form_CRM.PNG)
     * Add a new User (Ensure the form is Application user form)
     * Enter the same username in Primary Email and User Name fields. Add the Azure ApplicationId in Application ID field. 
     * Give any full name.
     * Hit Save. 
     * Click on Manage roles
     * Assign custom or OOB security role which contains read, write and assign role privileges. Example - System Adminitrator role. <br /> ![](https://github.com/Microsoft/AppSource/blob/master/Images/SecurityRole.png)
     * Also assign the application user the custom security role you have created for your Test Drive 
