1
What is Kiali ? 


---
2
Which of the following features does kiali provide for a service mesh?

a. Visualisation of a service mesh.

b. Health check of a service mesh.

c.Logs and metric of a service mesh.

d. API endpoint for a service mesh.

sol:
Kiali is a management console for Istio service mesh. It provides robust observability for your running mesh, letting you quickly identify issues and then troubleshoot those issues. Kiali offers in-depth traffic topology, health grades, powerful dashboards, and lets you drill into the component detail. Kiali offers correlated views of metrics, logs and tracing, as well as strong validations to pinpoint configuration issues. Kiali provides several wizards to help you add services to the mesh, define traffic routing, gateways, policies and more.

a,b,c 

---
3
What kind of operator is the Kiali operator?  
Helm Operator  
OpenShift Operator  
Kubernetes Operator  


Sol:
The Kiali Operator is a Kubernetes Operator.

---
4
Can kiali automatically generate istio configuration?

Yes, kiali can automatically generate istio configuration.

---
5
Install kiali via Istio Addons.


You can find the required template under /root/<istio-directory>/samples/addons. Please also note that the components will be created under istio-system namespace.

sol:

List the directories at /root path: -

ls -l /root/


You will see a directory called istio-1.20.8 (This may not be the same for your lab environment, you will see a different version).


Now, run the below command to deploy the resources: -

kubectl apply -f /root/istio-1.20.8/samples/addons


NOTE:- In the above example, the version used is 1.20.8. This may not be the same for your lab environment. Please make sure to use the correct version.

terminal :
```
root@controlplane ~ ➜  ls -l /root/
total 16
drwxr-x--- 6 root root 4096 Jun 26  2024 istio-1.20.8
-rw-r--r-- 1 root root 1133 Oct 12  2024 kiali-svc.yml
drwxr-xr-x 2 root root 4096 Oct 12  2024 redis
drwx------ 3 root root 4096 Oct  4  2024 snap

```

```
root@controlplane ~ ➜  ls
istio-1.20.8  kiali-svc.yml  redis  snap

```


```
root@controlplane ~ ➜  kubectl apply -f /root/istio-1.20.8/samples/addons
serviceaccount/grafana created
configmap/grafana created
service/grafana created
deployment.apps/grafana created
configmap/istio-grafana-dashboards created
configmap/istio-services-grafana-dashboards created
deployment.apps/jaeger created
service/tracing created
service/zipkin created
service/jaeger-collector created
serviceaccount/kiali created
configmap/kiali created
clusterrole.rbac.authorization.k8s.io/kiali-viewer created
clusterrole.rbac.authorization.k8s.io/kiali created
clusterrolebinding.rbac.authorization.k8s.io/kiali created
role.rbac.authorization.k8s.io/kiali-controlplane created
rolebinding.rbac.authorization.k8s.io/kiali-controlplane created
service/kiali created
deployment.apps/kiali created
serviceaccount/loki created
configmap/loki created
configmap/loki-runtime created
service/loki-memberlist created
service/loki-headless created
service/loki created
statefulset.apps/loki created
serviceaccount/prometheus created
configmap/prometheus created
clusterrole.rbac.authorization.k8s.io/prometheus created
clusterrolebinding.rbac.authorization.k8s.io/prometheus created
service/prometheus created
deployment.apps/prometheus created

```

---
6
Now you have deployed kiali so to access its UI lets create a service.


The service template /root/kiali-svc.yml is present on the controlplane node, use the same to create a service.

sol:  
Run the command kubectl apply -f /root/kiali-svc.yml

terminal: 
```
root@controlplane ~ ➜  kubectl apply -f /root/kiali-svc.yml
service/kiali configured
service/prometheus configured

root@controlplane ~ ➜  
```

---
7
Once done you can access the kiali UI using Kiali option on the top bar.

---
8

Now you can access the Kiali UI so let's try to deploy a simple app to see some data in kiali.


There are some templates under /root/redis directory, apply them all to create redis slave/master deployments and services. Once done, you should be able to see some data/traffic in kiali dashboard related to redis under default namespace.

hint:
Apply all templates present under /root/redis directory using kubectl

sol: 
Run the command kubectl apply -f /root/redis/

terminal:

```
root@controlplane ~ ➜  kubectl apply -f /root/redis/
deployment.apps/redis-master created
service/redis-master created
deployment.apps/redis-slave created
service/redis-slave created

root@controlplane ~ ➜  
```

---



