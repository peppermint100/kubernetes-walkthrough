

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: blue
  name: blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: blue
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: blue
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
	 affinity:
	   nodeAffinity:
		 <어피니티-유형>:
		   nodeSelectorTerms:
		   - matchExpressions:
			 - key: <키>
			   operator: <연산자>
			   values:
			   - <값>
status: {}
~                   
```

spec 내부 containers와 동일한 레벨에 설정

#### 1. 어피니티 유형
- **`requiredDuringSchedulingIgnoredDuringExecution` (Hard-Rule)**
    - 스케줄링 시점에 이 규칙을 만족하는 노드가 **반드시** 있어야만 파드를 배치
    - 만족하는 노드가 없으면 파드는 `Pending` 상태로 대기
    - 일단 파드가 실행된 후에는 노드의 레이블이 변경되어도 파드는 계속 실행(`IgnoredDuringExecution`)
        
- **`preferredDuringSchedulingIgnoredDuringExecution` (Soft-Rule)**
    - 스케줄링 시점에 규칙을 만족하는 노드를 **선호**하지만, 만족하는 노드가 없어도 다른 노드에 파드를 배치할 수 있습니다.
    - 실행 중인 파드에는 영향 없음
#### 2. nodeSelectorTerms
규칙들의 묶음이 필드는 배열(리스트) 형태이며, 여러 개의 `term`을 가진다.s

| 연산자 (Operator)     | 설명                                          | `values` 필요 여부 |
| ------------------ | ------------------------------------------- | -------------- |
| **`In`**           | 레이블의 값이 `values` 목록에 있는 값 중 **하나라도 일치**하면 참 | O              |
| **`NotIn`**        | 레이블의 값이 `values` 목록에 있는 값과 **모두 불일치**하면 참   | O              |
| **`Exists`**       | 노드에 해당 `<키>`를 가진 레이블이 **존재하기만 하면** 참        | X              |
| **`DoesNotExist`** | 노드에 해당 `<키>`를 가진 레이블이 **존재하지 않으면** 참        | X              |
| **`Gt`**           | 레이블의 값이 `values`에 지정된 값**보다 크면** 참 (숫자 비교)  | O (하나의 값)      |
| **`Lt`**           | 레이블의 값이 `values`에 지정된 값**보다 작으면** 참 (숫자 비교) | O (하나의 값)      |

## 키가 존재하기만 하면 통과시키는 nodeaffinity 예시
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: red
  name: red
spec:
  replicas: 2
  selector:
    matchLabels:
      app: red
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: red
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/control-plane
                operator: Exists
status: {}
~                                
```

