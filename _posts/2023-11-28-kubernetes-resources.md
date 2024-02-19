---
title: 쿠버네티스 리소스
date: 2023-11-28
categories: [ DevOps, k8s(kubernetes) ]
tags: [ kubernetes, k8s, devops ]
---

> 참조: 쿠버네티스 어나더 클래스(일프로)
{: .prompt-info }

![kubernetes-resources](/assets/img/posts/kubernetes-resources.png)

<br>

## 쿠버네티스 컨트롤러

> 타 컨트롤러나 오브젝트를 제어하는 리소스로 오브젝트 관리를 자동화하는 목적을 띈다.

> 쿠버네티스에서 공식적으로 제공하는 컨트롤러 외에 추가 기능을 제공하기 위한 커스텀 컨트롤러가 존재할 수 있는데 이를 Operator라고 부른다.
{: .prompt-info }

<br>

### HPA(Horizontal Pod Autoscaler)

![kubernetes-resources-hpa](/assets/img/posts/kubernetes-resources-hpa.png)

> 리소스 사용량을 포함한 부하량에 따라 파드의 개수를 자동으로 조정하기 위한 컨트롤러

<br>

### Deployment

![kubernetes-resources-deployment](/assets/img/posts/kubernetes-resources-deployment.png)

> 파드의 배포를 관리하기 위한 컨트롤러로 replicas 속성에 의해 ReplicaSet 오브젝트가 자동 생성되며 배포 전략을 설정할 수 있다.

![kubernetes-resources-deployment-strategies](/assets/img/posts/kubernetes-resources-deployment-strategies.png)

- 배포 방식: template 속성의 변경이 감지되면 ReplicaSet 오브젝트가 업데이트된다.
  - Recreate: 서비스의 점진적 배포와 관계없이 동시다발적으로 이전 서비스가 중단되고 새로운 서비스가 배포된다.
  - Rolling Update: 애플리케이션을 점진적으로 배포하는 방법으로 오브젝트 속성에서 직접 설정할 수 있고 **기본값** 전략이다.
  - Canary Update: 새 버전을 특정 사용자에게 먼저 배포하여 안정성을 검증하기 위한 방법으로 Ingress 설정이나 서비스 메쉬를 통해 트래픽을 리디렉션하여 구현한다.
  - Blue-green Update: 새로운 버전의 검증을 목적으로 사용하며 롤백이 굉장히 빠르다는 장점을 가진다. 이전 버전(Blue)과 새로운 버전(Green)을 공존한 상태로 두어 새로운 버전을 검증하는 방식으로 버전마다 별도의 Deployment로 만들어 구현한다.

> Deployment의 배포는 Rollout 발생 시 Revision을 통해 배포의 히스토리가 관리되기 때문에 롤백이 용이하다.
{: .prompt-info }
  
<br>

### ReplicaSet

> 지정된 수의 Pod 복제본이 항상 실행되도록 하기 위한 컨트롤러

> StatefulSet: 기본적인 기능은 ReplicaSet과 동일하나 상태 유지를 필요로 하는 애플리케이션에 특화되어 고정된 네트워크와 저장소 환경을 구축하기 위해 사용된다.
{: .prompt-info }

<br>

### DaemonSet

> 노드마다 하나씩 존재하며 백그라운드에서 실행되는 파드를 관리하는 컨트롤러

> 모든 노드에서 실행되므로 kube-scheduler에 의해 스케줄링되지 않는다.
{: .prompt-tip }

<br>

### Ingress

> Ingress 오브젝트의 설정을 적용하여 리버스 프록시 서버 또는 로드밸런서의 기능을 하는 컨트롤러

> nginx와 같은 리버스 프록시 서버 또는 로드 밸런서 도구의 Deployment 형태이며 Ingress 오브젝트의 설정을 구현하기 위해 반드시 필요하다.
{: .prompt-warning }

> Ingress 컨트롤러를 외부에 개방하기 위한 Service 리소스 생성도 필요하다.
{: .prompt-tip }

<br>

## 쿠버네티스 오브젝트
> 하나의 인프라 개념으로 단독 기능을 하는 리소스

