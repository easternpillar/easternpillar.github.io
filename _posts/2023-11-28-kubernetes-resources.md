---
title: 쿠버네티스 리소스
date: 2023-11-28
categories: [ DevOps ]
tags: [ kubernetes, k8s, devops ]
---

> 참조: 쿠버네티스 어나더 클래스(일프로)
{: .prompt-info }

![kubermentes-resources](/assets/img/posts/kubernetes-resources.png)

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

> 파드의 배포를 관리하기 위한 컨트롤러로 replicas 속성에 의해 ReplicaSet 오브젝트가 자동 생성되며 배포 방식을 정의할 수 있다.

![kubernetes-resources-deployment-strategies](/assets/img/posts/kubernetes-resources-deployment-strategies.png)

- 배포 방식: template 속성의 변경이 감지되면 ReplicaSet 오브젝트가 업데이트된다.
  - Recreate: 서비스의 점진적 배포와 관계없이 동시다발적으로 이전 서비스가 중단되고 새로운 서비스가 배포된다.
  - Rolling Update: 애플리케이션을 점진적으로 배포하는 방법으로 오브젝트 속성에서 직접 설정할 수 있다.
  - Canary Update: 새 버전을 특정 사용자에게 먼저 배포하여 안정성을 검증하기 위한 방법으로 Ingress 설정이나 서비스 메쉬를 통해 트래픽을 리디렉션하여 구현한다.
  - Blue-green Update: 새로운 버전의 검증을 목적으로 사용하며 롤백이 굉장히 빠르다는 장점을 가진다. 이전 버전(Blue)과 새로운 버전(Green)을 공존한 상태로 두어 새로운 버전을 검증하는 방식으로 버전마다 별도의 Deployment로 만들어 구현한다.

<br>

### ReplicaSet

> 지정된 수의 Pod 복제본이 항상 실행되도록 하기 위한 컨트롤러

> SatefulSet: 기본적인 기능은 ReplicaSet과 동잃하나 상태 유지를 필요로 하는 애플리케이션에 특화되어 고정된 네트워크와 저장소 환경을 구축하기 위해 사용된다.
{: .prompt-info }

<br>

### DaemonSet

> 노드마다 하나씩 존재하며 백그라운드에서 실행되는 파드를 관리하는 컨트롤러

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

<br>

### Secret

> 민감 정보를 저장하기 위한 오브젝트로 사전에 정의된 type도 존재한다.

> Secret의 값은 암호화 없이 Base64 인코딩되어 저장된다.
> 따라서, 리소스로 보안을 위해 ETCD에 암호화하여 저장하거나 SealedSecret 또는 Vault와 같은 써드파티 도구를 사용하는 등 암호화를 진행하는 것을 권장한다.
{: .prompt-warning }

<br>

### Namespace

> 리소스를 분리하기 위한 클러스터 수준 오브젝트

<br>

![kubernetes-resources-pv-pvc](/assets/img/posts/kubernetes-resources-pv-pvc.png)

### PV(Persistent Volume)
> **클러스터 수준**의 스토리지 볼륨 오브젝트

> local 과 nodeAffinity 속성 또는 Pod 오브젝트의 hostPath 속성을 통해 노드를 영구 스토리지처럼 사용할 수 있다.
> 해당 방법은 로깅과 같이 Pod가 노드의 정보를 조회할 수 있도록 하기 위해 존재하는 속성이고 노드 또는 Pod의 비정상 동작으로 인한 재가동은 데이터의 가용성에 치명적이므로 외부 스토리지 볼륨을 사용하는 것이 권장된다.
{: .prompt-warning }

<br>

### PVC(Persistent Volume Claim)

> PV를 사용하기 위한 요청 오브젝트

<br>

### Service

![kubernetes-resources-service](/assets/img/posts/kubernetes-resources-service.png)

> Pod의 네트워크 접근을 제공하는 오브젝트로 서비스 레지스트리의 기능과 로드밸런싱 기능을 가진다.

- 트래픽 개방 방법: type에 방식을 명시한다. 기본값은 ClusterIP이다.
  - ClusterIP: 클러스터 내부에서만 통신이 가능하다.
  - NodePort: ports 속성의 하위 속성인 nodePort에 포트 번호를, targetPort에 접근할 Pod의 포트 번호를 지정하면 노드와 Pod간 포워딩이 가능하다.
  - LoadBalancer: 별도의 외부 로드 밸런서를 제공하는 클라우드(AWS, Azure, GCP 등) 환경을 고려하여, 해당 로드 밸런서를 클러스터의 서비스로 프로비저닝한다.

> Pod는 생성 시마다 새로 IP를 할당받으므로 Service를 통해 접근되어야 한다.
{: .prompt-info}

> targetPort의 값은 실제 접근할 Pod의 포트 번호가 변경되면 같이 변경되어야 하는 불편함이 있을 수 있다. 이 때, Pod 오브젝트의 속성인 containers의 ports의 하위 속성으로 name과 containerPort에 이름과 포트번호를 지정한 후 targetPort 값에 해당 name 속성의 값을 지정하면 포트번호가 매핑되므로 Pod의 containerPort만 변경하면 되도록 구성할 수 있다.
{: .prompt-tip }

> 서비스 디스커버리: Service는 기본적으로 Service 이름을 도메인명으로 내부 DNS에 등록되며 Service 오브젝트의 ports 속성의 하위 속성인 port에 지정한 포트 번호와 함께 사용하여 Pod에 접근 가능하다. 외부 Namespace에서 접근하는 경우 `{서비스 도메인명}.{네임스페이스명}`으로 접근 가능하다.
{: .prompt-tip }

> 외부에 IP를 개방하지 않는 경우 ClusterIP를 사용하는 것이 적절하며, 외부에 별도의 강력한 로드밸런싱 기능을 필요로 하지 않는 노출이 필요한 경우 Ingress와 NodePort를 사용할 수도 있다. 강력한 로드밸런싱이 필요한 대규모 서비스의 경우 LoadBalancer가 적절하다.
{: .prompt-tip }

<br>

## labels, selector, naming

> 쿠버네티스 오브젝트들은 labels와 selector를 통해 상호 매핑이 이루어진다.

![kubermentes-resources-naming-example](/assets/img/posts/kubernetes-resources-naming-example.png)
![kubermentes-resources-labels-and-selector](/assets/img/posts/kubernetes-resouces-labels-and-selector.png)

> 파드가 특정 레이블을 가진 노드 그룹에 할당되도록 하기 위해서는 노드에 label을 지정하고 파드 spec의 `nodeSelector` 필드에 해당 노드의 label을 지정하면 된다. 다만, 보다 복잡한 표현식으로 노드를 선택할 수 없으며 이를 가능케 하려면 파드의 `NodeAffinity`를 통해 설정한다.
{: .prompt-tip }
