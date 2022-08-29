# K8-Mitre-Mapping

The definitions within the table are taken directly from the [weaveworks](
https://www.weave.works/blog/mitre-attack-matrix-for-kubernetes/) blog post which described the second version of Microsoft's threat matrix for Kubernetes in detail. The full breakdown, including Stratus Red Team mappings, is provided in the following blog post: <https://ohlinger.co/posts/mapping-k8s-to-mitre/>

The MITRE mappings were created by Microsoft and are not officially supported by the MITRE ATT&CK framework. Where possible, mappings from the Microsoft Kubernetes threat matrix to the existing MITRE ATT&CK techniques have been made. 

| Microsoft Threat Matrix Name | Description | Technique(s) | MITRE ATT&CK Mapping |
| -- | -- | -- | -- |
| Using Cloud Credentials | If the Kubernetes cluster is deployed in a public cloud and if a cloud credential is compromised, attackers may get access to the cluster’s management layer. A compromised credential may lead to a compromise of the entire cluster and may also lead to a cluster takeover. | Initial Access | [T1078.004](https://attack.mitre.org/techniques/T1078/004/) |
| Compromised Images in Registry | A compromised, untrusted or unsafe image running in a cluster can lead to its compromise. It may be downloaded from a public image registry (e.g., Docker Hub), built from untrusted base images, or added to a private registry by an attacker and pulled by a user. It may contain malicious code that allows an attacker to access a cluster. | Initial Access | [T1525](https://attack.mitre.org/techniques/T1525/) |
| Kubeconfig File | The kubeconfig file, used by kubectl, contains the location and credentials of clusters. A compromised client could issue cloud commands to download this file if the cluster is hosted as a cloud service. If a bad actor gets access to this file, they can use it to access the clusters. | Initial Access | [T1552.001](https://attack.mitre.org/techniques/T1552/001/) |
| Application Vulnerability | Containerized, public-facing applications with vulnerabilities leave an organization susceptible to exploits by threat actors. If they run in a cluster, the threat actor may gain initial access to the cluster. They may also exploit the vulnerability to reach other applications, access sensitive data, or launch a Denial of Service (DoS) attack. | Initial Access | [T1190](https://attack.mitre.org/techniques/T1190/) |
| Exposed Sensitive Interfaces | When a sensitive interface is exposed to the Internet, it creates a security risk. Frameworks that don’t require authentication by default are particularly vulnerable. Exposing such frameworks allows malicious actors to gain unauthenticated access to a sensitive interface and run code or deploy containers in the cluster. One such interface that has been exploited previously is the Kubernetes dashboard. | Initial Access | [T1133](https://attack.mitre.org/techniques/T1133/) |
| Exec Into Container | An attacker could use the exec command kubectl exec to remotely run malicious commands in cluster containers. If they have permissions, they may use legitimate images, such as an OS image as a backdoor container, to execute malicious code and compromise resources within a cluster. | Execution | [T1609](https://attack.mitre.org/techniques/T1609/) | 
| bash/cmd Inside Container | In this technique, an attacker with permissions could run a bash script inside a container to execute malicious code and compromise cluster resources. | Execution | [T1609](https://attack.mitre.org/techniques/T1609/) |
| New Container | A threat actor with permissions may use malicious code in the Kubernetes cluster by deploying a container. They may deploy a new pod or a controller in the cluster, such as Deployments, DaemonSets, or ReplicaSets. They can then create a new resource to execute their malicious code and compromise cluster resources. | Execution | [T1610](https://attack.mitre.org/techniques/T1610/) |
| Application Exploit (RCE) | Applications in some clusters may contain a vulnerability that allows for remote code execution. Attackers may exploit this vulnerability to execute malicious code in the cluster. They may also compromise other resources in the cluster, access sensitive data on metadata servers, or cause a DoS attack. If the service account is mounted to the container, they may use its credentials to send requests to the kubelet read-only API server. | Execution | [T1203](https://attack.mitre.org/techniques/T1203/) |
| SSH Server Running Inside Container | An SSH (Secure Socket Shell) server running inside a container is vulnerable to threat actors. If an attacker gains credentials to that container by brute force or phishing, they may remotely access the container to run malicious code and compromise resources. | Execution | [T1021.004](https://attack.mitre.org/techniques/T1021/004/) |
| Sidecar Injection | A sidecar container is an additional container that resides alongside the main container and shares storage and network resources with other containers in a Kubernetes pod. Attackers may inject a sidecar container into a legitimate Kubernetes pod in the cluster to run their malicious code and hide their activity. | Execution | [T1055](https://attack.mitre.org/techniques/T1055/) |
| Backdoor Container | An attacker could utilize Kubernetes controllers like DaemonSets or Deployments to ensure that a constant number of containers are always running in one or all nodes of the cluster. They may execute malicious code in a cluster container. | Persistence | [T1053.007](https://attack.mitre.org/techniques/T1053/007/) |
| Writeable hostPath Mount | The hostPath volume mounts a file or directory from the host to the container. If the attacker has permission to create a new container in the cluster, they may do so with a writable hostPath volume. This allows them to persist on the underlying container host, for example, by creating a cron job on the host.| Persistence | [T1611](https://attack.mitre.org/techniques/T1611/) |
| Kubernetes Cronjob | A Kubernetes Job controller creates one or more pods to accomplish a specific task. It also ensures that a specified number of pods terminate successfully. It may be used to run containers that perform finite tasks for batch jobs. Kubernetes CronJob creates Jobs on a recurring schedule. An attacker can leverage CronJob to schedule the execution of malicious code, which would run as a container in a cluster. | Persistence | [T1053.007](https://attack.mitre.org/techniques/T1053/007/) |
| Malicious Admission Controller | A threat actor may use a malicious admission controller in Kubernetes to access credentials. One such controller is ValidatingAdmissionWebhook, a generic, built-in controller whose behavior is determined by an admission webhook deployed in the cluster. Attackers may use this webhook to intercept sensitive information like requests to the API server, and to record secrets. | Persistence | [T1056](https://attack.mitre.org/techniques/T1056/) | 
| Privileged Container | A privileged container has all the capabilities of the host machine and none of the limitations of a regular container. If an attacker gains access to a privileged container or has the permissions to start a new privileged container, they can gain access to the host’s resources, or compromise other containers running on the same host. | Privilege Escalation | [T1611](https://attack.mitre.org/techniques/T1611/) |
| Cluster-Admin Binding | Cluster-admin is a built-in high privileged role in Kubernetes. Users with this role have full access to the cluster, and can potentially compromise it. Attackers with RBAC (Role-based access control) permissions to create bindings and cluster-bindings in the cluster can create a binding to the cluster-admin role or other roles with high privileges. | Privilege Escalation | [T1098](https://attack.mitre.org/techniques/T1098/) |
| hostPath Mount | The hostPath volume mounts a file or directory from the host to the container. This can allow attackers to gain access to the underlying host or resources, break from the container to the host, or compromise other containers running on the same host. | Privilege Escalation | [T1611](https://attack.mitre.org/techniques/T1611/) |
| Access Cloud Resources | An adversary may use this technique to gain access to a single container in a Kubernetes cloud cluster to access other cloud resources outside the cluster. For instance, if they gain access to the service principal credential file in Azure Kubernetes Service (AKS), they may be able to use these credentials to access or modify the cloud resources. | Privilege Escalation | [T1550](https://attack.mitre.org/techniques/T1550/) 
| Clear Container Logs | With this technique, an attacker may delete logs from the container runtime or operating system that capture the attacker’s activity within containers. | Defense Evasion | [T1070](https://attack.mitre.org/techniques/T1070/)  | 
| Delete K8s Events | Another technique an attacker may use to avoid detection is to delete Kubernetes audit logs, which provide a chronological record of security-relevant activities taken by users or system components within a Kubernetes cluster. | Defense Evasion | [T1562](https://attack.mitre.org/techniques/T1562/) |
| Pod / Container Name Similarity | This threat arises from the possibility that pods created by controllers such as Deployments or DaemonSets may have a random suffix in their names. An attacker could use a random suffix to obfuscate the presence of an unauthorized pod within the cluster that could be used to execute malicious code or gain access to additional resources. | Defense Evasion | [T1036.005](https://attack.mitre.org/techniques/T1036/005/)  |
| Connect From Proxy Server | To conceal their network origin, attackers may employ this technique by using proxy servers or anonymous networks to hide their IP address. | Defense Evasion | [T1090](https://attack.mitre.org/techniques/T1090/) | 
| List K8S Secrets | Kubernetes Secrets are objects that enable organizations to store and manage sensitive credentials, including passwords, tokens, keys, and connection strings in the cluster. A K8S Secret is by default, stored unencrypted in the API server's underlying data store (etcd), so anyone with access to etcd or API access can retrieve or modify a Secret. An attacker with permission to retrieve the secrets from the API server may access sensitive information, including credentials for key services, such as a database service. | Credential Access | [T1552.007](https://attack.mitre.org/techniques/T1552/007/) |
| Mount Service Principal | An attacker may use this technique to get access to a single container in the cluster. This may allow them to gain access to cloud credentials resources. For example, in Azure Kubernetes Service (AKS), each node contains a “service principal” credential stored in /etc/kubernetes/azure.json. Attackers who get access to this service principal file can use its credentials to access or modify the cloud resources. | Credential Access |  [T1528](https://attack.mitre.org/techniques/T1528/), [T1078.003](https://attack.mitre.org/techniques/T1078/003/) |
| Access Container Service Account | In Kubernetes, a service account (SA) is an application identity. By default, an SA is mounted to every pod in a Kubernetes cluster. It allows containers to send requests to the Kubernetes API server. An attacker who gains access to a pod can obtain the SA token and perform unauthorized actions in the cluster. If RBAC is enabled, SA privileges are determined by the role bindings associated with it. But if RBAC is not enabled, the SA token will grant the attacker unlimited permissions and full access to the cluster. | Credential Access |  [T1528](https://attack.mitre.org/techniques/T1528/) |
| Application Credentials in Configuration Files | Secrets are stored in the Kubernetes configuration files, for example, as environment variables in the pod configuration. If an attacker has access to these configurations, either by querying the API server or accessing files on the developer’s endpoint, they would be able to steal these secrets and use them to access cluster resources. | Credential Access | [T1552.001](https://attack.mitre.org/techniques/T1552/001/) |
| Access Managed Identity Credential | "Managed identities” and their secrets are managed by cloud providers, allocated to cloud resources such as virtual machines, and used to authenticate with cloud services. Applications can access the Instance Metadata Service (IMDS) to obtain the identity’s token. Attackers who get access to a Kubernetes pod can take advantage of their access to the IMDS endpoint to get the managed identity’s token, and then access cloud resources. | Credential Access | [T1552.005](https://attack.mitre.org/techniques/T1552/005/) |
| Malicious Adminssion Controller | Attackers may leverage generic, malicious admission controllers built into Kubernetes, such as ValidatingAdmissionWebhook to access credentials. This controller’s behavior is determined by an admission webhook deployed in the cluster. Attackers can use this webhook to intercept requests to the API server, or to record secrets and other sensitive information. | Credential Access | [T1056](https://attack.mitre.org/techniques/T1056/) | 
| Access the K8S API Server |The Kubernetes API server is a critical component that serves as the front end or gateway of the Kubernetes control plane. It exposes the Kubernetes RESTful API that allows various actions to be performed in the cluster. The API server also retrieves the status of the cluster, including all components deployed on it. An attacker who gains access to this API server can send API requests to probe the cluster and retrieve information and secrets about its resources. | Discovery | [T1613](https://attack.mitre.org/techniques/T1613/) |
| Access Kubelet API |The Kubelet is an agent installed on every Kubernetes node that ensures that pods assigned to the node execute properly. It exposes a read-only API service that does not require authentication on TCP port 10255. An attacker with network access to the host can query the Kubelet API with API requests to retrieve the running pods on the host and information about the host, such as CPU and memory consumption. | Discovery | [T1046](https://attack.mitre.org/techniques/T1046/) |
| Network Mapping | By default, Kubernetes does not restrict network traffic (communication) between pods. An attacker can take advantage of this fact. If they gain access to a single container, they may use it to probe the cluster network, map it, and discover information about running pods/applications, including scanning for known vulnerabilities. | Discovery | [T1046](https://attack.mitre.org/techniques/T1046/) |
| Access Kubernetes Dashboard | The Kubernetes Dashboard is used to monitor and manage the Kubernetes cluster. Users can perform actions in the cluster with the permissions that are determined by the binding or cluster-binding for its service account. However, an attacker with access to a single container in the cluster can subsequently access the Kubernetes Dashboard, and use its identity to retrieve information about the cluster resources. | Discovery | [T1538](https://attack.mitre.org/techniques/T1538/) |
| Instance Metadata API | Cloud providers provide a metadata service to retrieve information about a virtual machine (VM), including its network configuration, underlying hosts, disks, SSH public keys, and sensitive credentials. VMs can access this service via a non-routable IP address from within the VM only. If an attacker is able to access this metadata, they may be able to leverage it to access or compromise container or cloud resources. | Discovery | [T1552.005](https://attack.mitre.org/techniques/T1552/005/) |
| Access Cloud Resources | In this technique, attackers may gain access to a single container to compromise it, and then move from this compromised container to the cloud environment and its resources. | Lateral Movement | [T1550](https://attack.mitre.org/techniques/T1550/) |
| Container Service Account | By default, a service account (SA) in Kubernetes is mounted to every pod in a cluster. It allows containers to send requests to the Kubernetes API server. An attacker who gains access to a pod can obtain the SA token to send requests to the API server, and gain access to additional resources in the cluster. If RBAC is enabled, SA privileges are determined by the role bindings associated with it. But if RBAC is not enabled, the SA token will grant the attacker full access to the cluster. | Lateral Movement | [T1078.003](https://attack.mitre.org/techniques/T1078/003/) | 
| Cluster Internal Networking | By default, Kubernetes does not restrict network traffic (communication) between pods in the cluster. If an attacker gains access to a single container, they may use the compromised container to expand their network reachability, and gain access to other containers, pods, and applications in the cluster. | Lateral Movement | [T1599](https://attack.mitre.org/techniques/T1599/) |
| Application Credentials in Configuration Files |Secrets are stored in the Kubernetes configuration files, for example, as environment variables in the pod configuration. If an attacker has access to these configurations, they may be able to steal these secrets, and use them to access resources inside and outside the cluster. | Lateral Movement | [T1552.001](https://attack.mitre.org/techniques/T1552/001/) |
| Writeable Volume Mounts on the Host | In this technique, the hostPath volume mounts a file or directory to the container. Attackers may try to gain access to the underlying host from a compromised container and persist on the container host. | Lateral Movement | [T1611](https://attack.mitre.org/techniques/T1611/) |
| CoreDNS Poisoning | CoreDNS is the main Domain Name Server (DNS) service used in Kubernetes. Its configuration can be modified by a file named corefile which is stored in a ConfigMap object located at the kube-system namespace. An attacker with permission to modify the ConfigMap (say, by using the container’s service account) can change the behavior of the cluster’s DNS. It can then poison it, and take the network identity of other services. | Lateral Movement | [T1071.004](https://attack.mitre.org/techniques/T1071/004/) |
| ARP Poisoning and IP Spoofing | Kubernetes has multiple Container Network Interfaces (CNIs) – network plugins that can be used in the cluster. Often, the default network plugin is Kubenet. In this configuration, a bridge is created on each node (cbr0). Various pods are connected to nodes using veth pairs. The bridge is a level-2 component that carries cross-pod traffic, which makes ARP poisoning in the cluster possible. An attacker with access to a pod in the cluster can perform ARP poisoning, and spoof the traffic of other pods. They can perform several attacks at the network level, leading to lateral movements, such as DNS spoofing or stealing cloud identities of other pods. | Lateral Movement | [T1557.002](https://attack.mitre.org/techniques/T1557/002/) |
| Images from a Private Registry | Images running in the cluster can be stored in a private registry. To pull these images, the container runtime engine must have valid credentials to those registries. If the registry is hosted by the cloud provider, it is authenticated with cloud credentials. But if an attacker gets access to the cluster, they may be able to gain access to the private registry and pull its images. One way is to use the managed identity token by leveraging the access of a Kubernetes pod to the IMDS endpoint. | Collection | [T1213](https://attack.mitre.org/techniques/T1213/) |
| Data Destruction | An attacker may try to delete or remove resources in a Kubernetes cluster. These resources may include deployments, configurations, nodes, pods, storage, compute, or other data. | Impact | [T1485](https://attack.mitre.org/techniques/T1485/) |
| Resource Hijacking | In this technique, an attacker attempts to hijack and abuse a Kubernetes resource for a purpose that it was not originally intended for. One example is using compromised containers to run malicious tasks, such as digital currency mining (cryptomining), also known as a “cryptojacking” attack. | Impact | [T1496](https://attack.mitre.org/techniques/T1496/) |
| Denial of Service | An attacker may seek to make a service unavailable to legitimate users. They may impact the availability of components within the Kubernetes control plane, such as the API server, cluster nodes, or application components in pods. | Impact | [T1499](https://attack.mitre.org/techniques/T1499/), [T1498](https://attack.mitre.org/techniques/T1498/) |


## Contributions

If you have a request, an idea, or a issue to report, please do contact me. 
- **Modifying Mappings:** Please include a description of alternative MITRE ATT&CK IDs.
- **Additional Toolkits:** Please specify the toolkits that you are mapping or would like to see mapped within the project.
