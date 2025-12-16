<!-- Copilot / AI agent instructions tailored to this repository -->
# Copilot instructions — Online Voting Microservice App

Purpose: give an AI coding agent the minimal, high-value context to be immediately productive in this repo (multi-module Spring Boot microservices).

- **Project type:** Maven multi-module (`packaging: pom`) with modules under the root `pom.xml`. Currently present module: `eureka-server`.
- **Platform & versions:** Java 21, Spring Boot 3.4.3 (parent), Spring Cloud 2024.0.0 (dependency management).
- **Primary component:** `eureka-server` — a Netflix Eureka discovery server. Key files: `eureka-server/pom.xml`, `eureka-server/src/main/java/com/online/voting/eureka_server/EurekaServerApplication.java`, `eureka-server/src/main/resources/application.yml`.

Quick operational commands (Windows, repo root):

- Build all modules: ``.\mvnw.cmd -T 1C clean install``
- Build a single module (example `eureka-server`): ``.\mvnw.cmd -pl :eureka-server -am clean package``
- Run Eureka from repo root: ``.\mvnw.cmd -pl eureka-server spring-boot:run`` or ``cd eureka-server; ..\mvnw.cmd spring-boot:run``
- Run tests: ``.\mvnw.cmd test``

Important project-specific notes and patterns:

- **Module layout**: The top-level `pom.xml` is a parent aggregator (`<packaging>pom</packaging>`). Add new services as modules in that parent `pom.xml`.
- **Discovery / integration**: `eureka-server` runs on port `8761` (see `application.yml`) and is configured as a discovery server (Eureka). Other services should register with it via the usual Spring Cloud Eureka properties (e.g. `eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka/`).
- **YAML sensitivity**: `eureka-server/src/main/resources/application.yml` is indentation-sensitive; maintain 2-space indentation for nested keys. Current relevant snippet:

```
server:
  port: 8761

spring:
  application:
    name: eureka-server

eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
  server:
    enable-self-preservation: false
```

- **Package name inconsistencies**: Note inconsistent naming conventions in the repo — e.g. top-level app uses `com.online_voting_microservice_app` (underscores), while Eureka uses `com.online.voting.eureka_server`. Be careful when moving classes or refactoring packages; IDE refactors must update build paths and tests.
- **Actuator**: `eureka-server` includes Spring Boot Actuator in its `pom.xml` — use `/actuator/health` and other endpoints for health checks when running locally (if enabled by properties).

Files to inspect for big-picture changes:

- `pom.xml` (root) — parent coordinates versions and modules.
- `eureka-server/pom.xml` — module-specific dependencies (Eureka server dependency is the main integration point).
- `eureka-server/src/main/resources/application.yml` — runtime properties for the discovery server.
- `src/main/java/.../OnlineVotingMicroserviceAppApplication.java` — example main application class in root module.

Debugging and development workflow tips:

- For quick iteration, run individual modules with the Maven Spring Boot plugin rather than building whole repo every time.
- Use your IDE run configuration targeting the module's main class (e.g., `EurekaServerApplication`) and ensure JDK 21 is selected.
- If you see registration or discovery issues, check `application.yml` properties and the Eureka server logs for registration attempts.

When editing or adding files, follow these constraints:

- Keep indentation correct in YAML files.
- Match package names to directory layout; avoid mixing underscores/dots without updating package declarations.
- Prefer adding new microservice modules as separate Maven modules under the parent `pom.xml`.

If anything is unclear or you need more detail (run output, CI config, or missing modules), ask for the specific file or for permission to run a local build.

---
Please review these instructions and tell me if you want more detail for any section (build matrix, CI steps, or example service registration).
