
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
- 