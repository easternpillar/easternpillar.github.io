---
title: Java
date: 2025-04-20
categories: [ Programming Language, Java ]
tags: [language, java, javac, jar, main, bytecode, class, variable, literal, constantpool, jvm, error, exception, oop, object, solid ]
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

## 변수
> 값을 저장할 수 있는 메모리 공간에 대한 이름

<br>

### 변수의 선언 및 초기화
- 변수의 선언: 변수의 타입과 이름을 정의
- 변수의 초기화: 값을 설정
> Java는 초기화되지 않은 변수에 대한 참조를 컴파일 단계에서 확인한다.
{: .prompt-info }

<br>

### 자료형(Data Type)
> 자료형은 크게 기본형(Primitive)과 참조형(Reference)으로 나뉜다.

- 기본형: Java에서 기본으로 제공하는 자료형 ex) int, boolean 등
- 참조형: 문자열, 객체(인스턴스) 등 기본형 외의 자료형

> Java는 Copy-by-value를 기본으로 한다. 기본형인 경우 값 자체를 복사하여 전달하며 참조형인 경우 참조값(주소값)을 복사하여 전달한다.
{: .prompt-info }

<br>

> 기본형의 경우, 멤버변수에 속하면 자동 초기화된다.
{: .prompt-info }

<br>

### 리터럴(Literal)과 상수풀(Constant Pool)
- 리터럴: Java 코드 내에 직접적으로 명시된 값으로 컴파일 시점에 결정된다.

<br>

> 문자열 상수풀(String Constant Pool): JVM의 메모리 자원 효율을 위해, 문자열 리터럴 및 intern()을 통해 등록된 문자열을 JVM Heap 영역의 특별한 테이블에 저장하여 동일 문자열이 여러 번 사용될 경우에도 하나의 인스턴스를 재사용하도록 설계된 메모리 공간이다.
{: .prompt-info }

<br>

> 문자열 상수풀은 클래스/런타임 상수풀의 개념과 혼동되곤 한다. 클래스/런타임 상수 풀은 클래스 내 심볼(클래스 이름, 메서드, 필드, 리터럴 등)에 번호(인덱스)를 부여한 테이블을 바이트 코드 내에 구성하고 해당 테이블의 인덱스를 통해 심볼을 참조함으로써, 텍스트 기반 참조의 중복과 메모리 낭비를 방지하고 실행 효율성을 확보한다. 컴파일시 작성되는 이 참조 테이블을 클래스 상수풀이라고 하며, 실행시 JVM에 의해 Method 영역에 적재된 것을 런타임 상수풀이라고 한다.
{: .prompt-warning }

<br>

## 오류(Error)와 예외(Exception)
> Throwable 클래스의 하위 클래스이며 이를 통해 오류 및 예외를 제어한다.

### 오류(Error)
> Error는 JVM 자체에서 발생하는 심각한 시스템 수준의 문제를 나타내며, 일반적으로 개발자가 직접 처리하지 않고 JVM이 해당 오류를 던지면 프로그램은 비정상적으로 종료된다.

<br>

> 에러 객체는 예외처럼 처리할 수는 있지만, 복구 가능성이 매우 낮아 catch하지 않는 것이 일반적이다.
{: .prompt-info }

### 예외(Exception)
> Exception은 프로그램 실행 중에 발생할 수 있는 오류 상황을 객체로 표현한 것이며, 개발자가 이를 처리(복구)하거나 무시하거나 전달할 수 있도록 지원하는 자바의 구조적 메커니즘이다.

- Checked Exception: 컴파일러가 반드시 처리를 요구하는 예외 ex) IOException, SQLException, etc.
- Unchecked Exception: 컴파일러가 처리를 강제하지 않는 예외 ex) NullPointerException, IllegalArgumentException, etc.

<br>

## 클래스(Class)
> 객체 즉, 현실 세계의 개념이나 사물을 소프트웨어적으로 표현하는 틀을 생성하기 위한 설계도(blueprint)이며, 객체가 가져야 할 상태(필드)와 동작(메서드)을 정의한 코드 구조

### 생성자(Constructor)
> 클래스의 객체(인스턴스)를 생성하기 위한 메서드

<br>

- 정의된 생성자가 없다면 아무런 매개변수가 없는 기본생성자를 자동 생성한다.
- 여느 메서드와 동일하게 오버로딩 가능하다.

> 생성자 정의는 객체(인스턴스) 생성시 필요한 필수값을 강제할 수 있다.
{: .prompt-tip }

### 접근제어자(Access Modifier)
> 특정 필드 또는 메서드에 접근하는 것을 허용 또는 제한한다.

<br>

- public: 외부에서 접근 허용
- protected: 동일 패키지 내 또는 상속받은 클래스에 한해 접근 허용
- default: 동일 패키지 내 클래스에 한해 접근 허용
- private: 해당 클래스에서만 접근 가능

<br>

### 정적(static) 키워드
> 클래스 내 필드 또는 메서드에 붙여 클래스간 공통 멤버 변수 또는 기능 정의에 사용할 수 있다.

<br>

> static 메서드 내에서는 static 메서드나 static 변수 혹은 인자로 전달받은 변수 또는 객체의 값들만 사용 가능하다. 이는 메모리 구조를 알면 이해하기 쉬운데 static 은 클래스 로딩시 Method 영역에 적재되지만 일반적인 인스턴스 메서드/변수는 객체 생성시에 Heap 영역에 적재된다. 따라서, 아직 생성되지 않은 변수 또는 메서드에 접근할 수 없으며 인자로 전달받은 경우에는 기생성됨을 의미하기 때문에 사용 가능하다.
{: .prompt-info }


### final 키워드
> 필드에 선언시 값 변경 불가, 메서드에 선언시 오버라이딩 불가, 클래스에 선언시 상속 불가이다. 

## 객체 지향 프로그래밍(OOP;Object-Oriented Programming)
### 4대 핵심 개념
- 캡슐화(Encapsulation): 속성과 기능을 하나(객체)로 묶어서 필요한 기능을 메서드를 통해 외부에 제공하는 것을 말하며 접근 제어자를 통해 외부에 제공할 속성이나 기능을 제어한다.
- 다형성: 같은 타입(부모 클래스)로부터 다양한 동작(자식 클래스)를 표현 가능하다.
- 상속: 기존 클래스를 확장하여 속성과 기능을 물려받아 사용할 수 있다.
- 추상화: 복잡한 내부 구현을 숨기고, 필요한 기능만 외부에 공개할 수 있다.

> Java는 단일 상속만 허용하기 때문에 상속의 대상은 하나만 가능하다. 자식 인스턴스 생성시 과정은 다음과 같다.<br>
> 1. 클래스가 로딩되어 있지 않다면 Method 영역에 부모, 자식 클래스 로딩
> 2. Heap에 인스턴스 메모리 할당: 부모, 자식의 필드를 모두 포함할 수 있는 크기 할당
> 3. 생성된 인스턴스의 모든 필드값을 기본값으로 자동 초기화
> 4. 부모 클래스의 명시적 초기값으로 인스턴스 필드값 설정
> 5. 부모 클래스의 생성자 호출: 자식 클래스 인스턴스 생성시 호출한 생성자에 별도로 부모 클래스의 생성자 호출이 있으면 해당 생성자를 실행하고, 아니면 기본 생성자 자동 실행하여 필드값 설정
> 6. 자식 클래스의 명시적 초기값으로 인스턴스 필드값 설정
> 7. 자식 클래스의 생성자 호출: 호출한 생성자대로 인스턴스 필드값 설정
{: .prompt-info }

## 다형성
