---
title: Classpath와 Gradle 의존성
date: 2023-08-25
categories: [ Spring ]
tags: [ spring, classpath, gradle, dependency ]
---

## Classpath
> 사용자 정의 클래스 및 API 패키지의 경로를 지정하는 자바가상머신 또는 자바 컴파일러의 매개 변수

<br>


### 종류
1. 컴파일 Classpath: 컴파일 시점에만 사용되는 Classpath
2. 런타임 Classpath: 런타임 시점에만 사용되는 Classpath

## Gradle 의존성
> JVM 기반 애플리케이션은 실행에 필요한 의존성들의 위치를 알아야 리소스를 사용하여 동작할 수 있다. 따라서, 적절한 Classpath 관리가 필요하다.

<br>

### Dependency 종류
1. compileOnly: 컴파일 시점에만 사용 가능
2. runtimeOnly: 런타임 시점에만 사용 가능
3. implementation: 컴파일, 런타임 시점 모두 사용 가능
4. api: implementation과 동일하게 컴파일, 런타임 시점 모두 사용 가능하며 해당 의존성이 의존하는 내부 의존성도 사용할 수 있다.

> api 사용시 의존성의 내부 의존성이 가진 리소스에 접근하여 변경이 가능하기 때문에 의도치 않은 결합(Coupling), 호환성, 빌드 시간 증가 등의 문제가 발생할 수 있다.
{: .prompt-warning }
