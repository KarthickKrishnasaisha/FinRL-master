FROM stablebaselines/rl-baselines3-zoo

WORKDIR /src

# Switch to root to install system packages
USER root

# Install pip for system python3
RUN apt-get update \
 && apt-get install -y --no-install-recommends python3-pip \
 && rm -rf /var/lib/apt/lists/*

# Switch back to default user
USER sb3

COPY requirements.txt .

RUN python3 -m pip install --upgrade pip \
 && python3 -m pip install -r requirements.txt -I

RUN python3 -m pip install jupyterlab

# Add Tini. Tini operates as a process subreaper for jupyter.
ENV TINI_VERSION=v0.6.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini

COPY . .

ENTRYPOINT ["/usr/bin/tini", "--"]

CMD ["jupyter", "lab", "--port=8888", "--no-browser", "--ip=0.0.0.0", "--NotebookApp.token=''","--NotebookApp.password=''","--allow-root"]