<br>

### Pod

> 컨테이너 실행의 기본 단위로 컨테이너들의 집합이다. ReplicaSet에 의해 Pod의 복사본이 생성된다.

![kubernetes-resources-probe](/assets/img/posts/kubernetes-resources-probe.png)

- Probe: Pod의 상태를 점검하기 위한 설정으로 HTTP API 성공 여부, TCP 소켓 연결 여부, 컨테이너 내 특정 명령어 성공 여부를 통해 성패를 결정 지을 수 있다. 
  - startupProbe: Pod 내 서비스가 정상적으로 초기화되었는지 확인하는 설정이며 startupProbe가 성공해야만 readinessProbe와 livenessProbe 점검이 가능하다.
  - readinessProbe: 시스템 초기화뿐만 아니라 개발자가 의도한 설정이 정상적으로 초기화되었는지 확인하는 설정
  - livenessProbe: 시스템이 정상 동작중인지 확인하는 설정

> startupProbe와 readinessProbe는 성공할때까지 Service 리소스의 바인딩을 하지 않기 때문에 트래픽을 받을 수 없다.
{: .prompt-info }

> 파드 내에서 주 컨테이너(메인 애플리케이션)을 보조하는 역할을 하는 추가적인 컨테이너를 구성하는 경우가 있는데 이 컨테이너를 Sidecar라고 부른다.
{: .prompt-info }

<br>

### ConfigMap

> 설정 정보와 환경변수를 저장하기 위한 오브젝트

- 일반적인 리소스가 apiVersion, kind, metadata, spec으로 구성된 것과는 달리 spec 대신 data가 존재한다.

<br>

### Secret

> 민감 정보를 저장하기 위한 오브젝트로 사전에 정의된 type도 존재한다.

- `ConfigMap`과 동일한 포맷을 가진다.

> 파드 생성 시 Secret 정보가 필요한 경우 kubelet이 마스터 노드의 etcd에 정보를 요청하고 이를 파드가 실행되는 노드의 tmpfs에 저장한다. tmpfs는 리눅스 커널이 기본적으로 제공하는 메모리기반 파일시스템이며 이를 통해 민감 데이터를 보다 안전하게 저장할 수 있다.
{: .prompt-info }

> Secret의 값은 암호화 없이 Base64 인코딩되어 저장된다.
> 따라서, 리소스로 보안을 위해 ETCD에 암호화하여 저장하거나 SealedSecret 또는 Vault와 같은 써드파티 도구를 사용하는 등 암호화를 진행하는 것을 권장한다.
{: .prompt-danger }


<br>

### Namespace

> 리소스를 분리하기 위한 클러스터 수준 오브젝트

<br>

![kubernetes-resources-pv-pvc](/assets/img/posts/kubernetes-resources-pv-pvc.png)

### PV(Persistent Volume)
> **클러스터 수준**의 스토리지 볼륨 오브젝트

> local 과 nodeAffinity 속성 또는 Pod 오브젝트의 hostPath 속성을 통해 노드를 영구 스토리지처럼 사용할 수 있다.
> 해당 방법은 로깅과 같이 Pod가 노드의 정보를 조회할 수 있도록 하기 위해 존재하는 속성이고 노드 또는 Pod의 비정상 동작으로 인한 재가동은 데이터의 가용성에 치명적이므로 외부 스토리지 볼륨을 사용하는 것이 권장된다.
{: .prompt-danger }

- 접근 모드: PV가 허용하는 접근 모드를 accessModes 필드를 통해 설정한다.
  - ReadWriteOnce: 볼륨을 하나의 노드에서 읽기/쓰기 모드로 마운트할 수 있으며 단일 노드인 경우 적합하다.
  - ReadOnlyMany: 볼륨을 여러 노드에서 읽기 전용 모드로 마운트 수 있으며 여러 파드가 동시에 데이터를 읽을 수 있지만 아무도 변경할 수 없다.
  - ReadWriteMany: 볼륨을 여러 노드에서 읽고 쓸 수 있다.
