FROM uprev/base:ubuntu-18.04
LABEL org.opencontainers.image.source https://gitlab.com/uprev-public/devcontainers/nrf5.git

# Download tools and prerequisites
RUN apt-get update && apt-get install -y --no-install-recommends \ 
    git \
    unzip \
    bzip2 \
    gcc-multilib \
    srecord \
    pkg-config \
    libusb-1.0.0

# # Download and install ARM toolchain matching the SDK
RUN curl -SL https://armkeil.blob.core.windows.net/developer/Files/downloads/gnu-rm/9-2019q4/gcc-arm-none-eabi-9-2019-q4-major-x86_64-linux.tar.bz2 > /tmp/gcc-arm-none-eabi-9-2019-q4-major-linux.tar.bz2 && \
tar xvjf /tmp/gcc-arm-none-eabi-9-2019-q4-major-linux.tar.bz2 -C /usr/share && \
rm /tmp/gcc-arm-none-eabi-9-2019-q4-major-linux.tar.bz2

# Download NRF5 SDK v17.0.2 and extract nRF5 SDK to /nrf5/nRF5_SDK_17.0.2
RUN curl -SL https://developer.nordicsemi.com/nRF5_SDK/nRF5_SDK_v17.x.x/nRF5_SDK_17.0.2_d674dde.zip > /tmp/SDK_17.0.2.zip && \
mkdir -p /nrf && \
unzip -q /tmp/SDK_17.0.2.zip -d /nrf/ && \
mv /nrf/nRF5_SDK_17.0.2_d674dde /nrf/nRF5_SDK_17.0.2 && \
rm /tmp/SDK_17.0.2.zip

# Patch around what is likely to be an oversight in Nordic's SDK
# https://devzone.nordicsemi.com/f/nordic-q-a/68352/gcc-toolchain-version-for-sdk-17-0-2-on-posix
RUN \
echo "GNU_INSTALL_ROOT ?= /usr/share/gcc-arm-none-eabi-9-2019-q4-major/bin/" > /nrf/nRF5_SDK_17.0.2/components/toolchain/gcc/Makefile.posix && \
echo "GNU_VERSION ?= 9.2.1" >> /nrf/nRF5_SDK_17.0.2/components/toolchain/gcc/Makefile.posix && \
echo "GNU_PREFIX ?= arm-none-eabi" >> /nrf/nRF5_SDK_17.0.2/components/toolchain/gcc/Makefile.posix



# Install nRF Tools (makes it easy to build a DFU package) And mrtutils for mrt-ble
RUN pip3 install -U pip
RUN pip3 install nrfutil 

ENV NRF_SDK_PATH /nrf/nRF5_SDK_17.0.2 
ENV PATH="/usr/share/gcc-arm-none-eabi-9-2019-q4-major/bin:${PATH}"

# set gdbinit auto-load 
RUN echo "set auto-load safe-path /" >> /home/dev/.gdbinit


