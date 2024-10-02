1. Creating SSH keys and adding them to your account
2. Add a custom Arch Linux image using the web console
3. Use a cloud-init configuration file to automate initial setup tasks (e.g., user creation).
4. Connect to your server using your ssh keys.


 **What is a virtual machine? What is Digital Ocean?**

A _Virtual Machine_ is a simulated computer running on physical hardware. It allows us to segregate functions and programs only logically separate computers, running on the same hardware. Another value of virtual machines is that they are much lower cost to operate than to acquire and set up the equivalent hardware ourselves.

For the purposes of learning and experimentation, the ability to create Virtual Machines and access them remotely makes them even more accessible. There are many companies that offer this service as a product. we will be using Digital Ocean for this purpose. After you have created an account on Digital Ocean, we will proceed with setting up a Virtual Machine remotely. Individual machines on Digital Ocean are referred to as _Droplets_, which is a branded term for a Virtual Machine.


**So you've created an account, now what?**

To get started, we should choose which operating system our Virtual Machine will run. In this case, we'll be installing _Arch Linux_. Digital Ocean provides many distributions of Linux already, but for Arch Linux, we'll need to acquire an image ourselves. Images for Arch Linux can be found [here.](https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/1564#:~:text=14%20hours%20ago-,Arch%2DLinux%2Dx86_64%2Dcloudimg%2D20241001.267073.qcow2,-496.74%20MiB)

As we're creating a Virtual Machine on a cloud hosting provider, as opposed to on local hardware, we will need to download the image labelled 'cloudimg', with a file extension of .qcow2.

![image](https://github.com/user-attachments/assets/a28b9556-c47b-462b-8867-bca1d7faedb3)


After downloading the image, we can then add it to our Digital Ocean account. Back on your Digital Ocean Dashboard page, please click on 'Backups & Snapshots'. This is where we can store copies of systems or images for setting up new systems. Click on "Custom Images", then on the "Upload Image" button. This should bring up a box where you can select the image file you've downloaded. Select this file and it should begin to upload. This may take some time depending on your connection speed. Ounce the image is uploaded and ready, the text under 'Uploaded' shows the time since the image was uploaded. Now our Droplet is ready to create.

Next we can click on the green button at the top of the screen labelled 'Create'. On the dropdown menu, select "Droplets":

![image](https://github.com/user-attachments/assets/92be4955-be6f-4f70-a747-fb63d1b68580)


On the next screen, we can start by selecting the region where our virtual machine will be physically located.





**Creating and storing SSH keys**

To securely access your virtual machine remotely, you can
