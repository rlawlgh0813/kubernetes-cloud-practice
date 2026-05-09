# Kubernetes Cloud Practice

Kubernetes 기반 클라우드 인프라 실습을 정리한 저장소입니다.

Pod, Deployment, Service, Ingress, Storage처럼 Kubernetes에서 애플리케이션을 배포하고 운영할 때 필요한 핵심 리소스를 직접 생성하고, 각 리소스가 어떤 책임을 가지는지 실습 단위로 정리했습니다.

## Overview

| Area | Directory | What I practiced |
| --- | --- | --- |
| Node Operations | [01-node](01-node) | cordon, drain, taint/toleration 기반 노드 스케줄링 제어 |
| Namespace | [02-namespace](02-namespace) | namespace 생성, context 전환, 리소스 범위 분리 |
| Pod | [03-pod](03-pod) | Pod 생성, 환경변수, 다중 컨테이너, Pod의 한계 |
| Deployment | [04-deployment](04-deployment) | replica 유지, rolling update, rollback |
| StatefulSet | [05-statefulset](05-statefulset) | 고정된 Pod identity와 순차적 생성/삭제 |
| Job | [06-job](06-job) | 단발성 작업, retry, completions, parallelism |
| CronJob | [07-cronjob](07-cronjob) | 주기적 Job 실행과 history limit |
| DaemonSet | [08-daemonset](08-daemonset) | 노드마다 실행되는 agent workload |
| ConfigMap | [09-configmap](09-configmap) | 설정 값을 환경변수로 분리 |
| Secret | [10-secret](10-secret) | 민감 정보를 환경변수/파일로 주입 |
| Service | [11-service](11-service) | ClusterIP, NodePort, LoadBalancer |
| Ingress | [12-ingress](12-ingress) | domain/path/host 기반 HTTP routing |
| Storage | [13-storage](13-storage) | Volume, PV, PVC, StorageClass, NFS |

## Repository Structure

```text
kubernetes-cloud-practice/
├── 01-node/
├── 02-namespace/
├── 03-pod/
├── 04-deployment/
├── 05-statefulset/
├── 06-job/
├── 07-cronjob/
├── 08-daemonset/
├── 09-configmap/
├── 10-secret/
├── 11-service/
├── 12-ingress/
├── 13-storage/
├── docs/
└── README.md
```

각 디렉토리는 해당 Kubernetes 리소스의 개념 정리 README와 실습에 사용한 YAML manifest를 포함합니다.

## Environment

- Kubernetes cluster: control plane + worker nodes
- CLI: kubectl
- Ingress Controller: NGINX Ingress Controller
- Storage: local volume, NFS, StorageClass
- Practice style: CLI command + declarative YAML manifest

## Practice Flow

### 1. Cluster and Resource Boundaries

처음에는 클러스터에서 리소스가 어디에 배치되고, 어떤 범위로 관리되는지 확인했습니다.

- node scheduling control
- cordon / drain / uncordon
- taint / toleration
- namespace 기반 리소스 분리
- kubectl context namespace 전환

### 2. Workload Controllers

Pod를 직접 생성하는 수준에서 벗어나, Kubernetes controller가 원하는 상태를 유지하는 방식을 실습했습니다.

- Pod lifecycle
- Deployment replica management
- rolling update / rollback
- StatefulSet identity
- Job / CronJob batch workload
- DaemonSet node-level agent workload

### 3. Configuration and Secrets

컨테이너 이미지와 실행 설정을 분리하는 방식을 실습했습니다.

- ConfigMap을 환경변수로 주입
- ConfigMap 전체 key-value를 `envFrom`으로 제공
- Secret을 환경변수로 주입
- Secret을 file volume으로 mount
- 설정 변경과 Pod 재시작 관계 확인

### 4. Networking

Pod의 불안정한 IP를 직접 사용하지 않고, Service와 Ingress로 네트워크 접근을 추상화했습니다.

- ClusterIP 기반 내부 통신
- NodePort 기반 외부 접근
- LoadBalancer 구조 이해
- Ingress path routing
- Ingress host routing
- `nip.io` 기반 domain 실습

### 5. Storage

Pod 생명주기와 데이터를 분리하기 위한 Storage 리소스를 실습했습니다.

- `emptyDir`
- `hostPath`
- Local PV
- NFS PV
- PVC binding
- StorageClass 기반 provisioning

## What This Repository Shows

이 저장소는 Kubernetes 리소스를 단순히 생성해 보는 것보다, 각 리소스가 어떤 운영 문제를 해결하는지 이해하는 데 초점을 둡니다.

특히 다음 역량을 보여주는 데 초점을 둡니다.

- Pod와 Controller의 역할 구분
- 선언형 YAML manifest 작성
- rolling update와 rollback 흐름 이해
- Service / Ingress 기반 네트워크 추상화
- ConfigMap / Secret 기반 설정 분리
- PV / PVC 기반 상태 저장 구조 이해
- 노드 점검과 스케줄링 제어 흐름 이해

## Documents

- [Practice Report](docs/report.pdf)

## Notes

일부 실습은 CLI 중심으로 진행했고, 반복 재현이 필요한 리소스는 YAML manifest로 정리했습니다. 각 디렉토리의 README에는 실습 목적, 확인 명령, 주의할 점을 함께 기록했습니다.
