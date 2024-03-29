# HowToBasic.Kubernetes
<p align="center">
  <img width="820" height="450" src="https://github.com/Simp1y/HowToBasic.-Kubernetes/blob/master/img/kubernetes_by_google.jpg">
</p>

### How to install and configure
- [Install Google SDK](https://cloud.google.com/sdk/docs/)
- [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux)
##### Run command "source ~/.bashrc".
##### export KUBE_EDITOR="/bin/nano" - set nano editor as default(if need).

### Kubernetes concepts:
###### Nodes: Node це машина в кластері Kubernetes.
###### Pods: Pod це група контейнерів із загальними розділами, що запускаються як єдине ціле.
###### Replication Controllers: Гарантує, що певна кількість «реплік» pod'и будуть запущені в будь-який момент часу.
###### ReplicaSet: Гарантує, що певна кількість «реплік» pod'и будуть запущені в будь-який момент часу.
###### Services: Сервіс в Kubernetes це абстракція яка визначає логічний об'єднаний набір pod і політику доступу до них.
###### Volumes: Volume (розділ) це директорія, можливо, з даними в ній, яка доступна в контейнері.
###### Labels: Label'и це пари ключ / значення які прикріплюються до об'єктів, наприклад pod'ам. Label'и можуть бути використані для створення і вибору наборів об'єктів.
###### Kubectl Command Line Interface: kubectl інтерфейс командного рядка для управління Kubernetes. 
###### DaemonSet: Для запуска пода на всіх нодах кластера, по 1 екземпляру по заданих мітках ноди 
#### Доступ до внутрішніх сервісів ззовні
###### NodePort: для служби NodePort відкриваєтся порт на самому вузлі (звідси і назва) і перенаправляє трафік, одержуваний на цьому порту, в базову службу. Служба доступна не тільки через внутрішній IP-адреса і порт кластера, а й через виділений порт на всіх вузлах;
###### LoadBalancer: балансувальник навантаження перенаправляє трафік на порт вузла у всіх вузлах. Клієнти підключаються до служби через IP-адреса балансувальника навантаження;
###### Ingress: механізм предоставленя доступу до декількох служб через єдину IP-адреса - він працює на рівні HTTP
## Commands:

- kubectl create -f filename.yaml - створити под/сервіс/і тд.
- kubectl apply -f filename.yaml - внести зміни под/сервіс/і тд. якщо він вже існує
- kubectl get pod (kubectl get pods -o wide - більше інфи про поди)- показати всі існуючі поди
- kubectl get svc - показати всі існуючі сервіси
- kubectl get nodes - показати всі ноди
- kubectl describe node/pod/rc/etc "node/pod/rc-name" - детальний опис node(вузла)/pod'a
- kubectl delete pod/svc "pod/svc-name" - видалити под/service
- gcloud compute ssh "імя-вузла" - ввійти на ноду по ssh
- kubectl cluster-info - інфо по кластеру(ДНС, мастер і тд.)
- kubectl get pod "pod-name" -o yaml - получити маніфест pod'a в форматі yaml
- kubectl logs "pod-name" - показати логи контейнера на поді(ротація логів щоденно або по досягненні 10 мб)
- kubectl logs "pod-name" -c "container-name" - логи конкретного контейнера якщо на поді запущено 2+ контейнера
- kubectl port-forward "pod-name" 8888:8080 - переадресація портів без використання сервісів
- kubectl get pod -l app="kubia"  - показати всі поди з міткою app=kubia
- kubectl get pv - показати всі змонтовані диски
- kubectl get ns - показати всі namespaces
- kubectl get pod --namespace "pod-name" - показати поди які належать до namespace
- kubectl create -f pod/node/etc.yaml -n <custom-namespace> - створити ресурс в конкретному неймспейсі
- kubectl delete all --all - видалити все окрім стандартних сервісів 
- kubectl logs "pod-name" --previous - логи з попереднього контейнера(якій наприклад аварійно припинив роботу)
- kubectl label pod "pod-name" key="label" - в ручну задати лейбл(мітку)
- kubectl label pod "pod-name" app="new-label-name" --overwrite - переписати існуючий лейбл в ключі app
- kubectl get pod --show-labels - показати мітки подів
- kubectl exec "pod-name" --(подвійне тире не потрібно якщо команда немає знаків тире) "command" - виконати команду на поді(модулі)
- kubectl exec -it "pod-name" bash - запустити термінал на поді
- kubectl get endpoints "svc-name" - показати кінцеві точки сервісу 
### DemonSet -  контролює створення модулів(подів) (по 1шт) на кожній ноді або в залежності від мітки(якщо є необхідна мітка то створююєтся модуль) 
- kubectl get ds - показати демонсети(DaemonSet)  
### Replica controller (скоро буде визнано як застаріле і не буде підтримуватись) - контролює к-ть реплік(модулів)
- kubectl get rc - показати всі запущенні контроллери реплік
- kubectl edit rc "rc-name" - редагувати контроллер перлік
- kubectl scale rc "rc-name" --replicas=10 - горизонтальний скейлінг вручну
- kubectl delete rc "rc-name" --cascade=false - видалити rc залишивши при цьому всі поди

### ReplicaSet (нова rc) - rs (НАБОРИ РЕПЛІК) - контролює к-ть реплік(модулів)
- kubectl delete rs "rs-name" --cascade=false - видалити rc залишивши при цьому всі поди
### Jobs - одноразові модулі(конейнери), час виконання 2 хв. Cronjobs - аналогічно unix
- kubectl get jobs - показати всі джоби
- kubectl get cj - показати всі кронджоби
### GCP Firewall
- gcloud compute firewall-rules create "rule-name" --allow=tcp:30123 - встворити фаервол правило яке відкриє 30123 порт на зовні
### TLS(SSL) certificates for Ingress, etc.
- openssl genrsa -out tls.key 2048 - згенерувати закритий ключ
- openssl req -new -x509 -key tls.key -out tls.cert -days 360 -subj - згенерувати сертифікат
- kubectl create secret tls tls-secret --cert=tls.cert --key=tls.key - створюєм файл секрет з двох файлі для ssl зєднання
- kubectl certificate approve "name of the CSR" - підписати сертифікат за допомогою CertificateSigningRequest
- 
