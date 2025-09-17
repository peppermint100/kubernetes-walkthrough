
```
kubectl run nginx --image=nginx
```
- nginx 이미지로 pod를 생성한다.

```
kubectl desribe pod <POD_NAME>
```
- pod의 디테일한 정보를 본다.

```
kubectl delete pod <POD_NAME>
```
- 파드를 삭제한다.

```
kubectl edit pod redis
```
- 파드의 디테일 정보에 들어가서 파드를 수정한다. (이미지 등 수정가능)

```
kubectl scale replicaset [레플리카셋 이름] --replicas=[원하는 숫자]
```
- 레플리카셋의 레플리카 개수를 늘리는 법

```
kubectl get pods -A
kubectl get pods --all-namespaces
```
- 네임스페이스 상관없이 모든 파드 보는법

```
kubectl run redis --image=redis:alpine --labels="tier=db"
```
- label까지 imperative하게 파드 생성하기

```
kubectl run redis --image=redis:alpine --dry-run=client -o yaml > redis-pod.yaml
```
- redis:alpine 이미지를 만드는 파드 definition yaml 설정 파일 생성

```
kubectl expose pod redis --name=redis-service --port=6379 --target-port=6379
```
- redis라는 파드를 service를 통해 바로 imperative 하게 노출
- port: 이 서비스가 노출할 포트번호
- target-port: 서비스로 들어온 요청을 전달한 파드 내부 컨테이너의 포트번호

```
kubectl create deployment webapp --image=kodekloud/webapp-color --replicas=3
```
- 3개의 레플리카를 같은 deployment 생성

```
kubectl run custom-nginx --image=nginx --port=8080
```
- container-port를 8080으로 주는 파드 실행


```
kubectl create configmap webapp-config-map \
--from-literal=APP_COLOR=darkblue \
--from-literal=APP_OTHER=disregard
```
- 선언적인 방식으로 configmap 만들기

```
kubectl create secret generic db-secret --from-literal=DB_Host=sql01 --from-literal=DB_User=root --from-literal=DB_Password=password123 --dry-ru
n=client -o yaml > db-secret.yaml
```
- generic 타입으로 secret 만들기 

```
kubectl describe node <노드이름>
```
- 노드의 정보 확인

```
kubectl taint nodes <노드이름> <Key>=<Value>:<Rules>
```
- 노드에 taint 생성
- Rules
	- NoSchedule
	- PreferNoSchedule
	- NoExecute

```
kubectl get pod -o wide
```
- pod들의 자세한 정보를 볼 수 있다. IP, 배치된 노드 확인 

```
kubectl taint nodes <노드이름> <키>=<값>:효과-
```
`taint node` 명령어 마지막에 `-` 기호를 추가하여 존재하는 taint를 삭제할 수 있다.

```
kubectl label node <노드이름> <키>=<값>
kubectl label node <노드이름> <키>=<값> --override # (기존의 레이블 값을 덮어 씌우는 경우)
kubectl label nodes <노드이름> <키>- # 레이블을 삭제하는 경우
```
노드에 label을 설정