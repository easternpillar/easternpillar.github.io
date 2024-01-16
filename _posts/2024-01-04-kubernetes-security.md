---
title: 쿠버네티스 보안
date: 2024-01-03
categories: [ DevOps ]
tags: [ devops, kubernetes, k8s, security, authentication, authorization ]
---

<br>

## 인증(Authentication)

- 인증 구현 방식
  - 정적 파일 기반: Password나 Token을 정적 파일로 관리하는 것으로 간단하지만 보안이 취약하므로 권장되지 않는다.
    - Password 파일 관리
    - Token 파일 관리
  - 인증서 기반: TLS 인증서를 이용한 인증
  - SA(Service Account) 기반
  - 기타 써드 파티 기반: OpenID, LDAP, Kerberos, 웹훅 등을 이용한 인증

> 자체서명된 CA 인증서 발급을 통해 상호 통신하며 ETCD와 그 밖의 구성요소 간에 별도 CA를 구축하기도 한다.
{: .prompt-info }

> 쿠버네티스는 CSR(CertificateSigningRequest) 오브젝트가 존재하며 이를 활용한 인증서 API를 제공한다. kube-controller-manager 내부에 인증서 API 관련 컨트롤러가 존재한다.
{: .prompt-tip }

> 클라이언트에서 KubeConfig 파일(위치: HOME/.kube/config)을 통해 미리 쿠버네티스 클러스터-네임스페이스 및 사용할 사용자(인증서 기반)를 사전에 정의해 놓을 수 있다. 이를 통해, kubectl을 통한 매 클러스터 접근마다 명령어 옵션값으로 인증서를 명시할 필요가 없도록 구성할 수 있다.
{: .prompt-tip }

> 클라이언트에서 kubectl을 사용하지 않고 REST API 호출시 KubeConfig 파일을 참조하지 않는다. 이 때, kubectl proxy 명령어를 통해 로컬 프록시 서버를 실행하면 이후 REST API에서 KubeConfig 파일을 참조한다.
{: .prompt-tip }

> 쿠버네티스는 private docker registry 인증을 위한 시크릿 발급과 해당 시크릿을 통한 파드의 이미지 풀링이 가능하다.
{: .prompt-info }

> 도커 데몬은 호스트의 root 권한으로 실행되며 컨테이너 프로세스의 권한을 관리하기 위해 linux capability 기능을 추가/제거하거나 실행할 사용자를 명시하여 컨테이너를 실행하기도 한다. 쿠버네티스에서도 securityContext 하위 필드를 통해 이를 지정할 수 있다. 
{: .prompt-tip }

> 파드간 통신은 기본적으로 동일 클러스터 내부라면 모두 허용되지만 애플리케이션 특성에 의해 Ingress/Egress 정책 필요시 NetworkPolicy 오브젝트를 지원하는 CNI(Container Network Interface) 솔루션과 labels, selector 또는 ipBlock으로 제어한다. Ingress 허용시 요청을 보낸 컨테이너로의 응답은 Egress 설정이 별도로 필요하지 않다.
{: .prompt-info }

## 인가(Authorization)

- 인가 구현 방식
  - 노드 기반: 쿠버네티스 구성요소간 통신에 사용되는 방식으로 TLS 인증을 기본으로 사용하며 인증서에 지정된 그룹을 통해 API 서버가 권한을 판단한다.
  - ABAC(Attribute-Based Access Control) 기반: 사용자, 리소스와 같은 작은 단위의 속성들마다 권한을 정의하는 방식
  - RBAC(Role-Based Access Control) 기반: 역할을 정의하고 역할에 권한을 부여하는 방식
  - 기타 써드 파티 기반: 써드 파티 애플리케이션에서 권한을 관리하는 방식
- 인가 모드: API 서버에서의 인가 방식들을 선택하기 위한 설정이며 기본값은 모든 요청을 허용하는 AlwaysAllow이다.

> API 서버 옵션으로 설정한 인가 모드는 명시된 순서대로 인가 검증이 이루어지며 가장 먼저 성공한 모드의 인가 방식으로 동작된다.
{: .prompt-info }
