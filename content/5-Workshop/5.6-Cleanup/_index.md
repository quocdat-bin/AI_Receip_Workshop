---
title : "Cleaning Up Resources"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.5. </b> "
---

#### Cleaning Up Resources

#### Cleanup
**1. Delete Amazon Cognito**
- Go to Amazon Cognito → User pools.
- Select the AI Recipe Finder User Pool.
- Choose Delete user pool.
- Enter the User Pool name to confirm.

![hosted zone](/images/5-Workshop/5.6-Cleanup/1.jpg)
![hosted zone](/images/5-Workshop/5.6-Cleanup/2.jpg)

**2. Terminate Amazon EC2**
- Go to EC2 → Instances.
- Select the instance running the Backend.
- Choose Instance state → Terminate (delete) instance.
- Confirm the termination.
- Wait for the status to change to Terminated.
- Do not just choose Stop, because the EBS volume can keep incurring charges.
- Then go to EC2 → Elastic Block Store → Volumes:
- Check whether the instance's volume has been deleted automatically.


![hosted zone](/images/5-Workshop/5.6-Cleanup/3.jpg)
![hosted zone](/images/5-Workshop/5.6-Cleanup/4.jpg)

**3. Release the Elastic IP**
- Go to EC2 → Network & Security → Elastic IP addresses.
- Select the Elastic IP attached to the Backend.
- If it is still associated, choose Disassociate Elastic IP address.
- Then choose Release Elastic IP addresses.
- Confirm the release.

![hosted zone](/images/5-Workshop/5.6-Cleanup/5.jpg)

**4. Delete Amazon RDS**
- Go to Amazon RDS → Databases.
- Select the project's DB instance, for example food-db-4.
- Choose Actions → Delete.
- Choose not to create a final snapshot.
- Uncheck keep automated backups if you do not need to recover the data.
- Enter the deletion confirmation text as required by AWS.
- Choose Delete.

![hosted zone](/images/5-Workshop/5.6-Cleanup/6.jpg)
![hosted zone](/images/5-Workshop/5.6-Cleanup/7.jpg)
