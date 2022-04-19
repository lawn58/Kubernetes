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


# Services

**kubectl create deployment denis-deployment --image adv4000/k8sphp:latest**  # создаем deployment
**kubectl get deployment** # показать все deployments

**kubectl scale deployment denis-deployment --replicas 4**   # создать 4 реплики 

**kubectl expose deployment denis-deployment --type=ClusterIP --port 80**  # создать Service типа Cluster IP для Deployment

**kubectl expose deployment denis-deployment --type=NodePort --port 80** # создать Service типа Node Port для Deployment

**kubectl expose deployment denis-deploymennt --type=LoadBalancer --port 80** # создать Service типа Load Balancer для Deployment

**kubectl get services**   # показать все Services

**kubectl get svc**	       # показать все Services

**kubectl describe nodes | grep ExternalIP**  # показать все external (внешние) IP со всех Worket Nodes


