- 👋 Hi, I’m @vinodkumar0911
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
vinodkumar0911/vinodkumar0911 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
####### INSTALLING KUBERNETS CLUSTER #######
1) Install SSH
sudo apt-get install openssh-server

2) Login as a Root User
sudo su

3) Run an update
 apt-get update

4) Install Docker and then enable it
apt install -y docker.io 
sudo systemctl enable docker 

5) apt-get update && apt-get install -y apt-transport-https

6) curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

7) cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF

8) apt-get update

9) apt-get install -y kubelet kubeadm kubectl                 

10) Next, we will change the configuration file of Kubernetes. Run the following command:

nano /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

This will open a text editor, enter the following line after the last

“Environment” Variable.

Environment=”cgroup-driver=systemd/cgroup-driver=cgroupfs”

 Press Ctrl+X, then press Y, and then press Enter to 

IF YOU ARE USING ORACLE VM VIRTUALBOX FILES THEN ALSO RUN FOLLOWING COMMANDS OR USING ON PREM HOSTED MACHINES


10-A) Run following two commands on all your machines and then reboot all your machines

1) sudo swapoff -a
2) sudo sed -i '/ swap / s/^/#/' /etc/fstab
3) reboot the nodes



Run the below commands ONLY on the MASTER node
11)  kubeadm init --apiserver-advertise-address=<ip-address-of-kmaster-vm> --pod-network-cidr=192.168.0.0/16


kubeadm init --apiserver-advertise-address=10.122.0.8 --pod-network-cidr=192.168.0.0/16

     Run following command if "kubeadm init" command fails

kubeadm reset
systemctl restart kubelet

kubeadm init --apiserver-advertise-address=10.8.04 --pod-network-cidr=192.168.0.0/16

12) You will get following commands to run.  Run them as a normal user (only in master)
  mkdir -p $HOME/.kube
  
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  
  sudo chown $(id -u):$(id -g) $HOME/.kube/config


13) Save the second command for later and execute that on worker nodes to join the master

kubeadm join 172.31.81.24:6443 --token xoq8rf.rsmij5a41jo349jp \
    --discovery-token-ca-cert-hash sha256:367de9ffbdba10f633ff5594e62afc3186bb1b350b05e7f411d18e4f8e2810ad

	
	RUN FOLLOWING COMMANDS AS NORMAL USER on Master
	
14) kubectl get pods -o wide --all-namespaces 

15) You will notice from the previous command, all the pods are running except some like kube-dns. For resolving this we will install a pod network. To install the pod network, run the following command:
kubectl apply -f https://docs.projectcalico.org/v3.23/manifests/calico.yaml

16) watch kubectl get pods --all-namespaces

Ctrl + C to come out from watch

18) Run following command on your master to check the status of nodes:

      kubectl get nodes

Step 19 is only if you are creating your cluster with AWS EC2 instances.
19) Add Following Firewall rules in AWS Security to allow master to be accessed by worker nodes. 
All TCP	TCP	0 - 65535	0.0.0.0/0	-
Custom TCP	TCP	6443	0.0.0.0/0	-

20) It is time to join your node to the cluster! This is probably the only step that you will be doing on the node, 
after installing kubernetes on it. Run the join command that you saved, when you ran kubeadm init command on the master. Note: Run this command with “sudo”.

21) Run following command again on your master to check the status of the cluster:

    kubectl get nodes

Your Kubernetes Cluster is now ready! With a Master and a Node :-)


22) Following token command we will run  only on worker machines

kubeadm join 10.8.0.4:6443 --token fh6gwq.utnyi2eco4fylgy2 \
        --discovery-token-ca-cert-hash sha256:204d57fdf4b3a6f56ee2562fcde559273f6824e491eb19eaea61907116b349f8



23) Run following command again on your master to check the status of the cluster:

    kubectl get nodes

Your Kubernetes Cluster is now ready! With a Master and a Node :-)
