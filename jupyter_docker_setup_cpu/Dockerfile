# Base image with Python and Jupyter
FROM python:3.11-slim

# Arguments for user setup
ARG USERNAME
ARG UID=1000
ARG GID=1000

# Set up system and JupyterLab
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    git \
    curl \
    vim \
    && rm -rf /var/lib/apt/lists/*

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
