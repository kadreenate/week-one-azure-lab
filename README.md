# week-one-azure-lab

Create a new user and test their application rights. 

To create a user, log in to [Microsoft Entra ID](entra.microsoft.com) > select Users under Entra ID in the left navigation menu > On the Users page, select All users, select + New user, and then select Create new user > Fill the Preferred name and whether or not you want an auto-generated password > Select Review + Create. Then select Create on the review screen. The user is now created and registered to your organization. Here's the user I created: 

<img width="1245" height="177" alt="image" src="https://github.com/user-attachments/assets/1f774491-ee39-4f37-abcb-753aa9775196" />

Test the new user's application rights. Log in to Microsoft Entra ID with the auto-generated password (if you selected that option during account creation) > select Enterprise applications in the search bar. You'd see that the new user can't create new applications and can't consent to terms under User settings. 

Use your admin account to assign a role to the new user, which is ChrisGreen, for this lab. Go to Chris Green's account on Microsoft Entra ID. Do this by selecting Users under Entra ID in the left navigation menu > select All users > select the new user you created.

On the user page, scroll to Assigned roles > click on Add assignments on the new page > select "Application Administrator". Then click Next
 
<img width="1943" height="1130" alt="image" src="https://github.com/user-attachments/assets/35e90896-c8ba-4bea-81ca-ca27762861e2" />

On the settings page, determine the Assignment type ("Eligible" or "Active") and the role justification: 

<img width="1559" height="1035" alt="image" src="https://github.com/user-attachments/assets/f0b217b1-1fe0-4f13-9ba3-01bee11e61fd" />

Then click Assign: 

<img width="1201" height="387" alt="image" src="https://github.com/user-attachments/assets/b6be29f2-93e8-426c-a723-bf9b333bc1f3" />

Test the new role assignment for ChrisGreen (the new user) by going to Enterprise apps on the left navigation menu > + New application > + Create your own application. It won't be gryed out unlike before. 

<img width="1637" height="362" alt="image" src="https://github.com/user-attachments/assets/3bf21a2f-8c4d-41e2-bafc-8ad0776b60a1" />

Remove the Application Administrator role from ChrisGreen. To do this, go to Roles and admins under Entra ID on the left navigation bar > select Application Administrator on the list > On the Application administrator | Assignments page you should see Chris Green's name listed > Scroll all the way to the right on Chris Green > Select Remove from the options at the top of the dialog.

Alternatively, role assignments can be done through bulk operations. To do this, select "bulk create" from the Bulk operation dropdown in the User page. 

<img width="1186" height="286" alt="image" src="https://github.com/user-attachments/assets/31495ce5-bce9-43b2-94f7-6f24212f4f8a" />

You have the option to download the csv template to bulk create users or attach a preexisting template from my machine. For this lab, I chose the latter: 

<img width="494" height="652" alt="image" src="https://github.com/user-attachments/assets/0ca3c15b-1cc8-4299-80b9-5bb640af7aa9" />

The next step is using Powershell to configure the bulk scope for the bulk users. On Powershell, install Microsoft.Graph using the following two commands. Press Y when prompted to confirm: 

First command: Install-Module Microsoft.Graph -Scope CurrentUser -Verbose
Then use this command to confirm Microsoft.Graph has been installed: Get-InstalledModule Microsoft.Graph

<img width="1920" height="865" alt="image" src="https://github.com/user-attachments/assets/a8c29d04-d13d-4915-8fc1-fe5957f516ad" />

Then log in to Microsoft.Graph using this command: Connect-MgGraph -Scopes "User.ReadWrite.All". Your browser will open and you will be prompted to sign-in. Use the Administrator account to connect. Accept the permissions request; then close the browser window.

Use this command to verify that you're connected and can see existing users: Get-MgUser: 

<img width="1592" height="1044" alt="image" src="https://github.com/user-attachments/assets/97d91d55-6aea-424b-a09d-4afd92704ff9" />

Use this command to assign a temporary strong password to users: 

$PWProfile = @{
    Password = "<Enter a complex password you will>";
    ForceChangePasswordNextSignIn = $false
}
Use this command to create a new user: 

New-MgUser `
    -DisplayName "New PW User" `
    -GivenName "New" -Surname "User" `
    -MailNickname "newuser" `
    -UsageLocation "US" `
    -UserPrincipalName "newuser@<tenantname.com>" `
    -PasswordProfile $PWProfile -AccountEnabled `
    -Department "Research" -JobTitle "Trainer"

   Note: "<tenantname.com>" is a placeholder that should be replaced with an actual verified tenant, as shown below: 

   <img width="1595" height="611" alt="image" src="https://github.com/user-attachments/assets/cc062e10-3151-4f86-bcad-a257d3a4c492" />

**How to remove a user on Microsoft Entra ID. **

On the Entra ID homepage, browse to Users > All Users > Select the user you want to delete, which is ChrisGreen in this case" > select Delete > Yes

<img width="489" height="219" alt="image" src="https://github.com/user-attachments/assets/2e93a2a1-29d9-43bb-a3fd-7a5fe38a577d" />

To restore a deleted user, go to Deleted users on the Users page > select the user you want to restore > select Restore users from the ellipsis dropdown:  

<img width="780" height="370" alt="image" src="https://github.com/user-attachments/assets/b80c4a4e-f61c-478a-b860-ceda6291ec3f" />

**How to add a Windows licence to a user.** 

The first step is identifying the unlicensed user in your organization. This is because the assigned license of some users will be unable to access some products or may need an update to access products. To find an unlicensed user, navigate to All users > search for a particular user (Raul Razo in this case) > click the profile > licenses. Ensure the user (Raul) has "No license agreement" on their profile, as shown below:

<img width="1605" height="921" alt="image" src="https://github.com/user-attachments/assets/427b605a-ad73-46a2-aee5-cabfa5d2b94e" />

To assign a Windows license, go to the [Microsoft 365 admin center](https://admin.microsoft.com.) and sign in with your admin account > In the left navigation menu, select Billing, and then select Licenses > Select the product you want to assign from the list. For this lab, Windows 10/11 Enterprise E3 was the product I used: 

<img width="1588" height="943" alt="image" src="https://github.com/user-attachments/assets/0d5f4dde-0bfb-4cf9-8b69-35c86bdd4ee6" />

On the new page, select Assigned Licenses, then add the user and indicate the subscription, then click Assign Licenses: 

<img width="1180" height="835" alt="image" src="https://github.com/user-attachments/assets/a9ceceae-f6ee-4297-956b-8126f2a5757e" /> 

After license has been assigned: 

<img width="1188" height="375" alt="image" src="https://github.com/user-attachments/assets/3065acae-4ccf-4728-9ca1-59dbdaefe2b7" />

Verify the license by going to the user's profile: 

<img width="1177" height="848" alt="image" src="https://github.com/user-attachments/assets/9f6b7b91-ee3e-4793-8fa5-296ef8b8138b" />








