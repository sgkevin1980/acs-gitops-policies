# ACS Policies as Code — GitOps Managed

Security policies for RHACS, managed as Kubernetes Custom Resources via ArgoCD.

## How It Works

ACS 4.4+ supports `SecurityPolicy` CRDs (`config.stackrox.io/v1alpha1`).
Policies defined as CRs in this repo are synced by ArgoCD directly to the
`stackrox` namespace. ACS's `config-controller` picks them up automatically.

**No sync jobs. No credentials. No API calls. Just standard Kubernetes resources.**

## Structure

```
policies/
├── block-privileged.yaml      # SecurityPolicy CR
└── require-team-label.yaml    # SecurityPolicy CR
```

## Usage

### Add a new policy
1. Create a new YAML file in `policies/`
2. Commit and push
3. ArgoCD syncs → ACS enforces

### Modify a policy
1. Edit the YAML (e.g., change severity, add enforcement)
2. Commit and push
3. ArgoCD syncs → ACS updates the policy

### Delete a policy
1. Delete the YAML file
2. Commit and push
3. ArgoCD prunes the CR → ACS removes the policy
