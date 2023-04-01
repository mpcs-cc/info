Very , Very, VERY IMPORTANT WARNING: If you use VS Code as your editor/IDE you will run into major issues in this class that we cannot resolve. The issue is that VS Code does not behave nicely when editing files on a remote machine, which you will need to do in almost every assignment. For this reason, I very strongly recommend you use a different editor (like Sublime). See  below for details.

As the class progresses, you will find yourself doing most/all of your work directly on EC2 instances. Hence, a common request is to easily create an ssh/sftp connection between the text editor on your local machine and an EC2 instance, so you can edit files on the remote machine as if they were local. Depending on the editor you use, there may be a remote editing plug-in available that allows you to do just that.

If you use [Sublime](https://www.sublimetext.com/), I already have the rmate package included in the class base AMI. In order to enable remote editing you just need to do the following on your MacOS or Linux laptop (see below for Windows): 

1. Install the rsub plug-in for Sublime Text, available through the Package Control tool:
Go to Tools → Command Palette → Package Control: Install Package. 
Type ‘rsub’ and install.

2. Configure remote port forwarding for the EC2 instance in your ~/.ssh/config file to enable the connection. The easiest approach is to forward the port for all remote hosts (so that you don't need to update it as your instance DNS name changes):

```bash
Host *
    Hostname %h
    RemoteForward 52698 127.0.0.1:52698
```

Now, make sure Sublime is already open on your laptop, connect to the EC2 instance, and edit a file on your instance as follows:

`rmate <filename_to_edit>`

If it’s a system/root-owned file, you must use:

`sudo rmate <filename_to_edit>`

Alternatively, there are a few free sftp clients available that can be configured to open files in a local editor. If you don't mind paying for software, try Transmit (https://panic.com/transmit/) which I've used for years—it has great connectivity options and even allows you to browse your Amazon storage directly, which may come in handy later on in the course.
