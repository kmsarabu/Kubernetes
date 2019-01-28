configuring tab completion:
  
	source <(kubectl completion bash)
	
 port forwarding from local network port to pod port:
  
	kubectl port-forward <podname> <local-port>:<pod-port>
  
	 C02SC4SSG8WP:Kubernetes ksarabu$ kubectl port-forward kubia-manual-6475cb7fd5-crvvb 8888:8080 &
  
	   Forwarding from 127.0.0.1:8888 -> 8080
  
	 C02SC4SSG8WP:Kubernetes ksarabu$ curl localhost:8888
  
	   Handling connection for 8888
  
	   Hello World!

To switch namespace:

	alias kcd='kubectl config set-context $(kubectl config current-context) --namespace '
	
	kcd <namespace>

Starting a Proxy, rest API

	kubectl --kubeconfig /Users/ksarabu/.kube/config proxy --address=0.0.0.0 --port=8080 --accep-hosts=^*$ &

	curl -X GET http://localhost:8080/api/v1/namespaces/default/pods/kubia-manual-6db5774cfd-x4v2h
	
Enable Alpha features -> PodPreset

	Update /etc/kubernetes/manifests/kube-apiserver.yaml:
		
		- --enable-admission-plugins=NodeRestriction,PodPreset
		...
		- --runtime-config=settings.k8s.io/v1alpha1=true
	
	If the Kubernetes cluster is provisioned with kubeadm: Identify docker container id and restart it:
	
		docker restart <containername/ID>
