# tf-controller

A Terraform controller for Flux

## Quick start

Here's a simple example of how to GitOps-ify your Terraform resources with TF controller and Flux.

### Auto-mode

```yaml
apiVersion: infra.contrib.fluxcd.io
kind: Terraform
metadata:
  name: hello-world
  namespace: flux-system
spec:
  approvePlan: "auto"
  path: ./terraform-hello-world-example
  sourceRef:
    kind: GitRepository
    name: infra-repo
    namespace: flux-system
```

### Plan and manually approve

```diff
apiVersion: infra.contrib.fluxcd.io/v1alpha1
kind: Terraform
metadata:
  name: hello-world
  namespace: flux-system
spec:
- approvePlan: "auto"
+ approvePlan: "" # or you can omit this field 
  path: ./terraform-hello-world-example
  sourceRef:
    kind: GitRepository
    name: infra-repo
    namespace: flux-system
```

then use field `approvePlan` to approve the plan so that it apply the plan to create real resources.

```diff
apiVersion: infra.contrib.fluxcd.io/v1alpha1
kind: Terraform
metadata:
  name: hello-world
  namespace: flux-system
spec:
- approvePlan: ""
+ approvePlan: "plan-main-b8e362c206" # the format is plan-$(branch name)-$(10 digits of commit)
  path: ./terraform-hello-world-example
  sourceRef:
    kind: GitRepository
    name: infra-repo
    namespace: flux-system
```

## Roadmap

### Q1 2022
  * Terraform outputs as Kubernetes Secrets
  * Secret and ConfigMap as input variables 
  * Support the GitOps way to "plan" / "re-plan" 
  * Support the GitOps way to "apply"
  
### Q2 2022  
   
  * Interop with Kustomization controller's health checks (via the Output resources)
  * Interop with Notification controller's Events and Alert

### Q3 2022
  * Write back and show plan in PRs
  * Support auto-apply so that the reconciliation detect drifts and always make changes
