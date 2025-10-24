flowchart TD
    Dev[👨‍💻 Developer] -->|Pre-commit hook| Gitleaks[🔒 Gitleaks Secret Scan]
    Gitleaks -->|No secret| Push[Push code to GitHub]
    Gitleaks -->|Secret found| Fix[Fix and retry]

    subgraph CI/CD Pipeline - GitHub Actions
        A1[📦 Checkout code] --> A2[🔑 Secret Management\n(GitHub Secrets / Vault)]
        A2 --> A3[🧩 Dependency Audit\n(Dependabot / OWASP DC)]
        A3 --> A4[🧠 SAST\n(SonarCloud)]
        A4 --> A5[🐳 Container Build & Scan\n(Trivy + Dockle)]
        A5 --> A6[🚀 Deploy Staging Environment]
        A6 --> A7[🌐 DAST - OWASP ZAP]
        A7 --> A8[📝 Generate Security Report\n(security-report.md)]
        A8 --> A9[📤 Upload Artifacts / Commit Report]
    end

    A9 --> QA{✅ Quality Gate?}
    QA -->|Pass| Done[(✅ Secure Build)]
    QA -->|Fail| Block[❌ Stop Deployment]
