# Minikube vs. kind vs. k3s - What should use?
https://shipit.dev/posts/minikube-vs-kind-vs-k3s.html

### minikube

minikube is a Kubernetes SIGs project and has been started more than three years ago. It takes the approach of spawning a VM that is essentially a single node K8s cluster. Due to the support for a bunch of hypervisors it can be used on all of the major operating systems. This also allows you to create multiple instances in parallel.

From a user perspective minikube is a very beginner friendly tool. You start the cluster using <code class="language-plaintext highlighter-rouge">minikube start</code>, wait a few minutes and your <code class="language-plaintext highlighter-rouge">kubectl</code> is ready to go. To specify a Kubernetes version you can use the <code class="language-plaintext highlighter-rouge">--kubernetes-version</code> flag. A list of supported versions can be found <a href="https://minikube.sigs.k8s.io/docs/reference/configuration/kubernetes/">here</a>.

If you are new to Kubernetes the first class support for its dashboard that minikube offers may help you. With a simple <code class="language-plaintext highlighter-rouge">minikube dashboard</code> the application will open up giving you a nice overview of everything that is going on in your cluster. This is being achieved by <a href="minikube’s addon system">minikube’s addon system</a> that helps you integrating things like, <a href="https://helm.sh/">Helm</a>, <a href="https://developer.nvidia.com/kubernetes-gpu">Nvidia GPUs</a> and an <a href="https://docs.docker.com/registry/">image registry</a> with your cluster.

### kind

Kind is another Kubernetes SIGs project but is quite different compared to minikube. As the name suggests it moves the cluster into Docker containers. This leads to a significantly faster startup speed compared to spawning VM.

Creating a cluster is very similar to minikube’s approach. Executing <code class="language-plaintext highlighter-rouge">kind create cluster</code>, playing the waiting game and afterwards you are good to go. By using different names (<code class="language-plaintext highlighter-rouge">--name</code>) kind allows you to create multiple instances in parallel.

One feature that I personally enjoy is the ability to load my local images directly into the cluster. This saves me a few extra steps of setting up a registry and pushing my image each and every time I want to try out my changes. With a simple <code class="language-plaintext highlighter-rouge">kind load docker-image my-app:latest</code> the image is available for use in my cluster. Very nice!

If you are looking for a way to programmatically create a Kubernetes cluster, kind kindly (you have been for waiting for this, don’t you :P) publishes its Go packages that are used under the hood. If you want to get to know more have a look at the <a href="https://godoc.org/sigs.k8s.io/kind/pkg/cluster">GoDocs</a> and check out how <a href="https://github.com/kudobuilder/kudo/blob/f7b09025f5c2faf5492624facc1dc4c5c7a5ccad/pkg/test/harness.go#L105">KUDO uses kind for their integration tests</a>.

### k3s

K3s is a minified version of Kubernetes developed by Rancher Labs. By removing dispensable features (legacy, alpha, non-default, in-tree plugins) and using lightweight components (e.g. sqlite3 instead of etcd3) they achieved a significant downsizing. This results in a single binary with a size of around 60 MB.

The application is split into the K3s server and the agent. The former acts as a manager while the latter is responsible for handling the actual workload. I discourage you from running them on your workstation as this leads to some clutter in your local filesystem. Instead put k3s in a container (e.g. by using rancher/k3s) which also allows you to easily run several independent instances.

One feature that stands out is called auto deployment. It allows you to deploy your Kubernetes manifests and Helm charts by putting them in a specific directory. K3s watches for changes and takes care of applying them without any further interaction. This is especially useful for CI pipelines and IoT devices (both target use cases of K3s). Just create/update your configuration and K3s makes sure to keep your deployments up to date.


### Demo

![alt text][def]

## Summary

I was a long time minikube user as there where simply no alternatives (at least I never heard of one) and to be honest…it does a pretty good job at being a local Kubernetes development environment. You create the cluster, wait a few minutes and you are good to go. However for my use cases (mostly playing around with tools that run on K8s) I could fully replace it with kind due to the quicker setup time. If you are working in an environment with a tight resource pool or need an even quicker startup time, K3s is definitely a tool you should consider.

Below you can find a table that lists a few key facts of each tool.

|                                | minikube                                      | kind                   | k3s                                       |
|--------------------------------|-----------------------------------------------|------------------------|-------------------------------------------|
| runtime                        | VM                                            | container              | native                                    |
| supported architectures        | AMD64                                         | AMD64                  | AMD64, ARMv7, ARM64                       |
| supported container runtimes   | Docker,CRI-O,containerd,gvisor                | Docker                 | Docker, containerd                        |
| startup time initial/following | 5:19 / 3:15                                   | 2:48 / 1:06            | 0:15 / 0:15                               |
| memory requirements            | 2GB                                           | 8GB (Windows, MacOS)   | 512 MB                                    |
| requires root?                 | no                                            | no                     | yes (rootless is experimental)            |
| multi-cluster support          | yes                                           | yes                    | no (can be achieved using containers)     |
| multi-node support             | no                                            | yes                    | yes                                       |
| project page                   | <a href="https://minikube.sigs.k8s.io/"> minikube</a> | <a href="https://kind.sigs.k8s.io/">kind </a>  | <a href="https://k3s.io/">k3s </a> |

#### Minikube

1. Install minikube using the official documentation
2. Start minikube using the `minikube start` command
3. Verify that the cluster is running using the `kubectl cluster-info` command
4. Deploy an example application using `kubectl create deployment` and `kubectl expose deployment` commands
5. Access the application using the `minikube service` command

#### kind

1. Install kind using the official documentation
2. Create a Kubernetes cluster using kind using the `kind create cluster` command
3. Verify that the cluster is running using the `kubectl cluster-info` command
4. Deploy an example application using `kubectl create deployment` and `kubectl expose deployment` commands
5. Access the application using the `kubectl port-forward` command or `kubectl proxy` command

#### k3d

1. Install k3d using the official documentation
2. Create a Kubernetes cluster using k3d using the `k3d create cluster` command
3. Verify that the cluster is running using the `kubectl cluster-info` command
4. Deploy an example application using `kubectl create deployment` and `kubectl expose deployment` commands
5. Access the application using the `kubectl port-forward` command or `kubectl proxy` command

[def]: demo.gif