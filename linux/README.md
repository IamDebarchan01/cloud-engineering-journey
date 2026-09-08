What I Learned Today ?

Today I practiced Linux User Management and Group Management on an Amazon Linux EC2 instance.

The main topics I practiced were:

>Creating users
>Setting user passwords
>Viewing users
>Switching between users
>Navigating to the home directory
>Deleting users
>Changing usernames
>Understanding /etc/passwd
>Understanding /etc/shadow
>Creating groups
>Adding users to groups
>Removing users from groups
>Checking group membership
>Deleting groups
>Renaming groups
>Understanding the difference between a Linux user and a group
>Understanding sudo privileges
>Troubleshooting user-management errors

 Linux User Management :

Linux is a multi-user operating system.

Multiple users can access the same Linux machine and perform tasks simultaneously.

For example:

admin   → /home/admin
mike    → /home/mike
stallin → /home/stallin

Each user normally has a home directory where their personal files and configuration are stored.

Amazon Linux EC2

On Amazon Linux EC2 instances, the default login user is commonly:

ec2-user

The ec2-user account normally has sudo privileges, allowing administrative operations.

1. Create a New User
Command
sudo useradd <username>
Example
sudo useradd Deb

This creates a new user named Deb.

Verify
id Deb

or:

cat /etc/passwd
2. Set or Update a User Password
Command
sudo passwd <username>
Example
sudo passwd Deb

The command asks for the new password.

3. Display Users

The /etc/passwd file contains information about local user accounts.

Command
cat /etc/passwd

Example:

root:x:0:0:root:/root:/bin/bash
ec2-user:x:1000:1000:...:/home/ec2-user:/bin/bash
Deb:x:1001:1001:...:/home/Deb:/bin/bash
Important

/etc/passwd does not store users' actual passwords.

It stores account information such as:

Username
User ID (UID)
Primary Group ID (GID)
Home directory
Login shell
4. Switch to Another User
Command
su <username>
Example
su Deb

After switching, verify the current user:

whoami
5. Navigate to the Current User's Home Directory
Command
cd ~

The ~ represents the current user's home directory.

Check the current directory:

pwd

Example:

/home/Deb
6. Delete a User
Command
sudo userdel <username>

Example:

sudo userdel Deb

This removes the user account.

7. Delete a User Along With Their Home Directory
Command
sudo userdel --remove <username>

Short form:

sudo userdel -r <username>

Example:

sudo userdel -r Deb

The -r option removes the user's home directory and associated mail spool.

8. Change a Username
Command
sudo usermod -l <new-username> <old-username>

Example:

sudo usermod -l Debarchan Deb

This changes:

Deb

to:

Debarchan
Important

Changing the login name with usermod -l does not automatically rename the user's home directory.

For example:

/home/Deb

may still remain:

/home/Deb

Additional configuration is required if the home directory also needs to be renamed.

📁 Important Linux Files Related to Users
/etc/passwd

Contains general information about user accounts.

cat /etc/passwd

Typical format:

username:x:UID:GID:comment:home_directory:shell

Example:

Deb:x:1001:1001:Deb:/home/Deb:/bin/bash
/etc/shadow

Contains password hashes and password-aging/security information.

sudo cat /etc/shadow

Access to this file is restricted because it contains sensitive authentication information.

👥 Linux Group Management

Groups are used to manage permissions for multiple users.

Instead of giving permissions to every user individually, users can be placed into groups.

For example:

developers
designers
finance

A company could give the developers group access to development files.

Then adding a new developer to the group automatically gives them the permissions associated with that group.

9. Display All Groups
Command
cat /etc/group

Example:

root:x:0:
developers:x:1002:
finance:x:1003:
10. Create a New Group
Command
sudo groupadd <group-name>

Example:

sudo groupadd developers

Verify:

cat /etc/group
11. Add a User to a Group
Command
sudo usermod -aG <group-name> <username>

Example:

sudo usermod -aG developers Deb
Understanding -aG
-a → append
-G → supplementary groups

Therefore:

sudo usermod -aG developers Deb

means:

Add Deb to the developers supplementary group without removing the user's existing supplementary group memberships.

Why -a is important

If you use:

sudo usermod -G developers Deb

without -a, you can overwrite the user's existing supplementary group list.

So, when adding a user to an additional group, the safer/common form is:

sudo usermod -aG groupname username
12. Remove a User From a Group
Command
sudo gpasswd -d <username> <group-name>

Example:

sudo gpasswd -d Deb developers

This removes Deb from the developers group.

13. List Users in a Specific Group
Command
getent group <group-name>

Example:

getent group developers

Example output:

developers:x:1002:Deb

This shows the group information and members.

14. Check Which Groups a User Belongs To
Command
id <username>

Example:

id Deb

Example output:

uid=1001(Deb) gid=1001(Deb) groups=1001(Deb),1002(developers)

This tells us:

UID → User ID
GID → Primary Group ID
groups → Groups the user belongs to
15. Delete a Group
Command
sudo groupdel <group-name>

Example:

sudo groupdel developers
16. Rename a Group
Command
sudo groupmod -n <new-group-name> <old-group-name>

Example:

sudo groupmod -n engineers developers

This changes:

developers

to:

engineers
⚠️ Troubleshooting — Problems I Encountered

During today's practice, I encountered two important problems.

