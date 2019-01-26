configuring tab completion:
  
	> source <(kubectl completion bash)
	
 port forwarding from local network port to pod port:
  
	> kubectl port-forward <podname> <local-port>:<pod-port>
  
	>> C02SC4SSG8WP:Kubernetes ksarabu$ kubectl port-forward kubia-manual-6475cb7fd5-crvvb 8888:8080 &
  
	>>> Forwarding from 127.0.0.1:8888 -> 8080
  
	>> C02SC4SSG8WP:Kubernetes ksarabu$ curl localhost:8888
  
	>>> Handling connection for 8888
  
	>>> Hello World!

