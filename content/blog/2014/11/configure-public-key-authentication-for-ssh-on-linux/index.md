---
title: "Configure Public Key Authentication for SSH on Linux"
date: "2014-11-26"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "linux"
  - "pka"
  - "public-key-authentication"
  - "putty"
  - "ssh"
  - "sysadmin"
  - "technology"
  - "ubuntu"
  - "windows"
---

Very recently, I acquired myself a cheap Linux based VPS for personal use. Since passwords are barely considered a method of security these days, I decided one of the first things I needed to do was secure my SSH setup.

## Generate keys with PuTTYgen

My chosen method to SSH (and for many others) is via PuTTY. One of the handy things about PuTTY is the PuTTYgen application that comes along with it. PuTTYgen is used to generate and convert keys for use with PuTTY for authentication.

Go ahead and open up PuTTYgen and we will use it to generate both a public key (for the server) and private key (used for the client, _keep this one secure_).

1. Click **Generate**, you will be then instructed to move your mouse around like a madmen to assist in generating the random key code.
2. Upon generation, you will be given the opportunity to provide a passphrase for the key pair.
3. Save both the public key and private key as well as copy the code listed under **Public key for pasting into OpenSSH authorised\_keys file** into a text document

At this point we should have a public key for our Linux server, private key for use with PuTTY and the contents of the public key in a text document for easy copy/paste we will do later.

## Configure keys on host

Now SSH into the server. Once in, we will create a hidden folder in our user's home directory to store the public key and appropriately secure it.

```bash
mkdir ~/.ssh
chmod 700 ~/.ssh
```

Next, change directory to our new hidden folder, create and open up a file to store the key.

```bash
cd ~/.ssh/
nano authorized_keys
```

Once Nano text editor opens up, paste in the contents of the text document we created earlier with the contents of the public key and save.

Let's now secure the file

```bash
chmod 600 authorized_keys
```

## SSH to host with PuTTY

Now you are ready to SSH into the server with Public Key Authentication via PuTTY. To test this, open up PuTTy and fill in all the regular details for the server (IP address etc..). Under **Category** expand the **SSH** section and click on **Auth**. Now click the **Browse** button and navigate to the _Private_ key we saved earlier. Once selection, hit **Open** to start the connection.

If everything was done correctly you should now be connected into your server (or prompted for the passphrase of the private key if you set one).

Can we go further?

## Further secure host

I'm glad you asked; while we have just setup Public Key Authentication, it is still possible to authenticate with your plain old password. But, we can disable password authentication.

First, we are going to backup the current (hopefully default) SSH config by doing the following:

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
```

Next, we will elevate and open up the running SSH config in the Nano text editor

```bash
sudo nano /etc/ssh/sshd_config
```

Once in, look for the following line

```plain
#PasswordAuthentication yes
```

and change to

```plain
PasswordAuthentication no
```

Last but not least, you can add a little security by obscurity by changing the default port to another of your choosing. Simply look for the following line

```plain
Port 23
```

And change 23 to a number of your choosing.

Don't forget to restart the SSH service to allow the above changes to apply

```bash
sudo service ssh restart
```

Now your Linux server is just that little bit more secure.

{{< alert "circle-info" >}}
The above was tested on Ubuntu Server 14.04 LTS, your mileage may very on other versions and flavours
{{< /alert >}}
