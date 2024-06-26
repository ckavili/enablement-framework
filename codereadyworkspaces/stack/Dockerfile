FROM registry.access.redhat.com/ubi9/ubi:9.1

USER root

# rht-labs stuff
ENV ARGOCD_VERSION=2.7.1 \
    YQ_VERSION=4.23.1 \
    HELM_VERSION=3.11.2 \
    KUBESEAL_VERSION=0.20.5 \
    OC_VERSION=4.12.14 \
    TEKTON_VERSION=1.10.0 \
    JQ_VERSION=1.6 \
    CONFTEST_VERSION=0.41.0 \
    ROX_VERSION=3.74.3 \
    KUBELINTER_VERSION=0.6.3 \
    COSIGN_VERSION=2.2.3 \
    K6_VERSION=0.44.0 \
    JSONNET_VERSION=0.20.0 \
    SYFT_VERSION=0.82.0

ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk \
    PATH=$HOME/node_modules/.bin/:$HOME/.npm-global/bin/:/opt/app-root/src/.npm-global/bin/:/usr/lib/jvm/java-17-openjdk/bin:/usr/bin:$HOME/go/bin:$PATH \
    MAVEN_URL=https://archive.apache.org/dist/maven/maven-3/3.9.1/binaries/apache-maven-3.9.1-bin.tar.gz \
    HOME=/home/developer \
    PROJECTS=/projects \
    MAVEN_CONFIG=/projects/.m2

RUN dnf -y install \
    bash tar gzip unzip which findutils wget git procps-ng python3-pip bzip2 java-17-openjdk java-17-openjdk-devel java-17-openjdk-headless nodejs npm nodejs-nodemon nss_wrapper zsh iputils bind-utils net-tools go-toolset openssl && \
    dnf -y -q clean all && rm -rf /var/cache/yum && \
    pip3 install pylint && \
    mkdir -p ${HOME} ${PROJECTS} /usr/share/maven && \
    curl -fsSL ${MAVEN_URL} | tar -xzC /usr/share/maven --strip-components=1 && \
    ln -s /usr/share/maven/bin/mvn /usr/bin/mvn && \
    ln -s /usr/bin/node /usr/bin/nodejs

# argo
RUN curl -sL https://github.com/argoproj/argo-cd/releases/download/v${ARGOCD_VERSION}/argocd-linux-amd64 -o /usr/local/bin/argocd && \
    chmod -R 775 /usr/local/bin/argocd && \
    echo "🐙🐙🐙🐙🐙"

# kubeseal
RUN curl -sL https://github.com/bitnami-labs/sealed-secrets/releases/download/v${KUBESEAL_VERSION}/kubeseal-${KUBESEAL_VERSION}-linux-amd64.tar.gz | tar --no-same-owner -xzf - -C /usr/local/bin kubeseal && \
    chmod -R 755 /usr/local/bin/kubeseal && \
    echo "🦭🦭🦭🦭🦭"

# tekton
RUN curl -sL https://mirror.openshift.com/pub/openshift-v4/clients/pipeline/${TEKTON_VERSION}/tkn-linux-amd64.tar.gz | tar --no-same-owner -xzf - -C /usr/local/bin tkn && \
    chmod -R 755 /usr/local/bin/tkn && \
    echo "🐈🐈🐈🐈🐈"

# oc client
RUN rm -f /usr/bin/oc && \
    curl -sL https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/${OC_VERSION}/openshift-client-linux.tar.gz | tar -C /usr/local/bin -xzf - && \
    echo "🐨🐨🐨🐨🐨"

# roxctl client
RUN curl -sL -o /usr/local/bin/roxctl https://mirror.openshift.com/pub/rhacs/assets/${ROX_VERSION}/bin/Linux/roxctl && \
    chmod +x /usr/local/bin/roxctl && \
    echo "🦜🦜🦜🦜🦜"

# kube-linter
RUN curl -sL https://github.com/stackrox/kube-linter/releases/download/v${KUBELINTER_VERSION}/kube-linter-linux.tar.gz | tar -C /usr/local/bin -xzf - && \
    chmod +x /usr/local/bin/kube-linter && \
    echo "🐐🐐🐐🐐🐐"

# jq / yq
RUN curl -sLo /usr/local/bin/jq https://github.com/stedolan/jq/releases/download/jq-${JQ_VERSION}/jq-linux64 && \
    chmod +x /usr/local/bin/jq && \
    curl -sLo /usr/local/bin/yq https://github.com/mikefarah/yq/releases/download/v${YQ_VERSION}/yq_linux_amd64 && \
    chmod +x /usr/local/bin/yq && \
    echo "🦨🦨🦨🦨🦨"
 
# Conftest? This will make Gareth Happy :P
RUN curl --fail -skL https://github.com/open-policy-agent/conftest/releases/download/v${CONFTEST_VERSION}/conftest_${CONFTEST_VERSION}_Linux_x86_64.tar.gz | tar zxf - -C /usr/local/bin conftest && \
    echo "🌲🌲🌲🌲🌲"

