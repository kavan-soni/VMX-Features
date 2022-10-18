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
