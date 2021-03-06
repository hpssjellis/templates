FROM cloud9/workspace
# Android Build Dockerfile for Parafuzo.
#
# This is a fork from:
# https://github.com/lukin0110/docker-android-build
MAINTAINER Jeremy Ellis <keyfreemusic@gmail.com>

# -------------------------- Transfer from Cloud9 Github to Docker -----------------------------------------
# The Github is at https://github.com/c9/templates/tree/master/ws-android

ADD ./files /home/ubuntu
ADD ./files/profile.d-android.sh /etc/profile.d/android.sh





# -------------------------- Install necessary base packages ----------------------------------------- 

RUN chown -R ubuntu:ubuntu /home/ubuntu && \
    apt-get update && \
    apt-get -y install curl unzip python-software-properties software-properties-common openjdk-7-jdk lib32stdc++6 lib32z1


# -------------------------- Install the Android SDK -----------------------------------------

# refer to https://developer.android.com/sdk/index.html#Other

RUN SDK_VERSION=r24.3.3 && \
    set -x && \
    cd /usr/local/ && \
    curl -L -o android-sdk.tgz http://dl.google.com/android/android-sdk_${SDK_VERSION}-linux.tgz && \
    tar xf android-sdk.tgz && \
    rm android-sdk.tgz && \
    chmod -R +r /usr/local/android-sdk-* && \
    bash -c 'for file in $(find /usr/local/android-sdk-*); do [[ -x $file ]] && chmod +x "$file"; done'; \
    exit 0


# -------------------------- Install the Android Platforms -----------------------------------------

# This fancy "for" loop solves an install issue
# build-tools-23.0.2   worked
RUN for filter in tools platform-tools android-20 android-22 android-23 extra build-tools-23.0.3; do \
        echo y | /usr/local/android-sdk-linux/tools/android update sdk --filter $filter --no-ui --force --all; \
    done



# -------------------------- Install Gradle -----------------------------------------

ENV GRADLE_VERSION 2.9

RUN cd /usr/local && \
    curl -L https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip -o gradle-bin.zip && \
    unzip gradle-bin.zip && \
    rm gradle-bin.zip  && \
    chmod -R +r /usr/local/gradle-${GRADLE_VERSION} && \
    bash -c 'for file in $(find /usr/local/gradle-${GRADLE_VERSION}); do [[ -x $file ]] && chmod +x "$file"; done'; \
    exit 0

#Gradle Home and Path for pre-cloud9 testing
ENV GRADLE_HOME /usr/local/gradle-${GRADLE_VERSION}
ENV PATH $PATH:$GRADLE_HOME/bin


# activate gradle by running it once    
RUN gradle


# -------------------------- Final Cloud 9 Installation Actions -----------------------------------------
#set ownership, a few symlinks 
RUN chmod 775 /home/ubuntu/start-here && \
    cd /home/ubuntu/workspace && \
    ln -s /usr/local/android-sdk-linux /home/ubuntu/workspace/z-android-SDK-link && \
    ln -s /usr/lib/jvm/java-7-openjdk-amd64 /home/ubuntu/workspace/z-openjdk-link && \
    ln -s /usr/local/gradle-${GRADLE_VERSION} /home/ubuntu/workspace/z-Gradle-link && \
    ln -s /home/ubuntu/start-here /home/ubuntu/workspace/a-start-here-link  && \
    chown -R ubuntu:ubuntu /home/ubuntu && \
    chown -R ubuntu:ubuntu /usr/local/android-sdk-linux && \
    chown -R ubuntu:ubuntu /usr/local/gradle-${GRADLE_VERSION} && \
    chown -R ubuntu:ubuntu /usr/lib/jvm/java-7-openjdk-amd64 
    
