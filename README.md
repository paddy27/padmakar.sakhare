📘 Nutanix Kubernetes Platform (NKP) – Architecture & Operations Guide
Table of Contents

Overview

What is NKP

High-Level Architecture

Management Cluster

Workload Clusters

Cluster API (CAPI)

Infrastructure Integration (AHV)

Networking Architecture

Storage Architecture

Security Architecture

Lifecycle & Upgrade Management

Observability & Operations

End-to-End Workflow

NKP vs Other Kubernetes Platforms

Reference Architecture

Best Practices

Troubleshooting & Common Issues

Summary

1. Overview

Nutanix Kubernetes Platform (NKP) is an enterprise-grade Kubernetes lifecycle management platform that enables consistent deployment, management, and upgrades of Kubernetes clusters across:

Nutanix AHV (on-prem)

Bare metal

Public cloud (AWS / Azure)

NKP is built on Kubernetes Cluster API (CAPI) and follows a management cluster + workload cluster model.

2. What is NKP?

NKP provides:

Automated Kubernetes cluster creation

Multi-cluster lifecycle management

Native Nutanix AHV integration

Secure, upgrade-safe operations

Day-2 operational simplicity

Think of NKP as “Kubernetes managing Kubernetes”.

3. High-Level Architecture
flowchart TB
    Admin --> NKP_UI[NKP UI / API]
    NKP_UI --> MgmtCluster[Management Cluster]

    MgmtCluster -->|Manages| Workload1[Workload Cluster A]
    MgmtCluster -->|Manages| Workload2[Workload Cluster B]
    MgmtCluster -->|Manages| Workload3[Workload Cluster C]

4. Management Cluster

The management cluster is the control plane for NKP itself.

Responsibilities

Kubernetes cluster lifecycle

OS image & Kubernetes version management

Cluster scaling and deletion

Add-on orchestration

Central authentication & RBAC

Components

NKP Controller

Cluster API controllers

Infrastructure providers

Helm controllers

Usually one management cluster per environment.

5. Workload Clusters

Workload clusters are standard Kubernetes clusters where applications run.

flowchart LR
    CP[Control Plane Nodes] --> API[kube-apiserver]
    CP --> ETCD[etcd]
    Workers[Worker Nodes] --> Pods[Application Pods]

Control Plane Nodes

kube-apiserver

kube-scheduler

kube-controller-manager

etcd

Worker Nodes

kubelet

container runtime

CNI

application pods

6. Cluster API (CAPI)

NKP is built on Cluster API, enabling declarative Kubernetes cluster management.

flowchart LR
    NKP --> CAPI[Cluster API]
    CAPI --> InfraProvider[Nutanix Provider]
    InfraProvider --> Prism[Prism Central]
    Prism --> AHV[AHV Hypervisor]

Benefits

Declarative cluster definitions

Safe rolling upgrades

Predictable scaling

Infrastructure abstraction

7. Infrastructure Integration (AHV)

For Nutanix AHV:

NKP integrates with Prism Central

Nodes are provisioned as VMs

Networking and storage are auto-configured

Node Lifecycle

Create VM

Bootstrap Kubernetes

Join cluster

Managed upgrades & replacement

8. Networking Architecture
Container Networking (CNI)

Supported CNIs:

Calico

Cilium

Provides:

Pod-to-pod networking

Network policies

Service routing

Load Balancing

Options:

MetalLB (on-prem)

Cloud provider load balancers

Used for:

Service type=LoadBalancer

Ingress controllers

flowchart LR
    Pod --> Service
    Service --> LoadBalancer
    LoadBalancer --> ExternalTraffic

9. Storage Architecture

NKP uses CSI (Container Storage Interface) with Nutanix AOS.

flowchart LR
    Pod --> PVC
    PVC --> CSI[Nutanix CSI Driver]
    CSI --> AOS[Nutanix AOS Storage]

Features

Dynamic provisioning

Snapshots

Volume expansion

Stateful workloads

10. Security Architecture
Authentication & Authorization

Kubernetes RBAC

LDAP / AD / SAML integration

Per-cluster access control

Certificates

Automatic certificate generation

Secure communication between:

Management cluster

Workload clusters

Infrastructure APIs

Secrets

Kubernetes Secrets

Optional external secret managers

11. Lifecycle & Upgrade Management

One of NKP’s strongest features.

Managed Operations

Kubernetes version upgrades

OS upgrades

Node replacement

Add-on upgrades

Upgrade Strategy

Rolling upgrades

Zero / minimal downtime

Controlled via UI, CLI, or YAML

flowchart TB
    OldNode --> Drain
    Drain --> Replace
    Replace --> NewNode

12. Observability & Operations

NKP integrates with:

Prometheus

Grafana

Nutanix monitoring tools

Provides:

Cluster health

Node metrics

Pod metrics

Upgrade visibility

13. End-to-End Cluster Creation Workflow
sequenceDiagram
    participant Admin
    participant NKP
    participant CAPI
    participant Prism
    participant AHV

    Admin->>NKP: Create Cluster
    NKP->>CAPI: Apply Cluster YAML
    CAPI->>Prism: Request VMs
    Prism->>AHV: Provision Nodes
    AHV->>NKP: Nodes Ready

14. NKP vs Other Kubernetes Platforms
Feature	Vanilla Kubernetes	OpenShift	NKP
Multi-cluster mgmt	❌	⚠️	✅
AHV integration	❌	❌	✅
Cluster API based	❌	❌	✅
Upgrade safety	Manual	Managed	Managed
Day-2 ops	Complex	Moderate	Simple
15. Reference Architecture (Recommended)

1 × Management Cluster

3 × Control Plane nodes per workload cluster

N × Worker nodes

Dedicated storage classes

Dedicated ingress & LB

16. Best Practices

Separate management and workload clusters

Use odd number of control plane nodes

Enable network policies

Monitor certificate expiry

Use rolling upgrades only

17. Troubleshooting & Common Issues
Issue	Cause	Fix
Nodes not joining	Network / cert	Check CNI & certs
Upgrade stuck	Pod drain failure	Check PDBs
Storage issues	CSI misconfig	Validate storage class
API timeout	LB issue	Check control plane LB
18. Summary

Nutanix NKP is an enterprise Kubernetes lifecycle platform built on Cluster API, tightly integrated with Nutanix AHV, providing secure, scalable, and operationally simple Kubernetes management.
