```mermaid
sequenceDiagram;
    participant User;
    participant Flux CLI;
    participant Cluster;
    participant Config Repo;
    User ->> Flux CLI: Run `flux bootstrap`;
    Flux CLI ->> Cluster: Add source and controllers
    Flux CLI ->> Config Repo: Write manifests to git repository
    Cluster ->> Config Repo: Fetch yaml manifests
    Config Repo ->> Cluster: 
    Cluster ->> Cluster: Controllers reconcile manifests
    User ->> Config Repo: Add helm repository and helm release for Weave GitOps Core
    Cluster ->> Config Repo: Fetch yaml manifests
    Config Repo ->> Cluster: 
    Cluster ->> Cluster: Controllers install Weave GitOps
    User ->> Config Repo: Delete Weave GitOps manifests
    Cluster ->> Config Repo: Fetch yaml manifests
    Config Repo ->> Cluster: 
    Cluster ->> Cluster: Controllers uninstall Weave GitOps
```

```mermaid
sequenceDiagram;
    participant User;
    participant GitOps CLI;
    participant Cluster;
    participant Config Repo;
    User ->> GitOps CLI: Run `gitops bootstrap`;
    GitOps CLI ->> Cluster: Add source and controllers
    GitOps CLI ->> Config Repo: Write manifests to git repository including Weave GitOps
    Cluster ->> Config Repo: Fetch yaml manifests
    Config Repo ->> Cluster: 
    Cluster ->> Cluster: Controllers install Weave GitOps
    User ->> Config Repo: Delete Weave GitOps manifests
    Cluster ->> Config Repo: Fetch yaml manifests
    Config Repo ->> Cluster: 
    Cluster ->> Cluster: Controllers uninstall Weave GitOps
```

---

