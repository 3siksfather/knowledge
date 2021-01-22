
# Running 'oc whoami' results in Error from server (ServiceUnavailable) after OCP 4.x bare metal install

## 환경
OCP 4.X

## 문제
```
oc whoami 
Error from server (ServiceUnavailable): the server is currently unable to handle the request (get users.user.openshift.io ~)
```

## 해결
```
oc get csr
oc get csr -o go-template='{{range .items}}{{if not .status}}{{.metadata.name}}{{"\n"}}{{end}}{{end}}' | xargs oc adm certificate approve
```

설치 직후에 부트스트랩을 제거하고 오랜시간 방치하면 csr 인증이 pending 상태에서 유지되어 노드들이 NotReady 상태로 떨어짐.
```
oc get node
NAME                    STATUS   ROLES          AGE   VERSION
master01.paas.pst.com   NotReady    master         8d    v1.19.0+7070803
master02.paas.pst.com   NotReady    master         8d    v1.19.0+7070803
master03.paas.pst.com   NotReady    master         8d    v1.19.0+7070803
worker01.paas.pst.com   NotReady    infra,worker   8d    v1.19.0+7070803
worker02.paas.pst.com   NotReady    infra,worker   8d    v1.19.0+7070803
```
