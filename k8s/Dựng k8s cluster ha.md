# Dựng k8s cluster ha
### I. Chuẩn bị
- 6 VM ubuntu ver 20.04 

    |Node|IP|Role|
    |:--:|:--:|:--:|
    |node 1|192.168.20.45|master|
    |node 2|192.168.20.13|master|
    |node 3|192.168.20.24|master|
    |node 4|192.168.20.117|worker|
    |node 5|192.168.20.142|worker|
    |node 6|192.168.20.175|worker|

##### Node specification
- SSH vào master node bất kỳ 
- Các bước:
1.   Bật Password Login
		```
		$ echo  "PermitRootLogin yes" >> /etc/ssh/sshd_config 
		$ sed -i -E 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
2.  Bật password cho ROOT
	```
	$ sudo su 
	$ passwd
3. Tắt swap area (ROOT)
	```
	$ swapoff -a
	```
	Làm như vậy với tất cả các node còn lại.

	Tiếp theo ta cần chọn 1 node để chạy ansible => node 1.
	Cho phép node 1 SSH vào tất cả các node còn lại:

 4. Install parallel-ssh to run a command in parallel on nodes (ROOT)
	```
	$ apt-get update && apt-get install -y pssh
	```
5. Sửa file ``/etc/hosts`` như sau:
    ```
    $ vim /etc/hosts 
    192.168.20.117  worker1
    192.168.20.142  worker2
    192.168.20.175  worker3
    192.168.20.45   master1
    192.168.20.13   master2
    192.168.20.24   master3
    ```
6. Setup SSH access

    Tạo folder setup_ssh tại /~
    ```
    $ mkdir setup_ssh
    ```
    *Chuyển keypair vào folder này (nếu có).*
    
    Tạo file nodes như sau:
    ```
    $ vim ~/setup_ssh/nodes
    worker3
    worker2
    worker1
    master3
    master2
    master1
    ```
    Tạo ssh-key:
    ```
    $ ssh-keygen
    ```
    => SSH key được lưu ở ~/.ssh/id_rsa
    Copy ssh key id vào tất cả các node
    * Nếu không có keypair:
        ```
        $ for i in $(cat nodes); ssh-copy-id $i; done  
        ```
    * Nếu có keypair:
        ```
        $ for i in $(cat nodes); ssh-copy-id -f "-o IdentityFile keypair.pem" $i; done
        ```
### II. Kubespray Configuration

1. Clone the project
    ```
    $ git clone https://github.com/kubernetes-sigs/kubespray
    $ apt-get install -y python3-pip # install pip3 if not installed
    $ cd kubespray
    ```

2. Prepare environment
    ```
    # Install dependencies from ``requirements.txt``
    sudo pip3 install -r requirements.txt
    # Copy ``inventory/sample`` as ``inventory/mycluster``
    cp -rfp inventory/sample inventory/mycluster
    # Update Ansible inventory file with inventory builder
    declare -a IPS=(192.168.20.45 192.168.20.13 192.168.20.24 192.168.20.117 192.168.20.142 192.168.20.175)
    CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}
    ```
3. Sửa file ``inventory/mycluster/hosts.yaml`` như sau:
    ```
    all:
      hosts:
        node1:
          ansible_host: 192.168.20.45
          ip: 192.168.20.45
          access_ip: 192.168.20.45
        node2:
          ansible_host: 192.168.20.13
          ip: 192.168.20.13
          access_ip: 192.168.20.13
        node3:
          ansible_host: 192.168.20.24
          ip: 192.168.20.24
          access_ip: 192.168.20.24
        node4:
          ansible_host: 192.168.20.117
          ip: 192.168.20.117
          access_ip: 192.168.20.117
        node5:
          ansible_host: 192.168.20.142
          ip: 192.168.20.142
          access_ip: 192.168.20.142
        node6:
          ansible_host: 192.168.20.175
          ip: 192.168.20.175
          access_ip: 192.168.20.175
      children:
        kube-master:
          hosts:
            node1:
            node2:
            node3:
        kube-node:
          hosts:
            node4:
            node5:
            node6:
        etcd:
          hosts:
            node1:
            node2:
            node3:
        k8s-cluster:
          children:
            kube-master:
            kube-node:
        calico-rr:
          hosts: {}
    ```
4. Initialize cluster deployment
    ```
    $ ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root cluster.yml
    ```