# Deployments

**kubectl get deployments**

**kubectl create deployment denis-deployment --image adv4000/k8sphp:latest**                # создание deployment

**kubectl describe deployments denis-deployment**                                          # вывод информации о deployment

**kubectl scale deployment denis-deployment --replicas 4**                                 # создаем реплики деплоймента. (создаем еще 3 пода с приложением) (если один из подов выйдет из строя / будет удален, то кубер автоматически сделает еще один такой же) (так же дополнительные реплики равномерно распределяются по нодам)

**kubectl get rs**                                                                          ### просмотр replica set

**kubectl autoscale deployment denis-deployment --min=4 --max=6 --cpu-percent=80**          # настройка автоскейлинга. при достижении средней загруженности cpu на подах 80%, кубер добавляет еще поды с приложением для того чтобы расределить нагрузку между всеми подами

**kubectl get hpa**                                                                         # вывод информации об horizontall pod autoscaling

**kubectl rollout history deployment/denis-deployment**

### ОБНОВЛЕНИЕ IMAGE В ПОДАХ И ВОЗВРАТ К ПРЕДЫДУЩИМ ВЕРСИЯМ

**kubectl set image deployment/denis-deployment k8sphp=adv4000/k8sphp:version1 --record**   # заливаем новые image на поды

**kubectl rollout status deployment/denis-deployment**                                     # смотрим за статусом обновления image на подах

**kubectl rollout history deployment/denis-deployment**                                     # вывод истории обновления image на подах

**kubectl rollout undo deployment/denis-deployment --to-revision=6**                        # откат к версии image который был в 6 версии rollout history


# Services (в основном используется для настройки load balancer)

**kubectl create deployment denis-deployment --image adv4000/k8sphp:latest**  # создаем deployment
**kubectl get deployment** # показать все deployments

**kubectl scale deployment denis-deployment --replicas 4**   # создать 4 реплики 

**kubectl expose deployment denis-deployment --type=ClusterIP --port 80**  # создать Service типа Cluster IP для Deployment

**kubectl expose deployment denis-deployment --type=NodePort --port 80** # создать Service типа Node Port для Deployment

**kubectl expose deployment denis-deploymennt --type=LoadBalancer --port 80** # создать Service типа Load Balancer для Deployment

**kubectl get services**   # показать все Services

**kubectl get svc**	       # показать все Services

**kubectl describe nodes | grep ExternalIP**  # показать все external (внешние) IP со всех Worket Nodes


# Ingress (в основном используется для замены нескольких load balancer и проксирования трафика)

Здесь мы развернем 4 deployments у каждого будет свой сервис и у каждого будет 2 реплики  
Чтобы не использовать 4 load balansers мы настраиваем 1 contour ingress controller



### Install Ingress Controller: Contour
kubectl apply -f https://projectcontour.io/quickstart/contour.yaml

(сравнение разных ingress controllers https://docs.google.com/spreadsheets/d/191WWNpjJ2za6-nbG4ZoUMXMpUK8KlCIosvQB0f-oq3k/edit#gid=907731238)

kubectl get services -n projectcontour envoy -o wide

Get LoadBalancer IP or DNS Name and assign Your Domain to this DNS name

### Create Deployments
kubectl create deployment main   --image=adv4000/k8sphp:latest

kubectl create deployment web1   --image=adv4000/k8sphp:version1

kubectl create deployment web2   --image=adv4000/k8sphp:version2

kubectl create deployment webx   --image=adv4000/k8sphp:versionx

kubectl create deployment tomcat --image=tomcat:8.5.38

### Scale Deployments

kubectl scale deployment main  --replicas 2

kubectl scale deployment web1  --replicas 2

kubectl scale deployment web2  --replicas 2

kubectl scale deployment webx  --replicas 2

### Create Services, default type is: --type=ClusterIP

kubectl expose deployment main   --port 80

kubectl expose deployment web1   --port 80

kubectl expose deployment web2   --port 80

kubectl expose deployment webx   --port 80

kubectl expose deployment tomcat --port 8080

kubectl get services -o wide

kubectl apply -f ingress-hosts.yaml

kubectl apply -f ingress-paths.yaml

kubectl get ingress

kubectl describe ingress

### Completely delete Ingress Controller: Contour

kubectl delete ns projectcontour










