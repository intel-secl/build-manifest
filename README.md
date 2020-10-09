# Intel(R) SecL-DC: Quick Start

Build Tools for getting started with Intel(R) SecL-DC usecases


### Components per Use case

Use case | Sub-Usecase | ta | wla | sa | hvs | wls | shvs | sqvs | scs | kbs | ih | wpm | cms | aas
---------|---------|----|-----|----|-----|-----|------|------|-----|-----|----|-----|------|------
Foundational Security | \- | ✔️ | ❌ | ❌ | ✔️ | ❌ | ❌ | ❌ | ❌ | ❌ | ✔️ | ❌ | ✔️ | ✔️
Launch Time Protection | VM Confidentiality | ✔️ | ✔️ | ❌ | ✔️ | ✔️ | ❌ | ❌ | ❌ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️
\- | Container Confidentiality   & Integrity | ✔️ | ✔️ | ❌ | ✔️ | ✔️ | ❌ | ❌ | ❌ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️
Secure Key Caching | \- | ❌ | ❌ | ✔️ | ❌ | ❌ | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | ❌ | ✔️ | ✔️



### Manifest files

Use case | Sub-UseCase | manifest
---------|---------|----------
Foundational Security | \- | `manifest/fs.xml`
Launch Time Protection | VM Confidentiality | `manifest/vmc.xml`
\- | Container Confidentiality with Docker Runtime | `manifest/cc-docker.xml`
Container Confidentiality with CRIO Runtime | \- | `manifest/cc-crio.xml`
Secure Key Caching | \- | `manifest/skc.xml`


### Quick Start Guides

Foundational & Workload Security: https://github.com/intel-secl/docs/blob/master/quick-start-guides/Quick%20Start%20Guide%20-%20Intel%C2%AE%20Security%20Libraries%20-%20Foundational%20%26%20Workload%20Security.md

Secure Key Caching: https://github.com/intel-secl/docs/blob/master/quick-start-guides/Quick%20Start%20Guide%20-%20Intel%C2%AE%20Security%20Libraries%20-%20Secure%20Key%20Caching.md


### Prerequisites

* The repos can be built only as `root` user

* RHEL 8.2 VM for building repos

* Enable the following RHEL repos:

  * `rhel-8-for-x86_64-appstream-rpms`
  * `rhel-8-for-x86_64-baseos-rpms`

* For **Secure Key Caching** use case, In addition, enable following RHEL repo

  * `codeready-builder-for-rhel-8-x86_64-rpms`

* Install basic utilities for getting started

  ```shell
  dnf install git wget tar python3 yum-utils
  ```

* Create symlink for python3

  ```shell
  ln -s /usr/bin/python3 /usr/bin/python
  ln -s /usr/bin/pip3 /usr/bin/pip
  ```

* Install repo tool

  ```shell
  tmpdir=$(mktemp -d)
  git clone https://gerrit.googlesource.com/git-repo $tmpdir
  install -m 755 $tmpdir/repo /usr/local/bin
  rm -rf $tmpdir
  ```

* Extract Install `go` version > `go1.13` & <= `go1.14.4` from `https://golang.org/dl/` and set `GOROOT` & `PATH`

  ```shell
  export GOROOT=<path_to_go>
  export PATH=$GOROOT/bin:$PATH
  ```

* Extract and Install `Maven`, version >= `3.6.3` from `https://archive.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz` & set in `PATH`

  ```shell
  wget https://archive.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
  tar -xf apache-maven-3.6.3-bin.tar.gz
  export M2_HOME=<path_to_maven>
  export PATH=$M2_HOME/bin:$PATH
  ```

  * Add the below profile element under the `<profiles>` section of `settings.xml` located under `<path_to_maven>/conf/` folder

    ```xml
    <profile>
        <id>artifacts</id>
        <repositories>
        <repository>
            <id>mulesoft-releases</id>
            <name>MuleSoft Repository</name>
            <url>http://repository.mulesoft.org/releases/</url>
            <layout>default</layout>
        </repository>
        <repository>
            <id>maven-central</id>
            <snapshots><enabled>false</enabled></snapshots>
            <url>http://central.maven.org/maven2</url>
        </repository>
        </repositories>
    </profile>
    ```

  * Enable `<activeProfiles>` to include the above profile.

    ```xml
    <activeProfiles>
        <activeProfile>artifacts</activeProfile>
    </activeProfiles>
    ```

  * If you are behind a proxy, enable proxy setting under maven `settings.xml`

    ```xml
    <!-- proxies
    | This is a list of proxies which can be used on this machine to connect to the network.
    | Unless otherwise specified (by system property or command-line switch), the first proxy
    | specification in this list marked as active will be used.
    |-->
    <proxies>
        <!-- proxy
        | Specification for one proxy, to be used in connecting to the network.
        |
        <proxy>
        <id>optional</id>
        <active>true</active>
        <protocol>http</protocol>
        <username>proxyuser</username>
        <password>proxypass</password>
        <host>proxy.host.net</host>
        <port>80</port>
        <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
        </proxy>
        -->
    </proxies> 
    ```


