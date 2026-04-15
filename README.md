# OpenShift CLI (`oc`) Command Reference

> Complete guide covering 200+ `oc` commands. All commands use the `oc` prefix. Most `kubectl` commands also work.

---

## Table of Contents

1. [Login & Config](#1-login--config)
2. [Projects](#2-projects)
3. [Apps & Deployments](#3-apps--deployments)
4. [Pods](#4-pods)
5. [Services & Networking](#5-services--networking)
6. [Builds & Images](#6-builds--images)
7. [ConfigMaps & Secrets](#7-configmaps--secrets)
8. [Storage & PVCs](#8-storage--pvcs)
9. [RBAC & Security](#9-rbac--security)
10. [Resource Management](#10-resource-management)
11. [Templates & Operators](#11-templates--operators)
12. [Nodes & Cluster Admin](#12-nodes--cluster-admin)
13. [Debugging & Monitoring](#13-debugging--monitoring)
14. [Output & Formatting](#14-output--formatting)

---

## 1. Login & Config

| Command | Description |
|---------|-------------|
| `oc login https://api.cluster.example.com:6443` | Log in to an OpenShift cluster using the API server URL. Prompts for username and password. |
| `oc login -u admin -p password` | Log in directly with username and password on the command line. |
| `oc login --token=<token>` | Authenticate with a bearer token — common for service accounts and CI pipelines. |
| `oc logout` | Log out and clear the current cluster session credentials. |
| `oc whoami` | Display the currently logged-in username. |
| `oc whoami --show-server` | Show the API server URL of the cluster you are connected to. |
| `oc whoami --show-token` | Print the current authentication token (treat as a password). |
| `oc config view` | Display the full kubeconfig — clusters, users, and contexts. |
| `oc config get-contexts` | List all contexts defined in the kubeconfig file. |
| `oc config use-context <name>` | Switch to a different cluster or user context. |
| `oc config current-context` | Print the name of the currently active context. |

---

## 2. Projects

| Command | Description |
|---------|-------------|
| `oc new-project <name>` | Create a new project (OpenShift namespace) in the cluster. |
| `oc new-project <name> --description='...' --display-name='...'` | Create a project with a human-readable display name and description. |
| `oc project <name>` | Switch the active context to an existing project. |
| `oc project` | Show the name of the currently active project. |
| `oc projects` | List all projects you have access to. |
| `oc get projects` | List all projects in table format with their status. |
| `oc delete project <name>` | Permanently delete a project and everything inside it. |
| `oc describe project <name>` | Show project details including resource quotas and node selectors. |

---

## 3. Apps & Deployments

| Command | Description |
|---------|-------------|
| `oc new-app <image>` | Create an application from a container image in a registry. |
| `oc new-app <git-repo-url>` | Build and deploy from a Git repo using Source-to-Image (S2I). |
| `oc new-app --template=<name>` | Deploy an app using a predefined OpenShift template. |
| `oc new-app -f <file.yaml>` | Create an application from a local YAML or JSON definition file. |
| `oc new-app --image-stream=<name>` | Deploy from an existing ImageStream in the cluster. |
| `oc get deployments` | List all Kubernetes Deployment objects in the current project. |
| `oc get deploymentconfigs` | List all OpenShift DeploymentConfigs in the current project. |
| `oc get dc` | Shorthand alias for listing DeploymentConfigs. |
| `oc describe dc <name>` | Show full DeploymentConfig info: triggers, strategy, containers, and history. |
| `oc rollout status dc/<name>` | Watch and display the rollout progress of a DeploymentConfig. |
| `oc rollout history dc/<name>` | Show rollout revision history for a DeploymentConfig. |
| `oc rollout undo dc/<name>` | Roll back a DeploymentConfig to the previous revision. |
| `oc rollout undo dc/<name> --to-revision=2` | Roll back to a specific numbered revision. |
| `oc rollout pause dc/<name>` | Pause a rollout to safely batch multiple changes before redeploying. |
| `oc rollout resume dc/<name>` | Resume a previously paused rollout. |
| `oc scale dc/<name> --replicas=3` | Manually scale a DeploymentConfig to a specific replica count. |
| `oc scale deployment/<name> --replicas=3` | Scale a Kubernetes Deployment to the desired number of replicas. |
| `oc autoscale dc/<name> --min=2 --max=5 --cpu-percent=70` | Create a HorizontalPodAutoscaler to auto-scale based on CPU usage. |
| `oc set image dc/<name> <container>=<new-image>` | Update the container image used by a DeploymentConfig. |
| `oc set env dc/<name> KEY=VALUE` | Set or update an environment variable in a DeploymentConfig. |
| `oc set env dc/<name> --list` | List all environment variables set on a DeploymentConfig. |
| `oc set resources dc/<name> --limits=cpu=200m,memory=256Mi` | Set CPU and memory resource limits on DeploymentConfig containers. |
| `oc set probe dc/<name> --liveness --get-url=http://:8080/health` | Configure an HTTP liveness probe on a DeploymentConfig. |

---

## 4. Pods

| Command | Description |
|---------|-------------|
| `oc get pods` | List all pods in the current project. |
| `oc get pods -o wide` | List pods with extra detail: node name, IP, and nominated node. |
| `oc get pods --all-namespaces` | List pods across every project and namespace in the cluster. |
| `oc get pods -w` | Watch pod status in real-time as changes occur. |
| `oc get pods -l app=<label>` | Filter pods by a specific label selector. |
| `oc describe pod <name>` | Show detailed pod info: events, volumes, init containers, and conditions. |
| `oc logs <pod>` | Print logs from the main container of a running pod. |
| `oc logs <pod> -c <container>` | Print logs from a specific container in a multi-container pod. |
| `oc logs <pod> -f` | Stream pod logs in real-time as they are generated. |
| `oc logs <pod> --previous` | Show logs from the previous crashed container instance. |
| `oc logs <pod> --tail=100` | Show only the last N lines of pod logs. |
| `oc exec <pod> -- <command>` | Run a one-off command inside a running pod container. |
| `oc exec -it <pod> -- /bin/bash` | Open an interactive bash shell session inside a running pod. |
| `oc exec -it <pod> -c <container> -- sh` | Open an interactive shell in a specific container of a pod. |
| `oc rsh <pod>` | OpenShift shortcut to remote shell into a pod (auto-detects shell). |
| `oc delete pod <name>` | Delete a pod — it restarts automatically if managed by a controller. |
| `oc delete pod <name> --force --grace-period=0` | Immediately force-delete a stuck or terminating pod. |
| `oc cp <pod>:/path/file ./local` | Copy a file from a pod to your local machine. |
| `oc cp ./local-file <pod>:/remote/path` | Copy a file from your local machine into a running pod. |
| `oc top pods` | Display real-time CPU and memory usage for all pods. |
| `oc top pod <name>` | Show resource consumption for a specific named pod. |

---

## 5. Services & Networking

| Command | Description |
|---------|-------------|
| `oc get services` | List all Services in the current project. |
| `oc get svc` | Shorthand alias for listing Services. |
| `oc describe svc <name>` | Show service details: selector, ports, and backing endpoints. |
| `oc expose svc/<name>` | Create a Route to expose a Service externally via HTTP. |
| `oc expose svc/<name> --hostname=app.example.com` | Expose a service with a custom hostname in the Route. |
| `oc expose svc/<name> --port=8080` | Expose a service on a specific port via a Route. |
| `oc get routes` | List all Routes (external HTTP/HTTPS endpoints) in the project. |
| `oc describe route <name>` | Show route details: hostname, TLS config, and backend service. |
| `oc delete route <name>` | Remove an externally exposed Route from the project. |
| `oc port-forward <pod> 8080:8080` | Forward a local port to a pod port for local development and testing. |
| `oc get endpoints` | List Endpoint objects showing the pod IPs backing each service. |
| `oc get networkpolicies` | List NetworkPolicy objects controlling pod-to-pod traffic. |
| `oc get ingress` | List Kubernetes Ingress resources (used alongside Routes). |

---

## 6. Builds & Images

| Command | Description |
|---------|-------------|
| `oc get builds` | List all build runs in the current project. |
| `oc get buildconfigs` | List all BuildConfig pipeline definitions. |
| `oc get bc` | Shorthand alias for listing BuildConfigs. |
| `oc describe bc <name>` | Show BuildConfig details: source, strategy, triggers, and output image. |
| `oc start-build <bc>` | Manually trigger a new build from a BuildConfig. |
| `oc start-build <bc> --follow` | Trigger a build and stream its output logs to the terminal. |
| `oc start-build <bc> --from-file=<file>` | Start a build using a local file as the source input. |
| `oc start-build <bc> --from-dir=./src` | Start a build using a local directory as the source. |
| `oc cancel-build <build>` | Cancel a build that is currently in progress. |
| `oc logs build/<build>` | View logs from a specific build run. |
| `oc logs -f bc/<name>` | Stream the latest build logs from a BuildConfig. |
| `oc get imagestreams` | List all ImageStream objects (managed image metadata). |
| `oc get is` | Shorthand alias for listing ImageStreams. |
| `oc describe is <name>` | Show ImageStream details: tags, image references, and history. |
| `oc get imagestreamtags` | List all ImageStreamTag objects in the project. |
| `oc import-image <name> --from=<registry/image> --confirm` | Import an external image into an OpenShift ImageStream. |
| `oc tag <src> <dest-stream>:<tag>` | Tag an image from one location into an ImageStream. |

---

## 7. ConfigMaps & Secrets

| Command | Description |
|---------|-------------|
| `oc get configmaps` | List all ConfigMaps in the current project. |
| `oc get cm` | Shorthand alias for listing ConfigMaps. |
| `oc describe cm <name>` | Show ConfigMap contents and metadata. |
| `oc create configmap <name> --from-literal=KEY=VALUE` | Create a ConfigMap from one or more literal key-value pairs. |
| `oc create configmap <name> --from-file=<file>` | Create a ConfigMap from a file (filename becomes the key). |
| `oc create configmap <name> --from-env-file=.env` | Create a ConfigMap from a .env file with KEY=VALUE entries. |
| `oc get secrets` | List all Secrets in the current project (values are masked). |
| `oc describe secret <name>` | Show secret metadata and key names without revealing values. |
| `oc create secret generic <name> --from-literal=KEY=VALUE` | Create an opaque secret from literal key-value pairs. |
| `oc create secret generic <name> --from-file=./creds.json` | Create a secret from the contents of a file. |
| `oc create secret docker-registry <name> --docker-server=... --docker-username=... --docker-password=...` | Create a pull secret for a private container registry. |
| `oc create secret tls <name> --cert=tls.crt --key=tls.key` | Create a TLS secret from a certificate and private key file. |
| `oc get secret <name> -o jsonpath='{.data.KEY}' \| base64 -d` | Decode and print a specific base64-encoded secret value. |
| `oc set volume dc/<name> --add --name=cfg --configmap-name=<cm> --mount-path=/etc/config` | Mount a ConfigMap as a volume at a path inside a pod. |
| `oc set volume dc/<name> --add --name=sec --secret-name=<secret> --mount-path=/etc/secrets` | Mount a Secret as a volume at a path inside a pod. |

---

## 8. Storage & PVCs

| Command | Description |
|---------|-------------|
| `oc get pvc` | List all PersistentVolumeClaims in the current project. |
| `oc get pv` | List all PersistentVolumes in the cluster (cluster-scoped resource). |
| `oc describe pvc <name>` | Show PVC details: status, capacity, access modes, and bound PV. |
| `oc create -f pvc.yaml` | Create a PVC from a YAML definition file. |
| `oc delete pvc <name>` | Delete a PVC; PV behavior depends on the reclaim policy. |
| `oc get storageclass` | List StorageClasses available for dynamic volume provisioning. |
| `oc describe storageclass <name>` | Show StorageClass details: provisioner and configuration parameters. |
| `oc set volume dc/<name> --add --name=data --type=pvc --claim-name=<pvc> --mount-path=/data` | Attach an existing PVC to a DeploymentConfig as a mounted volume. |

---

## 9. RBAC & Security

| Command | Description |
|---------|-------------|
| `oc get roles` | List Roles scoped to the current project. |
| `oc get clusterroles` | List ClusterRoles available across the entire cluster. |
| `oc get rolebindings` | List RoleBindings in the current project. |
| `oc get clusterrolebindings` | List ClusterRoleBindings with cluster-wide permissions. |
| `oc adm policy add-role-to-user <role> <user>` | Grant a project-scoped Role to a specific user. |
| `oc adm policy remove-role-from-user <role> <user>` | Revoke a project-scoped Role from a specific user. |
| `oc adm policy add-cluster-role-to-user <role> <user>` | Grant a ClusterRole to a user across the entire cluster. |
| `oc adm policy add-role-to-group <role> <group>` | Grant a Role to a group of users in the current project. |
| `oc adm policy add-scc-to-user <scc> -z <serviceaccount>` | Assign a SecurityContextConstraints policy to a ServiceAccount. |
| `oc adm policy add-scc-to-group anyuid system:authenticated` | Grant anyuid SCC to all authenticated users — use with extreme caution. |
| `oc get scc` | List all SecurityContextConstraints in the cluster. |
| `oc describe scc <name>` | Show SCC details: capabilities, volume types, SELinux config. |
| `oc get serviceaccounts` | List all ServiceAccounts in the current project. |
| `oc get sa` | Shorthand alias for listing ServiceAccounts. |
| `oc create serviceaccount <name>` | Create a new ServiceAccount in the current project. |
| `oc describe sa <name>` | Show ServiceAccount details including its secrets and tokens. |
| `oc auth can-i <verb> <resource>` | Check whether the current user can perform a specific action on a resource. |

---

## 10. Resource Management

| Command | Description |
|---------|-------------|
| `oc get all` | List all common resources (pods, services, routes, builds) in the project. |
| `oc get all -l app=<name>` | List all resources filtered by an app label. |
| `oc get <resource> -o yaml` | Output a resource definition in YAML format. |
| `oc get <resource> -o json` | Output a resource definition in JSON format. |
| `oc get <resource> -o jsonpath='{.spec.replicas}'` | Extract a specific field value using a JSONPath expression. |
| `oc apply -f <file.yaml>` | Create or update resources from a file (declarative, idempotent). |
| `oc create -f <file.yaml>` | Create resources from a file; fails if the resource already exists. |
| `oc replace -f <file.yaml>` | Replace an existing resource entirely with the file contents. |
| `oc delete -f <file.yaml>` | Delete all resources defined in a YAML or JSON file. |
| `oc delete <resource> <name>` | Delete a specific named resource. |
| `oc delete <resource> --all` | Delete all resources of a given type in the current project. |
| `oc edit <resource> <name>` | Open a live resource in your editor to modify and save immediately. |
| `oc patch <resource> <name> -p '{"spec":{"replicas":3}}'` | Apply a JSON patch to update a specific resource field. |
| `oc label <resource> <name> key=value` | Add or update a label on a resource. |
| `oc label <resource> <name> key-` | Remove a label from a resource (note the trailing dash). |
| `oc annotate <resource> <name> key=value` | Add or update an annotation on a resource. |
| `oc get quota` | List ResourceQuota objects enforcing limits in the project. |
| `oc describe quota` | Show current resource usage versus defined quota limits. |
| `oc get limitrange` | List LimitRange objects defining default and max resource limits per pod. |
| `oc get events --sort-by='.lastTimestamp'` | Show recent cluster events sorted by time — essential for debugging. |

---

## 11. Templates & Operators

| Command | Description |
|---------|-------------|
| `oc get templates` | List all Templates in the current project. |
| `oc get templates -n openshift` | List built-in templates from the shared openshift namespace. |
| `oc describe template <name>` | Show template parameters, defaults, and the objects it will create. |
| `oc process <template> \| oc apply -f -` | Process a template with defaults and apply the output immediately. |
| `oc process <template> -p PARAM=value \| oc apply -f -` | Process a template with overridden parameter values and apply it. |
| `oc get csv` | List ClusterServiceVersions (installed Operators and their versions). |
| `oc get subscriptions` | List Operator subscriptions registered in OperatorHub. |
| `oc get catalogsource` | List Operator catalog sources providing available packages. |
| `oc get installplan` | List pending or approved Operator InstallPlans. |
| `oc get operatorgroups` | List OperatorGroups defining the namespace scope of Operators. |

---

## 12. Nodes & Cluster Admin

| Command | Description |
|---------|-------------|
| `oc get nodes` | List all nodes in the cluster with their Ready status. |
| `oc get nodes -o wide` | List nodes with extra detail: IPs, OS image, kernel, and runtime. |
| `oc describe node <name>` | Show full node info: capacity, resources, taints, and running pods. |
| `oc top nodes` | Display real-time CPU and memory usage for all nodes. |
| `oc adm cordon <node>` | Mark a node as unschedulable so no new pods are placed on it. |
| `oc adm uncordon <node>` | Re-enable scheduling on a previously cordoned node. |
| `oc adm drain <node> --ignore-daemonsets --delete-local-data` | Safely evict all pods from a node before maintenance. |
| `oc adm taint nodes <node> key=value:NoSchedule` | Add a taint to repel pods that lack a matching toleration. |
| `oc adm manage-node <node> --list-pods` | List all pods currently running on a specific node. |
| `oc get clusterversion` | Show the current OpenShift version and any ongoing update status. |
| `oc adm upgrade` | Check which cluster upgrades are available. |
| `oc adm upgrade --to=<version>` | Initiate a cluster upgrade to a specific target version. |
| `oc get clusteroperators` | List all cluster operators and their health — great for cluster checks. |
| `oc get co` | Shorthand alias for listing ClusterOperators. |
| `oc describe co <name>` | Show detailed conditions and messages for a ClusterOperator. |
| `oc get machineconfigpools` | List MachineConfigPools that group nodes sharing a configuration. |
| `oc get machineconfigs` | List all MachineConfig objects applied to cluster nodes. |
| `oc get csr` | List all Certificate Signing Requests in the cluster. |
| `oc adm certificate approve <name>` | Approve a pending CSR for a joining node or new user. |

---

## 13. Debugging & Monitoring

| Command | Description |
|---------|-------------|
| `oc debug pod/<name>` | Launch a debug container from a pod's spec for interactive troubleshooting. |
| `oc debug node/<node>` | Open a privileged debug shell on a cluster node (requires cluster-admin). |
| `oc debug dc/<name>` | Create a debug pod using a DeploymentConfig's container configuration. |
| `oc get events` | List all project events — warnings, scheduling changes, and errors. |
| `oc get events --field-selector type=Warning` | Filter events to show only Warning-type entries. |
| `oc status` | Show a high-level health overview of the current project and resources. |
| `oc version` | Show client, server, and Kubernetes version information. |
| `oc api-resources` | List all API resource types supported by the cluster. |
| `oc api-versions` | List all API group versions the cluster supports. |
| `oc explain <resource>` | Display API documentation for a resource type and its fields. |
| `oc explain pod.spec.containers` | Show documentation for a nested field path in a resource spec. |
| `oc adm diagnostics` | Run built-in diagnostic checks to surface common cluster issues. |
| `oc adm must-gather` | Collect cluster-wide diagnostic data (logs, configs) into a local directory. |
| `oc adm must-gather --image=<custom-image>` | Use a custom must-gather image for Operator-specific diagnostics. |
| `oc adm inspect co/<name>` | Collect detailed diagnostic data from a specific ClusterOperator. |

---

## 14. Output & Formatting

| Command | Description |
|---------|-------------|
| `oc get pods -o yaml` | Output complete pod definitions in YAML format. |
| `oc get pods -o json` | Output complete pod definitions in JSON format. |
| `oc get pods -o name` | Output only resource names — useful for scripting and piping. |
| `oc get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase` | Show a custom table using JSONPath expressions as column values. |
| `oc get pods --sort-by='.metadata.creationTimestamp'` | Sort output by any field using dot-notation. |
| `oc get pods -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}'` | Use a JSONPath template to extract and format fields from a list. |
| `oc get pods --no-headers` | Suppress the column header row from tabular output. |
| `oc get pods -A` | List pods in all namespaces (shorthand for --all-namespaces). |
| `oc get all --show-labels` | Show all resources along with their label key-value pairs. |

---

*Generated from the OpenShift CLI Command Reference — 200+ commands across 14 categories.*
