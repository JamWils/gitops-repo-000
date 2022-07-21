```mermaid
sequenceDiagram;
    participant User;
    participant GitOps Run;
    participant Config Repo;
    participant Cluster;
    User ->> Config Repo: Run `git init`;
    User ->> GitOps Run: Run `gitops run`;
    GitOps Run ->> Cluster: Flux controllers and CRDs are installed;
    GitOps Run ->> Config Repo: Manifests for flux-system are saved to disk;
    User ->> GitOps Run: ctrl+c `gitops run`;
    GitOps Run ->> Config Repo: Gets info about repo such as default branch;
    GitOps Run ->> Cluster: Creates reconciliation loop with Source and Kustomization;
    GitOps Run ->> Cluster: Cleanup temporary bucket and workload
    GitOps Run ->> GitOps Run: End process;
    User ->> Config Repo: Push to remote;
    Cluster ->> Config Repo: Pulls files from remote;
    Cluster ->> Cluster: Create workloads based on pulled CRDs;
    User ->> Cluster: The user can see the workloads on the cluster;
    User ->> User: GitOps...alright, alright, alright!
```

---

Here is a legend for executing `gitops run`
```mermaid
flowchart TD
    id1[GitOps Run path execution]
    id2[GitOps Run root directory]
    id3[Overridden Flux Object]
    id4[Files created by GitOps Run]
    id5[Synced directories]

    style id1 fill:#5fd2e8,stroke:#000,color:#000
    style id2 fill:#ebd08b,stroke:#000,color:#000
    style id3 fill:#d12f82,stroke:#000,color:#000
    style id4 fill:#6eed9e,stroke:#000,color:#000
```

---
### Criteria
1. This is a brand new cluster
2. Flux is not already installed on the cluster.
3. The **workload** does not exist on the cluster.
4. All manifests are in the same repository.
5. `gitops run` is run on the basic path

```mermaid
%%{init: { logLevel:0, startOnLoad: false, themeCSS:'.label { font-family: Source Sans Pro,Helvetica Neue,Arial,sans-serif; }' }}%%
flowchart LR
    run[gitops run] --> cluster
    root <--> run
    subgraph root[./ ]
    subgraph sgp6 [ ]
    subgraph clusters[./clusters]
    subgraph sgp1 [ ]
      subgraph my-cluster[./my-cluster]
        subgraph sgp2 [ ]
        dboard[gitops-dashboard.yaml]
        subgraph flux[./flux-system]
          a[gotk-components.yaml] 
          b[gotk-sync.yaml]
          c[kustomization.yaml]
        end
        end
      end
    end  
    end
    end
    
    end
    subgraph cluster
      direction TB
      subgraph sgp5 [ ]
        c-a[We install Flux CRDs and Controllers]
        c-b[Temp Bucket and Kustomization]
        c-c[Dev Bucket Server]
      end
    end
      
    
classDef subgraph_padding fill:#000,stroke:none
class sgp1,sgp2,sgp3,sgp4,sgp5 subgraph_padding

classDef basic fill:#000;
class clusters,my-cluster,flux,app,cluster basic;

style root fill:#ebd08b,stroke:#000,color:#000
style sgp6 fill:#5fd2e8,stroke:transparent,color:#000
classDef create fill:#6eed9e,stroke:#000,color:#000;
class a,b,c,dboard create
````

---
### Criteria
1. Flux is already installed on the cluster.
2. The **workload** already exists on the cluster.
3. The user is trying to update an existing workload and is running it in a specific directory.
4. All manifests are in the same repository.

```mermaid
%%{init: { logLevel:0, startOnLoad: false, themeCSS:'.label { font-family: Source Sans Pro,Helvetica Neue,Arial,sans-serif; }' }}%%
flowchart TD
    root --> 2g
    root --> 3g
    root --> clusters
    subgraph clusters[./clusters]
    subgraph sgp1 [ ]
      subgraph my-cluster[./my-cluster]
        subgraph sgp2 [ ]
        dboard[gitops-dashboard.yaml]
        subgraph app[./app]
          d[dev-ks.yaml]
        end
        subgraph flux[./flux-system]
          a[gotk-components.yaml] 
          b[gotk-sync.yaml]
          c[kustomization.yaml]
        end
        end
      end
    end  
    end
      
    subgraph 3g[./app]
    subgraph sgp3 [ ]
      31[app.yaml]
      end
    end
    subgraph 2g[./dev]
    subgraph sgp4 [ ]
      21[nginx.yaml]
      22[ns.yaml]
    end
    end
    
classDef subgraph_padding fill:none,stroke:none
class sgp1,sgp2,sgp3,sgp4 subgraph_padding

classDef basic fill:transparent;
class 3g,clusters,my-cluster,flux,app basic;

style 2g fill:#5fd2e8,stroke:#000,color:#000;
style root fill:#ebd08b,stroke:#000,color:#000;
style d fill:#d12f82,stroke:#000,color:#000;
````