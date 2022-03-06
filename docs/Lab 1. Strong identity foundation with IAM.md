

<h1> Lab 1. Strong identity foundation with IAM <h1> 


<h3> Step 1. Create a policy to enforce MFA sign-in </h3>

You begin by creating an IAM customer managed policy that denies all permissions except those required for IAM users to manage their own credentials and MFA devices.

1. Sign in to the AWS Management Console

2. Open the IAM console at https://console.aws.amazon.com/iam/.

3. In the navigation pane, choose **Policies**, and then choose **Create policy**.

   ![1.1.3](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.1.3.png)

   

4. Choose the **JSON** tab, copy the text from the following JSON policy document and

   paste the policy text into the **JSON** text box.

   ```
   {    
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "AllowViewAccountInfo",
               "Effect": "Allow",
               "Action": [
                   "iam:GetAccountPasswordPolicy",
                   
                   "iam:ListVirtualMFADevices"
               ],
               "Resource": "*"
           },       
           {
               "Sid": "AllowManageOwnPasswords",
               "Effect": "Allow",
               "Action": [
                   "iam:ChangePassword",
                   "iam:GetUser"
               ],
               "Resource": "arn:aws:iam::*:user/${aws:username}"
           },
           {
               "Sid": "AllowManageOwnAccessKeys",
               "Effect": "Allow",
               "Action": [
                   "iam:CreateAccessKey",
                   "iam:DeleteAccessKey",
                   "iam:ListAccessKeys",
                   "iam:UpdateAccessKey"
               ],
               "Resource": "arn:aws:iam::*:user/${aws:username}"
           },
           {
               "Sid": "AllowManageOwnVirtualMFADevice",
               "Effect": "Allow",
               "Action": [
                   "iam:CreateVirtualMFADevice",
                   "iam:DeleteVirtualMFADevice"
               ],
               "Resource": "arn:aws:iam::*:mfa/${aws:username}"
           },
           {
               "Sid": "AllowManageOwnUserMFA",
               "Effect": "Allow",
               "Action": [
                   "iam:DeactivateMFADevice",
                   "iam:EnableMFADevice",
                   "iam:ListMFADevices",
                   "iam:ResyncMFADevice"
               ],
               "Resource": "arn:aws:iam::*:user/${aws:username}"
           },
           {
               "Sid": "DenyAllExceptListedIfNoMFA",
               "Effect": "Deny",
               "NotAction": [
                   "iam:CreateVirtualMFADevice",
                   "iam:EnableMFADevice",
                   "iam:GetUser",
                   "iam:ListMFADevices",
                   "iam:ListVirtualMFADevices",
                   "iam:ResyncMFADevice",
                   "sts:GetSessionToken"
               ],
               "Resource": "*",
               "Condition": {
                   "BoolIfExists": {
                       "aws:MultiFactorAuthPresent": "false"
                   }
               }
           }
       ]
   }
   ```

   ![1.1.4](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.1.4.png)

5. Select **Next:Tags** on the bottom right of the page to proceed, Select **Next:Review** 

6. On the **Review** page, type `Force_MFA` for the policy name. For the policy description, type `This policy allows users to manage their own passwords and MFA devices but nothing else unless they authenticate with MFA` Review the policy **Summary** to see the permissions granted by your policy, scroll to the bottom of the page, and then choose **Create policy** to save your work. 

   ![image-20210916162831724](/Users/syunita/Library/Application Support/typora-user-images/image-20210916162831724.png)

7. The new policy appears in the list of managed policies and is ready to attach.



<h3> Step 2. Create new user and attach MFA policy</h3>

Create a user group whose members have full access to all Amazon EC2 actions if they sign in with MFA. To create such a user group, you attach both the AWS managed policy called `AmazonEC2FullAccess` and the customer managed policy you created in the first step.

1. Sign in to the AWS Management Console

2. Open the IAM console at https://console.aws.amazon.com/iam/.

3. In the navigation pane, choose **Users**, and then choose **Add Users**.

   ![1.2.3](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.2.3.png)

4. Type the name mfa_user, check **Password- AWS Management Console Access**, input your default password,  select **Next: Permissions**

   ![1.2.4](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.2.4.png)

5. Select **Attach existing policies directly** select `AdministratorAccess` and `Force_MFA` policy that we created before. Select **Next:Tags**

   ![1.2.5](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.2.5.png)

6. In the next page, select **Next:Review**. In the Add User page, make sure the mfa_user has both `Force_MFA` and `AdministratorAccess` attached. Select **Create user**

   ![1.2.6](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.2.6.png)

7. Take notes on your AWS  account ID on the top left of your console.

   ![1.2.7](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.2.7.png)

8. Sign Out from the console.



<h3> Step 3. Test user's access</h3>

1. Sign in to your AWS account as the user with the password you assigned in the previous section. Use the URL: `https://<alias or account ID number>.signin.aws.amazon.com/console`

   ![1.3.1](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.3.1.png)

2. Try to open EC2 dashboard on AWS console, you will find that the access is restricted due to the Policy that only allow access to other services once MFA is enabled.

   ![1.3.2](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.3.2.png)

3. In the navigation bar on the upper right, choose your user name, and then choose **My Security Credentials**.

   ![1.3.3](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.3.3.png)

4. Download Google Authenticator in your mobile device.

5. Scroll down on AWS Console and select **Assign MFA device** to setup multi factor authentication for your IAM user.

   ![1.3.6](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.3.5.png)

6. Select **Virtual MFA device**, select **Continue**

   ![1.3.7](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.3.6.png)

7. Select **Show QR code**, open your Google Authenticator and scan the QR code. Input the two consecutive MFA code from your Authenticator app.

   ![1.3.8](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.3.7.png)

8. You will get a success notification, the virtual MFA device is now ready to use with AWS.

   ![1.3.9](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.3.8.png)

9. Sign out of the console and then sign in as `mfa_user` again. This time AWS prompts you for an MFA code from your phone. When you get it, type the code in the box and then choose **Submit**.

   ![1.3.10](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.3.10.png)

10. Choose **EC2** to open the Amazon EC2 console again. Note that this time you can see all the information and perform any actions you want.

![1.3.11](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/iam/1.3.11.png)