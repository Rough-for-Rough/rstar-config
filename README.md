# Rstar-config

Rstar (Rising Star) is a seed project base on the Spring Cloud microservices architecture. It includes BOM project for version and properties definition, shared foundational components, and shared core services.

This Seed project is designed to minimize business-related functionality, enabling development teams to kickstart a project as quick as possible.

---

Rstar include:

| Functionality      | Desc                                                                                      |
| ------------------ | ----------------------------------------------------------------------------------------- |
| rstar-config       | All documents include architecture, branch strategy, Utils api etc.                       |
| rstar-flyway-db    | Basic DB design with flyway-db script, included authentication, authorization, trace log. |
| rstar-bom          | BOM project defined version and properties                                                |
| rstar-common-core  | Core profile that defined shared components                                               |
| rstar-common-feign | Feign Config defined backend api, controller, interface and Rq/Rs                         |
| rstar-common-bff   | Backend for frontend shared components                                                    |
| rstar-common-biz   | Core Businedss shared components                                                          |
| rstar-gateway      | Spring Cloud Gateway                                                                      |

---

## Pipeline

Rstar 使用 private runner, pipeline 的流程為

1. check out Bom 專案
2. install bom
3. check out core 專案
4. build core 專案
5. run unit test
6. upload to SonarQube
7. dependency track
