## Stratus Red Team Mappings


The definitions within the table are taken directly from the Stratus Red Team [toolkit](https://stratus-red-team.cloud/attack-techniques/kubernetes/). *Create Client Certificate Credential* was not mapped as the technique does not work on AWS EKS. 

Where possible, mappings from the Stratus Red Team Toolkit to the existing MITRE ATT&CK techniques have been made, while also relating them to the Micrsoft Threat Matrix.


| Stratus Red Team Name | Microsoft Threat Matrix Name | Definition |  Technique(s) | MITRE ATT&CK Mapping|
|:-----------------------------|:-----------------|:-----------------|:-----------------|--------:|
| Dump All Secrets	| List K8s Secrets |Dumps all Secrets from a Kubernetes cluster. This allow an attacker with the right permissions to trivially access all secrets in the cluster.| Credential Access |  [T1557.002](https://attack.mitre.org/techniques/T1557/002/)   | 
| Steal Pod Service Account Token |  Access Container Service Account |Steals a service account token from a running pod, by executing a command in the pod and reading /var/run/secrets/kubernetes.io/serviceaccount/token| Credential Access | [T1550](https://attack.mitre.org/techniques/T1550/) |
| Create Admin ClusterRole	| Cluster-Admin Binding |Creates a Service Account bound to a cluster administrator role.| Persistence, Privilege Escalation | [T1098](https://attack.mitre.org/techniques/T1098/) |
| Create Long-Lived Token | N/A | Creates a token with a large expiration for a service account. An attacker can create such a long-lived token to easily gain persistence on a compromised cluster.| Persistence | [T1098.001](https://attack.mitre.org/techniques/T1098/001/) |
| Container breakout via hostPath volume mount	| Writeable hostPath Mount |Creates a Pod with the entire node root filesystem as a hostPath volume mount| Privilege Escalation | [T1611](https://attack.mitre.org/techniques/T1611/) |
| Privilege escalation through node/proxy permissions | N/A |Uses the node proxy API to proxy a Kubelet request through a worker node. This is a vector of privilege escalation, allowing any principal with the nodes/proxy permission to escalate their privilege to cluster administrator, bypassing at the same time admission control checks and logging of the API server.| Privilege Escalation | [T1548](https://attack.mitre.org/techniques/T1548/) | 
| Run a Privileged Pod	| Privileged Container |Runs a privileged pod. Privileged pods are equivalent to running as root on the worker node, and can be used for privilege escalation.| Privilege Escalation | [T1611](https://attack.mitre.org/techniques/T1611/) |
