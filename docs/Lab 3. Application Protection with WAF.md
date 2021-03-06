<h1> Lab 3. Application Protection with WAF</h1>

<h3>Step 1. Deploy OWASP Juice Shop Application</h3>

In this workshop you will use the [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/). The Juice Shop is an Open Source web application that is intentionally insecure.

1. Deploy the CF template in North Virginia...

This CloudFormation stack will take approximately 5 minutes to complete.

https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-east-1.s3.us-east-1.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template

![3.1.1](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/waf/3.1.1.png)

2. In Step 2, enter the Stack name and leave the other parameter as default, select **Next**.

3. In Step 3, leave all the default parameters and select **Next**.

4. In the Step 4, check all the capabilities and select **Create stack**.

   ![3.1.4](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/waf/3.1.4.png)

5. Wait till the cloudformation has been deployed successfully. Go to the stacks and check the output in the TheSiteUrl fields.

   ![3.1.5](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/waf/3.1.5.png)

6. Open TheSiteUrl on  the new tab to check if your OWASP Juice Shop application has been deployed correctly.

   ![3.1.6](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/waf/3.1.6.png)





<h3>Step 2. Setup AWS WAF</h3>

1. Go back to AWS console, select **Create web ACL**

![3.2.1](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/waf/3.2.1.png)

2. Input `waf-workshop-juice-shop` under the Name fields, `web ACL for the aws-waf-workshop` under Description Field. In the Resource type, choose **Cloudfront distributions**. In the associated AWS resources, choose the Cloudfront distribution deployed by the CloudFormation template in the previous step.

![3.2.2](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/waf/3.2.2.png)

3. Select **Add rules** to and pick **Add managed rule groups**

![3.1.9](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/waf/3.1.9.png)

4. Select **Core rule set** and **SQL database** under the **AWS managed rule groups**

![3.1.10](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/waf/3.1.10.png)



5. Tick both of the rules selected and select **Next**.

   ![3.2.5](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/waf/3.2.5.png)

6. Leave the rule priority as default and select **Next**.

   ![3.2.6](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/waf/3.2.6.png)

7. Leave the Cloudwatch metrics setting as default.

8. Review the WAF ACL and select **Create webACL**

   ![3.2.8](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/waf/3.2.8.png)



<h3> Step 3. Test the AWS WAF rules </h3>

1.  Make sure the `JUICESHOP_URL` environment variable to contains the URL for your Juice Shop deployment.

   ```
   export JUICESHOP_URL=<Your Juice Shop URL>
   ```

2. Imitate a CSS attack by invoking the following commands

   ```
   # This imitates a Cross Site Scripting attack
   # This request should be blocked.
   curl -X POST  $JUICESHOP_URL -F "user='<script><alert>Hello></alert></script>'"
   ```

3. You will receive a HTML response stating the request was forbidden

   ```
   <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
   <html>
     <head>
       <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
       <title>ERROR: The request could not be satisfied</title>
     </head>
     <body>
       <h1>403 ERROR</h1>
       <!-- Omitted -->
     </body>
   </html>
   
   ```

4. If you receive a response like below, then the request wasn't blocked by WAF, recheck your WAF ACL configuration on the previous section.

   ```
   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="utf-8" />
       <title>OWASP Juice Shop</title>
       // Omitted for brevity
     </head>
   </html>
   ```

   

5. Imitate SQL Injection attack by the following command

   ```
   # This imitates a SQL Injection attack
   # This request should be blocked.
   curl -X POST $JUICESHOP_URL -F "user='AND 1=1;"
   ```

6. You will receive a HTML response stating the requet was forbidden.

   ```
   <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
   <html>
     <head>
       <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
       <title>ERROR: The request could not be satisfied</title>
     </head>
     <body>
       <h1>403 ERROR</h1>
       <!-- Omitted -->
     </body>
   </html>
   ```

7. If you receive a response like below, then the request wasn't blocked by WAF, recheck your WAF ACL configuration on the previous section.

   ```
   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="utf-8" />
       <title>OWASP Juice Shop</title>
       // Omitted for brevity
     </head>
   </html>
   ```



<h3> Step 4. Check the WAF Dashboard </h3>

1. Go to the WAF console, select Web ACLs on the menu and choose the **waf-workshop-juice-shop** Web ACL we created on the previous section.

![3.3.1](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/waf/3.3.1.png)

2. Explore the metric dashboard with the sampled request.

   ![3.3.2](https://github.com/yunitasalim/aws-security-essentials/blob/main/img/waf/3.3.2.png)
   

   