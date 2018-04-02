---
title: Docker 构建 Android 应用 - Docker镜像建立
category:
- Android
- Docker
tags:
- Android
- 工具
---

```Dockerfile
# 从openjdk8 基础上构建
FROM openjdk:8

# 设置各种变量
ENV SDK_URL="https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip" \
    ANDROID_HOME="/usr/local/android-sdk" \
    ANDROID_VERSION=26 \
    ANDROID_BUILD_TOOLS_VERSION=26.0.2 \
    GRADLE_URL="https://services.gradle.org/distributions/gradle-4.1-all.zip" \
    GRADLE_HOME="/usr/local/gradle"


# Download Android SDK
RUN mkdir "$ANDROID_HOME" .android "$GRADLE_HOME"\
    && cd "$ANDROID_HOME" \
    && curl -o sdk.zip $SDK_URL \
    && unzip sdk.zip \
    && rm sdk.zip \
    && yes | $ANDROID_HOME/tools/bin/sdkmanager --licenses

# Install Android Build Tool and Libraries
RUN $ANDROID_HOME/tools/bin/sdkmanager --update
RUN $ANDROID_HOME/tools/bin/sdkmanager "build-tools;${ANDROID_BUILD_TOOLS_VERSION}" \
    "platforms;android-${ANDROID_VERSION}" \
    "platform-tools"

# gradle 下载并配置
RUN cd "$GRADLE_HOME" \
    && wget -O gradle.zip $GRADLE_URL \
    && unzip gradle.zip \
    && rm gradle.zip

ENV PATH="$GRADLE_HOME/bin:$ANDROID_HOME/tools/bin:$PATH"

# Use machine cache gradle
VOLUME /root/.gradle

# 建立工作目录用于挂载外部卷指向APP源码构建
RUN mkdir /application
WORKDIR /application


```
