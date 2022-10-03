FROM alpine:3.7

RUN \
    # apk add installs the following
    apk add \
    curl \
    python \
    py-pip \
    py-boto \
    py-dateutil \
    py-httplib2 \
    py-jinja2 \
    py-paramiko \
    py-setuptools \
    py-yaml \
    openssh-client \
    bash \
    tar && \
    pip install --upgrade pip

# Makes the Ansible directories
RUN adduser -D ansible
RUN mkdir -p /home/ansible/.ssh /home/download /home/ansible/ansible
RUN chown -R ansible:ansible /home/ansible/.ssh
#COPY id_rsa_shared /home/user/.ssh/id_rsa

# Downloads the Ansible tar (curl) and saves it (-o)
RUN curl -fsSL https://releases.ansible.com/ansible/ansible-2.9.3.tar.gz -o /home/download/ansible.tar.gz

# Extracts Ansible from the tar file
RUN tar -xzf home/download/ansible.tar.gz -C home/ansible/ansible --strip-components 1

RUN rm -fr ansible.tar.gz /home/ansible/ansible/docs /home/ansible/ansible/examples /home/ansible/ansible/packaging

# Makes a directory for ansible playbooks
RUN mkdir -p /home/ansible/ansible/playbooks
# Makes the playbooks directory the working directory
WORKDIR /home/ansible/

# Sets environment variables
ENV ANSIBLE_GATHERING smart
ENV ANSIBLE_HOST_KEY_CHECKING False
ENV ANSIBLE_RETRY_FILES_ENABLED False
ENV ANSIBLE_ROLES_PATH /home/ansible/ansible/playbooks/roles
ENV ANSIBLE_SSH_PIPELINING True
ENV PATH /home/ansible/ansible/:$PATH
ENV PYTHONPATH /home/ansible/ansible/lib

USER ansible

ENTRYPOINT ["tail", "-f", "/dev/null"]