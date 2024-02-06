# Stage 1: Build
FROM arm64v8/ubuntu:22.04
MAINTAINER Mitchell Black <6lack.mitchell@gmail.com>

# Set the environment variables to avoid interactive prompts during installation
ENV DEBIAN_FRONTEND=noninteractive
ENV PATH = "${PATH}:/home/poetry/bin"

# Update the package list and install necessary packages
RUN apt-get update && apt-get install -y \
    make \
    build-essential \
    libssl-dev \
    zlib1g-dev \
    libbz2-dev \
    libreadline-dev \
    libsqlite3-dev \
    wget \
    curl \
    llvm \
    libncurses5-dev \
    libncursesw5-dev \
    xz-utils \
    tk-dev \
    libffi-dev \
    liblzma-dev \
    git \
    gnupg \
    lsb-release \
    software-properties-common \
    gnome-keyring \
    libc6-dev \
    libgirepository1.0-dev

# Enable universe repo
RUN add-apt-repository universe

# Add the ROS2 Humble repository
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

# Update the package list again
RUN apt update
RUN apt upgrade -y

# Install ROS2 Humble
RUN apt install -y ros-humble-desktop
RUN apt install -y ros-dev-tools

# Initialize ROS2 Iron
RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc

# Install python 3.10 and set as default
RUN apt-get install -y python3.10 python3-pip python3-dev
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.10 1
RUN apt-get update

# install VS Code (code-server)
RUN curl -fsSL https://code-server.dev/install.sh | sh

# install VS Code extensions
RUN code-server --install-extension ms-python.python \
                --install-extension ms-toolsai.jupyter

ARG YOUR_ENV

ENV YOUR_ENV=${YOUR_ENV} \
  PYTHONFAULTHANDLER=1 \
  PYTHONUNBUFFERED=1 \
  PYTHONHASHSEED=random \
  PIP_NO_CACHE_DIR=off \
  PIP_DISABLE_PIP_VERSION_CHECK=on \
  PIP_DEFAULT_TIMEOUT=100 \
  # Poetry's configuration:
  POETRY_NO_INTERACTION=1 \
  POETRY_VIRTUALENVS_CREATE=false \
  POETRY_CACHE_DIR='/var/cache/pypoetry' \
  POETRY_HOME='/home/poetry' \
  POETRY_VERSION=1.7.1

# Install poetry
RUN mkdir -p /home/poetry && curl -sSL https://install.python-poetry.org | python3 -

# Clean up
RUN rm -rf /var/lib/apt/lists/*
RUN apt-get remove -y curl && \
    apt-get clean

# Expose the port that Jupyter Notebook will run on
EXPOSE 8888
