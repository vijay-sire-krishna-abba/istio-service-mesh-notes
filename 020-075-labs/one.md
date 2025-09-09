1 / 9
Service Meshes help handle communication between services without having to change code within each micro-service.

Yes, we can handle the communication between services using a service mesh tool like istio.


---


Which of the following commands can be used to make sure that any new pods that are created in default namespace will automatically have an Istio sidecar proxy added to them?

kubectl label namespace default istio-sidecar=enabled
kubectl label namespace default istio-injection=enabled
kubectl label namespace default istio-injection-sidecar=enabled


Run the following command: kubectl label namespace default istio-injection=enabled

---

Which of the following is a valid `routing API` version of Istio?


networking.istio/v1
networking.istio.io/v1alpha1
networking.istio.io/v1alpha3
networking.istio/v2

networking.istio.io/v1alpha3 is the valid routing API version of Istio.

---

Which of the following command is used to validate the istio configuration?

istioctl analyze
istioctl authz
istiocti verify
istiocti validate


We can use the command istioctl analyze to validate the Istio configuration.

---

We will now deploy our sample Bookinfo application on the Kubernetes cluster.
There is an app setup template istio-sample.yml under the /root directory, deploy the same in Kubernetes cluster.


Please make sure PODs are in running state before hitting the check button.

Hint:
Use kubectl apply command.
Sol:
Run the command kubectl apply -f /root/istio-sample.yml to deploy.

___

So we just deployed the app in default namespace. Check if this namespace is enabled for Istio injection.

If enabled then proceed further, if not then enable the same and proceed.

Hint:
Using istioctl analyze, we can check if istio injection is enabled.

Solution:
Use the command kubectl label namespace default istio-injection=enabled to enable default namespace for istio injection.

___

Now istio injection has been enabled in the default namespace. We must now re-deploy the app for the changes to take effect. Use the same template we used earlier to delete and re-deploy the app.


Make sure all old PODs are terminated and all new PODs are in running state before hitting the check button.


Hint:
Delete the app first and then re-deploy using the same template.

Sol:
Run the command kubectl delete -f /root/istio-sample.yml to delete the resources and then run the command kubectl apply -f /root/istio-sample.yml to re-deploy the resources.

---

Istio has now injected the sidecar proxy in each pod, we can verify the same by describing the pod details.

Check and verify how many containers are running in each pod now.
We can check using the command kubectl get pods


---

Inspect the newly created pods and identify the image used by the sidecar container.

Options:
istio: 1.11.4
envoy:1.11.4
docker.io/istio/proxyv2:1.20.8
sidecar:1.11.4

Hint:
Run the command: kubectl describe pod <pod>. Use any pod created in the default namespace and look for the image used by the istio sidecar container.

Solution:
docker.io/istio/proxyv2:1.20.8

---
