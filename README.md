kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl get po -n argocd  -w
초기 비밀번호 확인

PS C:\WINDOWS\system32> kubectl get secret -n argocd argocd-initial-admin-secret `
>>   -o jsonpath="{.data.password}" | ForEach-Object {
>>     [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($_))
>>   }
>>
juGaEsEFkrj4v8Xm

kubectl port-forward -n argocd svc/argocd-server 8080:443

argocd-server 파드로 접근하여 admin : "위 secrets 값"으로 로그인한다.
$ kubectl exec -it -n argocd deployment/argocd-server -- /bin/bash
argocd login localhost:8080 --username admin --password juGaEsEFkrj4v8Xm --insecure

argocd login localhost:8080 --username admin --password 1q2w3e4r!! --insecure

Username: admin
Password:
'admin:login' logged in successfully
Context 'localhost:8080' updated


kubectl port-forward -n argocd svc/argocd-server 8080:443

포트포워딩 후 
https://localhost:8080
으로 접속 
admin / 1q2w3e4r!!
 

https://github.com/Jeong-SoRa/argocd-demo.git

$REPO = "https://github.com/Jeong-SoRa/argocd-demo.git"
argocd app create demo-web `
  --repo $REPO `
  --path k8s `
  --dest-server https://kubernetes.default.svc `
  --dest-namespace demo `
  --sync-policy automated `
  --self-heal `
  --auto-prune `
  --sync-option CreateNamespace=true


출처: https://wlsdn3004.tistory.com/37 [IT DevOps 기록:티스토리]
