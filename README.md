# VMX-Features
 CMPE283: Virtualization | Assignment 1: Discovering VMX Features

(1) Add the remaining code sections to the file.(struct, definitions and vm features)
| MSR Name |	MSR Index |	Description |	References |
| :--- | :--- | :--- | :--- |
| IA32_VMX_PINBASED_CTLS |	0x481 |	This MSR is used for pinbased controls if no true controls capability |	SDM volume 3C, section 24.6.1 |
| IA32_VMX_PROCBASED_CTLS |	0x482 |	This MSR is used for primary procbased controls if no true controls capability |	SDM volume 3C, section 24.6.2 |
| IA32_VMX_PROCBASED_CTLS2 |	0x48B |	This MSR is used for secondary procbased controls if available |	SDM volume 3C, section 24.6.2 |
| IA32_VMX_EXIT_CTLS |	0x483 |	This MSR is used for exit controls if no true controls capability |	SDM volume 3C, section 24.7.1 |
| IA32_VMX_ENTRY_CTLS |	0x484 |	This MSR is used for entry controls if no true controls capability |	SDM volume 3C, section 24.8.1 |

(2) Create a VM instance on GCP. The below command will create a VM with CentOS running on an Intel machine with nested virtualization enabled.
```
gcloud compute instances create instance-1 --project=cmpe-283-virtualization \
--enable-nested-virtualization \
--zone=us-central1-a --machine-type=n2-standard-4 \
--network-interface=network-tier=PREMIUM,subnet=default \
--maintenance-policy=MIGRATE --provisioning-model=STANDARD \
--service-account=1061092408272-compute@developer.gserviceaccount.com \
--scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append \
--tags=http-server,https-server \
--create-disk=auto-delete=yes,boot=yes,device-name=instance-1,image=projects/centos-cloud/global/images/centos-stream-8-v20221102,mode=rw,size=100,type=projects/cmpe-283-virtualization/zones/us-central1-a/diskTypes/pd-standard \ 
--no-shielded-secure-boot \
--shielded-vtpm \
--shielded-integrity-monitoring \
--reservation-affinity=any
```

(3) Install git.
```
sudo dnf update -y
sudo dnf install git -y
```

(4) Clone torvalds/linux master repo. 
```git clone https://github.com/torvalds/linux.git```

(5) Create folder cmpe283 inside the linux folder. Upload the edited cmpe283-1.c and Makefile downloaded from canvas.

(6) Install make.
```sudo yum install make```

(7) Execute ```make``` in the linux folder on vm instance. This will fail due to dependency errors.

(8) To solve the errors, install required packages -> flex, bison, binutils, libelf and libssl-dev.
```sudo yum -y install flex
sudo yum -y install bison
sudo dnf -y install binutils-x86_64-linux-gnu
sudo yum -y install elfutils-libelf-devel
sudo yum -y install openssl-devel
sudo yum install dnf-plugins-core
sudo yum config-manager --set-enabled powertools
sudo dnf install dwarves
```

(9) Execute ```make oldconfig```
 
(10) Execute ```make prepare```. This will throw error. We need to create a .config file having contents of the specific kernel.


(11) Copy the contents of specific kernel version to config file inside linux directory.
```cp /boot/config-4.18.0-408.el8.x86_64 .config```

(12) Inside the linux directory, open config file with ```vi .config``` and comment ```CONFIG_SYSTEM_TRUSTED_KEYS```, ```CONFIG_SYSTEM_TRUSTED_KEYRING```. Update ```CONFIG_SYSTEM_REVOCATION_KEYS=""```. Save and exit vim editor.

(13) Execute ```make prepare``` and ```make```.

(14) Execute the below command to install all the kernel modules.
```sudo make INSTALL_MOD_STRIP=1 modules_install```


(15) Since Ubuntu kernel 4.4.0-20 the ```EFI_SECURE_BOOT_SIG_ENFORCE``` kernel config is enabled. That prevents from loading unsigned third party modules if UEFI Secure Boot is enabled. The easiest way to fix this issue is to disable Secure Boot in UEFI (BIOS) settings. Execute the commands below.
```
sudo yum install mokutil
sudo mokutil --disable-validation
```

(16) It will require to create a password. The password should be at least 8 characters long. After you reboot, UEFI will ask if you want to change security settings. Choose "Yes".

(17) Execute ```sudo reboot```.

(18) Execute ```make``` to get the .ko kernel object.

(19) Execute ```sudo insmod cmpe283-1.ko``` command to insert modules in the kernel.

(20) Execute ```dmesg``` command to display the output printed on the system message buffer console.
