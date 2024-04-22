# AWS

Amazon Web Service stuff

## Create AWS Free Tier Account

- Google `aws free tier` and create a free account
- Go to `aws.amazon.com` and _Sign in to the Console_ -> Root user and log in
- Choose `N.Virginia` from the top right corner next to your username

#### Configure Account and Create a Budget and Alarm

Configure Account

- Go to `IAM Dashboard` (choose _IAM_ from services)
- Create Alias from the `AWS Account` section on the right
- Go to your account (Click your name on the top right -> Account)
- Active `IAM user role access to Billing information`
- Choose _Billing Preferences_ on the left navbar under the `Preferences and Settings`
- Edit _Alert preferences_ -> put ticks on both boxes
- Activate `Invoice delivery preferences`

Budget

- Click _Budget_ on the left under the `Budgets and Planning`
- `Create Budget` -> Use a template -> Monthly cost budget -> Give a name and your budget (2e)
- _Cost Explorer_ under the `Cost Analysis` shows where you are spending your money

#### Creating IAM Users and Groups

Group

- Go `IAM` -> Create User groups -> Create group
- Group name `Admins`
- `Attach permission policies` -> _AdministorAccess_ -> Create group

Users

- Click _Users_ under the `Access managment` -> Create user
- Give User name -> Provide user access to the AWS Managment Console -> I want to create an IAM user
- Set Custom password -> Remove the tick from 'Users must create a new password at next sign-in' -> Next
- Add user to group -> Admins -> Next -> Create user -> Return to users list
- Try your new user -> Go to aws signin page -> Choose IAM user -> `Account ID` is the Account name of your root user -> Give the user name and password that you just created and it works
- It's better to log in with the `IAM user` account than the root account

#### Install Tools and Configure AWS Cli

- Install VS Code & AWS CLI Command Line Interface

Cloudshell

- Search `Cloudshell` from AWS Services
- Test it with command `aws help` (q) to quit

## AWS Authentication and Access Control

#### Setup Multi-Factor Authentication (MFA)

- Go `IAM Dashboard` -> `Users` from the left navbar -> Click your name -> _Security credentials_ -> Assign MFA device
- Give Device name -> Authenticator App -> Read the QR-code etc

#### Switching IAM Roles

- `IAM Dashboard` -> Users -> Create user -> Give User name and Provide user access to the AWS Managment Console -> Create IAM user -> Custom Password -> Remove tick from _Users must create a new password..._ -> Next -> Create user -> Return to users list
- Sign in with that user -> A lot of _Access denied_ everywhere
- Go `EC2` Service -> Lot of red -> Log out
- Login with that `IAM user` with Admin access -> `IAM` -> Roles -> Create role -> AWS account -> Next -> Search EC2 -> AmazonEC2FullAccess -> Next
- Role name: _ec2-role_ -> Create role -> Copy the 'Link to switch roles in console'
- Log in with the IAM user with no permissions in a incognito tab -> Click username in the top right navbar -> Switch role -> Insert fields manually OR open a new tab and paste the copied link
- Add IAM role name to Display name also -> Switch role -> ?? Nothing happens because user doesn't have any permissions
- Copy the ARN from the ec2-role summary with the IAM user with Admin permissions
- Users -> click the username with no permissions -> Add permissions -> Create inline policy -> JSON from the top -> Add this (paste the ARN from role summary to "Resource")

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "sts:AssumeRole",
    "Resource": "arn:aws:iam::279455171996:role/ec2-role"
  }
}
```

-> Next -> Policy name _stsAssumeRole_ -> Create policy

- Go back to that incognite window and try to Switch role -> now you see your role on the top right of the navbar
- Go to `EC2` and now most of the errors are gone -> Switch back to normal role

#### IAM Identity Center in Action

User with Admin permissions

- Go to `AWS Organizations` -> Create an organization
- Go to `IAM Identity Center` -> Enable
- _Manage permissions for multiple AWS accounts_ -> Click -> Create permission set -> Predefined permission set -> Next, Next, Create
- Create another permission set -> Predefined -> ViewOnlyAccess -> Next, Next, Create
- `IAM Identity Center` Dashboard -> Groups -> Create group -> Group name _Management_ and Create group
- `IAM Identity Center` Dashboard -> Users -> Add user -> Fill the fields -> Next -> Add user to the _Management_ group -> Add user
- `IAM Identity Center` Multi-account permissions -> AWS accounts -> Select the management account and Assign users or groups -> Management -> Next -> Select both _ViewOnlyAccess_ and _AdministratorAccess_ -> Next, Submit
- Now check your email and accept the Invitation -> Set your password and sign in -> set MFA
- You can see the AWS access portal URL in the `IAM Identity Center` Dashboard in Settings summary

## AWS Compute Services
