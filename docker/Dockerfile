FROM stablebaselines/rl-baselines3-zoo

WORKDIR /src

COPY requirements.txt .

# Use python -m pip because pip is not on PATH in this image (conda-based)
RUN python -m pip install --upgrade pip \
 && python -m pip install -r requirements.txt -I

RUN python -m pip install jupyterlab

# Add Tini. Tini operates as a process subreaper for jupyter.
ENV TINI_VERSION=v0.6.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini

# Copy the code instead of mounting
COPY . .

ENTRYPOINT ["/usr/bin/tini", "--"]

CMD ["jupyter", "lab", "--port=8888", "--no-browser", "--ip=0.0.0.0", "--NotebookApp.token=''","--NotebookApp.password=''","--allow-root"]
