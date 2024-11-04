# Node-Red Kubernetes Setup

[Node-Red](https://nodered.org/) is a programming tool for wiring together hardware devices, APIs, and online services in creative ways.

## TL;DR

```bash
git clone https://github.com/triplom/node-red.git
kubectl apply -f nodered-setup.yml
```

## Prerequisites

- Kubernetes cluster (tested with v1.18.6)
- Persistent storage class available in the cluster
- `local-storage` storage class configured for persistent storage if using local volume

## Installing the Deployment

To install the deployment, services, and PVC in the `nodered` namespace:

```bash
git clone https://github.com/triplom/node-red.git
kubectl create namespace nodered
kubectl apply -f nodered-setup.yml
```

This command deploys Node-Red with the default configuration. **Any customization requires modifying the deployment file before applying it to the Kubernetes cluster.**

Refer to the [Configuration](#configuration) section to view adjustable parameters.

## Changes Made in This Version

1. **Internal Network Policy**: A NetworkPolicy has been added to allow internal communication between resources in the `nodered` namespace. This setup restricts access to Node-Red services from within the namespace only.
2. **Persistent Volume Claim (PVC)**: A new PVC (`nodered-claim-new`) is configured to store Node-Red data with a `2Gi` storage size. Ensure this matches your `PersistentVolume` settings.
3. **Service Type**: The service type has been changed to `ClusterIP` to restrict access to internal cluster traffic only. Update this to `LoadBalancer` if external access is required.

## Uninstalling Node-Red

To uninstall and delete the `nodered` namespace:

```bash
kubectl delete namespace nodered
```

This command removes all Kubernetes components associated with Node-Red, including the persistent volume claim.

## Configuration

The following parameters can be configured in `nodered-setup.yml`:

| Component                  | Description                                         | Default            |
|----------------------------|-----------------------------------------------------|--------------------|
| `persistence.storageClass` | Storage Class for the Persistent Volume Claim       | `local-storage`    |
| `persistence.size`         | Storage size requested for Node-Red data            | `2Gi`              |
| `service.type`             | Kubernetes service type to access Node-Red UI       | `ClusterIP`        |
| `deployment.replicas`      | Number of replicas                                  | `1`                |

To modify any parameters, edit the `nodered-setup.yml` file with your values before applying it.

## Persistence

The [Node-Red](https://nodered.org/docs/getting-started/docker#managing-user-data) image stores user configuration and palette data at the `/data` path in the container.

This setup mounts a [Persistent Volume](https://kubernetes.io/docs/user-guide/persistent-volumes/) at `/data/nodered` to retain data across pod restarts. The volume is configured with dynamic provisioning using the `local-storage` storage class.

---

This setup is designed to be a starting point for Node-Red deployments in a Kubernetes environment.
Please review and adjust the configuration to suit your specific needs.
