## 쿠버네티스란?

앱의 배포, 확장 및 관리를 자동화하기 위한 오픈소스시스템.

- 컨터이너 배포를 관리, 자동배포, 스케일링, 로드밸런싱 등

## Cluster

노드머신, `Master Node`, `Worker Node`, 배포혹은 원하는 최종상태를 구성하는 모든것의 컬렉션 세트

## Nodes

하나 또는 여러개의 `Pod`를 호스팅하는 특정 하드웨어 용량을 가지며 `Cluster`와 통신하거나 `Cluster` 내에서 통신하는 물리적인 머신 또는 가상 머신
모든 `Worker Node`를 걸쳐 `Pod`를 관리하는 컨트롤 플레인을 가진 `Master Node`가 있다. 다른 노드 타입으로 `Worker Node`도 있다. 이들은 `Pod`를 호스팅하는
실제 머신이며, 결국 앱 컨테이너와 이러한 컨테이너에 필요한 리소스를 실행함.

## Pods

애플리케이션 컨테이너와 요구 리소스가 `Pod`라고 하는 유닛으로 결합된다. 컨테이너랄 감싼 shell이다. 컨테이너를 시작하여 특정 컨테이너를 관리한다.
`Pod` 자체는 `Master Node`에 의해 관리된다. `Pod`가 생성되는것은 `Pod`에서 컨테이너를 실행하는것과 같다.

## Container

일반 Docker `Container`. `Pod`가 `Container`를 실행한다는 말은 결국엔 `Pod`가 내부적으로 docker run을 실행한다는것을 의미함.

## Services

`Pod`에 접근하는데 중요하다. 쿠버네티스 세계에서 특정 `Pod`를 외부에 노출하여 특정 IP주소 또는 도메인으로 특정 `Pod`에 연결할 수 있도록하는 용어이다.

## 주의사항

쿠버네티스는 인프라에 관심을 두지 않는다.

## 설치

- kubectl

  클러스터에 명령을 보내기위한 도구

  brew install kubectl

- minikube

  쿠버네티스를 가지고 놀고 테스트하기 위해 로컬에 설치하는 도구

  brew install minikube

  minikube start --driver=docker
