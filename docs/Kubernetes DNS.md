

# Useful lookups on Kubernetes Pods

```yaml
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        k8s-app: kube-registry
        kubernetes.io/cluster-service: "true"
        kubernetes.io/name: KubeRegistry
      name: images
      namespace: kube-system
    spec:
      clusterIP: 100.67.24.207
      ports:
      - name: registry
        port: 5000
        protocol: TCP
        targetPort: 5000
      - name: registry-ui
        port: 80
        protocol: TCP
        targetPort: 80
      selector:
        k8s-app: kube-registry
      type: ClusterIP

```

## Fully Qualified Lookup



## Intra-namespace Lookup

### SRV

`dig +short +search _<port_name>._<protocol>.<service_name> SRV`

`dig +short +search _registry._tcp.images SRV`

### A (typical forward lookup)

`dig +short +search <service_name>`

`dig +short +search images`


## Inter-namespace Lookup

### SRV

`dig +short +search _<port_name>._<protocol>.<service_name>.<namespace> SRV`

`dig +short +search _registry._tcp.images.kube-system SRV`


### A 

`dig +short +search <service_name>.<namespace>`

`dig +short +search images.kube-system`



---

[SRV RR-specific notes](./SRV Records)

