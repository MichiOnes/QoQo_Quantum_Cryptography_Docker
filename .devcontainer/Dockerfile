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

# Dockerfile encargado de Crear la imagen del Docker para el entorno de simulacion QoQo Quantum Cryptography Docker

# Imagen para hacer el build de liboqs
FROM ubuntu AS builder

# Para actualizar el sistema y poder instalar paquetes (para actualizar el path de donde se descargan los paquetes)
RUN apt-get update -y && apt-get upgrade -y

# Install dependencies
RUN apt-get install -y git 

# Instalar las dependencias necesarias para compilar liboqs
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

# Repositorio de liboqs
RUN git clone -b main https://github.com/open-quantum-safe/liboqs.git

# Instalar liboqs pero para que se pueda utilizar el warpper de python3
RUN cmake -S liboqs -B liboqs/build -DBUILD_SHARED_LIBS=ON
RUN cmake --build liboqs/build --parallel 8
RUN cmake --build liboqs/build --target install

# Imagen final sin archivos de compilación
FROM ubuntu AS final_image

# Para actualizar el sistema y poder instalar paquetes (para actualizar el path de donde se descargan los paquetes)
RUN apt-get update -y && apt-get upgrade -y

# Instlar dependencias necesarias
RUN apt-get install -y git python3-pip

# Copiar las librerias de liboqs y liboqs-python
COPY --from=builder /usr/local/lib/liboqs.so /usr/lib/
COPY --from=builder /usr/local/lib/liboqs.so.7 /usr/lib/
COPY --from=builder /usr/local/lib/liboqs.so.0.12.1-dev /usr/lib/

# Para que la librería de tno.quantum.communication.qkd_key_rate funcione correctamente
RUN pip3 install numpy==1.26.4 --break-system-packages

# Instalar wrapper de python3 de liboqs
RUN pip3 install git+https://github.com/open-quantum-safe/liboqs-python.git --break-system-packages

# Instalar qiskit-aer
RUN pip3 install qiskit-aer --break-system-packages

# Instalar qiskit-ibm-runtime
RUN pip3 install qiskit-ibm-runtime --break-system-packages

# Instalar tno.quantum.communication.qkd_key_rate
RUN pip3 install tno.quantum.communication.qkd_key_rate --break-system-packages

# Instalar cryptomite 
RUN pip3 install git+https://github.com/CQCL/cryptomite.git --break-system-packages

# Instalar jupyter para hacer cuadernillos 
RUN pip install jupyter --break-system-packages

# Exportar la librería de omp en la variable de entorno LD_PRELOAD en el bashrc y evitar el error de importar omp al utilizar qiskit
RUN echo 'export LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libgomp.so.1' >> ~/.bashrc

# Instalar la librería de cryptography
RUN pip install cryptography --break-system-packages
