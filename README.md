# VMX-Features
 CMPE283: Virtualization | Assignment 1: Discovering VMX Features

 #Step1: Configure Linux Machine in Google Cloud

 Create a linux VM instance on GCP. Follow this guide https://cloud.google.com/compute/docs/create-linux-vm-instance
 VM configurations details:

Read this series: https://medium.com/@michael.craig.fitzgerald/building-linux-from-on-a-google-cloud-virtual-machine-5785d6115877

 #Step2: Download and build Linux kernel

 https://phoenixnap.com/kb/build-linux-kernel
 Linux kernel version 6.0.2

got this error in make command
 make[1]: *** No rule to make target 'debian/certs/debian-uefi-certs.pem', needed by 'certs/x509_certificate_list'.  Stop.
make: *** [Makefile:1852: certs] Error 2

cp: cannot stat '/usr/src/linux-source-*/debian/canonical-*.pem': No such file or directory
https://stackoverflow.com/questions/67670169/compiling-kernel-gives-error-no-rule-to-make-target-debian-certs-debian-uefi-ce

 #Step3: Create new kernel module with assignment functionality

 #Step4: Load(insert) the new module

 #Step5: Verify proper output in system message log


Create a VM instance on GCP
gcloud compute instances create instance-1 --project=cmpe-283-virtualization --enable-nested-virtualization --zone=us-central1-a --machine-type=n2-standard-4 --network-interface=network-tier=PREMIUM,subnet=default --maintenance-policy=MIGRATE --provisioning-model=STANDARD --service-account=1061092408272-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=http-server,https-server --create-disk=auto-delete=yes,boot=yes,device-name=instance-1,image=projects/centos-cloud/global/images/centos-stream-8-v20221102,mode=rw,size=100,type=projects/cmpe-283-virtualization/zones/us-central1-a/diskTypes/pd-standard --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --reservation-affinity=any

Install git
sudo dnf update -y
sudo dnf install git -y

clone torvalds:master repo. Crete folder cmpe283 inside the linux folder. Upload the edited cmpe283-1.c and Makefile downloaded from canvas.
git clone https://github.com/torvalds/linux.git

Install make.
sudo yum install make

Execute '''make''' in the linux folder on vm instance. This will fail due to dependency errors.

to solve the errors, install required packages -> flex, bison, binutils, libelf and libssl-dev.
'''
sudo yum -y install flex
sudo yum -y install bison
sudo dnf -y install binutils-x86_64-linux-gnu
sudo yum -y install elfutils-libelf-devel
sudo yum -y install openssl-devel
'''

Execute '''make oldconfig''' 
Execute '''make prepare''' gives cerificate error.

So, we need to create a .config file having contents of the specific kernel.
Execute ```cp /boot/config-4.18.0-408.el8.x86_64 .config``` inside linux directory to copy the config file.

Inside the linux directory, open config file with '''vi .config''' and comment CONFIG_SYSTEM_TRUSTED_KEYS, CONFIG_SYSTEM_TRUSTED_KEYRING and change the value of CONFIG_SYSTEM_REVOCATION_KEYS to "". Save and exit vim editor.

Execute make prepare again.

