# «Конфигурация приложений»-Абрамов Сергей

### Цель задания

В тестовой среде Kubernetes необходимо создать конфигурацию и продемонстрировать работу приложения.

------

### Чеклист готовности к домашнему заданию

1. Установленное K8s-решение (например, MicroK8s).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключённым GitHub-репозиторием.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. [Описание](https://kubernetes.io/docs/concepts/configuration/secret/) Secret.
2. [Описание](https://kubernetes.io/docs/concepts/configuration/configmap/) ConfigMap.
3. [Описание](https://github.com/wbitt/Network-MultiTool) Multitool.

------

### Задание 1. Создать Deployment приложения и решить возникшую проблему с помощью ConfigMap. Добавить веб-страницу

1. Создать Deployment приложения, состоящего из контейнеров nginx и multitool.
2. Решить возникшую проблему с помощью ConfigMap.
3. Продемонстрировать, что pod стартовал и оба конейнера работают.
4. Сделать простую веб-страницу и подключить её к Nginx с помощью ConfigMap. Подключить Service и показать вывод curl или в браузере.
5. Предоставить манифесты, а также скриншоты или вывод необходимых команд.

------

### Решение

Создадим namespace для задания:

![k1]()

[service.yaml]()

![k2]()

[config_map]()

![k3]()

[deployment.yaml]()

![k4]()

```

kubectl describe pod netology-deployment-5b5648f5b8-xrvwn -n dz8
Name:             netology-deployment-5b5648f5b8-xrvwn
Namespace:        dz8
Priority:         0
Service Account:  default
Node:             k8sclaster/192.168.0.249
Start Time:       Mon, 25 Nov 2024 11:52:25 +0300
Labels:           app=main
                  pod-template-hash=5b5648f5b8
Annotations:      cni.projectcalico.org/containerID: ed2cb9d85437c3a77d605fddcbe4b9b98fc90bf8a271f94e2bc7ceae7b5eb3de
                  cni.projectcalico.org/podIP: 10.1.218.168/32
                  cni.projectcalico.org/podIPs: 10.1.218.168/32
Status:           Running
IP:               10.1.218.168
IPs:
  IP:           10.1.218.168
Controlled By:  ReplicaSet/netology-deployment-5b5648f5b8
Containers:
  nginx:
    Container ID:   containerd://94828cc72f7530192ee1b8acb2ae17c2aeb31fdc3bf037417488c57d64508c07
    Image:          nginx:1.19.2
    Image ID:       docker.io/library/nginx@sha256:c628b67d21744fce822d22fdcc0389f6bd763daac23a6b77147d0712ea7102d0
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Mon, 25 Nov 2024 11:52:25 +0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /usr/share/nginx/html/ from nginx-index-file (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-5xx4q (ro)
  multitool:
    Container ID:   containerd://cf1e1c84b1e532135116d7d6b38f8afda9c3285269c6ad28fcb645eb172dc2f7
    Image:          wbitt/network-multitool
    Image ID:       docker.io/wbitt/network-multitool@sha256:d1137e87af76ee15cd0b3d4c7e2fcd111ffbd510ccd0af076fc98dddfc50a735
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Mon, 25 Nov 2024 11:52:27 +0300
    Ready:          True
    Restart Count:  0
    Environment:
      HTTP_PORT:  <set to the key 'HTTP-PORT' of config map 'configmap-nginx-multitool'>  Optional: false
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-5xx4q (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 
Volumes:
  nginx-index-file:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      configmap-nginx-multitool
    Optional:  false
  kube-api-access-5xx4q:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  4m39s  default-scheduler  Successfully assigned dz8/netology-deployment-5b5648f5b8-xrvwn to k8sclaster
  Normal  Pulled     4m40s  kubelet            Container image "nginx:1.19.2" already present on machine
  Normal  Created    4m40s  kubelet            Created container nginx
  Normal  Started    4m40s  kubelet            Started container nginx
  Normal  Pulling    4m40s  kubelet            Pulling image "wbitt/network-multitool"
  Normal  Pulled     4m38s  kubelet            Successfully pulled image "wbitt/network-multitool" in 1.092s (1.092s including waiting). Image size: 25281012 bytes.
  Normal  Created    4m38s  kubelet            Created container multitool
  Normal  Started    4m38s  kubelet            Started container multitool

  ```

  ![k5]()

  ![k6]()

  ![k7]()


### Задание 2. Создать приложение с вашей веб-страницей, доступной по HTTPS 

1. Создать Deployment приложения, состоящего из Nginx.
2. Создать собственную веб-страницу и подключить её как ConfigMap к приложению.
3. Выпустить самоподписной сертификат SSL. Создать Secret для использования сертификата.
4. Создать Ingress и необходимый Service, подключить к нему SSL в вид. Продемонстировать доступ к приложению по HTTPS. 
4. Предоставить манифесты, а также скриншоты или вывод необходимых команд.

------

### Решение



### Правила приёма работы

1. Домашняя работа оформляется в своём GitHub-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, а также скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

------
