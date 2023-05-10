# Kubernetes 각종 레이블 관리와 모니터링
* ./Lab1.Kubespray/1.KubernetesBasic/README.md 파일을 참고 하여 문제를 푸세요.
*  답은 Solution.md 파일에 있습니다.
## 1. 모든 Pods를 출력하시오.
```
kubectl get pods
```

## 2. 아래의 조건을 만족하는 pods를 만드시오.
|pod Name|image Name|
|--------|----------|
|n1      |nginx     |
|u1      |ubuntu    |
|h1      |httpd     |

```
kubectl create deployment --image nginx n1
kubectl create deployment --image ubuntu u1
kubectl create deployment --image httpd h1
```

## 3. 만들어진 pods를 확인하시오.
```
kubectl get pods
```

## 4. ubuntu이미지가 잘 못 만들어진 원인은?
* kubernetes에서 사용하는 pod들은 기본적으로 detach(-d)방식으로 동작.


## 5. 아래의 yml파일을 가지고 다시 생성하시오.
```
kubectl ~
cat <<EOF>u1.yml
apiVersion: v1
kind: Pod
metadata:
  name: u1
  labels:
    server: ubuntu
spec:
  containers:
  - image: ubuntu
    command:
      - "sleep"
      - "604800"
    imagePullPolicy: IfNotPresent
    name: u1
  restartPolicy: Always
EOF
kubectl create -f u1.yml
```


## 6. pods의 Label이 server=ubuntu인 모든 파드를 출력하시오.
```
kubectl get pods -l server=ubuntu
```

## 7. n1,h1 pods의 label을 server=ubuntu, app=web  이라고 지정하시오.
```
kubectl label pods n1-9949596c4-9d6jb server=ubuntu app=web --overwrite
kubectl label pods h1-55cdf57468-p6lfh server=ubuntu app=web --overwrite
```
## 8. 모든 pods의 label과 함께 출력하시오.
```
kubectl get pods --show-labels=true 
```

## 9. app=web인 Pods만 출력하시오.
```
kubectl get pods -l app=web
```

## 10. Label이 app=web인 모든 Pods를 삭제 하시오.
```
kubectl delete pods -l app=web
```

## 11. 모든 파드를 지우시오.
```
kubectl delete deployments.apps h1
kubectl delete deployments.apps n1
```
* cf)
```
kubectl get pods |awk 'NR>1{print $1}'|xargs -i{} kubectl delete po/{}
# or
kubectl get pods -o json|jq .items[].metadata.name |xargs -i{} kubectl delete po/{}

```
