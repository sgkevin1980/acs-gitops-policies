# ACS Policies - GitOps Managed

Security policies for RHACS (Red Hat Advanced Cluster Security), deployed via ArgoCD.

## Structure

```
├── base/                  # Infrastructure (namespace, credentials, sync job)
│   ├── namespace.yaml
│   ├── credentials.yaml
│   └── sync-job.yaml      # ArgoCD PostSync hook - pushes policies to ACS
├── policies/              # Policy definitions (one ConfigMap per policy)
│   ├── 01-block-privileged.yaml
│   └── 02-require-team-label.yaml
└── README.md
```

## How It Works

1. Define/modify a policy in `policies/` directory
2. Commit and push to this repo
3. ArgoCD detects the change and syncs ConfigMaps to the cluster
4. The sync Job (PostSync hook) reads ConfigMaps and pushes to ACS Central via API
5. Policy is live in ACS

## Adding a New Policy

1. Create a new file in `policies/` with the ACS policy JSON inside a ConfigMap
2. Add a volume mount for the new ConfigMap in `base/sync-job.yaml`
3. Commit, push, and ArgoCD handles the rest

## Modifying a Policy

1. Edit the policy JSON in the relevant `policies/*.yaml` file
2. Commit and push
3. The sync Job will detect the existing policy by name and UPDATE it (not duplicate)
