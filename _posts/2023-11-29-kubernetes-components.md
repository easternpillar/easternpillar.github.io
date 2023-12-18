---
title: 쿠버네티스 구성요소
date: 2023-11-29
categories: [ DevOps ]
tags: [ kubernetes, k8s, devops ]
---
> 참조: 쿠버네티스 어나더 클래스(일프로)

![kubernetes-components-overall](/assets/img/posts/kubernetes-components-overall.png)

<br>

## 컨트롤 플레인 구성요소

> 마스터 노드에서 클러스터의 전반적인 관리와 제어 역할을 하는 구성요소

> 쿠버네티스가 기본적으로 제공하는 컨트롤 플레인 구성요소 외에 클러스터 수준에서 추가적인 기능을 제공하기 위해 별도의 파드를 추가할 수 있는데 이를 Add-on 파드라고 부른다.
{: .prompt-info }

<br>

### kube-apiserver

> 클러스터와의 모든 상호작용을 위한 API 엔드포인트를 제공하는 구성요소

<br>

### etcd

> 클러스터의 모든 상태 및 데이터를 저장하는 분산 키-값 저장소 **(NoSQL)**

<br>

### kube-scheduler

> 새로 생성된 파드를 적절한 노드에 할당하기 위한 스케줄링 역할의 구성요소

> 파드의 spec 중 `nodeName`을 통해 어떤 노드에 해당 파드를 실행할지 결정하는데, 이 필드가 존재하지 않더라도 스케줄러에 의해 자동으로 노드에 배치된다. 만약, 스케줄러가 없다면 이 필드를 명시하거나 `Binding` 리소스를 정의하여 노드와 결합해야 한다.
{: .prompt-info }

- `Taints`와 `Tolerations`
  - Taints: 노드에 파드가 스케줄링되는 것을 제어하는데 사용되며 해당 노드의 `Taints`를 `Tolerations`로 가지지 않는 파드는 해당 노드에 스케줄링될 수 없다.
    - Effect 유형
      - NoSchedule: 해당 노드에 `Tolerations`가 없는 파드가 스케줄링되지 않는다.
      - PreferNoSchedule: 클러스터 내 리소스 상황이나 다른 스케줄링 제약 등의 불가피한 상황이 아니라면 스케줄링되지 않는다.
      - NoExecute: 해당 노드에 `Tolerations`가 없는 파드는 스케줄링될 수 없으며 기존에 실행되던 파드들도 검열된다. 
  - Tolerations: 파드의 `spec`에 정의하여 노드의 `Taints`와 동일하게 설정하여 해당 노드에서 실행가능함을 명시한다.
- NodeAffinity: `Taints`와 `Tolerations`가 노드에 파드가 할당되는 것을 막는 용도라면 `NodeAffinity`는 파드가 노드에 할당되도록하는 설정이다.
  - 유형: DuringScheduling은 스케줄링 과정에서의 판단을 의미하고, DuringExecution은 노드에 할당되어 실행되고 있는 이후에도 영향을 받을지에 관한 설정이다.
    - requiredDuringSchedulingIgnoredDuringExecution: 표현식에 부합하는 노드에서만 실행될 수 있고 실행된 이후의 설정에 영향받지 않는다.
    - preferredDuringSchedulingIgnoredDuringExecution: 표현식에 부합하는 노드에서 실행되는 것을 최우선으로 하되 불가피한 경우에는 다른 노드에서 실행될 수 있고 실행된 이후의 설정에 영향받지 않는다.

> `NodeAffinity`는 `NodeSelector`를 사용하는 것보다 더 복잡한 표현식을 통해 파드를 실행할 노드를 결정할 수 있다. 


> 마스터 노드는 관리 목적의 구성요소를 제외한 파드들이 실행되지 않는 것이 관례이며 이것은 기본적으로 `Taints`에 의해 기본값으로 설정되어 있다.
{: .prompt-info }

<br>

### kube-controller-manager

> 클러스터 상태를 원하는 상태로 유지하기 위해 etcd를 관찰하고 컨트롤러를 실행시키는 구성요소

<br>

### cloud-controller-manager

> 기본적으로 kube-controller-manager와 동일한 기능을 하나 클라우드 서비스 제공자에 특화된 컨트롤러를 실행시키는 구성요소

<br>

## 워커 노드 구성요소

> 실제 애플리케이션의 파드가 실행되는 노드의 구성요소

<br>

### kubelet

> 각 노드에서 실행되어 컨테이너 런타임을 제어하여 파드의 생명주기를 관리하는 구성요소

<br>

### kube-proxy

> 각 노드의 네트워크 프록시 역할을 담당하며 네트워크 규칙을 관리하고 제어하는 구성요소
