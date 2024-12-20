---
layout: post
title:  "Setup Java environment on macOS"
date:   2024-12-13 15:40:50 +0800
categories: macOS Java
---
## Setup Java environment on macOS

First install [SDK MAN](https://sdkman.io/install/)

Then install `java` and `gradle`

```shell
sdk install java 21.0.5-amzn
sdk install gradle 8.10.2
```

Then create gradle project

```shell
mkdir java-demos
cd java-demos

gradle init
```

Then create gradle wrapper
```shell
gradle wrapper
```

after then, we can use wrapper `./gradlew` to build project

## Run a file

```shell
./gradlew build 
./gradlew runClass -PclassName=io.petesong.leetcode.LeetCode2235
```

## Check

```shell
./gradlew check
```
Then you can see the reports after checking in the folder `./build/reports`

## Reference
[https://github.com/PeteSong/demos/tree/main/java-demos](https://github.com/PeteSong/demos/tree/main/java-demos)
