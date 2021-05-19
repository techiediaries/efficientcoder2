---
layout: post
title:  "Fix /dev/kvm is Not Found and Device Permission Denied Errors on Linux/Ubuntu 20.04/19.04"
date:   2020-04-25
tags: [ ubuntu ]
---

When working with Android SDK and Android Studio to develop mobile apps on your Linux Ubuntu 20.04,  you often need to use emulators to test your apps. In this case, you will need to use an AVD (Android Virtual Device) to create a new device, but sometimes, you'll  encouter problems when creating new virtual devices with the `/dev/kvm is not found` and `/dev/kvm` device permission denied error messages. 

## What's KVM and How to Enable It?

KVM stands for Kernel-based Virtual Machine and it's a full virtualization solution for Linux on x86 hardware containing virtualization extensions (Intel VT or AMD-V). 

### Solving `/dev/kvm is not found` Error

You need to enable KVM, from the BIOS by pressing  `F1`  key before the system boot. Next, go to the `Security`  tab and enable `Intel Virtualization Technology`  and  `Intel VT-d Feature`. Save the new settings by pressing  `F10`. Finally, exit and restart your computer. 

### Solving `/dev/kvm device permission denied` Error

After enabling KVM, you'll likely have another error message that says `/dev/kvm device permission denied`

To fix this error. you need to install  `qemu-kvm`  and add your username to the  `kvm`  group.

Head over to your terminal and run the following command to install `qemu-kvm`:

```bash
sudo apt install qemu-kvm  
```


Next, you need to add the user  `your-username`  to the kvm group using the following command:

```bash
sudo adduser username kvm  
```

Next, in some cases, you also need to run the following command:

```bash
sudo chown username /dev/kvm  
```


You can get your username using the following command:

```bash
whoami  
```

Now, you can verify if your username is added to `kvm` group using the following command:

```bash
grep kvm /etc/group  
```

If your user name is added. Finish by restarting your Ubuntu 20 system.

Next, follow the steps tp create a new AVD, to start using your Android emulator. 

