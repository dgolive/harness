# Changelog

All notable changes to this project will be documented in this file.

## [2026-01-27] - Pipeline Fixes

### Fixed
- Resolved "ERROR: pom.xml not found" by enabling `cloneCodebase: true` in CI stages.
- Corrected working directory paths in Maven and Docker build scripts to `jenkins/java-app-1`.
- Updated Kubernetes manifest paths in the deployment stage to correctly point to `jenkins/java-app-1/k8s/`.

### Changed
- Updated CI stage images:
    - Maven build/test: `maven:3.9-eclipse-temurin-17-alpine` (includes Java 17).
    - Docker build/push: Replaced manual `docker build/push` steps with the native Harness `BuildAndPushDockerRegistry` step. 
    - Connector Update: Updated `localdockerreg` connector in `connectores.yaml` to use `UsernamePassword` authentication. This is required by the `BuildAndPushDockerRegistry` step to successfully push images to a registry.
- Refactored `maven_build_script` to remove manual Java installation logic, leveraging the new Maven image.

