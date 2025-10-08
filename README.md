# experimental_.basket-system

## Content

this repo contains system-level infrastructure and database resources for the basket system

all backstage entities will be in namespace `experiments`, for all references use full qualified name <kind>:<namespace>/<name>
all entities have owner group:experiments/team-bravo and belongs under system `basket`

## System Infrastructure

### GitHub Repository Management

* **Kind: Resource, type: github-repository**
  * experimental_basket-mobile - Git repository for basket mobile components
  * owner: group:experiments/team-bravo
  * repositoryConfig includes:
    * Branch protection rules with Copilot rulesets
    * Pull request requirements (2 approvals, code owner reviews)
    * Issue tracking, projects, wiki, and discussions enabled
    * Topics: backstage-include, nuget-package
* **Kind: Resource, type: github-repository**
  * experimental_basket-backend - Git repository for basket web api
  * owner: group:experiments/team-bravo
  * repositoryConfig includes:
    * Branch protection rules with Copilot rulesets
    * Pull request requirements (2 approvals, code owner reviews)
    * Issue tracking, projects, wiki, and discussions enabled
    * Topics: backstage-include, nuget-package
* **Kind: Resource, type: github-repository**
  * experimental_basket-react-component - Git repository for basket FE components
  * owner: group:experiments/team-bravo
  * repositoryConfig includes:
    * Branch protection rules with Copilot rulesets
    * Pull request requirements (2 approvals, code owner reviews)
    * Issue tracking, projects, wiki, and discussions enabled
    * Topics: backstage-include, nuget-package

### Database Resources


* **Kind: Resource, type: kafka-topic**
  * kafka-topic-basket
  * provideAccess: component:experiments/dotnet-api-basket
  * dependsOn: resource:experiments/kafka-production-cluster

* **Kind: Resource, type: kafka-topic-access** (3x environments)
  * kafka-access-basket-dotnet-api-development
  * provideAccess: component:experiments/dotnet-api-basket
  * dependsOn: resource:experiments/kafka-production-cluster
### Kubernetes Resources

* **Kind: Resource, type: k8s-namespace**
  * basket-k8s-namespace - Kubernetes namespace for checkout
  * namespaceName: basket
  * environments: development, staging, production
  * dependsOn:
    * resource:experiments/k8s-cluster-development
    * resource:experiments/k8s-cluster-staging
    * resource:experiments/k8s-cluster-production

## Structure

```
experimental_.basket-system/
├── catalog-info.yaml (GitHub repositories, Kafka topic, and location references)
├── kafka/
│   └── kafka-topic-access.yaml (Kafka access for all environments)
└── kubernetes/
    └── k8s-namespace.yaml (Kubernetes namespace configuration)
```

## Purpose

This repository serves as the system-level infrastructure catalog for the basket system, providing:
- GitHub repository configuration and governance (mobile, backend, react-component)
- Kafka infrastructure and access management
- Kubernetes namespace provisioning across environments
- Environment-specific resource configurations
- Foundation for component repositories to build upon

## Related Repositories

- **Mobile Repository**: https://github.com/padreSVK/experimental_basket-mobile
  - Contains mobile application components and resources
  - Managed and created by this system repository

- **Backend Repository**: https://github.com/padreSVK/experimental_basket-backend
  - Contains backend API components and resources
  - Managed and created by this system repository

- **React Component Repository**: https://github.com/padreSVK/experimental_basket-react-component
  - Contains React component library
  - Managed and created by this system repository

## Migration Notes

- Kafka access resources are generated via Scaffolder when services request topic access
- Future consideration for porch + kpt integration for configuration management
- Kubernetes resources can be auto-ingested using @vrabbi/backstage-plugin-kubernetes-ingestor

## Issue types in this repo

* System wide: 
  * Incidents => public? SLO?
  * Requests => public? SLO?
  * Bugs
  * Risks?
    * support of confidential issues
