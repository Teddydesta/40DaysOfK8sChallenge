### #40DaysK8sChallenge
### Day 07: Pod In Kubernetes Explained | Imperative VS Declarative Way | YAML

![image](https://github.com/user-attachments/assets/0ab913d9-88cf-48f7-8655-66a376a178b4)

**Welcome** back to my Kubernetes journey! On **Day 7**, I explored creating Pods using both imperative and declarative methods. Here’s what I learned:

**Introduction**
In Kubernetes, there are two primary ways to manage resources: imperative and declarative. Each approach has its pros and cons. In this post, I’ll demonstrate both methods by creating a Pod and modifying its configuration.

### Imperative Approach
The imperative approach involves directly executing commands to create and manage Kubernetes resources. It’s straightforward and quick for simple tasks but can become unwieldy for more complex configurations.

### Task 1: Creating a Pod Using the Imperative Command
Command:

`kubectl run nginx --image=nginx --restart=Never
`

**Explanation**:

kubectl run nginx: Creates a new pod named nginx.
--image=nginx: Uses the nginx image.
--restart=Never: Ensures a pod is created instead of a deployment.
**Pros:**
- Quick and easy for small tasks.
- Immediate execution.

**Cons:**

- Not ideal for complex configurations.
- Hard to maintain and version control.
###  Declarative Approach
- The declarative approach involves defining the desired state of resources in YAML or JSON files and applying them using commands. This method is preferred for managing complex configurations and maintaining consistency.

### Task 2: Creating the YAML from the Nginx Pod and Using It to Create a New Pod
1. Generate YAML from the Existing Pod:

`kubectl get pod nginx -o yaml > nginx-pod.yaml
`

2. Update the Pod Name in the YAML File: Open nginx-pod.yaml and update the metadata.name field from nginx to nginx-new.

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-new
spec:
  containers:
  - image: nginx
    name: nginx
```

3. Create the New Pod Using the Updated YAML:

kubectl apply -f nginx-pod.yaml
Pros:

Suitable for complex and large-scale configurations.
Easy to version control and maintain.
Promotes consistency and repeatability.
Cons:

Requires more initial setup.
Changes take effect after applying the updated files.
### Task 3: Troubleshooting YAML Configurations
In the real world, YAML configurations might have errors that need fixing. Let’s troubleshoot a provided YAML file for a Pod.

Original YAML:

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: test
  name: redis
spec:
  containers:
  - image: rediss
    name: redis
```

Steps to Fix Errors:

Apply the YAML:

`kubectl apply -f redis-pod.yaml
`

**2. Error Encountered:**

The Pod "redis" is invalid: spec.containers[0].image: Invalid value: "rediss": must be a valid image
**3. Fix the Image Name: Edit redis-pod.yaml and correct the image name to redis.**
```

apiVersion: v1
kind: Pod
metadata:
  labels:
    app: test
  name: redis
spec:
  containers:
  - image: redis
    name: redis
```

4. Reapply the Corrected YAML:

`kubectl apply -f redis-pod.yaml
`

### Conclusion
In summary, both imperative and declarative methods have their place in Kubernetes management. The imperative approach is excellent for quick, one-off tasks, while the declarative approach excels in managing complex, version-controlled configurations. Understanding both methods is crucial for efficient Kubernetes operations.

Thank you for joining me on Day 07 of the #40DaysOfKubernetes Challenge! 🙏🚀 Stay tuned for more insights and hands-on tutorials as we continue to dive deeper into the world of Kubernetes and containerization.

**Resources:**
For more detailed information and further reading, check out this link here 👉 https://www.youtube.com/watch?v=_f9ql2Y5Xcc&t=1244s