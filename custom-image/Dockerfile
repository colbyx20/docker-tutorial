FROM ubuntu:latest

USER root
RUN apt update -y && apt install -y curl vim

ARG name="colby"

ENV USER=${name}
ENV ENVIRONMENT="dev"


# create user colby with home directory and /bin/bash as default terminal
# -m: Create the user's home directory (/home/colby)
# -s /bin/bash: Set the default shell to /bin/bash
RUN useradd -m -s /bin/bash colby

# Download Golang and give permissions to colby
RUN curl -LsO https://go.dev/dl/go1.24.4.linux-amd64.tar.gz \
 && tar -C /usr/local -xzf go1.24.4.linux-amd64.tar.gz \
 && rm go1.24.4.linux-amd64.tar.gz \
 && chown -R colby:colby /usr/local/go

# Add golang bin to path env
ENV PATH="${PATH}:/usr/local/go/bin"

WORKDIR /app
COPY --chown=colby:colby --chmod=755 ./main.go /app
RUN chown colby:colby /app
# COPY --chown=colby:colby ./main.go /app/
USER colby

CMD ["go", "run", "/app/main.go"]