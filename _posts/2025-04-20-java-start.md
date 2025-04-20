---
title: [학습 노트] 김영한의 자바 입문 - 코드로 시작하는 자바 첫걸음
date: 2025-04-20
categories: [ Programming Language, Java ]
tags: [language, java, javac, jar, main, bytecode, class ]
---

<br>

## Java의 실행
- 코드 작성 및 작성된 코드 실행 과정
  1. java 소스코드(.java) 작성
  2. java 소스코드 컴파일: .class 바이트코드 결과물 생성
  3. 바이트코드 실행: JVM(Java Virtual Machine)이 바이트코드를 해석(Interpret)하여 실행
- 실행 방법
  1. javac 명령어: java 소스(.java)를 바이트 코드(.class)로 컴파일한다.
  2. java 명령어: 컴파일 결과물(바이트 코드 = .class)의 main 메서드를 찾아 최초 시작점으로 실행한다.

<br>

> main 메서드는 public static void main (String[] args) 형태이며 시그니처까지 일치해야 한다. (단, varargs 형태 String ... args 는 허용)
{: .prompt-warning }

<br>

> java 명령어의 경우, 컴파일된 바이트 코드 결과물의 확장자를 제외한 클래스명만 전달해야하며 해당 클래스의 FQCN(Fully-Qualified Class Name;클래스의 경로를 포함한 전체 이름)이어야 한다.
{: .prompt-warning }

<br>

> 11버전 이후부터 {main 메서드를 포함하는 java 소스 코드}.java 형태로 전달하면 자동으로 컴파일하여 실행한다.
{: .prompt-info }

<br>

> Jar(Java ARchive) 파일은 java 명령어에 -jar 옵션을 통해 실행 가능하며 이 경우, main 메서드를 찾기 위해 Jar 압축 파일 내 MANIFEST.MF 파일에 정의된 main 메서드 경로를 참조한다.
{: .prompt-tip }

<br>

## Java의 표준 스펙
> Java는 컴파일러, 실행 라이브러리, 가상 머신의 스펙이 규정되어 있으며 이를 활용하여 규격을 준수하는 다양한 자바 구현이 존재한다.

<br>

### Java의 운영체제 독립성
> "Write Once, Run Anywhere" 라는 슬로건에 맞게 Java는 운영체제와 독립적인 형태로 배포 가능하다.
> 컴파일된 바이트코드는 운영체제에 독립적이며 각 JVM 벤더는 Java 표준 스펙에 맞게 JVM을 운영체제별로 개발되어 네이티브 코드로 변환해 실행되므로 플랫폼 독립성이 실현된다.

<br>

## 클래스(Class)
> Java는 OOP(Object-Oriented Programming;객체지향 프로그래밍) 언어이므로 클래스를 통해 구현된다.
> 

