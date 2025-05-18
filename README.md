# OpenShift-IPI-Installation-on-AWS
Red Hat OpenShift Installer-Provisioned-Installation on AWS

[Documentation Author: Ankit Jain](https://www.linkedin.com/in/ankitkkjain/)

Setting up Red Hat OpenShift on AWS using **Installer-Provisioned Infrastructure (IPI)** can be tricky, but I’ve got you covered! Here’s a detailed step-by-step guide to help you troubleshoot and install it correctly:

### **Prerequisites**
Before starting, ensure you have:
- An **AWS account** with necessary permissions.
- **AWS CLI** installed and configured.
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
Connecting to mirror.openshift.com (mirror.openshift.com)|65.9.112.114|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 86927978 (83M) [application/x-tar]
Saving to: ‘openshift-install-linux.tar.gz’

openshift-install-linux.tar.gz                    0%[                                                                                                      ] 123.54K   284KB/s
openshift-install-linux.tar.gz                    0%[                                                                                                      ] 364.50K   565KB/s
openshift-install-linux.tar.gz                  100%[=====================================================================================================>]  82.90M  8.95MB/s    in 10s

2025-05-18 15:54:24 (8.25 MB/s) - ‘openshift-install-linux.tar.gz’ saved [86927978/86927978]
```
- A **pull secret** from Red Hat.
- A **domain configured in AWS Route 53** (for DNS resolution).

### **Step-by-Step Installation**
#### **1. Download and Install CLI Tools**
- Go to [Red Hat OpenShift Install](https://keithtenzer.com/openshift/openshift-4-aws-ipi-installation-getting-started-guide/) and download the OpenShift installer and CLI tools.
- Extract and install:
  ```bash
  sudo tar xvf openshift-client-linux.tar.gz -C /usr/bin
  sudo tar xvf openshift-install-linux.tar.gz -C /usr/bin
  ```
- Verify installation:
  ```bash
  openshift-install version
  ```

#### **2. Configure AWS Credentials**
- Ensure AWS CLI is installed and configured:
  ```bash
  aws configure
  ```
- Verify permissions:
  ```bash
  rosa verify permissions
  rosa verify quota
  ```

#### **3. Create Install Configuration**
- Generate an install config file:
  ```bash
  openshift-install create install-config
  ```
- Provide:
  - **Platform:** AWS
  - **Cluster name**
  - **Region**
  - **Pull secret**
  - **Base domain** (configured in Route 53)

#### **4. Deploy OpenShift Cluster**
- Run the installation:
  ```bash
  openshift-install create cluster
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
