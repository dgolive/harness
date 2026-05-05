# Changelog

## 2026-02-05

### Fixed

- **Artifactory Pods Failing to Start:** Resolved an issue where Artifactory and PostgreSQL pods were stuck in `CrashLoopBackOff` due to "Permission denied" errors on `hostPath` PersistentVolumes. The fix involves ensuring the host directories `/data/artifactory-registry/` and `/data/artifactory-postgresql/` exist on the Kubernetes node and have appropriate write permissions for the container users.
