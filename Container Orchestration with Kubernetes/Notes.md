# Container Orchestration with Kubernetes

Overall, in this lesson we will explore:

- Docker for Application Packaging
- Container Orchestration with Kubernetes
- Kubernetes Resources
- Declarative Kubernetes Manifests

## Docker for Application Packaging

A Dockerfile is a set of instructions used to create a Docker image. Each instruction is an operation used to package the application, such as installing dependencies, compile the code, or impersonate a specific user.

To construct a Dockerfile, it is necessary to use the pre-defined instructions, such as:

    FROM -  to set the base image
    RUN - to execute a command
    COPY & ADD  - to copy files from host to the container
    CMD - to set the default command to execute when the container starts
    EXPOSE - to expose an application port 

Below is an example of a Dockerfile that targets to package a Python hello-world application:

    # set the base image. Since we're running 
    # a Python application a Python base image is used
    FROM python:3.8
    # set a key-value label for the Docker image
    LABEL maintainer="Shivansh Srivastava"
    # copy files from the host to the container filesystem. 
    # For example, all the files in the current directory
    # to the  `/app` directory in the container
    COPY . /app
    #  defines the working directory within the container
    WORKDIR /app
    # run commands within the container. 
    # For example, invoke a pip command 
    # to install dependencies defined in the requirements.txt file. 
    RUN pip install -r requirements.txt
    # provide a command to run on container start. 
    # For example, start the `app.py` application.
    CMD [ "python", "app.py" ]

For example, to build the image of the Python hello-world application from the Dockerfile, the following command can be used:

    # build an image using the Dockerfile from the current directory
    docker build -t python-helloworld .

    # build an image using the Dockerfile from the `lesson1/python-app` directory
    docker build -t python-helloworld lesson1/python-app

For example, to run the Python hello-world application, using the created image, the following command can be used:

**Note: To access the application in a browser, we need to bind the Docker container port to a port on the host or local machine. In this case, 5111 is the host port that we use to access the application e.g. http://127.0.0.1:5111/. The 5000 is the container port that the application is listening to for incoming requests.**

    # run the `python-helloworld` image, in detached mode and expose it on port `5111`
    docker run -d -p 5111:5000 python-helloworld

