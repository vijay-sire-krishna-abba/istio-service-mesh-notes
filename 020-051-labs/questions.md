What is istio?

istio is a health check tool for Kubernetes cluster.
istio is just an API service within Kubernetes cluster.
Istio is an open source service mesh platform.

---

Does istio provide any kind of encryption for service-to-service network operations?
yes

---
Is istio installed on this cluster?
Check if istio PODs are running using kubectl get pod -n istio-system command in the terminal.

Which of the following statements is false?

false = a. istio can only run on Kubernetes.

b. istio can run on a variety of platforms like Kubernetes, Mesos, Nomad & Consul etc.

c. istio simplifies service-to-service network operations like traffic management, authorization, and encryption etc.

d. istio provides tracing, monitoring, and logging features as well.

---

Install istio on Kubernetes cluster.
Kubernetes cluster is already setup.


HINT : Take help from this doc: https://istio.io/latest/docs/setup/getting-started/#download

solution:

```
curl -L https://istio.io/downloadIstio | sh -
cd istio-<version-number>
export PATH=$PWD/bin:$PATH
istioctl install --set profile=demo -y
```
---

Awesome! You have now successfully installed Istio on the kubernetes cluster. Can you identify which version of Istio was installed?

Use the command `istioctl version` to find the version of Istio installed.
