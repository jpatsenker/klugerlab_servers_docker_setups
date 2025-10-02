# Use NVIDIA CUDA base with Python
FROM nvidia/cuda:12.2.0-runtime-ubuntu22.04

# Arguments for user setup
ARG USERNAME
ARG UID=1000
ARG GID=1000

# Install Python + system deps
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 python3-pip python3-dev build-essential \
    git curl vim \
    && rm -rf /var/lib/apt/lists/*

# Make python3 default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1 \
    && update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1

# Create group and user with matching UID/GID
RUN groupadd -g ${GID} ${USERNAME} \
    && useradd -m -u ${UID} -g ${GID} -s /bin/bash ${USERNAME}

# Set working directory to /home/$USERNAME
WORKDIR /home/${USERNAME}

# Copy requirements and install as the user
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt \
    && pip install --no-cache-dir jupyterlab

# Make sure user owns home directory
RUN chown -R ${USERNAME}:${USERNAME} /home/${USERNAME}

# Switch to user
USER ${USERNAME}

# Expose JupyterLab port
EXPOSE 8888

# Run JupyterLab on container start
CMD ["jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--NotebookApp.token=''"]
