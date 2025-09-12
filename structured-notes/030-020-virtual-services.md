Got it 👍 You want me to **rewrite your raw extracted notes** into a **clear, structured, and detailed explanation**—like a study guide—with proper sections, bullet points, explanations, and keeping your provided screenshots (`![Screenshot](...)`) exactly where they are.

Here’s the polished and structured Markdown version for your notes on **Istio Virtual Services**:

---

# 030-020 - Virtual Services

## 🔹 Introduction

In Istio, **Virtual Services** define how requests are routed within the **Service Mesh**. They provide powerful, flexible rules that allow fine-grained traffic management.

Think of it this way:

* The **Gateway** (e.g., `bookinfo.app`) is the *entry point* into the mesh.
* The **Virtual Service** decides *where exactly inside the mesh* the traffic should go.

---

## 🔹 Why Virtual Services?

* Without Virtual Services, traffic from the Ingress Gateway would have no routing rules.
* With Virtual Services, we can:

  * Route specific paths (`/productpage`, `/login`, `/static`, etc.) to specific services.
  * Perform **advanced routing** (A/B testing, canary deployments, traffic splitting).
  * Control traffic **independent of the number of pods**.
  * Use **exact matches** or **prefix matches** (e.g., `/static/*`).

---

## Example 1: Routing to `productpage`

Traffic to `bookinfo.app` should reach the `productpage` service.

### Steps

1. **Define the Virtual Service:**

   * API version: `networking.istio.io/v1alpha3`
   * Kind: `VirtualService`
   * Metadata: `bookinfo`
   * Host: `bookinfo.app`
   * Gateway: `bookinfo-gateway`
   * HTTP rules for paths: `/productpage`, `/static`, `/login`, `/logout`, `/api/v1/products`

2. **YAML Definition:**

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: bookinfo
spec:
  hosts:
    - "bookinfo.app"
  gateways:
    - bookinfo-gateway
  http:
    - match:
        - uri:
            exact: /productpage
        - uri:
            prefix: /static
        - uri:
            exact: /login
        - uri:
            exact: /logout
        - uri:
            prefix: /api/v1/products
      route:
        - destination:
            host: productpage
            port:
              number: 9080
```

📌 This ensures all traffic to those URIs routes directly to the `productpage` service.

**Timestamp:** 01:45
![Screenshot](/030-020-virtual-services/01_45_240.png)

**Timestamp:** 03:47
![Screenshot](/030-020-virtual-services/03_47_216.png)

**Timestamp:** 04:02
![Screenshot](/030-020-virtual-services/04_02_714.png)

---

## Example 2: Reviews Service (Multiple Versions)

The `productpage` service needs to call `reviews`. Over time, new versions (`v2`, `v3`) are introduced.

### Without Istio (Plain Kubernetes)

1. **v1 Deployment**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: reviews-v1
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: reviews
        version: v1
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: reviews
spec:
  ports:
    - port: 9080
      name: http
  selector:
    app: reviews
```

**Result:**

* 100% traffic goes to `reviews v1`.

**Timestamp:** 06:02
![Screenshot](/030-020-virtual-services/06_02_223.png)

---

2. **Introduce v2 (1 Pod)**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: reviews-v2
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: reviews
        version: v2
```

* Now service has 4 pods:

  * 3 pods of v1 (75%)
  * 1 pod of v2 (25%)

**Timestamp:** 06:44
![Screenshot](/030-020-virtual-services/06_44_132.png)

---

3. **Introduce v3 (1 Pod)**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: reviews-v3
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: reviews
        version: v3
```

* Service now routes:

  * v1 → 60%
  * v2 → 20%
  * v3 → 20%

**Timestamp:** 07:12
![Screenshot](/030-020-virtual-services/07_12_995.png)

---

4. **Promote v3 to 100% Traffic**

```bash
kubectl scale deployment reviews-v3 --replicas=3
```

**Timestamp:** 07:40
![Screenshot](/030-020-virtual-services/07_40_274.png)

```bash
kubectl scale deployment reviews-v3 --replicas=3
kubectl scale deployment reviews-v2 --replicas=0
kubectl scale deployment reviews-v1 --replicas=0
```
**Timestamp:** 07:49
![Screenshot](/030-020-virtual-services/07_49_606.png)

✅ Canary testing is achieved by scaling pods, but this has **limitations**:

* No fine-grained control (cannot do 1% / 99%).
* Traffic split tied to number of pods.

---

## Example 3: With Istio Virtual Service

Instead of relying on pod scaling, we use **Virtual Services** with traffic weights.

### YAML Example:

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts:
    - reviews
  http:
    - route:
        - destination:
            host: reviews
            subset: v1
          weight: 99
        - destination:
            host: reviews
            subset: v2
          weight: 1
```

📌 Now we can send:

* 99% traffic → `reviews v1`
* 1% traffic → `reviews v2`

💡 Number of pods **does not matter** anymore.

**Timestamp:** 10:32
![Screenshot](/030-020-virtual-services/10_32_660.png)

---

## 🔹 Key Takeaways

* Virtual Services **decouple traffic routing from Kubernetes pod counts**.
* Support **exact** and **prefix** matching for URIs.
* Enable **canary deployments** and **A/B testing** without scaling hacks.
* Traffic split is controlled by **weights**, independent of replicas.
* Subsets (covered later) group pods by labels for routing.

---

Would you like me to also add a **Mermaid diagram** (like flowcharts of traffic) for `Gateway → VirtualService → Service → Pod Versions` to make it even more visual?
