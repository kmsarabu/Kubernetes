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

	kubectl --kubeconfig /Users/ksarabu/.kube/config proxy --address=0.0.0.0 --port=8080 --accep-hosts=^*$ --port=8080 &

	curl -X GET http://localhost:8080/api/v1/namespaces/default/pods/kubia-manual-6db5774cfd-x4v2h
	
rest API without the Proxy

	$ kubectl config view -o jsonpath='{range .clusters[*]}{.cluster.server}{"\n"}{end}'
	
	https://192.168.99.20:6443
	
	$ export APISERVER=https://192.168.99.20:6443
	
	$ export TOKEN=$(kubectl get secrets -o jsonpath="{.items[?(@.metadata.annotations['kubernetes\.io/service-account\.name']=='default')].data.token}"|base64 -D)
	
	$ curl -X GET $APISERVER/api --header "Authorization: Bearer $TOKEN" --insecure
	
	
	
Enable Alpha features -> PodPreset

	Update /etc/kubernetes/manifests/kube-apiserver.yaml:
		
		- --enable-admission-plugins=NodeRestriction,PodPreset
		...
		- --runtime-config=settings.k8s.io/v1alpha1=true
	
	If the Kubernetes cluster is provisioned with kubeadm: Identify docker container id and restart it:
	
		docker restart <containername/ID>



Ch1:

root@km1:~/kubernetes/ch1# cat app.js
const http = require('http');
const os = require('os');
console.log("Kubia server starting...");
var handler = function(request, response) {
  console.log("Received request from " + request.connection.remoteAddress);
  response.writeHead(200);
  response.end("You've hit " + os.hostname() + "\n");
};
var www = http.createServer(handler);
www.listen(8080);

root@km1:~/kubernetes/ch1# cat Dockerfile 
FROM docker.vmntech.com:5000/node:7
ADD app.js /app.js
ENTRYPOINT ["node", "app.js"]


kubectl run kubia --image=docker.vmntech.com:5000/ch1e1 --port=8080 --generator=run/v1 --output=yaml --dry-run=true
