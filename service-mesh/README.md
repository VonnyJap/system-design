#### Running Service Mesh using istio
- istiod: control plane, do monitoring, tracing, and visualization
- ingressgateway: like NLB
- envoy: sidecar injected by the istiod


#### Setup
- create cluster
- install istio from the [official docs](https://istio.io/latest/docs/setup/getting-started/#download)
- we need istioctl - `istioctl install`
  - istio core installed
  - istiod installed
  - ingressgateway installed
- download https://github.com/GoogleCloudPlatform/microservices-demo
- istio-injection=enabled
- can also install monitoring and it comes with the istio download

#### Why we need the sidecar injection?
- Just for the MTLS?
- because we still know how to route traffic from outside even without the sidecar injection
- promotheus and grafana (visualization)
- jaeger for tracing

#### MTLS conn
- https://istio.io/latest/docs/ops/configuration/traffic-management/tls-configuration/
- ingress gateway is also envoy injected
- if the mode is passthrough then it will be passed over to the service as is
- if mutual then it will ask client cert
- between gateway and services envoy if strict -> then mutual tls needs to happen
- else then use permissive

### Playlist
https://www.youtube.com/playlist?list=PLI4xy7phW54nbfjf7ZMnlEx1O5cHneigh

### With istio sidecar
- all communications will be intercepted by the proxy
- and since istio is a proxy -> we need the ca cert to do mutual TLS
- we also need athenz because sometimes thats what application layer is using
- and since proxy intercepting all communications, it needs to know about this
- we also need the self signed for workload signing between istio components and applications
- The cacerts below will be used by Istio CA (Citadel) to sign workload certificates.
  - It needs to be created before running the Istiod control plane. (istio has a mechanism to upgrade the communication to be TLSed when an application in a mesh needs to talk to another application)
  - Most likely these are needed only for communications between proxies and the istio components or maybe when there is no destination rules and everything is istio_mutual
  - And sounds like the control plane talks to data plane in one way communication
  - Will it be possible for envoy send requests to control plane? and in that case what will it be used as client cert? probably the athenz X509 certs since the cacerts is actually used for signing?
[Mutual vs Istio Mutual](https://support.f5.com/csp/article/K29450727#:~:text=MUTUAL%3A%20Secure%20connections%20to%20the,presenting%20client%20certificates%20for%20authentication.)
Destination rules by default are using ISTIO_MUTUAL mode so TLS certificates used by istio proxies created and managed by Istiod CA authority and provisioned using SDS API.
So with istio mutual - istio CA is in charge of signing the workload - hence no config about the tls clients.
when control plane calls the istio proxy - this proxy is acting like a server and it will present its cert/key (athenz) to the control plane and control plane needs to trust it hence the chaining. 
```
$ kubectl create secret generic cacerts -n istio-system --from-file=us-west-2/ca-cert.pem --from-file=us-west-2/ca-key.pem --from-file=us-west-2/root-cert.pem --from-file=us-west-2/cert-chain.pem --dry-run=client -o yaml > cacerts.yaml
```
- For mtls between proxies, the athenz X509 certs get used (and since mutual TLS then server also needs to present cert/key - it is also using the athenz signed), i.e.
```
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: default
spec:
  host: "httpbin.default.svc.cluster.local"
  trafficPolicy:
    tls:
      mode: MUTUAL
      caCertificates: /etc/certs/root-cert.pem
      clientCertificate: /etc/certs/cert-chain.pem
      privateKey: /etc/certs/key.pem
```
- how does istio-agent working to monitor the certificate request?

#### Istiod
- this is the control plane
- Components
  - Pilot: provides service discovery and pushes config changes to all envoy sidecars
  - Citadel: service to service and end-user auth out of the box
    - provides encryption of traffic between proxies using MTLS
  - Galley: istio configuration validation, ingestion, processing and distribution component.
    - reads yaml config and converts into istio format

#### Pod requirements
- pod should run behind services
- pod should not run as 1337
- should run NET_ADMIN and NET_RAW
- labels app/version

#### Istio Traffic Management
- Routing Rules
- Canary rollouts, A/B testing
- Elegant load balancing
- Components in traffic management
  - Virtual Services: like k8s ingress
  - Destination Rules
  - Gateways
  - Service Entries
  - Sidecars
- When Virtual Services applied to the service mesh, all of the envoy proxies in the mesh will be injected with the routing rules. Virtual Service is actually very lightweight. 

#### Gateway
- Ingress and Egress
- Steps to create:
  - create deployment and services
- Envoy proxy is running as main container here
- Routing rules (Virtual Service and Destination rules) can be applied to the proxies
- Need to create gateway object
- VirtualService with gateways selector only sent to the envoy proxy running ingress gateway

#### Istio Configmap
- Configure traffic out
- We can create ServiceEntry for allowing traffic out

### Whats difference between mutual TLS and oidc?

#### Plugging in existing certificates and key
Suppose we want to have Istio’s CA use an existing signing (CA) certificate ca-cert.pem and key ca-key.pem. Furthermore, the certificate ca-cert.pem is signed by the root certificate root-cert.pem. We would like to use root-cert.pem as the root certificate for Istio workloads.

In the following example, Istio CA’s signing (CA) certificate (ca-cert.pem) is different from the root certificate (root-cert.pem), so the workload cannot validate the workload certificates directly from the root certificate. The workload needs a cert-chain.pem file to specify the chain of trust, which should include the certificates of all the intermediate CAs between the workloads and the root CA. In our example, it contains Istio CA’s signing certificate, so cert-chain.pem is the same as ca-cert.pem. Note that if your ca-cert.pem is the same as root-cert.pem, the cert-chain.pem file should be empty.

#### Kubernetes service discovery
- etcd control plane API server is acting as service registry
  - a service mesh can implement logic to use an API for service discovery. (istio can implement this)
- kube proxy (runs on each node)

#### What about node manager that takes care which pods belong to which nodes?
- Node controller which is part of the control plane

#### Theory of the istio agent
- maybe we need both self-signed and the athenz because of the istio_mutual
- or in case containers are missing the sia eks
- hence containers can still communicate mtls way

#### What is registration authority (RA)?
- A registration authority (RA) is an authority in a network that verifies user requests for a digital certificate and tells the certificate authority (CA) to issue it.
- The main purpose of an RA is to ensure that a user or device is allowed to request a digital certificate from a specific website or application. If the request is allowed, the RA forwards the certificate request to the CA, which completes the digital certificate request process.
- subordinate of CA, authenticate the digital cert requests 

### PKI
- Public Key Infrastructure

### Certificate Authority
- the trusted party or entity that issues a digital security cert
- The issued certificate can be validated against the CA's public key to ensure that the certificate is indeed valid and that the connection to the remote resource is trusted.

### Publishing Services
- ClusterIP: Exposes the Service on a cluster-internal IP. Choosing this value makes the Service only reachable from within the cluster. This is the default that is used if you don't explicitly specify a type for a Service. You can expose the service to the public with an Ingress or the Gateway API.
- NodePort: Exposes the Service on each Node's IP at a static port (the NodePort). To make the node port available, Kubernetes sets up a cluster IP address, the same as if you had requested a Service of type: ClusterIP.
- LoadBalancer: Exposes the Service externally using a cloud provider's load balancer.
- ExternalName: Maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value. No proxying of any kind is set up.

### How does istio-ingressgateway working without being LB? or NodePort?
- maybe this is exposed by the gateway?
- read again the gateway and ingress stuffs and the youtube


