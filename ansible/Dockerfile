FROM quay.io/ansible/ansible-runner

RUN set -x && \
    dnf --noplugins update -y && \
    dnf --noplugins install -y python39 python39-pip openssh-clients podman jq && \
    dnf --noplugins remove -y python38 python38-pip && \
    dnf --noplugins upgrade -y && dnf --noplugins clean all

ARG USER=runner
ARG UID=1001
ARG HOME=/home/$USER

RUN set -x && \
    useradd --create-home --uid $UID --gid 0 $USER

WORKDIR $HOME/ansible

RUN chown -R ${USER}:0 $HOME

USER $USER

ENV PATH $HOME/.local/bin:$PATH

RUN set -x && \
    python3 -m pip install --no-cache-dir --user --upgrade pip && \
    python3 -m pip install --no-cache-dir --user --upgrade ansible

COPY --chown=${USER}:0 . .

LABEL network-services.description="Ansible automation for network services I run at home."
LABEL network-services.maintainer="Glenn Marcy <network-services@gmarcy.com>"

ENTRYPOINT ["ansible-playbook", "-e", "running_in_container=true"]
