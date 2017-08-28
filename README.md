# android-sdk-docker

This docker image can be used to build Android Gradle projects with Java 7. The image is available on [DockerHub](https://hub.docker.com/r/lerk/android/)

Contains:

* Android SDK: full

## Build image

```bash
docker build -t lerk/android .
```

## Push build version to repository

```bash
docker push lerk/android
```

## Usage

### GitLab CI

This is what my .gitlab-ci.yml looks like:

```yaml
image: lerk/android

stages:
  - build

build:
  stage: build
  script:
    - ./gradlew build
  only:
    - master

```

### Without GitLab

```bash
docker pull lerk/android
```

Change directory to your project directory, then run:

```bash
docker run --tty --interactive --volume=$(pwd):/opt/workspace --workdir=/opt/workspace --rm lerk/android  /bin/sh -c "
echo y | android update sdk --no-ui --all --filter build-tools-23.0.1,android-23,extra-android-m2repository && echo y | android update sdk --no-ui --all --filter "extra-google-m2repository" && chmod 775 ./gradlew && ./gradlew test && ./gradlew clean assembleRelease"
```