Problem 1 — sudo Permission Error

I tried to rename a user using:

sudo usermod -l Debarchan Deb

But I received:

Debarchan is not in the sudoers file.
This incident will be reported.
Why did this happen?

I was logged in as:

Debarchan

but Debarchan did not have permission to use sudo.

sudo allows a user to execute commands with administrative/root privileges.

The command:

sudo usermod -l Debarchan Deb

requires administrative privileges because modifying user accounts is a system-level operation.

The system therefore checked whether Debarchan was authorized to use sudo.

It wasn't.

Therefore the command was rejected.

Important Learning

Seeing:

[sudo] password for Debarchan:

does not mean that the user has sudo privileges.

Linux first asks for the user's password and then checks whether that user is authorized to use sudo.

In my case:

Password entered
      ↓
sudo checks authorization
      ↓
Debarchan is not authorized
      ↓
Command rejected
How I Resolved It

I switched back to an account with administrative privileges:

ec2-user

Then I performed administrative user-management commands using:

sudo
Lesson

User-management operations generally require root or sudo privileges.

Problem 2 — useradd Says the Group Already Exists

While logged in as ec2-user, I tried:

sudo useradd Debarchan

I received:

useradd: group Debarchan exists - if you want to add this user to that group, use -g.

This was confusing because I expected useradd to create the user.

What Actually Happened?

The important point is:

A Linux user and a Linux group are separate objects.

I already had a group named:

Debarchan

but I did not have a user named:

Debarchan

So the system looked like:

USER
Debarchan ❌ does not exist

GROUP
Debarchan ✅ exists
Why Did useradd Fail?

Normally, when creating a user, Linux may create a group with the same name depending on the system's user-creation configuration.

But in my case, the group:

Debarchan

already existed.

Therefore Linux refused to create the user using the default behavior and told me to explicitly specify the existing group.

Then I Tried usermod

I ran:

sudo usermod -aG Debarchan Debarchan

and received:

usermod: user 'Debarchan' does not exist
Why?

The syntax is:

usermod -aG GROUP USER

So my command meant:

Add USER:
Debarchan

to GROUP:
Debarchan

But the user didn't exist yet.

Therefore:

Group Debarchan → exists ✅

User Debarchan → doesn't exist ❌

usermod can modify an existing user; it does not create a new user.

How I Resolved Problem 2

Since the group already existed, I could create the user and explicitly specify that existing group as the primary group:

sudo useradd -g Debarchan Debarchan

Here:

-g Debarchan
       ↓
Use the existing Debarchan group

Debarchan
       ↓
Create the user named Debarchan

Then I verified the user:

id Debarchan
🧠 Key Difference: -g vs -aG

This was one of the most important lessons from today's practice.

-g

Used to specify the user's primary group during user creation.

Example:

sudo useradd -g developers Deb

Meaning:

Create user Deb and make developers the primary group.

-aG

Used to add an existing user to one or more supplementary groups.

Example:

sudo usermod -aG developers Deb

Meaning:

Add the existing user Deb to the developers supplementary group.

Simple Mental Model
useradd
   ↓
CREATE a user

usermod
   ↓
MODIFY an existing user

-g
   ↓
PRIMARY group

-aG
   ↓
ADD user to SUPPLEMENTARY group(s)
 Commands I Practiced
Command	Purpose
sudo useradd <user>	Create a user
sudo passwd <user>	Set/change password
cat /etc/passwd	View user account information
su <user>	Switch user
cd ~	Go to current user's home directory
sudo userdel <user>	Delete user
sudo userdel -r <user>	Delete user + home directory
sudo usermod -l <new> <old>	Rename user
cat /etc/group	View groups
sudo groupadd <group>	Create group
sudo usermod -aG <group> <user>	Add user to supplementary group
sudo gpasswd -d <user> <group>	Remove user from group
getent group <group>	View group members
id <user>	View UID, GID and groups
sudo groupdel <group>	Delete group
sudo groupmod -n <new> <old>	Rename group

  Key Takeaways
1. Users and groups are different

A user and a group can have the same name, but they are separate Linux objects.

Debarchan USER  ≠  Debarchan GROUP
2. useradd creates users
sudo useradd Debarchan
3. usermod modifies existing users
sudo usermod -aG developers Debarchan

The user must already exist.

4. -g and -aG have different purposes
-g   → primary group
-aG  → supplementary groups
5. sudo requires authorization

Having a password does not automatically mean a user can use sudo.

The user must be authorized through the sudo configuration.

6. Important files
/etc/passwd → User account information
/etc/shadow → Password hashes + password aging information
/etc/group  → Group information

  Practice Checklist

Create a user

Set a user password

View users

Switch users

Navigate to home directory

Delete users

Rename a user

View groups

Create groups

Add users to groups

Remove users from groups

Check group membership

Delete groups

Rename groups

Troubleshoot sudo permission error

Troubleshoot existing-group/user-creation error

Understand -g vs -aG

  Day 2 Summary

Today I moved beyond basic Linux commands and started working with Linux account administration.

The most valuable part of today's practice was not only learning the commands, but understanding the errors I encountered.

The two major concepts I learned were:

1. sudo permissions
2. User vs Group

And the most important command distinction was:

-g  → Primary group
-aG → Add user to supplementary group

These concepts are fundamental for Linux administration, cloud infrastructure, and eventually working with AW