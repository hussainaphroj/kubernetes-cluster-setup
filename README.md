# kubernetes-cluster-setup
Kubernetes cluster with Kubernetes Dashboard setup locally on Ubuntu using Ansible  

First thing first, Please clone the repository to download the code locally.  
``` git clone https://github.com/hussainaphroj/kubernetes-cluster-setup.git ```  

Optionally, you can create 3 Ubuntu machines by using **Vagrantfile** but you have to install the [Virtualbox](https://www.virtualbox.org/wiki/Downloads) and [Vagrant](https://www.vagrantup.com/docs/installation) on your system before creating the machines. 

Once the installation of Virtualbox and Vagrant finishes, browse to clone directory and run ``` vagrant up ``` . This will create the 3 machines. You can change the **IP**, **Hostname** in the given **Vagrantfile**  

# Kubernetes cluster setup  
 I have assumed that you have already set up the Ansible server, if you haven't then please set the  [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) first.  

 Before running the ansible-playbook, please edit the give **host** and **kube.yml** with your IP addresses of the system.  

 I have used the following IP and variables name in the **host** file.  
 ```
 master  ansible_host=192.168.56.80 master="true"  
node1 ansible_host=192.168.56.81 master="false"  
node2 ansible_host=192.168.56.82 master="false"  
```  
Where **master**,  **node1** and **node2** are the **hostname** and last **master** variable decide which server will be master node based **true** values. **master** server will be the kubernetes master in my case.  

We have only 2 variables in our playbook  
```
  vars:
    node_port: '192.168.56.80'
    user: 'vagrant'
````
node_port: IP address of our kubernetes master server  
user: Name of a local user on each node and make sure it's **home directory** should be present and it has **sudo** permission.

We have completed all the prerequisites and it's time to play now. Run the ansible-playbook as:  
```
ansible-playbook -i host kube.yml -u vagrant --ask-pass -b
```
Where **vagrant** is the name of the user who has sudo access to the system. You can use **--ask-become-pass** if your user **requires password** to become sudo.  

Once the Ansible-plabook runs successfully, you will have your cluster ready and can be verified login to your **master** server by running ``` kubectl get nodes ```

Please note that I have used the nodeport type for dashboard service so you can get your port for that service using ``` kubectl get services -n kubernetes-dashboard``` and used master IP and port to access the dashboard. It is ``` https://192.168.56.80:30319 ``` in my case.

The kubernetes dashboard require token to logi, so you can get the token from master using ``` kubectl -n kubernetes-dashboard get secret $(kubectl -n kubernetes-dashboard get sa/admin-user -o jsonpath="{.secrets[0].name}") -o go-template="{{.data.token | base64decode}}" ```

# Setup recording
![setup reconding ](k8_setup.gif)  

That's all my friends!!! start using your Kubernetes cluster and keep your eyes on this space, I will be posting the Kubernetes related stuff here!

**Happy Learning!!!**