### Building

#### Foundational Security Usecase

* Sync the repo

  ```shell
  mkdir -p /root/isecl/fs && cd /root/isecl/fs
  repo init -u https://github.com/intel-secl/build-manifest.git -b refs/tags/v3.1.0 -m manifest/fs.xml
  repo sync
  ```

* Run the `pre-requisites` setup script

  ```shell
  cd utils/build/foundational-security/
  chmod +x fs-prereq.sh
  ./fs-prereq.sh
  ```

* Build all repos

  ```shell
  cd /root/isecl/fs/
  make all
  ```

* Built Binaries

  ```shell
  /root/isecl/fs/binaries
  ```


#### Workload Security Usecase

* Sync the repo

  ```shell
  #Container Confidentiality with Docker Runtime
  mkdir -p /root/isecl/cc-docker && cd /root/isecl/cc-docker
  repo init -u https://github.com/intel-secl/build-manifest.git -b refs/tags/v3.1.0 -m manifest/cc-docker.xml
  repo sync
  
  or
  
  #Container Confidentiality with CRIO Runtime
  mkdir -p /root/isecl/cc-crio && cd /root/isecl/cc-crio
  repo init -u https://github.com/intel-secl/build-manifest.git -b refs/tags/v3.1.0 -m manifest/cc-crio.xml
  repo sync
  
  or 
  
  #VM Confidentiality
  mkdir -p /root/isecl/vmc && cd /root/isecl/vmc
  repo init -u https://github.com/intel-secl/build-manifest.git -b refs/tags/v3.1.0 -m manifest/vmc.xml
  repo sync
  ```

* Run the `pre-requisites` script

  ```shell
  cd utils/build/workload-security
  chmod +x ws-prereq.sh
  ./ws-prereq.sh
  ```

* Download go dependencies

  ```shell
  cd /root/
  go get github.com/cpuguy83/go-md2man
  mv /root/go/bin/go-md2man /usr/bin/
  ```

* Enable and start the Docker daemon

  ```shell
  systemctl enable docker
  systemctl start docker
  ```

* Ignore the below steps if not running behind a proxy

  ```shell
  mkdir -p /etc/systemd/system/docker.service.d
  touch /etc/systemd/system/docker.service.d/proxy.conf
  
  #Add the below lines in proxy.conf
  [Service]
  Environment="HTTP_PROXY=<http_proxy>"
  Environment="HTTPS_PROXY=<https_proxy>"
  Environment="NO_PROXY=<no_proxy>"
  ```

  ```shell
  #Reload docker
  systemctl daemon-reload
  systemctl restart docker
  ```
  
* Build all repos

  ```shell
  #Container Confidentiality with Docker Runtime
  cd /root/isecl/cc-docker/
  make all

  or 

   #Container Confidentiality with CRIO Runtime
  cd /root/isecl/cc-crio/
  make all

  or 

  #VM Confidentiality
  cd /root/isecl/vmc/
  make all
  ```

* Built binaries

  ```shell
  #Container Confidentiality with Docker Runtime
  /root/isecl/cc-docker/binaries/
  
  #Container Confidentiality with CRIO Runtime
  /root/isecl/cc-crio/binaries/
  
  #VM Confidentiality
  /root/isecl/vmc/binaries
  ```

#### Secure Key Caching Usecase

* Additional packages for **Secure Key Caching** usecase

  ```shell
  dnf install java-1.8.0-openjdk.x86_64 wget gcc gcc-c++ ant git patch zip make openssl-devel
  ```
    
  ```shell
  dnf install https://download-ib01.fedoraproject.org/pub/epel/7/x86_64/Packages/m/makeself-2.2.0-3.el7.noarch.rpm
  ```

**Pulling Source Code**

```
mkdir -p /root/workspace && cd /root/workspace
repo init -u https://github.com/intel-secl/build-manifest.git -b refs/tags/v3.1.0 -m manifest/skc.xml
repo sync
```

**Building All SKC Components**
```
make

This script installs the following packages
    wget gcc gcc-c++ ant git zip java-1.8.0 make makeself

```


**Copy Binaries to a clean folder**

```
copy the generated binaries directory to the /root directory on the CSP/Enterprise VM
```

### Known Issues

On some occassions , the java repos might not get built and fail unexpected due to pom.xml issue. This is a known issue and a quick fix for this would be to copy the `settings.xml` of maven to `.m2` folder and re build the java repos






