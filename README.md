## Architecture

### System Flow

A code change follows this lifecycle:

Developer → Source Control → CI Pipeline → Container Image → Kubernetes → Users

The application logic is intentionally minimal.  
The objective of this project is to demonstrate controlled release behaviour rather than feature complexity.

---

### Component Roles

Service (Go)  
Provides a minimal HTTP server exposing `/` and `/healthz` endpoints used for traffic eligibility checks.

Container (Docker)  
Packages the application into an immutable runtime artifact ensuring identical execution across environments.

CI/CD Pipeline (Jenkins)  
Automatically builds, tests and publishes the container image after a change is introduced.

Orchestrator (Kubernetes)  
Maintains desired state, schedules instances and manages rollout behaviour.

Release Strategy (Canary Deployment)  
Introduces a new version gradually while the previous version continues serving users.

---

### Deployment Behaviour

1. A new version is deployed alongside the existing version
2. Kubernetes checks instance readiness using health probes
3. Traffic is allowed only to healthy instances
4. A small percentage of requests reach the new version
5. If stable, rollout continues
6. If unstable, rollback occurs
