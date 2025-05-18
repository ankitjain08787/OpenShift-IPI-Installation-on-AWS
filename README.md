# OpenShift-IPI-Installation-on-AWS
Red Hat OpenShift Installer-Provisioned-Installation on AWS

[Documentation Author: Ankit Jain](https://www.linkedin.com/in/ankitkkjain/)

Setting up Red Hat OpenShift on AWS using **Installer-Provisioned Infrastructure (IPI)** can be tricky, but I’ve got you covered! Here’s a detailed step-by-step guide to help you troubleshoot and install it correctly:

### **Prerequisites**
Before starting, ensure you have:
- An **AWS account** with necessary permissions.
- **AWS CLI** installed and configured.
- **OpenShift CLI** (`oc`) and **OpenShift Installer** downloaded.
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
