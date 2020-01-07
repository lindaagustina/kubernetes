# KUBERNETES

## Membuat sebuah Deployment menggunakan Python+Flask

Pod dalam Kubernetes adalah kumpulan dari satu atau banyak Container yang saling terhubung untuk kebutuhan administrasi dan jaringan. Deployment dalam Kubernetes selalu memeriksa kesehatan Pod dan melakukan restart saat Kontainer di dalam Pod tersebut mati. Deployment digunakan untuk membuat dan mereplikasi Pod.
1. Menggunakan perintah `kubectl create` untuk membuat Deployment. Pod menjalankan Container berdasarkan image docker yang digunakan. Disini saya menggunakan image docker lindaagustina/python-flask:v1 (image ini saya buat pada pertemuan 8, yang telah saya push ke Docker Hub). Pada Deployment ini Pod hanya memiliki 1 Container saja.

`kubectl create deployment python-flask --image=lindaagustina/python-flask:v1`

Output :

`deployment.apps/python-flask created`

2. Melihat Deployment yang telah dibuat

`kubectl get deployments`

Output :
`NAME           READY   UP-TO-DATE   AVAILABLE   AGE
python-flask   1/1     1            1           3m23s`

3. Melihat Pod yang telah dibuat

`kubectl get pods`

Output :

`NAME                            READY   STATUS    RESTARTS   AGE
python-flask-6db7946645-swlkl   1/1     Running   0          3m29s`

4. Melihat event yang terjadi pada cluster

`kubectl get events`

Output :

`LAST SEEN   TYPE     REASON                    OBJECT                               MESSAGE
7m8s        Normal   NodeHasSufficientMemory   node/minikube                        Node minikube status is now: NodeHasSufficientMemory
7m8s        Normal   NodeHasNoDiskPressure     node/minikube                        Node minikube status is now: NodeHasNoDiskPressure
7m8s        Normal   NodeHasSufficientPID      node/minikube                        Node minikube status is now: NodeHasSufficientPID
6m48s       Normal   Starting                  node/minikube                        Starting kubelet.
6m48s       Normal   NodeHasSufficientMemory   node/minikube                        Node minikube status is now: NodeHasSufficientMemory
6m48s       Normal   NodeHasNoDiskPressure     node/minikube                        Node minikube status is now: NodeHasNoDiskPressure
6m48s       Normal   NodeHasSufficientPID      node/minikube                        Node minikube status is now: NodeHasSufficientPID
6m47s       Normal   NodeAllocatableEnforced   node/minikube                        Updated Node Allocatable limit across pods
6m46s       Normal   RegisteredNode            node/minikube                        Node minikube event: Registered Node minikube in Controller
6m43s       Normal   Starting                  node/minikube                        Starting kube-proxy.
6m38s       Normal   NodeReady                 node/minikube                        Node minikube status is now: NodeReady
3m37s       Normal   Scheduled                 pod/python-flask-6db7946645-swlkl    Successfully assigned default/python-flask-6db7946645-swlkl to minikube
3m36s       Normal   Pulling                   pod/python-flask-6db7946645-swlkl    Pulling image "lindaagustina/python-flask:v1"
3m3s        Normal   Pulled                    pod/python-flask-6db7946645-swlkl    Successfully pulled image "lindaagustina/python-flask:v1"
3m3s        Normal   Created                   pod/python-flask-6db7946645-swlkl    Created container python-flask
3m2s        Normal   Started                   pod/python-flask-6db7946645-swlkl    Started container python-flask
3m37s       Normal   SuccessfulCreate          replicaset/python-flask-6db7946645   Created pod: python-flask-6db7946645-swlkl
3m37s       Normal   ScalingReplicaSet         deployment/python-flask              Scaled up replica set python-flask-6db7946645 to 1`

5. Melihat konfigurasi `kubectl`

`kubectl config view`

Output :

`apiVersion: v1
clusters:
- cluster:
    certificate-authority: /root/.minikube/ca.crt
    server: https://172.17.0.22:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /root/.minikube/client.crt
    client-key: /root/.minikube/client.key`

## Membuat sebuah Service

Secara default, Pod hanya bisa diakses melalui alamat IP internal di dalam cluster Kubernetes. Supaya Container python-flask bisa diakses dari luar jaringan virtual Kubernetes, saya harus ekspos Pod sebagai Service Kubernetes.

1. Ekspos Pod pada internet publik menggunakan perintah `kubectl expose` `--type-LoadBalancer` digunakan untuk ekspos Service keluar dari Cluster .

`kubectl expose deployment python-flask --type=LoadBalancer --port=5000`

Output :

`service/python-flask exposed`

2. Melihat Service yang telah dibuat

`kubectl get services`

Output :

`NAME           TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
hello-node     LoadBalancer   10.96.82.224    <pending>     8080:31300/TCP   3m8s
kubernetes     ClusterIP      10.96.0.1       <none>        443/TCP          13m
python-flask   LoadBalancer   10.96.222.184   <pending>     5000:30775/TCP   10s`

3. Akses pada browser menggunakan port 30775 (dapat dilihat pada service).
