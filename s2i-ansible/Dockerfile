FROM quay.io/jksrc/ubuntu:18.04

LABEL io.k8s.description="Base image for Ansibl-Builder" \
      io.k8s.display-name="OpenShift ansible builder" \
      io.openshift.s2i.scripts-url="image:///usr/libexec/s2i" \
      io.openshift.tags="builder, ansible"

ENV WORKSPACE_DIR=/opt/ansible-workspace
ENV ANSIBLE_LOCAL_TEMP=/tmp/ansible-tmp
ENV ANSIBLE_REMOTE_TEMP=${ANSIBLE_LOCAL_TEMP}

RUN apt update && \
    apt install -y ansible

COPY ./s2i/bin/ /usr/libexec/s2i

RUN useradd -u 1001 -r -g 0 -d ${HOME} -s /sbin/nologin -c "Default Application User" default \
   && mkdir -p ${HOME} \
   && chown -R 1001:0 ${HOME} && chmod -R g+rwX ${HOME}


RUN mkdir -p ${ANSIBLE_REMOTE_TEMP} && \
    chown -R 1001:1001 ${ANSIBLE_REMOTE_TEMP} && \
    chgrp -R 0 ${ANSIBLE_REMOTE_TEMP} && \
    chmod -R g=u ${ANSIBLE_REMOTE_TEMP}

RUN mkdir -p ${WORKSPACE_DIR} && \
    chown -R 1001:1001 ${WORKSPACE_DIR} && \
    chgrp -R 0 ${WORKSPACE_DIR} && \
    chmod -R g=u ${WORKSPACE_DIR} && \
    chmod +x /usr/libexec/s2i/assemble /usr/libexec/s2i/run /usr/libexec/s2i/usage

WORKDIR ${WORKSPACE_DIR}

USER 1001

CMD ["/usr/libexec/s2i/usage"]
