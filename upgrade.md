apt update

apt-cache policy kubeadm

apt-get install -s kubeadm=1.16.2-00

kubectl drain master --ignore-daemonsets

kubeadm upgrade plan

kubeadm upgrade apply v1.16.2

apt-get install -s kubelet=1.16.2-00 kubectl=1.16.2-00 

systemctl restart kubelet

kubectl uncordon master



kubectl drain node01 --ignore-daemonsets

apt-get install -s kubeadm=1.16.2-00

apt-get install -s kubelet=1.16.2-00

kubeadm upgrade node config --kubelet-version v1.16.2-00

apt-get install -s kubectl=1.16.2-00

systemctl restart kubelet

kubectl uncordon node01
