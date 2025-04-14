# Copyright (C) 2025 Miguel Salas Heras

# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.

# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
# GNU General Public License for more details.

# You should have received a copy of the GNU General Public License
# along with this program. If not, see <https://www.gnu.org/licenses/>.

# Dockerfile responsible for creating the Docker image for the QoQo Quantum Cryptography simulation environment

# Image for building liboqs
FROM ubuntu AS builder

# For updating the system and being able to install packages (to update the path where the packages are downloaded)
RUN apt-get update -y && apt-get upgrade -y

# Install dependencies
RUN apt-get install -y git 

# Install the necessary dependencies to compile liboqs
RUN apt install -y \ 
    astyle \
    cmake \
    gcc \
    ninja-build \
    libssl-dev \
    python3-pytest \
    python3-pytest-xdist \
    unzip \
    xsltproc \
    doxygen \
    graphviz \
    python3-yaml \
    valgrind 

# Clone liboqs repository
RUN git clone -b main https://github.com/open-quantum-safe/liboqs.git

# Install liboqs, but in a way that allows the Python 3 wrapper to be used.
RUN cmake -S liboqs -B liboqs/build -DBUILD_SHARED_LIBS=ON
RUN cmake --build liboqs/build --parallel 8
RUN cmake --build liboqs/build --target install

# Final image without compilation files
FROM ubuntu AS final_image

# For updating the system and being able to install packages (to update the path where the packages are downloaded)
RUN apt-get update -y && apt-get upgrade -y

# Install necessary dependencies
RUN apt-get install -y git python3-pip

# To upgrade pip to the latest version
RUN python3 -m pip install --upgrade pip --break-system-packages --ignore-installed

# Copy the liboqs libraries installed in the previous stage
COPY --from=builder /usr/local/lib/liboqs.so* /usr/lib/

# So that the tno.quantum.communication.qkd_key_rate library works correctly
RUN pip3 install numpy==1.26.4 --break-system-packages

# Install the Python 3 wrapper for liboqs
RUN pip3 install git+https://github.com/open-quantum-safe/liboqs-python.git --break-system-packages

# Install qiskit-aer
RUN pip3 install qiskit-aer --break-system-packages

# Install qiskit-ibm-runtime
RUN pip3 install qiskit-ibm-runtime --break-system-packages

# Install tno.quantum.communication.qkd_key_rate
RUN pip3 install tno.quantum.communication.qkd_key_rate --break-system-packages

# Install Jupyter to work with notebooks 
RUN pip3 install jupyter --break-system-packages

# Export the OMP library in the LD_PRELOAD environment variable in .bashrc to avoid the OMP import error when using Qiskit
RUN echo 'export LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libgomp.so.1' >> ~/.bashrc

# Install cryptography
RUN pip3 install cryptography --break-system-packages

# Install Cryptomite 
# Clone the package repository from GitHub.
RUN git clone https://github.com/CQCL/cryptomite.git

# Set the working directory
WORKDIR /cryptomite

# Add a Python script to change the pybind11 version in cryptomite/CMakeLists.txt
RUN echo 'import requests\n\
import re\n\
r = requests.get("https://api.github.com/repos/pybind/pybind11/releases/latest")\n\
latest_version = r.json()["tag_name"]\n\
filename = "cryptomite/CMakeLists.txt"\n\
patron = re.compile(r"(URL\\s+https://github\\.com/pybind/pybind11/archive/refs/tags/)v[0-9]+\\.[0-9]+\\.[0-9]+(\\.tar\\.gz)")\n\
with open(filename, "r") as f:\n\
    content = f.read()\n\
new_content = patron.sub(rf"\\1{latest_version}\\2", content)\n\
with open(filename, "w") as f:\n\
    f.write(new_content)\n\
print(f"Version of pybind11 updated to {latest_version} in {filename}")' \
> update_version_pybind11_cmakelists_txt.py

# Execute Python Script
RUN python3 update_version_pybind11_cmakelists_txt.py

# Install the package
RUN pip3 install . --break-system-packages

# Clean up installation files
WORKDIR /
RUN rm -rf /cryptomite
