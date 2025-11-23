AKS-With-ArgoCD - Applications

This folder contains Kubernetes manifests for a simple TODO application (frontend + backend services).

Files in `applications/`:
- `backend-add-task.yaml`  - Production-ready Deployment, Service, Ingress, HPA, PDB for the "add" backend.
- `backend-delete-task.yaml` - Production-ready Deployment, Service, Ingress, HPA, PDB for the "delete" backend.
- `backend-get-task.yaml` - Production-ready Deployment, Service, Ingress, HPA, PDB for the "get" backend.
- `frontend.yaml` - Production-ready Deployment, Service, Ingress, HPA, PDB for the frontend.

Quick usage
1. Ensure you have `kubectl` configured for your cluster.
2. Create required Secret in the `todo` namespace (example):

   ```powershell
   kubectl create namespace todo
   kubectl create secret generic todosecret -n todo --from-literal=CONNECTION_STRING="<your-connection-string>"
   ```

3. Dry-run validate the manifests locally:

   ```powershell
   kubectl apply -f .\applications\ --dry-run=client
   ```

4. Apply to the cluster:

   ```powershell
   kubectl apply -f .\applications\
   ```

Notes
- The manifests create resources in the `todo` namespace. If you use a different namespace, update the `namespace` fields accordingly.
- Health checks are configured to `/healthz`. Update probe paths if your services expose different endpoints.
- Horizontal Pod Autoscalers require Metrics Server (or another metrics provider) in the cluster.
- Ingresses are annotated for Azure Application Gateway. If you use a different ingress controller, adjust annotations and `ingressClassName`.
- Images are set to `:stable`; replace with your CI/CD image tags or digests for immutable deployments.

If you'd like, I can:
- Create an Argo CD `Application` manifest to manage these resources.
- Add RBAC Roles/RoleBindings for the service accounts.
- Add TLS configuration to the Ingress resources.
