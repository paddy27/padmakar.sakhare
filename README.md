📘 Nutanix NKP Architecture – Explained
Overview

Nutanix Kubernetes Platform (NKP) is Nutanix’s enterprise Kubernetes platform that provides full lifecycle management for Kubernetes clusters across:

On-prem (Nutanix AHV)

Bare metal

Public cloud (AWS / Azure)

NKP follows a management plane + workload plane architecture and is built on Kubernetes Cluster API (CAPI).

High-Level Architecture
+----------------------+
|   Management Cluster |
|----------------------|
| NKP UI / API         |
| Cluster API (CAPI)   |
| Lifecycle Mgmt       |
| Auth / RBAC          |
+----------+-----------+
           |
           | manages
           v
+----------------------+
|   Workload Clusters  |
|----------------------|
| Control Plane Nodes  |
| Worker Nodes         |
| Applications         |
+----------------------+

1. Management Cluster

The management cluster is the control center of NKP.

Responsibilities

Cluster lifecycle (create / scale / upgrade / delete)

Kubernetes version management

OS image management

Add-on management

Central authentication & RBAC

Key Components

NKP Controller

Cluster API (CAPI)

Infrastructure providers

Helm controllers

Typically, one management cluster per environment is sufficient.

2. Workload (User) Clusters

Workload clusters are standard Kubernetes clusters where applications run.

Each workload cluster contains:

Control Plane Nodes

kube-apiserver

kube-controller-manager

kube-scheduler

etcd

Worker Nodes

kubelet

container runtime

CNI plugin

application pods

All workload clusters are fully managed by the management cluster.

Core Technology Stack
Kubernetes Cluster API (CAPI)

NKP is built on Cluster API, which allows Kubernetes to manage Kubernetes declaratively.

NKP → Cluster API → Infrastructure Provider → Platform

Nutanix Infrastructure Provider

Integrates with Prism Central

Automates VM provisioning on AHV

Handles node lifecycle operations

Node Architecture
Control Plane Nodes

Run Kubernetes control services

Highly available (odd number recommended)

Managed upgrades and rolling restarts

Worker Nodes

Run application workloads

Auto-scaled and replaceable

Managed via MachineSets

Networking Architecture
Container Networking (CNI)

Supported CNIs:

Calico

Cilium

Provides:

Pod-to-pod networking

Network policies

Service routing

Load Balancing

Options include:

MetalLB (on-prem)

Cloud provider LBs

Environment-specific integrations

Used for:

Kubernetes Service type=LoadBalancer

Ingress controllers

Storage Architecture

NKP uses CSI (Container Storage Interface) with Nutanix AOS.

Pod → PVC → CSI Driver → Nutanix AOS → Disk

Features

Dynamic volume provisioning

Volume expansion

Snapshots

Stateful workload support

Security Architecture
Authentication & Authorization

Kubernetes RBAC

LDAP / AD / SAML integration

Per-cluster access control

Certificates

Automated certificate generation

Secure communication between:

Management cluster

Workload clusters

Infrastructure APIs

Lifecycle Management (Key Advantage)

NKP simplifies Day-2 operations:

Managed Operations

Kubernetes version upgrades

OS image upgrades

Node replacement

Add-on upgrades

Upgrade Strategy

Rolling upgrades

Minimal downtime

Declarative workflows (YAML / UI / CLI)

Observability & Operations

Integrated monitoring provides:

Cluster health

Node health

Pod metrics

Upgrade progress

Supports:

Prometheus

Grafana

Nutanix monitoring tools

End-to-End Cluster Creation Flow
Admin
  → NKP UI / CLI
    → Cluster API
      → Nutanix Provider
        → Prism Central
          → AHV creates VMs
            → Kubernetes bootstrap
              → Cluster Ready

NKP vs Vanilla Kubernetes
Feature	Vanilla Kubernetes	NKP
Cluster creation	Manual	Automated
Multi-cluster mgmt	Complex	Built-in
AHV integration	None	Native
Upgrades	Risk-prone	Controlled
Enterprise support	Limited	Full
When to Use NKP

NKP is ideal when:

You already use Nutanix infrastructure

You want on-prem Kubernetes with cloud-like operations

You manage multiple Kubernetes clusters

You want simplified Day-2 operations

Summary

Nutanix NKP is an enterprise Kubernetes lifecycle management platform built on Cluster API, deeply integrated with Nutanix AHV, enabling secure, scalable, and operationally simple Kubernetes deployments.
