## 环境准备

由于 Istio 原生运行于 Kubernetes 环境，因此我们需要预先**准备好一个 Kubernetes 集群**。

这里，为了简单，可以使用 kubeadm 完成集群环境的搭建，具体步骤如下：

1. 准备至少 2 台装有 CentOS 7 操作系统的虚拟机或物理机；
2. [安装 Docker](https://docs.docker.com/install/linux/docker-ce/centos/)

   ```sh
   $ docker version
   Client:
    Version:           18.09.6
    API version:       1.39
    Go version:        go1.10.8
    Git commit:        481bc77156
    Built:             Sat May  4 02:34:58 2019
    OS/Arch:           linux/amd64
    Experimental:      false

    Server: Docker Engine - Community
    Engine:
    Version:          18.09.6
    API version:      1.39 (minimum version 1.12)
    Go version:       go1.10.8
    Git commit:       481bc77
    Built:            Sat May  4 02:02:43 2019
    OS/Arch:          linux/amd64
    Experimental:     false

   ```

3. [安装 kubeadm](https://kubernetes.io/docs/setup/independent/install-kubeadm/)

   ```sh
   $ kubeadm version
   kubeadm version: &version.Info{Major:"1", Minor:"14", GitVersion:"v1.14.2", GitCommit:"66049e3b21efe110454d67df4fa62b08ea79a19b", GitTreeState:"clean", BuildDate:"2019-05-16T16:20:34Z", GoVersion:"go1.12.5", Compiler:"gc", Platform:"linux/amd64"}
   ```

4. 选择一台主机当做集群主节点，执行初始化：
    ```sh
    $ kubeadm init --pod-network-cidr=192.200.0.0/16
    I0528 23:43:07.283286    5631 version.go:96] could not fetch a Kubernetes version from the internet: unable to get URL "https://dl.k8s.io/release/stable-1.txt": Get https://dl.k8s.io/release/stable-1.txt: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
    I0528 23:43:07.283352    5631 version.go:97] falling back to the local client version: v1.14.2
    [init] Using Kubernetes version: v1.14.2
    [preflight] Running pre-flight checks
        [WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
    [preflight] Pulling images required for setting up a Kubernetes cluster
    [preflight] This might take a minute or two, depending on the speed of your internet connection
    [preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
    [kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
    [kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
    [kubelet-start] Activating the kubelet service
    [certs] Using certificateDir folder "/etc/kubernetes/pki"
    [certs] Generating "front-proxy-ca" certificate and key
    [certs] Generating "front-proxy-client" certificate and key
    [certs] Generating "etcd/ca" certificate and key
    [certs] Generating "etcd/server" certificate and key
    [certs] etcd/server serving cert is signed for DNS names [node-mjfk-bocloud localhost] and IPs [192.168.137.58 127.0.0.1 ::1]
    [certs] Generating "apiserver-etcd-client" certificate and key
    [certs] Generating "etcd/peer" certificate and key
    [certs] etcd/peer serving cert is signed for DNS names [node-mjfk-bocloud localhost] and IPs [192.168.137.58 127.0.0.1 ::1]
    [certs] Generating "etcd/healthcheck-client" certificate and key
    [certs] Generating "ca" certificate and key
    [certs] Generating "apiserver" certificate and key
    [certs] apiserver serving cert is signed for DNS names [node-mjfk-bocloud kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 192.168.137.58]
    [certs] Generating "apiserver-kubelet-client" certificate and key
    [certs] Generating "sa" key and public key
    [kubeconfig] Using kubeconfig folder "/etc/kubernetes"
    [kubeconfig] Writing "admin.conf" kubeconfig file
    [kubeconfig] Writing "kubelet.conf" kubeconfig file
    [kubeconfig] Writing "controller-manager.conf" kubeconfig file
    [kubeconfig] Writing "scheduler.conf" kubeconfig file
    [control-plane] Using manifest folder "/etc/kubernetes/manifests"
    [control-plane] Creating static Pod manifest for "kube-apiserver"
    [control-plane] Creating static Pod manifest for "kube-controller-manager"
    [control-plane] Creating static Pod manifest for "kube-scheduler"
    [etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
    [wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
    [apiclient] All control plane components are healthy after 18.502249 seconds
    [upload-config] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
    [kubelet] Creating a ConfigMap "kubelet-config-1.14" in namespace kube-system with the configuration for the kubelets in the cluster
    [upload-certs] Skipping phase. Please see --experimental-upload-certs
    [mark-control-plane] Marking the node node-mjfk-bocloud as control-plane by adding the label "node-role.kubernetes.io/master=''"
    [mark-control-plane] Marking the node node-mjfk-bocloud as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]
    [bootstrap-token] Using token: vf0kxr.mg9hiv4ew6n47o6d
    [bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
    [bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
    [bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
    [bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
    [bootstrap-token] creating the "cluster-info" ConfigMap in the "kube-public" namespace
    [addons] Applied essential addon: CoreDNS
    [addons] Applied essential addon: kube-proxy

    Your Kubernetes control-plane has initialized successfully!

    To start using your cluster, you need to run the following as a regular user:

    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config

    You should now deploy a pod network to the cluster.
    Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
    https://kubernetes.io/docs/concepts/cluster-administration/addons/

    Then you can join any number of worker nodes by running the following on each as root:

    kubeadm join 192.168.137.58:6443 --token vf0kxr.mg9hiv4ew6n47o6d \
        --discovery-token-ca-cert-hash sha256:f7800768775c18e9384a6ed48f1d58dbf82388754379175aa83ec88653c10f37
    ```

    配置 kubectl

    ```sh
    $ mkdir -p $HOME/.kube
    $ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    $ sudo chown $(id -u):$(id -g) $HOME/.kube/config
    ```

5. 在其他从节点执行 join 操作，加入集群
    ```sh
    $ kubeadm join 192.168.137.58:6443 --token vf0kxr.mg9hiv4ew6n47o6d     --discovery-token-ca-cert-hash sha256:f7800768775c18e9384a6ed48f1d58dbf82388754379175aa83ec88653c10f37
    [preflight] Running pre-flight checks
        [WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
    [preflight] Reading configuration from the cluster...
    [preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
    [kubelet-start] Downloading configuration for the kubelet from the "kubelet-config-1.14" ConfigMap in the kube-system namespace
    [kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
    [kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
    [kubelet-start] Activating the kubelet service
    [kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

    This node has joined the cluster:
    * Certificate signing request was sent to apiserver and a response was received.
    * The Kubelet was informed of the new secure connection details.

    Run 'kubectl get nodes' on the control-plane to see this node join the cluster.

    ```

6. 安装 Calico 网络
    准备 `calico.yaml` 文件，内容如下：
    ```yaml
    ---
    # Source: calico/templates/calico-config.yaml
    # This ConfigMap is used to configure a self-hosted Calico installation.
    kind: ConfigMap
    apiVersion: v1
    metadata:
    name: calico-config
    namespace: kube-system
    data:
    # Typha is disabled.
    typha_service_name: "none"
    # Configure the backend to use.
    calico_backend: "bird"

    # Configure the MTU to use
    veth_mtu: "1440"

    # The CNI network configuration to install on each node.  The special
    # values in this config will be automatically populated.
    cni_network_config: |-
        {
        "name": "k8s-pod-network",
        "cniVersion": "0.3.0",
        "plugins": [
            {
            "type": "calico",
            "log_level": "info",
            "datastore_type": "kubernetes",
            "nodename": "__KUBERNETES_NODE_NAME__",
            "mtu": __CNI_MTU__,
            "ipam": {
                "type": "calico-ipam"
            },
            "policy": {
                "type": "k8s"
            },
            "kubernetes": {
                "kubeconfig": "__KUBECONFIG_FILEPATH__"
            }
            },
            {
            "type": "portmap",
            "snat": true,
            "capabilities": {"portMappings": true}
            }
        ]
        }

    ---
    # Source: calico/templates/kdd-crds.yaml
    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: felixconfigurations.crd.projectcalico.org
    spec:
    scope: Cluster
    group: crd.projectcalico.org
    version: v1
    names:
        kind: FelixConfiguration
        plural: felixconfigurations
        singular: felixconfiguration
    ---

    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: ipamblocks.crd.projectcalico.org
    spec:
    scope: Cluster
    group: crd.projectcalico.org
    version: v1
    names:
        kind: IPAMBlock
        plural: ipamblocks
        singular: ipamblock

    ---

    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: blockaffinities.crd.projectcalico.org
    spec:
    scope: Cluster
    group: crd.projectcalico.org
    version: v1
    names:
        kind: BlockAffinity
        plural: blockaffinities
        singular: blockaffinity

    ---

    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: ipamhandles.crd.projectcalico.org
    spec:
    scope: Cluster
    group: crd.projectcalico.org
    version: v1
    names:
        kind: IPAMHandle
        plural: ipamhandles
        singular: ipamhandle

    ---

    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: ipamconfigs.crd.projectcalico.org
    spec:
    scope: Cluster
    group: crd.projectcalico.org
    version: v1
    names:
        kind: IPAMConfig
        plural: ipamconfigs
        singular: ipamconfig

    ---

    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: bgppeers.crd.projectcalico.org
    spec:
    scope: Cluster
    group: crd.projectcalico.org
    version: v1
    names:
        kind: BGPPeer
        plural: bgppeers
        singular: bgppeer

    ---

    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: bgpconfigurations.crd.projectcalico.org
    spec:
    scope: Cluster
    group: crd.projectcalico.org
    version: v1
    names:
        kind: BGPConfiguration
        plural: bgpconfigurations
        singular: bgpconfiguration

    ---

    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: ippools.crd.projectcalico.org
    spec:
    scope: Cluster
    group: crd.projectcalico.org
    version: v1
    names:
        kind: IPPool
        plural: ippools
        singular: ippool

    ---

    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: hostendpoints.crd.projectcalico.org
    spec:
    scope: Cluster
    group: crd.projectcalico.org
    version: v1
    names:
        kind: HostEndpoint
        plural: hostendpoints
        singular: hostendpoint

    ---

    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: clusterinformations.crd.projectcalico.org
    spec:
    scope: Cluster
    group: crd.projectcalico.org
    version: v1
    names:
        kind: ClusterInformation
        plural: clusterinformations
        singular: clusterinformation

    ---

    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: globalnetworkpolicies.crd.projectcalico.org
    spec:
    scope: Cluster
    group: crd.projectcalico.org
    version: v1
    names:
        kind: GlobalNetworkPolicy
        plural: globalnetworkpolicies
        singular: globalnetworkpolicy

    ---

    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: globalnetworksets.crd.projectcalico.org
    spec:
    scope: Cluster
    group: crd.projectcalico.org
    version: v1
    names:
        kind: GlobalNetworkSet
        plural: globalnetworksets
        singular: globalnetworkset

    ---

    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: networkpolicies.crd.projectcalico.org
    spec:
    scope: Namespaced
    group: crd.projectcalico.org
    version: v1
    names:
        kind: NetworkPolicy
        plural: networkpolicies
        singular: networkpolicy

    ---

    apiVersion: apiextensions.k8s.io/v1beta1
    kind: CustomResourceDefinition
    metadata:
    name: networksets.crd.projectcalico.org
    spec:
    scope: Namespaced
    group: crd.projectcalico.org
    version: v1
    names:
        kind: NetworkSet
        plural: networksets
        singular: networkset
    ---
    # Source: calico/templates/rbac.yaml

    # Include a clusterrole for the kube-controllers component,
    # and bind it to the calico-kube-controllers serviceaccount.
    kind: ClusterRole
    apiVersion: rbac.authorization.k8s.io/v1beta1
    metadata:
    name: calico-kube-controllers
    rules:
    # Nodes are watched to monitor for deletions.
    - apiGroups: [""]
        resources:
        - nodes
        verbs:
        - watch
        - list
        - get
    # Pods are queried to check for existence.
    - apiGroups: [""]
        resources:
        - pods
        verbs:
        - get
    # IPAM resources are manipulated when nodes are deleted.
    - apiGroups: ["crd.projectcalico.org"]
        resources:
        - ippools
        verbs:
        - list
    - apiGroups: ["crd.projectcalico.org"]
        resources:
        - blockaffinities
        - ipamblocks
        - ipamhandles
        verbs:
        - get
        - list
        - create
        - update
        - delete
    # Needs access to update clusterinformations.
    - apiGroups: ["crd.projectcalico.org"]
        resources:
        - clusterinformations
        verbs:
        - get
        - create
        - update
    ---
    kind: ClusterRoleBinding
    apiVersion: rbac.authorization.k8s.io/v1beta1
    metadata:
    name: calico-kube-controllers
    roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: ClusterRole
    name: calico-kube-controllers
    subjects:
    - kind: ServiceAccount
    name: calico-kube-controllers
    namespace: kube-system
    ---
    # Include a clusterrole for the calico-node DaemonSet,
    # and bind it to the calico-node serviceaccount.
    kind: ClusterRole
    apiVersion: rbac.authorization.k8s.io/v1beta1
    metadata:
    name: calico-node
    rules:
    # The CNI plugin needs to get pods, nodes, and namespaces.
    - apiGroups: [""]
        resources:
        - pods
        - nodes
        - namespaces
        verbs:
        - get
    - apiGroups: [""]
        resources:
        - endpoints
        - services
        verbs:
        # Used to discover service IPs for advertisement.
        - watch
        - list
        # Used to discover Typhas.
        - get
    - apiGroups: [""]
        resources:
        - nodes/status
        verbs:
        # Needed for clearing NodeNetworkUnavailable flag.
        - patch
        # Calico stores some configuration information in node annotations.
        - update
    # Watch for changes to Kubernetes NetworkPolicies.
    - apiGroups: ["networking.k8s.io"]
        resources:
        - networkpolicies
        verbs:
        - watch
        - list
    # Used by Calico for policy information.
    - apiGroups: [""]
        resources:
        - pods
        - namespaces
        - serviceaccounts
        verbs:
        - list
        - watch
    # The CNI plugin patches pods/status.
    - apiGroups: [""]
        resources:
        - pods/status
        verbs:
        - patch
    # Calico monitors various CRDs for config.
    - apiGroups: ["crd.projectcalico.org"]
        resources:
        - globalfelixconfigs
        - felixconfigurations
        - bgppeers
        - globalbgpconfigs
        - bgpconfigurations
        - ippools
        - ipamblocks
        - globalnetworkpolicies
        - globalnetworksets
        - networkpolicies
        - networksets
        - clusterinformations
        - hostendpoints
        verbs:
        - get
        - list
        - watch
    # Calico must create and update some CRDs on startup.
    - apiGroups: ["crd.projectcalico.org"]
        resources:
        - ippools
        - felixconfigurations
        - clusterinformations
        verbs:
        - create
        - update
    # Calico stores some configuration information on the node.
    - apiGroups: [""]
        resources:
        - nodes
        verbs:
        - get
        - list
        - watch
    # These permissions are only requried for upgrade from v2.6, and can
    # be removed after upgrade or on fresh installations.
    - apiGroups: ["crd.projectcalico.org"]
        resources:
        - bgpconfigurations
        - bgppeers
        verbs:
        - create
        - update
    # These permissions are required for Calico CNI to perform IPAM allocations.
    - apiGroups: ["crd.projectcalico.org"]
        resources:
        - blockaffinities
        - ipamblocks
        - ipamhandles
        verbs:
        - get
        - list
        - create
        - update
        - delete
    - apiGroups: ["crd.projectcalico.org"]
        resources:
        - ipamconfigs
        verbs:
        - get
    # Block affinities must also be watchable by confd for route aggregation.
    - apiGroups: ["crd.projectcalico.org"]
        resources:
        - blockaffinities
        verbs:
        - watch
    # The Calico IPAM migration needs to get daemonsets. These permissions can be
    # removed if not upgrading from an installation using host-local IPAM.
    - apiGroups: ["apps"]
        resources:
        - daemonsets
        verbs:
        - get
    ---
    apiVersion: rbac.authorization.k8s.io/v1beta1
    kind: ClusterRoleBinding
    metadata:
    name: calico-node
    roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: ClusterRole
    name: calico-node
    subjects:
    - kind: ServiceAccount
    name: calico-node
    namespace: kube-system

    ---
    # Source: calico/templates/calico-node.yaml
    # This manifest installs the calico-node container, as well
    # as the CNI plugins and network config on
    # each master and worker node in a Kubernetes cluster.
    kind: DaemonSet
    apiVersion: extensions/v1beta1
    metadata:
    name: calico-node
    namespace: kube-system
    labels:
        k8s-app: calico-node
    spec:
    selector:
        matchLabels:
        k8s-app: calico-node
    updateStrategy:
        type: RollingUpdate
        rollingUpdate:
        maxUnavailable: 1
    template:
        metadata:
        labels:
            k8s-app: calico-node
        annotations:
            # This, along with the CriticalAddonsOnly toleration below,
            # marks the pod as a critical add-on, ensuring it gets
            # priority scheduling and that its resources are reserved
            # if it ever gets evicted.
            scheduler.alpha.kubernetes.io/critical-pod: ''
        spec:
        nodeSelector:
            beta.kubernetes.io/os: linux
        hostNetwork: true
        tolerations:
            # Make sure calico-node gets scheduled on all nodes.
            - effect: NoSchedule
            operator: Exists
            # Mark the pod as a critical add-on for rescheduling.
            - key: CriticalAddonsOnly
            operator: Exists
            - effect: NoExecute
            operator: Exists
        serviceAccountName: calico-node
        # Minimize downtime during a rolling upgrade or deletion; tell Kubernetes to do a "force
        # deletion": https://kubernetes.io/docs/concepts/workloads/pods/pod/#termination-of-pods.
        terminationGracePeriodSeconds: 0
        initContainers:
            # This container performs upgrade from host-local IPAM to calico-ipam.
            # It can be deleted if this is a fresh installation, or if you have already
            # upgraded to use calico-ipam.
            - name: upgrade-ipam
            image: calico/cni:v3.7.2
            command: ["/opt/cni/bin/calico-ipam", "-upgrade"]
            env:
                - name: KUBERNETES_NODE_NAME
                valueFrom:
                    fieldRef:
                    fieldPath: spec.nodeName
                - name: CALICO_NETWORKING_BACKEND
                valueFrom:
                    configMapKeyRef:
                    name: calico-config
                    key: calico_backend
            volumeMounts:
                - mountPath: /var/lib/cni/networks
                name: host-local-net-dir
                - mountPath: /host/opt/cni/bin
                name: cni-bin-dir
            # This container installs the CNI binaries
            # and CNI network config file on each node.
            - name: install-cni
            image: calico/cni:v3.7.2
            command: ["/install-cni.sh"]
            env:
                # Name of the CNI config file to create.
                - name: CNI_CONF_NAME
                value: "10-calico.conflist"
                # The CNI network config to install on each node.
                - name: CNI_NETWORK_CONFIG
                valueFrom:
                    configMapKeyRef:
                    name: calico-config
                    key: cni_network_config
                # Set the hostname based on the k8s node name.
                - name: KUBERNETES_NODE_NAME
                valueFrom:
                    fieldRef:
                    fieldPath: spec.nodeName
                # CNI MTU Config variable
                - name: CNI_MTU
                valueFrom:
                    configMapKeyRef:
                    name: calico-config
                    key: veth_mtu
                # Prevents the container from sleeping forever.
                - name: SLEEP
                value: "false"
            volumeMounts:
                - mountPath: /host/opt/cni/bin
                name: cni-bin-dir
                - mountPath: /host/etc/cni/net.d
                name: cni-net-dir
        containers:
            # Runs calico-node container on each Kubernetes node.  This
            # container programs network policy and routes on each
            # host.
            - name: calico-node
            image: calico/node:v3.7.2
            env:
                # Use Kubernetes API as the backing datastore.
                - name: DATASTORE_TYPE
                value: "kubernetes"
                # Wait for the datastore.
                - name: WAIT_FOR_DATASTORE
                value: "true"
                # Set based on the k8s node name.
                - name: NODENAME
                valueFrom:
                    fieldRef:
                    fieldPath: spec.nodeName
                # Choose the backend to use.
                - name: CALICO_NETWORKING_BACKEND
                valueFrom:
                    configMapKeyRef:
                    name: calico-config
                    key: calico_backend
                # Cluster type to identify the deployment type
                - name: CLUSTER_TYPE
                value: "k8s,bgp"
                # Auto-detect the BGP IP address.
                - name: IP
                value: "autodetect"
                # Enable IPIP
                - name: CALICO_IPV4POOL_IPIP
                value: "Always"
                # Set MTU for tunnel device used if ipip is enabled
                - name: FELIX_IPINIPMTU
                valueFrom:
                    configMapKeyRef:
                    name: calico-config
                    key: veth_mtu
                # The default IPv4 pool to create on startup if none exists. Pod IPs will be
                # chosen from this range. Changing this value after installation will have
                # no effect. This should fall within `--cluster-cidr`.
                - name: CALICO_IPV4POOL_CIDR
                value: "192.200.0.0/16"
                # Disable file logging so `kubectl logs` works.
                - name: CALICO_DISABLE_FILE_LOGGING
                value: "true"
                # Set Felix endpoint to host default action to ACCEPT.
                - name: FELIX_DEFAULTENDPOINTTOHOSTACTION
                value: "ACCEPT"
                # Disable IPv6 on Kubernetes.
                - name: FELIX_IPV6SUPPORT
                value: "false"
                # Set Felix logging to "info"
                - name: FELIX_LOGSEVERITYSCREEN
                value: "info"
                - name: FELIX_HEALTHENABLED
                value: "true"
            securityContext:
                privileged: true
            resources:
                requests:
                cpu: 250m
            livenessProbe:
                httpGet:
                path: /liveness
                port: 9099
                host: localhost
                periodSeconds: 10
                initialDelaySeconds: 10
                failureThreshold: 6
            readinessProbe:
                exec:
                command:
                - /bin/calico-node
                - -bird-ready
                - -felix-ready
                periodSeconds: 10
            volumeMounts:
                - mountPath: /lib/modules
                name: lib-modules
                readOnly: true
                - mountPath: /run/xtables.lock
                name: xtables-lock
                readOnly: false
                - mountPath: /var/run/calico
                name: var-run-calico
                readOnly: false
                - mountPath: /var/lib/calico
                name: var-lib-calico
                readOnly: false
        volumes:
            # Used by calico-node.
            - name: lib-modules
            hostPath:
                path: /lib/modules
            - name: var-run-calico
            hostPath:
                path: /var/run/calico
            - name: var-lib-calico
            hostPath:
                path: /var/lib/calico
            - name: xtables-lock
            hostPath:
                path: /run/xtables.lock
                type: FileOrCreate
            # Used to install CNI.
            - name: cni-bin-dir
            hostPath:
                path: /opt/cni/bin
            - name: cni-net-dir
            hostPath:
                path: /etc/cni/net.d
            # Mount in the directory for host-local IPAM allocations. This is
            # used when upgrading from host-local to calico-ipam, and can be removed
            # if not using the upgrade-ipam init container.
            - name: host-local-net-dir
            hostPath:
                path: /var/lib/cni/networks
    ---

    apiVersion: v1
    kind: ServiceAccount
    metadata:
    name: calico-node
    namespace: kube-system

    ---
    # Source: calico/templates/calico-kube-controllers.yaml
    # See https://github.com/projectcalico/kube-controllers
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
    name: calico-kube-controllers
    namespace: kube-system
    labels:
        k8s-app: calico-kube-controllers
    annotations:
        scheduler.alpha.kubernetes.io/critical-pod: ''
    spec:
    # The controller can only have a single active instance.
    replicas: 1
    strategy:
        type: Recreate
    template:
        metadata:
        name: calico-kube-controllers
        namespace: kube-system
        labels:
            k8s-app: calico-kube-controllers
        spec:
        nodeSelector:
            beta.kubernetes.io/os: linux
        tolerations:
            # Mark the pod as a critical add-on for rescheduling.
            - key: CriticalAddonsOnly
            operator: Exists
            - key: node-role.kubernetes.io/master
            effect: NoSchedule
        serviceAccountName: calico-kube-controllers
        containers:
            - name: calico-kube-controllers
            image: calico/kube-controllers:v3.7.2
            env:
                # Choose which controllers to run.
                - name: ENABLED_CONTROLLERS
                value: node
                - name: DATASTORE_TYPE
                value: kubernetes
            readinessProbe:
                exec:
                command:
                - /usr/bin/check-status
                - -r

    ---

    apiVersion: v1
    kind: ServiceAccount
    metadata:
    name: calico-kube-controllers
    namespace: kube-system
    ---
    # Source: calico/templates/calico-etcd-secrets.yaml

    ---
    # Source: calico/templates/calico-typha.yaml

    ---
    # Source: calico/templates/configure-canal.yaml

    ```

    使用 kubectl 部署网络
    ```sh
    $ kubectl apply -f calico.yaml
    configmap/calico-config created
    customresourcedefinition.apiextensions.k8s.io/felixconfigurations.crd.projectcalico.org created
    customresourcedefinition.apiextensions.k8s.io/ipamblocks.crd.projectcalico.org created
    customresourcedefinition.apiextensions.k8s.io/blockaffinities.crd.projectcalico.org created
    customresourcedefinition.apiextensions.k8s.io/ipamhandles.crd.projectcalico.org created
    customresourcedefinition.apiextensions.k8s.io/ipamconfigs.crd.projectcalico.org created
    customresourcedefinition.apiextensions.k8s.io/bgppeers.crd.projectcalico.org created
    customresourcedefinition.apiextensions.k8s.io/bgpconfigurations.crd.projectcalico.org created
    customresourcedefinition.apiextensions.k8s.io/ippools.crd.projectcalico.org created
    customresourcedefinition.apiextensions.k8s.io/hostendpoints.crd.projectcalico.org created
    customresourcedefinition.apiextensions.k8s.io/clusterinformations.crd.projectcalico.org created
    customresourcedefinition.apiextensions.k8s.io/globalnetworkpolicies.crd.projectcalico.org created
    customresourcedefinition.apiextensions.k8s.io/globalnetworksets.crd.projectcalico.org created
    customresourcedefinition.apiextensions.k8s.io/networkpolicies.crd.projectcalico.org created
    customresourcedefinition.apiextensions.k8s.io/networksets.crd.projectcalico.org created
    clusterrole.rbac.authorization.k8s.io/calico-kube-controllers created
    clusterrolebinding.rbac.authorization.k8s.io/calico-kube-controllers created
    clusterrole.rbac.authorization.k8s.io/calico-node created
    clusterrolebinding.rbac.authorization.k8s.io/calico-node created
    daemonset.extensions/calico-node created
    serviceaccount/calico-node created
    deployment.extensions/calico-kube-controllers created
    serviceaccount/calico-kube-controllers created
    ```

7. 检查集群状态

    ```sh
    $ kubectl get node
    NAME                STATUS   ROLES    AGE   VERSION
    node-algt-bocloud   Ready    <none>   17s   v1.14.2
    node-mjfk-bocloud   Ready    master   18m   v1.14.2
    node-ttnm-bocloud   Ready    <none>   13s   v1.14.2
    node-vtdo-bocloud   Ready    <none>   15m   v1.14.2
    ```

    所有 node 都变为 `Ready` 状态，就证明我们的集群可以使用了。
