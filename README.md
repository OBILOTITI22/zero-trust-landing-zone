# Zero-Trust Landing Zone

This repository provides a reference implementation of a zero-trust Azure Landing Zone using Terraform and Bicep. The goal is to minimize the attack surface of your cloud environment by enforcing strict network segmentation, identity-driven access controls, and policy-based governance.

## Key Principles

- **Least Privilege**: Users and services are granted only the permissions they need. Managed identities and role-based access control (RBAC) are used throughout.
- **Network Micro-Segmentation**: Deploys a hub-and-spoke virtual network topology with subnets isolated via NSGs and Azure Firewall. Traffic between spokes flows through inspection points and is limited to required ports and protocols.
- **Private Connectivity**: Uses Private Link and Service Endpoints to ensure PaaS services are reachable only via private IPs. Public access to storage accounts, key vaults, and databases is disabled.
- **Policy and Compliance**: Azure Policy assignments enforce rules such as encryption at rest, SKU restrictions, and tagging requirements. Remediation tasks are included to fix non-compliant resources automatically.
- **Secure DevOps**: The infrastructure is deployed via CI/CD pipelines that include security scanning (e.g., Checkov, tfsec) and policy evaluation with Open Policy Agent (OPA).

## Repository Structure

- `networking/` – Terraform modules to create hub, spoke, and firewall subnets, route tables, and DNS settings.
- `security/` – Modules for Azure Firewall, Azure Bastion, JIT VM access, and NSG rules.
- `identity/` – Setup of Azure AD groups, managed identities, and RBAC role assignments.
- `policy/` – Terraform and Bicep definitions for custom policies and initiatives, plus assignments.
- `.github/workflows/` – GitHub Actions pipelines for linting, security scanning, plan review, and deployment.
- `docs/` – Architectural diagrams and guidance on zero-trust design.

## Getting Started

1. Fork this repository and clone it.
2. Review the variables in `terraform.tfvars` and adjust for your subscription, regions, and naming conventions.
3. Provision a service principal and set repository secrets: `ARM_CLIENT_ID`, `ARM_CLIENT_SECRET`, `ARM_SUBSCRIPTION_ID`, `ARM_TENANT_ID`.
4. Run `terraform init` and `terraform plan` locally or open a pull request to trigger the pipeline.
5. Approve the plan to deploy the landing zone. All network endpoints will be private; ensure your on-premises network can reach the hub via VPN or ExpressRoute.

## Documentation

Detailed diagrams and explanations of the zero-trust architecture, including subnet layouts and firewall rules, can be found in `docs/architecture.md`. Guidance on customizing policies and integrating with Azure Sentinel for monitoring is provided in `docs/security_guidance.md`.

---

Feel free to open issues or submit pull requests to improve this zero-trust blueprint.
