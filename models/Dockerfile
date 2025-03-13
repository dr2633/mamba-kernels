FROM nvidia/cuda:12.2.2-devel-ubuntu22.04

# Set non-interactive build
ENV DEBIAN_FRONTEND=noninteractive

# Install Python and essential dependencies explicitly
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    python3 \
    python3-pip \
    python3-dev \
    python3-setuptools \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Explicitly set working directory
WORKDIR /workspace

# Upgrade pip explicitly
RUN python3 -m pip install --upgrade pip

# Explicitly copy requirements file (create this first!)
COPY requirements.txt /workspace/requirements.txt
RUN pip install -r requirements.txt

# Clone Mamba explicitly from source if needed
RUN git clone https://github.com/state-spaces/mamba.git external/mamba
RUN pip install -e external/mamba

# Copy your code explicitly
COPY . /workspace

# Explicitly set PYTHONPATH
ENV PYTHONPATH=/workspace:${PYTHONPATH}

CMD ["/bin/bash"]
