# This file contains the steps required to install TAP via a GITOPS install

1. Create a Tanzu Network account (if one not created)
    - https://network.tanzu.vmware.com/

2. Prerequisite tools:
    - kubectl
    - k9s (optional)
        - https://k9scli.io/topics/install/
    - git
        - https://github.com/git-guides/install-git#install-git
    - github
        - https://github.com
    - SOPS
        - https://github.com/getsops/sops/releases
    - age
        - https://github.com/FiloSottile/age#installation
        - get Age Key from Tanzu SE
    - Tanzu CLI with plugins, setup below for after install (optional)
        - https://docs.vmware.com/en/VMware-Tanzu-CLI/1.0/tanzu-cli/index.html
        - tanzu context create
        - tanzu plugin install --group vmware-tap/default:v1.4.10

3. Create a cluster with 1 master and 2 workload nodes. Workload should have 8vCPU and 8GB of ram

4. Install Cluster Essentials:
    - setup the following environmental variables:
        export INSTALL_BUNDLE=export INSTALL_BUNDLE=registry.tanzu.vmware.com/tanzu-cluster-essentials/cluster-essentials-bundle@sha256:0378d8592f368b28495153871c497143035747b22f398642220eb0cae596b09d
        export INSTALL_REGISTRY_HOSTNAME=registry.tanzu.vmware.com
        export INSTALL_REGISTRY_USERNAME=
        export INSTALL_REGISTRY_PASSWORD=''
    - Select your distribution and execute install.sh
        - tanzu-cluster-essentials-darwin-1.4.5
        - tanzu-clustser-essentials-linux-1.4.5

5. Install Tanzu Application Platform:
    - setup the following environmental variables:
        export INSTALL_REGISTRY_HOSTNAME=harbor.kal.automatestuff.io
        export INSTALL_REGISTRY_USERNAME=
        export INSTALL_REGISTRY_PASSWORD=
        export TAP_VERSION=1.4.10
        export INSTALL_REPO=idaho
        export GIT_SSH_PRIVATE_KEY=P$(cat $HOME/.ssh/id_rsa)
        export GIT_KNOWN_HOSTS=$(ssh-keyscan github.com)
        export SOPS_AGE_KEY=$(cat ~/AGE-SAFE/key.txt)
        export TAP_PKGR_REPO=${INSTALL_REGISTRY_HOSTNAME}/${INSTALL_REPO}/tap-packages
    - execute the command within the git repo
        ./clusters/idaho-tap-cluster/tanzu-sync/scripts/deploy.sh
