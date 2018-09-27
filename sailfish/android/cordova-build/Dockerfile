FROM newma/java:8

RUN apt-get update \
    && apt-get -y install git-all \
    && apt-get -y install curl \
    && apt-get -y install unzip

# set up android
ENV SDK_URL="https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip" \
    ANDROID_HOME="/usr/local/android-sdk" \
    ANDROID_VERSION=27 \
    ANDROID_BUILD_TOOLS_VERSION=27.0.3

RUN mkdir "$ANDROID_HOME" .android \
    && cd "$ANDROID_HOME" \
    && curl -o sdk.zip $SDK_URL \
    && unzip sdk.zip \
    && rm sdk.zip \
    && yes | $ANDROID_HOME/tools/bin/sdkmanager --licenses

RUN $ANDROID_HOME/tools/bin/sdkmanager --update
RUN $ANDROID_HOME/tools/bin/sdkmanager "build-tools;${ANDROID_BUILD_TOOLS_VERSION}" \
    "platforms;android-${ANDROID_VERSION}" \
    "platform-tools"

# install node npm and cordova
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g cordova \
    && cordova telemetry off

# install gradle
RUN cd "/opt" \
    && curl -o gradle-4.4-all.zip "https://downloads.gradle.org/distributions/gradle-4.4-all.zip" \
    && unzip gradle-4.4-all.zip

ENV GRADLE_HOME="/opt/gradle-4.4" \
    CORDOVA_ANDROID_GRADLE_DISTRIBUTION_URL="file:///opt/gradle-4.4-all.zip"
ENV PATH="${GRADLE_HOME}/bin:${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/platform-tools:${PATH}"

RUN mkdir /application
WORKDIR /application

CMD cordova -v
