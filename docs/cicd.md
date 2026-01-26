### 🔄 CI/CD Pipeline Flow

Developer
   |
   v
Git Push (GitHub / GitLab)
   |
   v
CI Tool (Jenkins / GitHub Actions)
   |
   |-- Build
   |-- Test
   |-- Security Scan
   |
   v
Artifact Repository (Nexus / Artifactory)
   |
   v
CD Tool (ArgoCD / Jenkins)
   |
   v
Kubernetes / Cloud Server
