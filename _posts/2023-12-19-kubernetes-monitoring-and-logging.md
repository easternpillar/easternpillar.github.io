---
title: 쿠버네티스 모니터링과 로깅
date: 2023-12-19
categories: [ DevOps ]
tags: [ devops, k8s, kubernetes, monitoring, logging ]
---

## 쿠버네티스 모니터링

> 쿠버네티스는 내장된 모니터링 솔루션을 제공하지 않으며 별도의 솔루션을 도입하여 사용하여야 한다. 과거 Heapster라는 오픈소스 프로젝트가 쿠버네티스 모니터링에 주로 사용되었으나 확장성과 유연성 문제로 Deprecate 되었고 현재 Metrics server를 통해 모니터링 데이터를 수집한다. 이 솔루션은 모니터링 데이터를 메모리에 저장하는 경량 솔루션이기 때문에 이를 저장하여 관리하기 위한 별도 솔루션을 설치하여 사용하는 것이 권장된다.  
{: .prompt-tip }

> 모니터링 데이터는 kubelet에 내장된 cAdvisor(Container Advisor)에 의해 수집된다.
{: .prompt-info }

<br>

## 쿠버네티스 로깅

- kubectl logs 명령어 실행시 특정한 파드가 내보내는 표준 출력(stdout)을 보여준다.

> 도커를 포함한 다양한 컨테이너 런타임도 동일한 동작방식을 가진다.
{: .prompt-tip }