# helm
RUN curl -skL -o /tmp/helm.tar.gz https://get.helm.sh/helm-v${HELM_VERSION}-linux-amd64.tar.gz && \
    tar -C /tmp -xzf /tmp/helm.tar.gz && \
    mv -v /tmp/linux-amd64/helm /usr/local/bin && \
    chmod -R 775 /usr/local/bin/helm && \
    rm -rf /tmp/linux-amd64 && \
    echo "⚓️⚓️⚓️⚓️⚓️"

# cosign
RUN curl -skL -o /usr/local/bin/cosign https://github.com/sigstore/cosign/releases/download/v${COSIGN_VERSION}/cosign-linux-amd64 && \
    chmod -R 775 /usr/local/bin/cosign && \
    echo "🔒🔒🔒🔒"

# syft
RUN curl -sSfL https://raw.githubusercontent.com/anchore/syft/v${SYFT_VERSION}/install.sh | sh -s -- -b /usr/local/bin && \
    echo "🛼🛼🛼"

# hey
RUN curl -skL -o /usr/local/bin/hey https://hey-release.s3.us-east-2.amazonaws.com/hey_linux_amd64 && \
    chmod 775 /usr/local/bin/hey && \
    echo "👋👋👋"

# k6
RUN curl -skL https://github.com/grafana/k6/releases/download/v${K6_VERSION}/k6-v${K6_VERSION}-linux-amd64.tar.gz | tar zxf - -C /usr/local/bin --strip-components=1 k6-v${K6_VERSION}-linux-amd64/k6 && \
    chmod 775 /usr/local/bin/k6 && \
    echo "🐶🐶🐶"

# docsify-cli
RUN npm i -g docsify-cli && \
    echo "📚📚📚📚📚"

# oh-my-zsh 
RUN git clone https://github.com/robbyrussell/oh-my-zsh.git $HOME/.oh-my-zsh && \
    cp $HOME/.oh-my-zsh/templates/zshrc.zsh-template $HOME/.zshrc && \
    chmod 660 $HOME/.zshrc && \
    sed -i '1iZSH_DISABLE_COMPFIX=true' $HOME/.zshrc && \
    sed -i '/^# DISABLE_MAGIC_FUNCTIONS.*/s/^#//' $HOME/.zshrc && \
    echo "setopt PROMPT_CR" >> $HOME/.zshrc && \
    echo "setopt PROMPT_SP" >> $HOME/.zshrc && \
    echo "export PROMPT_EOL_MARK=\"\"" >> $HOME/.zshrc && \
    echo "😎😎😎😎😎"

# sort out git
RUN echo "git config --global http.sslVerify false" | tee -a /etc/bashrc -a /etc/zshrc && \
    echo "git config --global user.name 'Derek Dinosaur'" | tee -a /etc/bashrc -a /etc/zshrc&& \
    echo "git config --global user.email 'derek@dinosaur.com'" | tee -a /etc/bashrc -a /etc/zshrc && \
    echo "git config --global credential.helper 'cache --timeout=172800'" | tee -a /etc/bashrc -a /etc/zshrc && \
    echo "git config --global pull.rebase 'false'" | tee -a /etc/bashrc -a /etc/zshrc && \
    echo "git config --global push.default 'simple'" | tee -a /etc/bashrc -a /etc/zshrc && \
    echo "🤗🤗🤗🤗🤗"

# needed for DevSpaces nodejs agent version for now, not in ubi - libcrypto.so.1.1
RUN dnf install -y https://rpmfind.net/linux/fedora/linux/releases/37/Everything/x86_64/os/Packages/o/openssl1.1-1.1.1q-2.fc37.x86_64.rpm && \
  dnf -y -q clean all && rm -rf /var/cache/yum

# custom user functions
RUN echo "source /usr/local/bin/user-functions.sh" | tee -a /etc/bashrc -a /etc/zshrc

# create user && clean up
RUN rm -rf /tmp/* && \
    useradd -u 1000 -G wheel,root -d /home/developer --shell /bin/zsh -M developer && \
    for f in "${HOME}" "/etc/passwd" "/etc/group" "/projects" "${HOME}/.cache"; do \
        chgrp -R 0 ${f} && \
        chmod -R g+rwX ${f}; \
    done && \
    echo "🧹🧹🧹🧹🧹" && \
    cat /etc/passwd | \
    sed s#developer:x.*#developer:x:\${USER_ID}:\${GROUP_ID}::\${HOME}:/bin/zsh#g \
    > ${HOME}/passwd.template && \
    cat /etc/group | \
    sed s#root:x:0:#root:x:0:0,\${USER_ID}:#g \
    > ${HOME}/group.template && \
    echo "🐟🐟🐟🐟🐟"

# Prepare entrypoint
COPY root /

# runtime
USER developer
ENTRYPOINT [ "/usr/local/bin/entrypoint.sh" ] 
WORKDIR /projects
