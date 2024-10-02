# **Creating a Remote Server Using Digital Ocean**


 
 ### **What is a Virtual Machine, and what is Digital Ocean?**

A _Virtual Machine_ is a simulated computer running on physical hardware. It allows us to segregate functions and programs only logically separate computers, running on the same hardware. Another value of virtual machines is that they are much lower cost to operate than to acquire and set up the equivalent hardware ourselves.

For the purposes of learning and experimentation, the ability to create Virtual Machines and access them remotely makes them even more accessible. There are many companies that offer this service as a product. we will be using Digital Ocean for this purpose. After you have created an account on Digital Ocean, we will proceed with setting up a Virtual Machine remotely. Individual machines on Digital Ocean are referred to as _Droplets_, which is a branded term for a Virtual Machine.

 ### **So you've created an account, now what?**

To get started, we should choose which operating system our Virtual Machine will run. In this case, we'll be installing _Arch Linux_. Digital Ocean provides many distributions of Linux already, but for Arch Linux, we'll need to acquire an image ourselves. Images for Arch Linux can be found [here.](https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/1564#:~:text=14%20hours%20ago-,Arch%2DLinux%2Dx86_64%2Dcloudimg%2D20241001.267073.qcow2,-496.74%20MiB)

As we're creating a Virtual Machine on a cloud hosting provider, as opposed to on local hardware, we will need to download the image labelled 'cloudimg', with a file extension of .qcow2.

![image](https://github.com/user-attachments/assets/a28b9556-c47b-462b-8867-bca1d7faedb3)


After downloading the image, we can then add it to our Digital Ocean account. Back on your Digital Ocean Dashboard page, please click on 'Backups & Snapshots'. This is where we can store copies of systems or images for setting up new systems. Click on "Custom Images", then on the "Upload Image" button. This should bring up a box where you can select the image file you've downloaded. Select this file and it should begin to upload. This may take some time depending on your connection speed. Ounce the image is uploaded and ready, the text under 'Uploaded' shows the time since the image was uploaded. Now our Droplet is ready to create.

#### **Creating a Droplet**

Next we can click on the green button at the top of the screen labelled 'Create'. On the dropdown menu, select "Droplets":

![image](https://github.com/user-attachments/assets/92be4955-be6f-4f70-a747-fb63d1b68580)


On the next screen, we can start by selecting the region where our virtual machine will be physically located. For the sake of performance, we should choose the location which is closest to us. Let's select San Francisco. For Datacenter, we'll select Datacenter 3 (SFO3). 

Next we'll need to choose an operating system for our Droplet. Let's click on "Custom images". Here we should see the Arch Linux image we uploaded earlier. Let's select it. 

Our hardware requirements are fairly low, so under "Droplet Type", we'll choose "Basic". Under "CPU options", select "Premium AMD", along with the $7/mo plan. Choosing a higher cost setup may provide us more performance, but as we won't need it for basic needs, we can save our money.

##### **Security!**

We're getting closer to deploying our Droplet, but we have an important question to answer. How will we connect to our Virtual Machine, and how will we make sure the wrong people _can't_ connect. We can definitely set a password, and probably should on our Linux root account, but even better is to use an SSH Key. SSH provides a well established, open source standard for connecting to remote machines. We can use it by creating a key to be stored on our Droplet, and a corresponding key to be stored on our local machine. When we connect using these matching keys, it provides much greater security than a password alone, and can even relieve us of having to type a password at all.

Using the ssh-keygen tool, we can generate a _public key_ for our Droplet, and a _private key_ to be stored on our local machine.

Open your Terminal application on your computer and enter the following command:

```
ssh-keygen -t ed25519 -f C:/Users/USER/.ssh/my-key -C myownmessage
```

> _Note:_ the -t flag allows us to set the encryption type. In this case, we selected ed25519, which provides the best security, at the time of writing this guide.

Where "USER" is your Windows username and the text following -C is a comment appended to the end of your public key, so you can identify it. After pressing enter, you'll be asked to enter a passphrase. This step is not necessary for our needs, but you can enter one if you would like. After this, you will see a confirmation that your keys have been created.

Navigate to your C:/Users/USER/.ssh/ folder, and you will see 2 files named "my-key". The one ending in .pub is your public key, the file without an extension is your private key. Open your public key file in notepad, and copy the contents to your clipboard. Return to our Digital Ocean page and click on "New SSH Key". You can then paste your key into the text box. Give your key a name on the line below, then click "Add SSH Key".

![Pasted image 20241001223123](https://github.com/user-attachments/assets/5e4c999b-3bdd-4b2a-a0e1-c1e8057bb299)


##### **Automation!**

Now we could go ahead and finish creating our droplet, but then we'd have to wait till it's deployed before we can log in and start configuring our settings. With the use of a _cloud-config_ script, we can take care of some of that basic setup automatically. Using a program called Cloud-Init, we can provide the script when deploying our Droplet, and Cloud-Init will automatically perform the steps in our script the first time it boots. Lucky for us, Cloud-Init is already contained in the image we used to create our Droplet.

Let's click on _advanced options_, then check the box labelled _Add Initialization scripts (free)_. After that, we can paste the following code in the box below:
```
#Cloud-Config
users:
	-name: username #use your desired username here
	 primary_group: group #use your desired group name here
	 groups: wheel #assigns user to 'wheel' group
	 shell: /bin/bash #designates default shell
	 sudo: ['ALL=(ALL) NOPASSWD:ALL'] #provides passwordless access to sudo
	 ssh-authorized-keys:
		 -#your public key goes here, starting with 'ssh'

disable_root: true
```


##### **More SSH**

After doing that, we can click on 'Create Droplet', and our Virtual Machine will begin to deploy. While we're waiting for that, let's go through one final step to make connecting to our Droplet easier. Open your home directory, and open the folder named _.ssh_. In this folder, we want to create a file called _config_. If this file already exists, open it in a text editor. We want to add the following entry, after any existing text then save the file.

```
host VMname #replace this with your Droplet name
    Hostname 000.000.000.000 #replace this with your Droplet ip address
    User username #enter the name you used in your cloud-config script
    PreferredAuthentications publickey
    StrictHostKeyChecking no
    IdentityFile ~/.ssh/my-key
    UserKnownHostsFile /dev/null
```


#### **Connecting**

Now the moment to test our work. Open your preferred terminal application and enter:
```
ssh VMname #this should be the name you chose in your 'config' file
```

After pressing Enter, we should see the terminal connect to our cloud hosted Virtual Machine, just like mine here.

![image](https://github.com/user-attachments/assets/82da3d56-a35e-4512-81c2-97de92d0aedd)


