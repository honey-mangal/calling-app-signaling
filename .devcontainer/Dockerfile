FROM ubuntu:25.04

# Install base packages + Java
RUN apt-get update && apt-get install -y \
    git curl unzip xz-utils zip libglu1-mesa openjdk-11-jdk qemu-kvm libvirt-clients libvirt-daemon-system && \
    rm -rf /var/lib/apt/lists/*

# Set Android SDK root and add to PATH
ENV ANDROID_SDK_ROOT=/opt/android/sdk
ENV PATH="$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools:$ANDROID_SDK_ROOT/emulator:${PATH}"

# Create SDK directories
RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools && \
    mkdir -p /root/.android && \
    touch /root/.android/repositories.cfg

# Download and install command-line tools
RUN curl -o sdk-tools.zip https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip && \
    unzip sdk-tools.zip -d ${ANDROID_SDK_ROOT}/cmdline-tools/latest && \
    rm sdk-tools.zip

# Accept licenses
RUN yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} --licenses

# Install required SDK packages: platform-tools, build-tools, emulator, system image
RUN sdkmanager --sdk_root=${ANDROID_SDK_ROOT} \
    "platform-tools" \
    "platforms;android-33" \
    "build-tools;33.0.2" \
    "emulator" \
    "system-images;android-33;google_apis;x86_64"

# Create an Android Virtual Device (AVD)
RUN echo no | avdmanager create avd \
    --name flutter_emulator \
    --package "system-images;android-33;google_apis;x86_64" \
    --device "pixel"

# Clone Flutter SDK
RUN git clone https://github.com/flutter/flutter.git /usr/local/flutter

# Add Flutter to PATH
ENV PATH="/usr/local/flutter/bin:/usr/local/flutter/bin/cache/dart-sdk/bin:${PATH}"

# Pre-warm Flutter and verify
RUN flutter doctor -v

CMD ["bash"]
