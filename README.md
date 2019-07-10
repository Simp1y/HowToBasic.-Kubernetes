# HowToBasic.Kubernetes
<p align="center">
  <img width="780" height="350" src="https://github.com/Simp1y/HowToBasic.-Kubernetes/blob/master/img/Kubernetes_New.png">
</p>

### How to install and configure
- [Install Google SDK](https://cloud.google.com/sdk/docs/)
- [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux)
##### Run command "source ~/.bashrc".
##### export KUBE_EDITOR="/bin/nano" - set nano editor as default(if need).

## Commands:

kubectl create -f filename.yaml - створити под/сервіс/і тд.
kubectl apply -f filename.yaml - внести зміни под/сервіс/і тд. якщо він вже існує
kubectl get pod (kubectl get pods -o wide - більше інфи про поди)- показати всі існуючі поди
kubectl get svc - показати всі існуючі сервіси
kubectl get nodes <node-name> - показати всі ноди
kubectl describe node/pod/rc/etc <node/pod/rc-name> - детальний опис node(вузла)/pod'a
kubectl delete pod/svc <pod/svc-name> - видалити под/service
gcloud compute ssh <імя-вузла> - ввійти на ноду по ssh
kubectl cluster-info - інфо по кластеру(ДНС, мастер і тд.)
kubectl get pod <pod-name> -o yaml - получити маніфест pod'a в форматі yaml
kubectl logs <pod-name> - показати логи контейнера на поді(ротація логів щоденно або по досягненні 10 мб)
kubectl logs <pod-name> -c <container-name> - логи конкретного контейнера якщо на поді запущено 2+ контейнера
kubectl port-forward <pod-name> 8888:8080 - переадресація портів без використання сервісів
kubectl get pod -l app=kubia  - показати всі поди з міткою app=kubia
kubectl get ns - показати всі namespaces
kubectl get pod --namespace <pod-name> - показати поди які належать до namespace
kubectl create -f <pod/node/etc>.yaml -n <custom-namespace> - створити ресурс в конкретному неймспейсі
kubectl delete all --all - видалити все окрім стандартних сервісів 
kubectl logs <pod-name> --previous - логи з попереднього контейнера(якій наприклад аварійно припинив роботу)
kubectl label pod <pod-name> <key>=<label> - в ручну задати лейбл(мітку)
kubectl label pod <pod-name> app=<new-label-name> --overwrite - переписати існуючий лейбл в ключі app
kubectl get pod --show-labels - показати мітки подів
  
### Replica controller (скоро буде визнано як застаріле і не буде підтримуватись)
kubectl get rc - показати всі запущенні контроллери реплік
kubectl edit rc <rc-name> - редагувати контроллер перлік
kubectl scale rc <rc-name> --replicas=10 - горизонтальний скейлінг вручну
kubectl delete rc <rc-name> --cascade=false - видалити rc залишивши при цьому всі поди

### ReplicaSet (нова rc) - rs (НАБОРИ РЕПЛІК)
 

kubectl delete rs <rs-name> --cascade=false - видалити rc залишивши при цьому всі поди
