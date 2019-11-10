etcdctl snapshot save snapshot.db \
 --endpoints=https://127.0.0.1:2379 \
 --cacert=/etc/etcd/ca.crt \
 --cert=/etc/etcd/etcd-server.crt \
 --key=/etc/etcd/etcd-server.key
 

ls snapshot.db

etcdctl snapshot status snapshot.db

service kube-apiserver stop

etcdctl snapshot restore --data-dr /var/lib/etcd-newdb --initial-cluster master1=host:port,host:port \
--initial-cluster-token etcd-cluster-1 --initial-advertise-peer-urls <host>:2380

update etcd.service with new data location, cluster token, etc

systemctl daemon-reload

service etcd restart

service kube-apiserver start
