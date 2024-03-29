# Stage 1: Build
FROM ubuntu:20.04
MAINTAINER Mitchell Black <6lack.mitchell@gmail.com>

# Set the environment variables to avoid interactive prompts during installation
ENV DEBIAN_FRONTEND=noninteractive

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

# Add the ROS Noetic repository
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros1-latest.list'
RUN curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add -

# Update the package list again
RUN apt update
RUN apt upgrade -y

# Install ROS Noetic
RUN apt-get install -y ros-noetic-desktop
RUN apt install -y ros-dev-tools

# Initialize ROS Noetic
RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc

# Install python 3.8 and set as default
RUN apt-get install -y python3.8 python3-pip python3-dev
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1
RUN apt-get update

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
  POETRY_VIRTUALENVS_CREATE=1 \
  POETRY_CACHE_DIR='/var/cache/pypoetry' \
  POETRY_HOME='/usr/local' \
  POETRY_VERSION=1.7.1

# Install poetry
RUN curl -sSL https://install.python-poetry.org | python3 -

# Clean up
RUN rm -rf /var/lib/apt/lists/*

# Expose the port that Jupyter Notebook will run on
EXPOSE 8888