- PV 재요청 정책: PVC 오브젝트의 persistentVolumeReclaimPolicy 필드를 통해 설정할 수 있다.
  - Retain: 바인딩된 PVC가 제거되더라도 PV는 남아있다.
  - Delete: PVC가 제거되면 PV도 사라진다.

<br>

### PVC(Persistent Volume Claim)

> PV에 바인딩하기 위한 사용하기 위한 네임스페이스 수준의 오브젝트

- 프로비저닝 방식
  - 정적 프로비저닝: 관리자에 의해 수동으로 PV와 PVC를 생성하고 바인딩하는 방식
  - 동적 프로비저닝: PVC 생성시 자동으로 필요한 PV를 동적 생성하여 바인딩하는 방식
- 바인딩 방식
  - 명시적 바인딩: PVC에 바인딩할 PV를 명시적으로 지정하여 바인딩하는 방식
  - 셀렉터 바인딩: 라벨과 셀렉터를 통해 특정 라벨을 가진 PV에만 바인딩하는 방식
- 바인딩 모드: volumeBindingMode 필드에 의해 결정된다.
  - Immediate: PVC 생성 즉시 바인딩 시도 **(기본값)**
  - WaitForFirstConsumer: 파드가 스케줄링된 후 바인딩 시도

<br>

### StorageClass

> PVC에 설정한 PV 스토리지 유형을 동적으로 프로비저닝하여 해당 스토리지가 실제 생성되도록 정의하기 위한 오브젝트

> 다양한 스토리지 솔루션을 제공하며 솔루션마다 API 인자가 상이할 수 있으므로 문서를 참조해야 한다.
{: .prompt-tip }

<br>

### Service

![kubernetes-resources-service](/assets/img/posts/kubernetes-resources-service.png)

> Pod의 네트워크 접근을 제공하는 오브젝트로 서비스 레지스트리의 기능과 로드밸런싱 기능을 가진다.

- 트래픽 개방 방법: type에 방식을 명시한다. 기본값은 ClusterIP이다.
  - ClusterIP: 클러스터 내부에서만 통신이 가능하다.
  - NodePort: ports 속성의 하위 속성인 nodePort에 포트 번호를, targetPort에 접근할 Pod의 포트 번호를 지정하면 노드와 Pod간 포워딩이 가능하다.
  - LoadBalancer: 별도의 외부 로드 밸런서를 제공하는 클라우드(AWS, Azure, GCP 등) 환경을 고려하여, 해당 로드 밸런서를 클러스터의 서비스로 프로비저닝한다.

> Pod는 기본적으로 생성 시마다 동적으로 새로운 IP를 할당받으므로 Service를 통해 접근되어야 한다.
{: .prompt-info}

> targetPort의 값은 실제 접근할 Pod의 포트 번호가 변경되면 같이 변경되어야 하는 불편함이 있을 수 있다. 이 때, Pod 오브젝트의 속성인 containers의 ports의 하위 속성으로 name과 containerPort에 이름과 포트번호를 지정한 후 targetPort 값에 해당 name 속성의 값을 지정하면 포트번호가 매핑되므로 Pod의 containerPort만 변경하면 되도록 구성할 수 있다.
{: .prompt-tip }

> **서비스 디스커버리:** 서비스에 VIP(Virtual IP)를 선언하고 이 VIP(=ClusterIP 모드의 IP)를 이용한 네트워크 정책이 설정된다.
> 쿠버네티스 DNS는 kube-system 네임스페이스에서 Deployment에 의해 생성된 파드 형태로 존재하며 특정 쿠버네티스 네임스페이스에 국한되지 않고 클러스터 수준으로 존재한다.
> 때문에 단일 접근점을 위한 Service 리소스가 기본적으로 생성되며 도메인명은 기본적으로 `{서비스명}.{네임스페이스명}.svc.{클러스터 도메인}` 
> 또는 `<파드IP 대시(-)로 구분>.<네임스페이스>.pod.<클러스터 도메인>` 형태이다.
{: .prompt-tip }

