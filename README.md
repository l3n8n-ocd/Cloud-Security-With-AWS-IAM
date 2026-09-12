<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Cloud Security with AWS IAM

**Project Link:** [View Project](http://nextwork.ai/projects/aws-security-iam)

**Author:** Lenon Adriano  
**Email:** ladriano0209@gmail.com

---

![Image](http://nextwork.ai/fulfilled_blue_joyful_clownfish/uploads/aws-security-iam_1c864649)

---

## Introducing Today's Project!

### Project overview

In this project, I will demonstrate how to secure AWS resources using AWS IAM by creating customized access policies, setting up user groups, and onboarding an intern with restricted permissions. I'm doing this project to learn how to manage cloud access, enforce least-privilege security, and control EC2 instance operations based on environment tags.

### Tools and concepts

The key services I used were Amazon EC2 (to launch and manage virtual servers) and AWS IAM (to configure access policies, user groups, users, and account aliases).

Key concepts I learnt include:

● Least Privilege Access: Restricting permissions so users only have the minimum access required for their role.

● Tag-Based Access Control (ABAC): Using resource tags (like Env: development) to dynamically control operations like starting or stopping instances.

● IAM Structure: Managing security efficiently using policies, user groups, and condition keys rather than assigning permissions individually.

### Project reflection

This project took me approximately 30 minutes to complete. The most challenging part was configuring and testing the JSON IAM policy conditions to ensure tag-based access worked accurately without unintended permissions. It was most rewarding to log in as the intern user and see the "Access Denied" error live when attempting to modify the production instance, confirming that the security controls were successfully enforced.

---

## Tags

### What I did in this step

In this step, I will launch two Amazon EC2 instances—one tagged for production and one tagged for development—to scale computing power for expected website traffic and establish resources to test environment-based access policies.

### Understanding tags

Tags are key-value metadata labels attached to AWS resources that help organize, manage, and filter infrastructure. They are useful for cost allocation, identifying resources across different environments (such as distinguishing production from development), and controlling access by enforcing attribute-based security policies.

### My tag configuration

The tag I’ve used on my EC2 instances is called Env (along with the default Name tag). The values I’ve assigned for my instances are production (for nextwork-prod-exemprium) and development (for nextwork-dev-exemprium).

![Image](http://nextwork.ai/fulfilled_blue_joyful_clownfish/uploads/aws-security-iam_2e0e5a5d)

---

## IAM Policies

### What I did in this step

In this step, I will create a custom IAM policy using the visual editor to allow EC2 instance management based on environment tags because I need to grant our intern access to manage development instances while keeping production instances secure.

### Understanding IAM policies

IAM Policies are JSON documents in AWS that define permissions, determining what actions users, groups, or roles can perform on specific AWS resources.

### The policy I set up

For this project, I’ve set up a policy using the JSON editor inside the IAM console by pasting a JSON policy document that grants permissions to manage EC2 instances tagged with Env: development.

### Policy effect

I’ve created a policy that explicitly allows management actions (such as starting, stopping, and rebooting EC2 instances) only when the instance has a tag where Env equals development. This ensures that anyone attached to this policy can only interact with development resources while automatically denying access to perform state changes on production instances.

### Understanding Effect, Action, and Resource

The Effect, Action, and Resource attributes in a JSON IAM policy define the core rules of permissions:

● Effect: Specifies whether the policy allows or denies access. It is set to either "Allow" or "Deny".
● Action: Lists the specific API operations or tasks that are allowed or denied (for example, ec2:StartInstances, ec2:StopInstances, or ec2:RebootInstances).
● Resource: Identifies the specific AWS resources (using ARNs or wildcards like *) to which the actions apply.

---

## My JSON Policy

![Image](http://nextwork.ai/fulfilled_blue_joyful_clownfish/uploads/aws-security-iam_1c864649)

---

## Account Alias

### What I did in this step

In this step, I will create a custom AWS Account Alias because replacing the default 12-digit AWS account ID with a memorable URL makes it much easier and more convenient for the intern to log in.

### Understanding account aliases

An account alias is a custom, human-readable name created to replace the default 12-digit AWS account ID, providing a unique and personalized sign-in URL (such as [https://your-alias.signin.aws.amazon.com/console](https://your-alias.signin.aws.amazon.com/console)) for IAM users to log in easily.

### Setting up my account alias

Creating an account alias took me a minute. Now, my new AWS console sign-in URL is https://nextwork-alias-exemprium.signin.aws.amazon.com/console

![Image](http://nextwork.ai/fulfilled_blue_joyful_clownfish/uploads/aws-security-iam_0eb4439b)

---

## IAM Users and User Groups

### What I did in this step

In this step, I will create a dedicated IAM user group for NextWork interns and attach our custom policy to it, then create an IAM user account for the new intern because managing permissions through a group ensures scalable and centralized access control.

### Understanding user groups

IAM user groups are collections of IAM users that allow you to specify permissions for multiple users at once. Instead of attaching individual permissions to each user account, you can attach policies directly to the group; all users within that group automatically inherit those permissions, simplifying access control and administrative overhead.

### Attaching policies to user groups

I attached the policy I created to this user group, which means all IAM users added to the group automatically inherit its permissions. This allows any user in the group to manage EC2 instances tagged for development while restricting access to production resources, removing the need to configure permissions for each individual user.

### Understanding IAM users

IAM users are entities created within AWS that represent a specific person or application, consisting of a name and credentials (such as a password or access keys) used to authenticate and interact securely with AWS resources.

---

## Logging in as an IAM User

### Sharing sign-in details

The first way is to email the sign-in instructions to the user, whereas the second way is to download the credentials file containing the Console sign-in URL, username, and auto-generated password, and securely send/transfer it to the user.

### Observations from the IAM user dashboard

Once I logged in as my IAM user, I noticed explicit "Access Denied" errors and permission warnings when trying to view global dashboard metrics or perform actions across unrestricted resources. This was because the custom IAM policy attached to my user group strictly enforces tag-based access control, allowing permission only for actions specifically targeted at resources tagged with Env: development.

![Image](http://nextwork.ai/fulfilled_blue_joyful_clownfish/uploads/aws-security-iam_6f2ab446)

---

## Testing IAM Policies

### What I did in this step

In this step, I will test the intern's IAM user access by signing in with their credentials and verifying that they can manage the development EC2 instance while being restricted from performing state changes on the production instance.

### Testing policy actions

I tested my JSON IAM policy by attempting to stop both EC2 instances while logged in as the intern IAM user. The action successfully stopped the development instance (nextwork-dev-exemprium), but AWS returned an "Access Denied" error when I tried to stop the production instance (nextwork-prod-exemprium), proving that the tag-based permission policy was working as intended.

### Stopping the production instance

When I tried to stop the production instance, AWS returned an "Access Denied" authorization error message. This was because the custom IAM policy attached to my user group strictly enforces tag-based condition rules that allow EC2 instance management actions only when the resource has a tag where Env equals development. Since the production instance is tagged with Env: production, the policy denied permission to perform state changes like stopping it.

![Image](http://nextwork.ai/fulfilled_blue_joyful_clownfish/uploads/aws-security-iam_0e7a9d6a)

### Stopping the development instance

Next, when I tried to stop the development instance, the operation completed successfully and the instance successfully transitioned to the stopping state. This was because the custom IAM policy explicitly allowed EC2 instance management actions (such as ec2:StopInstances) on resources tagged with Env: development.

![Image](http://nextwork.ai/fulfilled_blue_joyful_clownfish/uploads/aws-security-iam_1811801c)

---

## IAM Policy Simulator

In this extension step, I will use the IAM Policy Simulator to test and validate my custom IAM policy permissions because it allows me to safely verify that the intern's tag-based access controls work as expected without impacting live production resources.

### Understanding the IAM Policy Simulator

The IAM Policy Simulator is a testing tool provided by AWS that allows you to test and troubleshoot identity-based policies, resource-based policies, and boundary conditions. It is useful for safely verifying permissions, testing complex condition keys (such as tag-based access), and debugging "Access Denied" errors without having to make live changes or risk impacting real AWS resources.

### How I used the simulator

I set up a simulation testing EC2 management actions (such as `ec2:StopInstances`) specifically for the development instance. The policy simulator evaluated the request attributes against the tag condition rule and returned an allowed decision, confirming that the policy correctly grants permissions for resources tagged with `Env: development`.

![Image](http://nextwork.ai/fulfilled_blue_joyful_clownfish/uploads/aws-security-iam_069d8a621)

---

---
