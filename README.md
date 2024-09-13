# Automating-Website-Hosting-on-a-CentOS-VM-with-Vagrant

![image](https://github.com/user-attachments/assets/0868af5e-99b9-4c44-b4d2-2834e385d523)

In my previous article, I provided a manual guide on hosting a website on a CentOS VM. In this post, I'll walk you through the process of automating the hosting of an HTML template on CentOS using httpd and Vagrant.
We will create a Vagrantfile that provisions the website automatically, leveraging the power of Infrastructure as Code (IaC).


### **Tools used**

- **Ready-made template:** Free HTML website templates from tooplate.com
- **Vagrant:** Automates the creation and management of virtual environments.
- **CentOS:** A free, enterprise-class Linux distribution, ideal for server environments.
- **Git Bash:** A command-line tool that allows users to run Git commands and other Unix-based utilities on Windows.
- **VsCode:** A source-code editor for development


### **Understanding Infrastructure as Code (IaC)**

**Infrastructure as Code (IaC)** is the practice of managing and provisioning computing infrastructure through machine-readable configuration files. This approach allows for consistent, repeatable, and automated deployment processes, eliminating the need for manual setup.


### **Step-by-Step Setup**

**1. Create a CentOS VM with Vagrant**
First, create a CentOS VM using Vagrant. For guidance, refer to my previous article for a detailed walkthrough.



**2. Prepare Your Workspace in VSCode**

- Open **Visual Studio Code**
- Click on **File** > **Open Folder**
- Navigate to the folder of the **`CentOS VM`** you created
  
  ![image](https://github.com/user-attachments/assets/081bea72-769e-4e57-88ed-a91c4e3d45f0)



**3. Rename the Project Folder**
- **Rename** the Folder:
- Change the name of your project folder (e.g., from **`finance`**) to **`financeIAC`**.
- This signifies that the project now utilizes Infrastructure as Code principles.

![image](https://github.com/user-attachments/assets/b567e288-fe98-436f-a2fb-db6f49180fbb)

![image](https://github.com/user-attachments/assets/6223df1b-890a-4d6b-93ac-e60ed0b59f8d)



**4. Remove the .vagrant Directory**
- Delete the **`.vagrant`** Folder:
- In the **`financeIA`C** directoru, delete the **`.vagrant`** directory to ensure a clean environment. This folder contains state information that Vagrant uses, and removing it allows Vagrant to recreate the VM from scratch.

![image](https://github.com/user-attachments/assets/6a7844ec-f3c6-4251-92bc-a6699464f559)



**5. Modify the Vagrantfile**
**a. Update Network Configuration**
- Open **`Vagrantfile`** in **`VSCode`**.
- Change the **`IP Address`** to **`192.168.56.28`**:

![image](https://github.com/user-attachments/assets/ffe6e91c-9381-46f6-9756-07b520d5878c)

**b. Add Provisioning Script**
- Add the Following Script at the bottom of the **`Vagrantfile`**:

```config.vm.provision "shell", inline: <<-SHELL
  yum install httpd wget unzip vim -y
  systemctl start httpd
  systemctl enable httpd
  mkdir -p /tmp/finance
  cd /tmp/finance
  wget https://www.tooplate.com/zip-templates/2135_mini_finance.zip
  unzip -o 2135_mini_finance.zip
  cp -r 2135_mini_finance/* /var/www/html/
  systemctl restart httpd
  cd /tmp/
  rm -rf /tmp/finance
SHELL
```

![image](https://github.com/user-attachments/assets/5b7d6f00-5366-4837-bd84-d0421d4aaec1)

- **Save** the **`Vagrantfile`** **(use Ctrl + S)**.



**6. Launch the VM Using Git Bash**
**a. **Open** Git Bash:**
- Navigate to the **`financeIAC`** directory:
- Use the command **`cd /vagrant-vms/financeIAC`**

![image](https://github.com/user-attachments/assets/cf1af099-fa0c-4e23-a96d-1bc6a413bdbd)

b. Start the **VM**, using the **`vagrant up`** command.
- Vagrant will create the VM and automatically run the provisioning script defined in the **`Vagrantfile`**.

![image](https://github.com/user-attachments/assets/43ca2adc-d96a-4b70-9684-9f2ac42bcaec)

![image](https://github.com/user-attachments/assets/5cc39c09-a9bb-4e76-9b47-db890997cca5)



**7. Access the Deployed Website**
- Open a Web Browser, and Navigate to **`http://192.168.56.28/`**
- You should see the hosted website loaded from the VM.

  ![image](https://github.com/user-attachments/assets/2920cf13-3dfe-4dcd-bdbb-9dbb3123d445)



## **Conclusion**
By automating the provisioning of the CentOS VM and the deployment of the website, we've transformed a manual process into a reproducible and efficient workflow. Leveraging Vagrant and the principles of Infrastructure as Code, you can easily manage and deploy environments consistently.