> 외부에 IP를 개방하지 않는 경우 ClusterIP를 사용하는 것이 적절하며, 외부에 별도의 강력한 로드밸런싱 기능을 필요로 하지 않는 노출이 필요한 경우 Ingress와 NodePort를 사용할 수도 있다. 강력한 로드밸런싱이 필요한 대규모 서비스의 경우 LoadBalancer가 적절하다.
> 이 때, LoadBalancer의 경우에는 클라우드 플랫폼이 제공하는 로드밸런싱 기능을 통해 포트 번호 없이 특정 도메인 접근을 애플리케이션까지 포워딩 해주지만 NodePort는 nginx와 같은 별도 리버스 프록시 서버나 로드밸런서,Ingress 설정 등을 구축해야 외부 사용자가 순수 도메인만으로 접근가능하도록 구성할 수 있다.
{: .prompt-tip }

<br>

### Ingress

> 외부 트래픽을 특정 서비스 리소스로 라우팅하기 위한 규칙을 정의하는 오브젝트

- 주요 기능: 외부 접근에 관한 트래픽 제어
  - 호스트 기반 라우팅
  - 경로 기반 라우팅
  - TLS 지원
  - 로드 밸런싱

<br>


### PriorityClass

> 스케줄링 큐에서 파드의 스케줄링 우선순위를 정의하는 리소스이다.

<br>

### KubeSchedulerConfiguration

> kube-scheduler 설정을 정의하고 구성하는 리소스

- Profile을 통해 스케줄러들을 그룹화하여 관리할 수 있다.
- 각 Profile에 따라 스케줄러의 각 과정에서의 플러그인 사용 여부를 결정 지을 수 있다.

### Role과 RoleBinding

> RBAC 인가에서 사용되며 Role 오브젝트에 역할이 가질 API 그룹, 그룹 내 리소스, 리소스의 Verb 권한을 명시하고 RoleBinding을 통해 사용자에게 Role을 부여한다. 

> Role과 RoleBinding은 특정 네임스페이스 수준에서 사용된다. 클러스터 수준에서의 RBAC는 `ClusterRole`과 `ClusterRoleBinding` 오브젝트를 통해 구현한다.
{: .prompt-info }

<br>

### ServiceAccount

> 사용자(사람)가 아닌 애플리케이션(머신)이 쿠버네티스 API를 사용하기 위한 인증 수단이다.

> 클러스터 외부에서 해당 API를 접근하기 위해서는 수동으로 토큰을 전달해야하지만 클러스터 내부 애플리케이션은 자동으로 토큰을 볼륨 마운트하여 사용할 수 있다.
{: .prompt-info }
> 자동으로 생성되는 Secret 오브젝트의 Token은 ServiceAccount가 존재하는한 계속 유효한 상태이므로 보안에 취약할 수 있다. 때문에, 쿠버네티스 v1.24 부터는 토큰을 자동 생성하지 않고 토큰 발급 API를 통해 발급하며 토큰의 만료기간을 포함하여 필요에 따라 세부 설정된 토큰을 발급하게 된다.
{: .prompt-warning }

### NetworkPolicy

> 컨테이너의 Ingress/Egress 네트워크 정책을 제어하기 위한 오브젝트

<br>

## 기타 리소스

<br>

### Event

> 클러스터의 작업 및 동작과 관련된 정보가 기록된 리소스이다.

<br>

## labels, selector, naming

> 쿠버네티스 오브젝트들은 labels와 selector를 통해 상호 매핑이 이루어진다.

![kubernetes-resources-naming-example](/assets/img/posts/kubernetes-resources-naming-example.png)
![kubernetes-resources-labels-and-selector](/assets/img/posts/kubernetes-resouces-labels-and-selector.png)

> 파드가 특정 레이블을 가진 노드 그룹에 할당되도록 하기 위해서는 노드에 label을 지정하고 파드 spec의 `nodeSelector` 필드에 해당 노드의 label을 지정하면 된다. 다만, 보다 복잡한 표현식으로 노드를 선택할 수 없으며 이를 가능케 하려면 파드의 `NodeAffinity`를 통해 설정한다.
{: .prompt-tip }
