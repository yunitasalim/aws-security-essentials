<h1> Lab 2. Intelligent Detection with GuardDuty<h1> 


<h3> Step.1 Setup Event and Notification </h3>

1. Go to AWS SNS on the AWS Console, input the topic name `guard-duty-notif` and select **Next step**

   ![2.1.1](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/guardduty/2.1.1.png)

2. Leave the default configuration as it is and select **Create topic**.

3. Select Create subscription in the Amazon SNS Topics page.

   ![2.1.3](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/guardduty/2.1.3.png)

4. Select **Email** for the Protocol, type your email in the Endpoint input. Select **Create subscription**.

   ![2.1.4](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/guardduty/2.1.4.png)

5. Check your email and confirm the subscription link.

6. Open AWS Cloudwatch Services on the console

7. Select **Rules** under **Events** and select **Create rule**.

   ![2.1.7](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/guardduty/2.1.7.png)

8. Tick **Event Pattern** select **GuardDuty** as the Service Name and add **GuardDuty Finding** in the EventType. Select  `guard-duty-notif` that we created previously as the Targets. Proceed to **Configure Details**.

   ![2.1.8](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/guardduty/2.1.8.png)

9. Input the Name `guardduty-findings-rule` and Description `notify security admins by email upon GuardDuty findings`. Select **Create rule**.

   ![2.1.9](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/guardduty/2.1.9.png)

10. The Cloudwatch event rule for GuardDuty findings has been configured.

    

<h3> Step 2. Setup Amazon GuardDuty </h3>

1. Open GuardDuty in the AWS Console https://console.aws.amazon.com/guardduty

   ![2.2.1](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/guardduty/2.2.1.png)

2. Select **EnableGuardDuty**

   ![2.2.2](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/guardduty/2.2.2.png)

3. You will see there Findings dashboard, you will not have any findings listed as there is no malicious activity detected yet.

   ![2.2.3](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/guardduty/2.2.3.png)

4. Select **Settings** on the menu, then select **Generate sample findings** to simulate GuardDuty findings.

   ![2.2.5](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/guardduty/2.2.5.png)

5. Select Findings on the menu, you will find that the sample findings has been populated on the dashboard. Click the items to see the details of each findings.

   ![2.2.6](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/guardduty/2.2.6.png)

6. Verify that you will also receive email notification for each of the GuardDuty findings collected. This may take some time.
   

   

   