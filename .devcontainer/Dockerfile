FROM python:3.10.5-bullseye

RUN apt update && apt install -y \
    git \
    curl \
    vim \
    htop \
    jq \
    nkf \
    libgl1-mesa-dev

RUN curl -fsSL https://get.docker.com | sh

RUN pip install --upgrade pip
RUN pip install --upgrade setuptools
RUN pip install --upgrade ipykernel

WORKDIR /app
COPY . /app

RUN pip install -r requirements.txt

EXPOSE 30000
EXPOSE 50000

RUN useradd -m user

ARG NONROOT_USER=user
RUN echo "#!/bin/sh\n\
    sudoIf() { if [ \"\$(id -u)\" -ne 0 ]; then sudo \"\$@\"; else \"\$@\"; fi }\n\
    SOCKET_GID=\$(stat -c '%g' /var/run/docker.sock) \n\
    if [ \"${SOCKET_GID}\" != '0' ]; then\n\
        if [ \"\$(cat /etc/group | grep :\${SOCKET_GID}:)\" = '' ]; then sudoIf groupadd --gid \${SOCKET_GID} docker-host; fi \n\
        if [ \"\$(id ${NONROOT_USER} | grep -E \"groups=.*(=|,)\${SOCKET_GID}\(\")\" = '' ]; then sudoIf usermod -aG \${SOCKET_GID} ${NONROOT_USER}; fi\n\
    fi\n\
    exec \"\$@\"" > /usr/local/share/docker-init.sh \
    && chmod +x /usr/local/share/docker-init.sh

ENTRYPOINT [ "/usr/local/share/docker-init.sh" ]
CMD [ "sleep", "infinity" ]
