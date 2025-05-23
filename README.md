# OpenShift-IPI-Installation-on-AWS
Red Hat OpenShift Installer-Provisioned-Installation on AWS

[Documentation Author: Ankit Jain](https://www.linkedin.com/in/ankitkkjain/)

Setting up Red Hat OpenShift on AWS using **Installer-Provisioned Infrastructure (IPI)** can be tricky, but I’ve got you covered! Here’s a detailed step-by-step guide to help you troubleshoot and install it correctly:

### **Prerequisites**
Before starting, ensure you have:
- An **AWS account** with necessary permissions.
- **AWS CLI** installed and configured.
  <img width="953" alt="image" src="https://github.com/user-attachments/assets/509a5ae5-6b8e-4b63-8429-b83cd497d32a" />
  <img width="742" alt="image" src="https://github.com/user-attachments/assets/dde303e6-3a8c-4e9c-9007-d8478a90a2cc" />

  ```
  sudo yum install awscli
  ```
- **OpenShift CLI** (`oc`) and **OpenShift Installer** downloaded.
  ```
  [root@ip-172-31-3-124 ~]# wget https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/4.6.0/openshift-client-linux.tar.gz
  --2025-05-18 15:53:54--  https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/4.6.0/openshift-client-linux.tar.gz
  Resolving mirror.openshift.com (mirror.openshift.com)... 65.9.112.70, 65.9.112.103, 65.9.112.43, ...
  Connecting to mirror.openshift.com (mirror.openshift.com)|65.9.112.70|:443... connected.
  HTTP request sent, awaiting response... 200 OK
  Length: 24316608 (23M) [application/x-tar]
  Saving to: ‘openshift-client-linux.tar.gz’
  openshift-client-linux.tar.gz                   100%[=====================================================================================================>]  23.19M  7.51MB/s    in 3.5s
  2025-05-18 15:53:59 (6.56 MB/s) - ‘openshift-client-linux.tar.gz’ saved [24316608/24316608]
  ```
  ```
  [root@ip-172-31-3-124 ~]# wget https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/4.6.0/openshift-install-linux.tar.gz
  --2025-05-18 15:54:13--  https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/4.6.0/openshift-install-linux.tar.gz
  Resolving mirror.openshift.com (mirror.openshift.com)... 65.9.112.114, 65.9.112.43, 65.9.112.103, ...
  Connecting to mirror.openshift.com (mirror.openshift.com)|65.9.112.114|:443... connected.HTTP request sent, awaiting response... 200 OK
  Length: 86927978 (83M) [application/x-tar]
  Saving to: ‘openshift-install-linux.tar.gz’
  openshift-install-linux.tar.gz                    0%[                                                                                                      ] 123.54K   284KB/s
  openshift-install-linux.tar.gz                    0%[                                                                                                      ] 364.50K   565KB/s
  openshift-install-linux.tar.gz                  100%[=====================================================================================================>]  82.90M  8.95MB/s    in 10s
  2025-05-18 15:54:24 (8.25 MB/s) - ‘openshift-install-linux.tar.gz’ saved [86927978/86927978]
  ```
- A **pull secret** from [Red Hat Hybrid Cloud Console](https://console.redhat.com/openshift/downloads).
  ![image](https://github.com/user-attachments/assets/8aa85ff2-b8ca-423e-9b5c-64352326f772)

- A **domain configured in AWS Route 53** (for DNS resolution).
- <img width="371" alt="image" src="https://github.com/user-attachments/assets/6d8bec73-a33f-4e2e-9f88-4d77f3292b2c" />


### **Step-by-Step Installation**


#### **1. Configure AWS Credentials on Putty**
- Ensure AWS CLI is installed and configured:
  ```bash
  aws configure
  ```

#### **2. Create Install Configuration**
- Generate an install config file:
  ```bash
  openshift-install create install-config
  ```
- Provide:
  ```
  - **Platform:** AWS
  - **Cluster name**
  - **Region**
  - **Pull secret**
  - **Base domain** (configured in Route 53)
  ```
  
#### **4. Deploy OpenShift Cluster**
- Run the installation:
  ```bash
  openshift-install create cluster  --dir=</Directory>
  ```
  ```
  [root@ip-172-31-0-139]# openshift-install create cluster --dir=/clusterconfig/
  INFO Consuming "Install Config" from target directory
  INFO Creating infrastructure resources...
  INFO Waiting up to 30m0s for the Kubernetes API at https://api.openshift-training.kloudguru.in:6443...
  INFO API v1.14.6+ae5fcd2 up
  INFO Waiting up to 30m0s for bootstrapping to complete...
  INFO Destroying the bootstrap resources...
  INFO Waiting up to 30m0s for the cluster at https://api.openshift-training.kloudguru.in:6443 to initialize...
  INFO Waiting up to 10m0s for the openshift-console route to be created...
  INFO Install complete!
  INFO To access the cluster as the system:admin user when using 'oc', run 'export KUBECONFIG=/clusterconfig/auth/kubeconfig'
  INFO Access the OpenShift web-console here: https://console-openshift-console.apps.openshift-training.kloudguru.in
  INFO Login to the console with user: kubeadmin, password: xxxx-xxxx-xxxx-xxxx
  [root@ip-172-31-0-139]#
  ```
- Monitor logs:
  ```bash
  tail -f .openshift_install.log
  ```

#### **5. Verify Cluster Deployment**
- Check nodes:
  ```bash
  oc get nodes
  ```
- Check namespaces:
  ```bash
  oc get ns
  ```

### **Troubleshooting Instructions for Common Issues**
- **Installation stuck?** Check AWS IAM permissions.
- **DNS issues?** Ensure Route 53 is correctly configured.
- **Cluster not reachable?** Verify security groups and VPC settings.

For more details, check [AWS Prescriptive Guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/red-hat-openshift-on-aws-implementation/installation-options.html) and [OpenShift Installation Guide](https://keithtenzer.com/openshift/openshift-4-aws-ipi-installation-getting-started-guide/).
