---
author: pmpaulino
title: Exploring Kustomize
date: 2025-02-05
description: A Helm User's Perspective
tags:
  - k8s
  - helm
  - kustomize
ShowToc: false
TocOpen: false
---

As a DevOps engineer with a background heavily focused on using Helm to manage Kubernetes applications, I've recently started delving into Kustomize. Kustomize is intriguing due to its tight integration with Kubernetes and its purely declarative approach to configuration management. In this post, I'll share insights on why Kustomize might be a viable tool for certain scenarios, especially for those of us accustomed to Helm.

### Why Consider Kustomize?

**Seamless Kubernetes Integration**: Kustomize is integrated into `kubectl` as of Kubernetes 1.14. This means you can apply Kustomize configurations directly using `kubectl apply -k`, streamlining the deployment process without additional tools.

**Purely Declarative**: Unlike Helm, which relies on templates and scripting within YAML files, Kustomize operates on a purely declarative model. You specify what your application should look like without dictating how to achieve that configuration, which can simplify understanding and maintaining your configurations.

**Overlay System**: This is a standout feature for managing different environments (like dev, staging, and prod) without duplicating the entire configuration. Here’s how you might structure this:

```yaml
# Base deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 1
  template:
    spec:
      containers:
      - name: my-app
        image: my-app:tag
        ports:
        - containerPort: 80
```

You can then override this base configuration for different environments:

```yaml
# Overlays for development
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
bases:
  - ../../base
patchesStrategicMerge:
  - dev_replicas.yaml
```

**Simplicity and Transparency**: Kustomize modifies and appends configurations directly, offering high transparency. You see the exact final form of your YAML files, unlike Helm’s rendered templates.

### Guardrails in Kustomize

While Kustomize does not natively support the complex logic you can embed in Helm charts, it can be paired with other tools to enforce deployment standards. Suppose you want to ensure all containers run as non-root and have a read-only root filesystem. Here’s how you might set this up:

1. **Define a Custom Resource Definition (CRD)** for a new kind called `SecureDeployment`:

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: securedeployments.mycompany.com
spec:
  group: mycompany.com
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                image:
                  type: string
                replicas:
                  type: integer
                securityContext:
                  type: object
                  required: ["runAsNonRoot", "readOnlyRootFilesystem"]
                  properties:
                    runAsNonRoot:
                      type: boolean
                    readOnlyRootFilesystem:
                      type: boolean
  scope: Namespaced
  names:
    plural: securedeployments
    singular: securedeployment
    kind: SecureDeployment
    shortNames:
      - secdp
```

2. **Deploy the CRD and use the `SecureDeployment` kind in your deployments**:

```yaml
apiVersion: mycompany.com/v1
kind: SecureDeployment
metadata:
  name: example-secure-deployment
spec:
  image: "nginx:latest"
  replicas: 2
  securityContext:
    runAsNonRoot: true
    readOnlyRootFilesystem: true
```

### Conclusion

For those of us familiar with Helm, Kustomize offers a different yet compelling approach to Kubernetes configuration management. Its integration with Kubernetes, combined with its straightforward, overlay-based system, makes it a powerful tool for managing configurations across multiple environments. The shift from Helm’s dynamic templating to Kustomize’s structured, YAML-driven management requires a different mindset but can lead to clearer, more maintainable configurations in the Kubernetes landscape.
