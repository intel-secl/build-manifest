# Intel(R) SecL-DC: Quick Start 

Build Tools for getting started with Intel(R) SecL-DC usecases


### Components per Use case

Use case | Sub-Usecase | ta | wla | sa | hvs | wls | shvs | sqvs | scs | kbs | ih | wpm | cms | aas | skc   
---------|---------|----|-----|----|-----|-----|------|------|-----|-----|----|-----|------|------|------   
Foundational Security | \- | ✔️ | ❌ | ❌ | ✔️ | ❌ | ❌ | ❌ | ❌ | ❌ | ✔️ | ❌ | ✔️ | ✔️ | ❌   
Launch Time Protection | Container Confidentiality | ✔️ | ✔️ | ❌ | ✔️ | ✔️ | ❌ | ❌ | ❌ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | ❌  
Secure Key Caching | \- | ❌ | ❌ | ✔️ | ❌ | ❌ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | ❌ | ✔️ | ✔️ | ✔️   
Sgx Orchestration  | \- | ❌ | ❌ | ✔️ | ❌ | ❌ | ✔️ | ❌ | ✔️ | ❌ | ✔️ | ❌ | ✔️ | ✔️ | ❌   
All | \- | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️     



### Manifest files

Use case | Sub-UseCase | manifest
---------|---------|----------
Foundational Security | \- | `manifest/fs.xml`
Container Confidentiality with CRIO Runtime | \- | `manifest/cc-crio.xml`
Secure Key Caching | \- | `manifest/skc.xml`
All Components | \- | `manifest/all-components.xml`