Resource: [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/#from)

## Container Orchestration with Kubernetes

Kubernetes is widely adopted in the industry today, with most organizations using it in production. Kubernetes currently is a graduated CNCF project, which highlights its maturity and reported success from end-user companies. This is because Kubernetes solutionizes portability, scalability, resilience, service discovery, extensibility, and operational cost of containers.

Solutions | Definition
---|---
Portability | Kubernetes is a highly portable tool. This is due to its open-source nature and vendor agnosticism. As such, Kubernetes can be hosted on any available infrastructure, including public, private, and hybrid cloud.
Scalability | Building for scale is a cornerstone of any modern infrastructure, enabling an application to scale based on the amount of incoming traffic. Kubernetes has in-build resources, such as HPA (Horizontal Pod Autoscaler), to determine the required amount of replicas for a service. Elasticity is a core feature that is highly automated within Kubernetes.
Resilience | Failure is expected on any platform. However, it is more important to be able to recover from failure fast and build a set of playbooks that minimizes the downtime of an application. Kubernetes uses functionalities like ReplicaSet, readiness, and liveness probes to handle most of the container failures, which enables powerful self-healing capability.
Service Discovery | Service discovery encapsulates the ability to automatically identify and reach new services once these are available. Kubernetes provide cluster level DNS (or Domain Name System), which simplifies the accessibility of workloads within the cluster. Additionally, Kubernetes provides routing and load balancing of incoming traffic, ensuring that all requests are served without application overload.
Extensibility | Kubernetes is a highly extensible mechanism that uses the building-block principle. It has a set of basic resources that can be easily adjusted. Additionally, it provides a rich API interface, that can be extended to accommodate new resources or CRDs (Custom Resource Definitions).
Operational Cost | Operational cost refers to the efficiency of resource consumption within a Kubernetes cluster, such as CPU and memory. Kubernetes has a powerful scheduling mechanism that places an application on the node with sufficient resources to ensure the successful execution of the service. As a result, most of the available infrastructure resources are allocated on-demand. Additionally, it is possible to automatically scale the size of the cluster based on the current incoming traffic. This capability is provisioned by the cluster-autoscaler, which guarantees that the cluster size is directly proportional to the traffic that it needs to handle.

**Kubernetes Architecture**

![K8sArch](https://video.udacity-data.com/topher/2020/December/5fdfdd93_screenshot-2020-12-20-at-23.25.47/screenshot-2020-12-20-at-23.25.47.png)

The suite of master nodes, represents the control plane, while the collection of worker nodes constructs the data plane.

**Control Plane**

![ControlPlane](https://video.udacity-data.com/topher/2020/December/5fdfdecf_screenshot-2020-12-20-at-23.31.19/screenshot-2020-12-20-at-23.31.19.png)

The control plane consists of components that make global decisions about the cluster. These components are the:

- kube-apiserver - the nucleus of the cluster that exposes the Kubernetes API, and handles and triggers any operations within the cluster
- kube-scheduler - the mechanism that places the new workloads on a node with sufficient satisfactory resource requirements
- kube-controller-manager - the component that handles controller processes. It ensures that the desired configuration is propagated to resources
- etcd - the key-value store, used for backs-up and keeping manifests for the entire cluster

**Data Plane**

![DataPlane](https://video.udacity-data.com/topher/2020/December/5fdfe11d_screenshot-2020-12-20-at-23.41.02/screenshot-2020-12-20-at-23.41.02.png)

The data plane consists of the compute used to host workloads. The components installed on a worker node are the:

- kubelet - the agent that runs on every node and notifies the kube- apiserver that this node is part of the cluster
- kube-proxy - a network proxy that ensures the reachability and accessibility of workloads places on this specific node

## Kubernetes Resources

Term|Link
---|---
Pods| https://kubernetes.io/docs/concepts/workloads/pods/
Deployment|https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
Service| https://kubernetes.io/docs/concepts/services-networking/service/
Ingress| https://kubernetes.io/docs/concepts/services-networking/ingress/
ConfigMap| https://kubernetes.io/docs/concepts/configuration/configmap/

For more information, refer [Kubernetes Docs](https://kubernetes.io/docs/home/)
## Declarative Kubernetes Manifests

**YAML Manifest structure**

A YAML manifest consists of 4 obligatory sections:

    apiversion - API version used to create a Kubernetes object
    kind - object type to be created or configured
    metadata - stores data that makes the object identifiable, such as its name, namespace, and labels
    spec - defines the desired configuration state of the resource

To get the YAML manifest of any resource within the cluster, use the kubectl get command, associated with the -o yaml flag, which requests the output in YAML format. Additionally, to explore all configurable parameters for a resource it is highly recommended to reference the official Kubernetes documentation.

**Useful command**

Kubernetes YAML manifests can be created using the kubectl apply command, with the following syntax:

    # create a resource defined in the YAML manifests with the name manifest.yaml
    kubectl apply -f manifest.yaml

To delete a resource using a YAML manifest, the kubectl delete command, with the following syntax:

    # delete a resource defined in the YAML manifests with the name manifest.yaml
    kubectl delete -f manifest.yaml

Kubernetes documentation is the best place to explore the available parameters for YAML manifests. However, a support YAML template can be constructed using kubectl commands. This is possible by using the --dry-run=client and -o yamlflags which instructs that the command should be evaluated on the client-side only and output the result in YAML format.

    # get YAML template for a resource 
    kubectl create RESOURCE [REQUIRED FLAGS] --dry-run=client -o yaml

For example, to get the template for a Deployment resource, we need to use the create command, pass the required parameters, and associated with the --dry-run and -o yaml flags. This outputs the base template, which can be used further for more advanced configuration.

    # get the base YAML templated for a demo Deployment running a nxing application
     kubectl create deploy demo --image=nginx --dry-run=client -o yaml
     
**NOTE: If you find any topic missing or need elaborate explanation on a particular topic/heading, do open an issue or post it in the discussions!**

Hope this helps! Happy Coding :tada:
