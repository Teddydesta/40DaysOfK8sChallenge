#40DaysK8sChallenge
Day 06: Kubernetes Multi Node Cluster Setup🚀

Introduction
Welcome to Day 6 of the #40DaysOfKubernetes Challenge! This challenge is prepared by Piyush Sachdeva. In today’s session, we’ll be exploring the different options available for hosting a Kubernetes cluster. Additionally, we’ll walk through step-by-step tutorials on setting up both single-node and multi-node clusters using Kind (Kubernetes in Docker). Finally, we’ll learn how to set the context to switch between multiple clusters seamlessly. This knowledge is crucial for anyone looking to efficiently manage and deploy applications across various Kubernetes environments.

In this article, we will cover the following topics:

Kubernetes Hosting Options
Single Node Cluster Setup 🖥️
Multi-Node Cluster Setup 🖥️🖥️
Switching Contexts Between Multiple Clusters 🔄
I. Kubernetes Hosting Options 🌐
Self-Hosted Clusters 🏠
2. Managed Kubernetes Services 🛠️

Google Kubernetes Engine (GKE) 🌳
Amazon Elastic Kubernetes Service (EKS) 🛡️
Azure Kubernetes Service (AKS) ☁️
3. Kubernetes in Docker (Kind) 🐳

4. Minikube 🚀

5. MicroK8s 🔧

Even though there are numerous Kubernetes hosting options available, for the purposes of this challenge, we will focus on “Kind” (Kubernetes in Docker). “Kind” is a tool designed for running local Kubernetes clusters using Docker containers as nodes. It is particularly useful for testing and development purposes, as it allows you to create a Kubernetes cluster quickly and easily on your local machine without the need for a full-fledged cloud provider setup.

Why Use Kind?

Simplicity: Kind is straightforward to set up and use. It leverages Docker to create Kubernetes nodes, making it accessible for anyone familiar with Docker.
Lightweight: It doesn’t require a heavy infrastructure setup, making it ideal for local development and testing.
Flexibility: You can create multi-node clusters, which is great for testing more complex configurations and workloads.
Speed: Since it runs locally, you can quickly spin up and tear down clusters without the overhead of cloud-based solutions.
Throughout this challenge, we will use Kind to set up and manage our Kubernetes clusters. This approach will provide us with a hands-on experience that can be easily replicated and modified as we progress through the various concepts and tasks in Kubernetes.

II. Single Node Cluster Setup 🖥️
Pre-Requisites
Install Docker — You must have docker installed and running in your system. If not, you can get it from here as per your OS 👉
Install Docker Engine
Learn how to choose the best method for you to install Docker Engine. This client-server application is available on…
docs.docker.com

Install Kubectl (optional) — kind does not require kubectl, but kubectl is needed to manage and interact with Kubernetes clusters, perform operations on resources.
Follow this documentation to install and set up kubectl.
before Creating a single Node cluster we need to install Kind on our machine or computer, so use the following method to install Kind:-

For Linux User:-

# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64
# For ARM64
[ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-arm64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
For Windows Powershell User:-

curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.23.0/kind-windows-amd64
Move-Item .\kind-windows-amd64.exe c:\some-dir-in-your-PATH\kind.exe
For macOS User:-

# For Intel Macs
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-darwin-amd64
# For M1 / ARM Macs
[ $(uname -m) = arm64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-darwin-arm64
chmod +x ./kind
mv ./kind /some-dir-in-your-PATH/kind
To see if KIND is installed on your system, you can use the command kind version to see what version of KIND is installed.


Here we go , we have installed “kind” on our local machine, now lets create single-node Cluster

Creating a single-node cluster without any config file. Just use the below command:

# Create a Kubernetes cluster using kind with a specified node image version
kind create cluster --image --name:<version>

# In my case, I use kind version 1.29.4
kind create cluster --image kindest/node:v1.29.4


This process will leverage a pre-configured node image to initialize a Kubernetes cluster. These prebuilt images are available at kindest/node.

To confirm that your single-node cluster is operating properly, you can use the command below:-

kubectl get nodes

Changing node image:
To use a different image, apply the “— image` flag with the command: `kind create cluster — image=…”

Let say you need to change the node image version to v1.30.0 use the below command:-

kind create cluster --image kindest/node:v1.30.0@sha256:047357ac0cfea04663786a612ba1eaba9702bef25227a794b52890dd8bcd692e
By using a different image, you can alter the Kubernetes version of the created cluster. Each version of kind supports a specific set of Kubernetes versions, which you can find listed on the release page here 👉 https://github.com/kubernetes-sigs/kind/releases

Deleting Cluster
You can delete the cluster using the following commands:


#  For default clusters:

  kind delete cluster
 

# For clusters with a specific context name:

  kind delete cluster --name <context-name>
III. Creating Multi-node Cluster
To set up a multi-node `kind` cluster environment, copy and paste the following config file to your preferred code editor, In my case used “nano ”


# this config file contains all config fields with comments
# NOTE: this is not a particularly useful config file
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
# patch the generated kubeadm config with some extra settings
kubeadmConfigPatches:
- |
  apiVersion: kubelet.config.k8s.io/v1beta1
  kind: KubeletConfiguration
  evictionHard:
    nodefs.available: "0%"
# patch it further using a JSON 6902 patch
kubeadmConfigPatchesJSON6902:
- group: kubeadm.k8s.io
  version: v1beta3
  kind: ClusterConfiguration
  patch: |
    - op: add
      path: /apiServer/certSANs/-
      value: my-hostname
# 1 control plane node and 3 workers
nodes:
# the control plane node config
- role: control-plane
# the three workers
- role: worker
- role: worker
- role: worker

In this configuration file, we set up a multi-node cluster with one control plane and three worker nodes. You can adjust the number of nodes based on your needs.

Save the above config file as config.yaml .

You can create a cluster using a predefined configuration file with the below command:

kind create cluster - config <file name> 

You can verify the multi-node clusters by executing the bellow command to ensure that all nodes are functioning properly.

`kubectl get nodes
Here it is: as shown in the terminal output, we have successfully created a multi-node cluster with one control plane and three worker nodes.


To list all the running containers you can run docker ps .


IV. Switching Contexts Between Multiple Clusters 🔄
By default, the cluster is named `kind`. Use the “— name” flag to assign a different context name to the cluster.

You can use the bellow command to list the currently active clusters.

kubectl config get-contexts

To switch between different cluster, you can use the bellow command:-

kubectl config use-context <cluster-name>

Interacting with the cluster:
Once the cluster is created, you can use `kubectl` to interact with the cluster set up by “kind”.

# For the default cluster:
kubectl cluster-info

# For a cluster with a specified context name:
kubectl cluster-info --context <context-name>

Conclusion
Setting up both single-node and multi-node Kubernetes clusters is a foundational skill for effective Kubernetes management. By mastering Kind (Kubernetes in Docker), you can quickly create and manage clusters for development and testing. Understanding how to configure clusters, switch contexts, and interact with your cluster using kubectl enhances your ability to deploy and troubleshoot applications efficiently.

Thank you for joining me on Day Six of the #40DaysOfKubernetes Challenge! 🙏🚀 Stay tuned for more insights and hands-on tutorials as we continue to dive deeper into the world of Kubernetes and containerization.

Resources: — For more detailed information and further reading, check out this link here 👉 https://www.youtube.com/watch?v=RORhczcOrWs&t=326s





