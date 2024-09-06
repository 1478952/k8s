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

  `minikube start --driver=docker`

## Objects (리소스) 이해하기

특정 명령을 실행함으로 객체를 생성가능하다.

### 명령적 방식

docerhub와 같은 레지스트리에 이미지가 등록되어야한다.

- 이미지 생성
  `docker build -t {tag} .`

- 이미지 태그 부여
  `docker tag {이미지} {태그명}`

- 이미지 레지스트리에 등록
  `docker push {태그명}`

- Deployment 객체 생성
  `kubectl create deployment {deployment명} --image={리모트이미지명}`

- Deployment 확인
  `kubectl get deployments`

- deployment에서 생성된 pod 확인
  `kubectl get pods`

- service 생성
  `kubectl expose deployment {deployment명} --type={ClusterIP(클러스터 내부에서만 연결) | NodePort(실행중인 워커노드의 ip주소를 통해 노출) | LoadBalancer(service에 대한 고유한주소생성, 트래픽분산) } --port={포트번호}`

- service 확인
  `kubectl get services`

\*\* minikube 사용시 ip 바인딩
`minikube service {서비스명}`

- scale 설정
  `kubectl scale deployment/{서비스명} --replicas={pod숫자}`

- 이미지 변경
  `kubectl set image deployment/{서비스명} {변경할 컨테이너명}={리모트이미지명}`

- 업데이트 상태 확인
  `kubectl rollout status deployment/{서비스명}`

- 이미지 롤백
  `kubectl rollout undo deployment/{서비스명}`

- 더 이전의 버전으로 롤백하기위해 히스토리 확인
  `kubectl rollout history deployment/{서비스명} --revision={revision번호}`

- 더 이전의 버전으로 롤백
  `kubectl rollout undo deployment/{서비스명} --to-revision={revision번호}`

- service 삭제
  `kubectl delete service {서비스명}`

- deployment 삭제
  `kubectl delete deployment {deployment 명}`

- storage class 조회
  `kubectl get sc`

- 영구볼륨 리스트 조회
  `kubectl get pv`

### 선언적 방식

`kubectl apply -f config.yaml`
`kubectl delete -f config.yaml`

- 특정 라벨의 deployment, service만 삭제
  `kubectl delete deployments,services -l {label key=label value}`

- Pods
  - 쿠버네티스가 상호작용하는 가장 작은 유닛
  - 하나 또는 여러개의 컨테이너를 포함하고 실행함 일반적으로는 pod하나당 컨테이너 하나를 사용
  - 컨테이너를 실행하고 클러스터의 특정 워커 노드에서 pod 생성
  - 컨테이너를 활용하는 volume과 같은 공유 리소스를 가지고 있음.
  - pod나 외부 세계와 통신이 가능함.
  - 컨테이너와 같이 휘발성 데이터이다.
- Deployments
  - 일반적으로 수동으로 pod객체를 직접 생성하고 특정 워커노드로 이동하지는 않는다.
  - Deployments에서 생성하고 관리해야하는 pod의 수와 컨테이너 수에 대한 지침을 제공한다.
  - 하나 이상의 pod를 제어할 수 있다.
  - 무언가 엉망이되어 배포를 실패하더라도 롤백하고 이전에 작동한 deployment로 동라갈 수 있다.
- Services
  - pod를 외부에 노출하기 위해 필요하다.
  - pod에는 디폴트로 이미 내부 ip 주소가 존재한다.
    - 클러스터외부에서 pod에 엑세스하는데 사용할 수 없다.
    - pod가 교체될 때마다 변경된다.
  - pod를 그룹화하고 공유주소, 공유ip 주소를 제공한다. 변경되지 않음. 클러스터 외부에도 노출가능
- Volume
- ...
- Storage Class
  - 관리자에게 스토리지 관리 방법과 볼륨 구성 방법을 세부적으로 제어할 수 있게 해준다.

## 볼륨, 볼륨 클레임

pod의 일부로 시작될 컨테이너에 볼륨을 탑재해야 한다는 지침을 pod template에 추가할 수 있다.
도커의 볼륨과는 조금 다름. 조금 더 강력하다

- 다양한 드라이버와 유형이 존재한다. 데이터가 저장되는 위치를 완벽하게 제어가 가능함.
- 볼륨이 반드시 영구적인것은 아니다. 컨테이너 재시작에는 살아남으나 pod재시작에는 살아남지 못한다

### 볼륨 수명

- pod의 일부이기 때문에 pod의 수명에 따라 다르다.
- 컨테이너를 다시 시작하고 제거해도 살아남는다.
- pod가 파괴되면 볼륨도 제거됨.

### 영구 볼륨(Persistent Volume)

pod와 node에 독립적이다.
클러스터 내에 새 리소스, 새 엔터티를 가지게 되며 노드 및 pod에서 분리된다.
PV Claim이라는것을 만들어 pod의 일부에 추가한다.
pv에 도달하여 엑세스요청이 가능함.

### 볼륨 vs 영구볼륨

- 어떤 볼륨을 사용하는 볼륨을 통해 데이터를 유지할 수 있다.

#### 볼륨

- pod와는 별개가 아니다.
- 각각의 pod에 연결되며 그들이 생명주기가 된다.
- pod가 삭제되면 볼륨도 지워진다.
- pod를 정의할 때와 동일한 파일에 볼륨 구성을 정의하고 기록한다.
- 글로벌 수준에서 관리하기가 어렵다.

#### 영구볼륨

- 클러스터의 standalone resource이다
- 특정 pod에 연결되지 않는다.
- pvc로 연결된다.
- 재사용이 쉽다. 작성을 반복하지않아도됨.
- 글로벌 수준에서 관리가 가능하다.

## 환경변수

configmap을 사용하여 값을 분리할수있다.

`kubectl apply -f environment.yaml`
`kubectl get configmap`
