## Exam Environment Setup

### Terminal Shortcuts/Aliases

The following are useful terminal shortcut aliases/shortcuts to use during the exam.

Add the following to the end of `~/.bashrc` file:

```bashrc
alias k='kubectl                                       # <-- Most general and useful shortcut!

alias kd='kubectl delete --force --grace-period=0      # <-- Fast deletion of resources

alias kc="kubectl create"                              # <-- Create a resource
alias kc-dry='kubectl create --dry-run=client -o yaml  # <-- Create a YAML template of resource

alias kr='kubectl run'                                 # <-- Run/Create a resource (typically pod)
alias kr-dry='kubectl run --dry-run=client -o yaml     # <-- Create a YAML template of resource

# If kc-dry and kr-dry do not autocomplete, add the following
export do="dry-run=client -o yaml"                     # <-- Create the YAML tamplate (usage: $do)
```

The following are some example usages:

```bash
k get nodes -o wide
kc deploymentmy my-dep --image=nginx --replicas=3
kr-dry my-pod --image=nginx --command sleep 36000
kr-dry --image=busybox -- "/bin/sh" "-c" "sleep 36000"
kr --image=busybox -- "/bin/sh" "-c" "sleep 36000" $do
```

### Terminal Command Completion

The following is useful so that you can use the TAB key to auto-complete a command, allowing you to
not always have to remember the exact keyword or spelling.

Type the following into the terminal:

- `kubectl completion bash >> ~/.bashrc` - `kubectl` command completion
- `kubeadm completion bash >> ~/.bashrc` - `kubeadm` command completion
- `exec $SHELL` - Reload shell to enable all added completion

### VIM

The exam will have VIM or nano terminal text editor tools available. If you are using
VIM ensure that you create a `~/.vimrc` file and add the following:

```vimrc
set ts=2             " <-- tabstop - how many spaces is \t worth
set sw=2             " <-- shiftwidth - how many spaces is indentation
set et               " <-- expandtab - Use spaces, never \t values
set mouse=a          " <-- Enable mouse support
```

Or simply:

```vimrc
set ts=2 sw=2 et mouse=a
```

Also know VIM basics are as follows. Maybe a good idea to take a quick VIM course.

- `vim my-file.yaml` - If file exists, open it, else create it for editing
- `:w` - Save
- `:x` - Save and exit
- `:q` - Exit
- `:q!` - Exit without saving
- `i` - Insert mode, regular text editor mode
- `v` - Visual mode for selection
- `ESC` - Normal mode

#### Pasting Text Into VIM

Often times you will want to paste text or code from the Kubernetes documentation into
into a VIM terminal. If you simply do that, the tabs will do funky things.

Do the following inside VIM before pasting your copied text:

1. In `NORMAL` mode, type: `:set paste`
2. Now enter `INSERT` mode
  - You should see `-- INSERT (paste) --` at the bottom of the screen
3. Paste the text
  - You can right click with mouse and select Paste or `CTRL + SHIFT + v` 

### `tmux`

`tmux` will allow you to use multiple terminal windows in one (aka terminal multiplexing).
Make sure you know the basics for `tmux` usage:

> NOTE: `CTRL + b` is the prefix to anything in `tmux`

- `tmux` - Turn and enter `tmux`
- `CTRL + b  "` - Split the window vertically (line is horizontal)
- `CTRL + b  %` - Split the window horizontally (line is vertical)
- `CTRL + b  <ARROW KEY>` - Switch between window panes
- `CTRL + b (hold)  <ARROW KEY>` - Resize current window pane
- `CTRL + b  z` - Toggle full terminal/screen a pane (good for looking at a full document)
- `CTRL + d` or `exit` - Close a window pane


## Preperation

### Study Resources

