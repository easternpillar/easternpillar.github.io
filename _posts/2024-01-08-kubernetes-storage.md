---
title: 쿠버네티스 저장소
date: 2024-01-08
categories: [ DevOps ]
tags: [ devops, kubernetes, k8s, storage, volume ]
---

<br>

## 저장소 드라이버

> 이미지 및 컨테이너의 파일 시스템 레이어를 관리하기 위한 도구로 이미지 빌드, 레이어 캐싱, 데이터 Read/Write 최적화에 관여한다.

> __도커 저장소__: 도커를 사용하여 이미지를 생성할 때 사용하는 Dockerfile은 Layered Architecture로 이루어져 있으며 스크립트의 각 단계(Layer)가 호스트 파일시스템에 캐싱되어 동일한 스크립트를 사용하는 다른 이미지 생성에 재사용이 가능하다.
> 이렇게 빌드된 이미지는 Read-only의 불변성 특성을 가지며 이를 컨테이너로 실행 후 컨테이너 내에서 변경이 발생하면 Copy-on-Write 방식으로 동작한다. 
{: .prompt-info }

<br>

## 볼륨 드라이버

> 컨테이너 외부에서 데이터를 관리하고 영속적으로 저장하기 위한 도구로 컨테이너의 볼륨을 호스트 파일시스템에 마운트하고 영속화에 관여한다.

## CSI(Container Storage Interface)

> CNI, CRI와 같이 다양한 Storage 관련 플러그인 또는 기술간의 표준화된 방식을 제공하기 위한 표준 인터페이스를 말한다.

> 세가지 인터페이스 모두 RPC(Remote Procedure Call) 방식으로 동작한다.
{: .prompt-tip }

