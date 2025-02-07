---
title: "Add Karpenter nodeSelector"
category: Karpenter
version: 1.6.0
subject: Pod
policyType: "mutate"
description: >
    Selecting the correct Node(s) provisioned by Karpenter is a way to specify the appropriate resource landing zone for a workload. This policy injects a nodeSelector map into the Pod based on the Namespace type where it is deployed.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//karpenter/add-karpenter-nodeselector/add-karpenter-nodeselector.yaml" target="-blank">/karpenter/add-karpenter-nodeselector/add-karpenter-nodeselector.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: add-karpenter-nodeselector
  annotations:
    policies.kyverno.io/title: Add Karpenter nodeSelector
    policies.kyverno.io/category: Karpenter
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Pod
    kyverno.io/kyverno-version: 1.7.1
    policies.kyverno.io/minversion: 1.6.0
    kyverno.io/kubernetes-version: "1.23"
    policies.kyverno.io/description: >- 
      Selecting the correct Node(s) provisioned by Karpenter is a way to specify
      the appropriate resource landing zone for a workload. This policy injects a
      nodeSelector map into the Pod based on the Namespace type where it is deployed.
spec:
  rules:
  - name: add-medium
    match:
      any:
      - resources:
          kinds:
          - Pod
          namespaceSelector:
            matchLabels:
              type: medium 
    mutate:
      patchStrategicMerge:
        spec:
          nodeSelector:
            kubernetes.io/arch: amd64
            karpenter.sh/capacity-type: spot
  - name: add-large
    match:
      any:
      - resources:
          kinds:
          - Pod
          namespaceSelector:
            matchLabels:
              type: large
    mutate:
      patchStrategicMerge:
        spec:
          nodeSelector:
            kubernetes.io/arch: amd64
            karpenter.sh/capacity-type: on-demand

```