- [Official Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [A Cloud Guru Course with Practice Exam](https://acloudguru.com/course/certified-kubernetes-administrator-cka)
- [The Kubernetes Book - Nigel Poulton](https://www.amazon.com/Kubernetes-Book-Nigel-Poulton/dp/1521823634)
- [CKA Tips and Tricks](https://youtu.be/TJSAcwUP0pE)

### Practice
- [Play with Kubernetes](https://labs.play-with-k8s.com/)
- [killer.sh Practice questions and environment](https://killer.sh/cka)
- [Practice questions YouTube series](https://youtu.be/uSbqo4X9Zoo)
- [CKA Excample Questions](https://levelup.gitconnected.com/kubernetes-cka-example-questions-practical-challenge-86318d85b4d)
- [GitHub gist of parctive questions](https://gist.github.com/texasdave2/8f4ce19a467180b6e3a02d7be0c765e7)
- [CKAD-Practice-Questions](https://github.com/bbachi/CKAD-Practice-Questions)
- [More CKAD-Practice-Questions](https://github.com/dgkanatsios/CKAD-exercises)



##  Architecture
### What is K8's & Its Atchitecture 
Kubernetes is an open-source container orchestration tool or system that is used to automate tasks such as the management, monitoring, scaling, and deployment of containerized applications. It is used to easily manage several containers 
(since it can handle grouping of containers), which provides for logical units that can be discovered and managed.

### Kuberenets Features
1.  Automated Scheduling : K8 provieds advanced scheduler to launch container on based on their resource requirements and other constraints .
2. Self Healing Capabilites: Rescheduling ,Replacing and restarting the containers which are died.
3. Authomated rollouts and rollback : supports rollouts and rollbacks for the desired state of containerized application.
4. Horizontal scaling and load Balancung : k8 can scale up and scale down the application as per the requirements.
5. service Discovery & Load Balancing :  K8 will automatically assign Ip addresses to containers and a singel DNs name for a set of containers, that can load-balance traffic inside the cluster.
6. Storage Orchestration : local storage, public cloud provieder such as aws,nfs, etc.


### How Container orchestration is beneficial?
* Provisioning and deployment of containers
* Upscaling or removing containers to divide application load evenly all across host infrastructure
* Redundancy and availability of containers
* Moving of containers from one host to another in case there is a shortage of resources in a host (or when the host dies)
* Allocation resources across containers
* Health monitoring of containers and hosts
* Externally exposing services running in a container to the outside world
* Load balancing of service discovery across containers
* Configuring an application relative to the containers running it


## Control Plane
-   The master Node is responsible for the management of k8 cluster,it is mainly entry point for all administrative tasks ,it handles the orchestration of the worker nodes.there can be more then one master node in the cluster for fault tolerance.

- Manages the cluster
- Components
    - **kube-api-server**
       - Serves Kubernetes API
       - Primary interface to control plane and cluster itself
       - Kube API server interacts with API ,its a frontend of the k8 control plane.
       - communication center for developers ,sysadmin and other kubernetes components.
    - **etcd**
        - Backend data store for Kubernetes cluster
        - Distrubeted key value data store(database) of k8.
        - k8 cluster state(info) some of the data maintained in etcd is nodes,deployement ,services,nodes.
    - **kube-scheduler**
        - Selects an available node in the cluster on which to run containers
        - scheduler watches the pods and assigns the pods to run on specific hosts.
    - **kube-controller-manager**
        - Runs collection of multiple controller utilities in a single process
    - **cloud-controller-manager**
        - Interface between Kubernetes and various cloud platforms
        - Only used when using with cloud-based resources (i.e. GCP, AWS, Azure)

## Worker Nodes

- Machines where the containers managed by the cluster are run
- Components
    - **kubelet**
        - Kubernetes agent that runs on a node
        - Communicates with control plane
        - Handles reporting /container status and other data to control plane
    - **Container runtime**
        - Not build into Kubernetes. Separate software.
        - Responsible for running containers on machine
        - Kubernetes supports multiple, i.e. Docker, containerd, etc.
    - **kube-proxy**
        - Network proxy that runs on each node
        - Provides network between containers and cluster

## Kubernetes CLI tools

- `kubeadm`
    - Tool that will simply the process of setting up our Kubernetes cluster
    - Can be used to set up multi-node Kubernetes cluster

- `kubectl`
    - Controls the Kubernetes cluster
    - Communicates with the control plane


- Useful `minikube` commands
  - Start the cluster: `minikube start`
  - Stop the cluster: `minikube stop`
  - Delete the cluster: `minikube delete`
  - Add a control plane node: `minikube node add --control-plane`
  - Add a worker node: `minikube node add --worker`
  - Delete a node: `minikube node delete`
  - Show the dashboard: `minikube dashboard`


# Namespaces (ns)

Docs: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/

- Mechanism for isolating groups of resources (i.e. pods, containers) within a single cluster
- Names of resources need to be unique within a namespace, but not across namespaces.
- A way to divide cluster resources between multiple users
- Virtual clusters backed by same physical cluster (similar to Virtual private networks)
- All clusters have the `default` namespace
- `kube-system` namespace is for system components
- Some resources like nodes and persistantVolumes are not in any namespace


## Working with Namespaces

- Listing existing namespaces
    - `kubectl get namespaces`

- Can specify where the command can run with `--namespace`
    - Example: `kubectl get pods --namespace my-namespace`

- Can specify to look at all namespaces with `--all-namespaces` (or `-A`)
    - Can be slow to complete
    - Example `kubectl get pods --all-namespaces`

- Create a custom namespace
    - `kubectl create namespace my-namespace`

- Set a default namespace for all subsequent commands
    - `kubectl config set-context --current --namespace=<NAMESPACE-NAME>`

- Delecrative way
    - ```
    apiVersion: v1
    kind: Namespace
    metadata:
    name: <insert-namespace-name-here>
     ```


# POD

* A pod always runs on a node.
* Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.
* A Pod is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers
* Containers in the same pod share a local network and the same resources, allowing them to easily communicate with other containers.
* A pod is  a group of one or more containers which will be running on some node.
* Each pod has its unique IP address within the cluster.

### Imperative Way
- create a pod
    - `kubectl run <nameofPod> <image>  <option> <namespace>`
    - `kubectl run nginxpod --image=nginx -n test-ns`

### Delecrative way
**Example:** The following is an example of a Pod which consists of a container running the image `nginx:1.14.2`.

#### pods/simple-pod.yaml
    - Create Pod :
        ``` bash 
        apiVersion: v1
        kind: Pod
        metadata:
        name: nginx
        spec:
        containers:
        - name: nginx
            image: nginx:1.14.2
            ports:
            - containerPort: 80
        #kubectl apply -f pod.yml
        ```

#### Pod Template 
    - Teamplate :
        ```bash
        apiVersion: v1
        kind: Pod
        metadata:
        name: <Podname>
        labels:
            <key>:<value>
        namespace:<nsName>
        spec:
        containers:
        - name: <name of container>
            image: <image>
            ports:
            - containerPort: <containerPort>
            volumeMounts:
            - name: <volname>
            mountpath: <containerDirPath>
            resoure:
            requets:
            cpu: 200m
            memory: 256Mi
            limits:
            cpu: 500m
            memory: 512Mi
        volumes:
        - name: <volName>
            hostPath:
            path: <Hostfolder>
        # kubectl get pods -o wide
        # kubectl describe pod <podname>
        # kubectl describe pod nginx
        ```