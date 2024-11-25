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

![k1](https://github.com/smabramov/K8s-1-8/blob/82c4534e9bae959d5bbefb02b8b440b6740b48e6/png/k1.png)

[service.yaml](https://github.com/smabramov/K8s-1-8/blob/82c4534e9bae959d5bbefb02b8b440b6740b48e6/code/1/service.yaml)

![k2](https://github.com/smabramov/K8s-1-8/blob/82c4534e9bae959d5bbefb02b8b440b6740b48e6/png/k2.png)

[config_map](https://github.com/smabramov/K8s-1-8/blob/82c4534e9bae959d5bbefb02b8b440b6740b48e6/code/1/config_map.yaml)

![k3](https://github.com/smabramov/K8s-1-8/blob/82c4534e9bae959d5bbefb02b8b440b6740b48e6/png/k3.png)

[deployment.yaml](https://github.com/smabramov/K8s-1-8/blob/82c4534e9bae959d5bbefb02b8b440b6740b48e6/code/1/deployment.yaml)

![k4](https://github.com/smabramov/K8s-1-8/blob/82c4534e9bae959d5bbefb02b8b440b6740b48e6/png/k4.png)

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

  ![k5](https://github.com/smabramov/K8s-1-8/blob/82c4534e9bae959d5bbefb02b8b440b6740b48e6/png/k5.png)

  ![k6](https://github.com/smabramov/K8s-1-8/blob/82c4534e9bae959d5bbefb02b8b440b6740b48e6/png/k6.png)

  ![k7](https://github.com/smabramov/K8s-1-8/blob/82c4534e9bae959d5bbefb02b8b440b6740b48e6/png/k7.png)


### Задание 2. Создать приложение с вашей веб-страницей, доступной по HTTPS 

1. Создать Deployment приложения, состоящего из Nginx.
2. Создать собственную веб-страницу и подключить её как ConfigMap к приложению.
3. Выпустить самоподписной сертификат SSL. Создать Secret для использования сертификата.
4. Создать Ingress и необходимый Service, подключить к нему SSL в вид. Продемонстировать доступ к приложению по HTTPS. 
4. Предоставить манифесты, а также скриншоты или вывод необходимых команд.

------

### Решение

[deployment.yaml](https://github.com/smabramov/K8s-1-8/blob/b40bc6ac68d4222b61ebe04b32d87fdfb63f0f0a/code/2/deployment.yaml)

[config_map_my.yaml](https://github.com/smabramov/K8s-1-8/blob/b40bc6ac68d4222b61ebe04b32d87fdfb63f0f0a/code/2/config_map_my.yaml)

Сертификаты:

[tls.crt](https://github.com/smabramov/K8s-1-8/blob/b40bc6ac68d4222b61ebe04b32d87fdfb63f0f0a/code/2/cert/tls.crt)

[tls.key](https://github.com/smabramov/K8s-1-8/blob/b40bc6ac68d4222b61ebe04b32d87fdfb63f0f0a/code/2/cert/tls.key)

![k8](https://github.com/smabramov/K8s-1-8/blob/b40bc6ac68d4222b61ebe04b32d87fdfb63f0f0a/png/k8.png)

![K9](https://github.com/smabramov/K8s-1-8/blob/b40bc6ac68d4222b61ebe04b32d87fdfb63f0f0a/png/k9.png)

```

serg@k8snode:~/git/K8s-1-8/code/2$ kubectl get secret -o yaml -n dz8
apiVersion: v1
items:
- apiVersion: v1
  data:
    tls.crt: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUZDekNDQXZPZ0F3SUJBZ0lVSjJMejMyalYwNjZIb0crUk5uNGxMaTRhYnlVd0RRWUpLb1pJaHZjTkFRRUwKQlFBd0ZURVRNQkVHQTFVRUF3d0tiWGt0WVhCd0xtTnZiVEFlRncweU5ERXhNalV3T1RVd016UmFGdzB5TlRFeApNalV3T1RVd016UmFNQlV4RXpBUkJnTlZCQU1NQ20xNUxXRndjQzVqYjIwd2dnSWlNQTBHQ1NxR1NJYjNEUUVCCkFRVUFBNElDRHdBd2dnSUtBb0lDQVFDeElFR0ZJL054U0F4VlBYaDFvK1I3TkcvV1VMRGQ4QmdKWmdyTDJFVnUKdWZaaDhVWW9ZSnNSbFd2MDd2QXpxMDdEemdaVXp6NXFuamlMOXBHYjFDMk9HZC9RY29nMXUxUGtLRVZ1RjJSKwp0MGY5aHFHWXkzRlVyOHNpc1RKaHZQSGFZTjBFWUtxVHFEWlhPSFpIbkF1ZWJCeWpYaWYxZEFwZUkzMnpVQ2ZsCmowcDlJTkFHUUpvS0VoNHRrNmp2cnZvd291dEl5N0lqdnJ2U2ovTnVaRnpvaXpmK1IrN3BKc0dwUndSNXdQN0MKS003YlhVc2ZTY1pTUnM0ZE1VK0t6T0NnaW1obHBMMkRObG0vRkwwM0JtR2xFWWZ4Q29rNEY0ZXUvMmx4OVVxSQphTUk1dytKK1YrNVhwRi9IUzdsbTlKc1hldnRuQ0JUcDVnS3F2eDkzUkZhK0o1dDhZazB3SmtxQmRXOGZlSEdWCkYvMExoMTNzbFF5c1ZMNjEwL01rc3FuM0c5ZWJENG14UWN1bTZKZDFOVjlMd0ZjSFpCTFhjN2QwQXBSTVdjM0wKTWRuWmZPQ0VMd3U1Mk5rVExmZ3R1djJwb1hSOGYvRUtJM1AwTXNFYmpaSmRBeXVJNnJUMzliTy9pWTUrZVpCQgovMm8xdzdKVEYzb1N5ZmxOK0hqd21leW81dTBXbW51blVBdDl3dnZoZ2FxQVBIVXJrQU9POTdEekNMKys4ODhwCks4QUh1cFhsT1huaEpiWjFCb0VHZFhydU91SmxzdU5qTktHanJ0aUNYdkcwYUpnTmpQcFRmSVF5bkxMYkJId1YKMnJIYlNPcEFwZHpUZTZkRGZ3a2RsMWgxWEU2SUFVdUhJR2NTQWNKQlIyd253bDd0MzF4blBMMUpxK3BVUW45TAo5d0lEQVFBQm8xTXdVVEFkQmdOVkhRNEVGZ1FVeWhXKzY2aU1xbGZtSWUvUmFvbWZ4dWxLVWhZd0h3WURWUjBqCkJCZ3dGb0FVeWhXKzY2aU1xbGZtSWUvUmFvbWZ4dWxLVWhZd0R3WURWUjBUQVFIL0JBVXdBd0VCL3pBTkJna3EKaGtpRzl3MEJBUXNGQUFPQ0FnRUFPVUluZHR0RXI0Vk8vQzUzNU1hVWN5cHhpQXYyS0lRQ2N3RWl4dHJIenFOaAppODU0SWlRdEc4ZCt2RWlLMHJBYzFOeXpRQzFYUG5hUUpoMlNZaWRwaE5Tc3RsK2hIci9xSkswWG9vRE9pN1FCCmd6QTVuS20zZ0g4TWkrSU1qdDNoRTF3R3g3elBtR1MvM0hCK1R3WjVwSURwNGNuS1NQdkdTUEZUOGNjTnhEcUgKR2g0cHk3WVgwOE44M0p2eUVrbW5LVEljZGIvYWo3bXF6WEMyd244UHhxM3lReTRWeXpzamwrdmVmS1ZWL3J3UwpvUFdjcndsNDVaaXRwbElUMVkyYU56TTBnWVViMWwwRlRDb3pZYUdFczFjV2lnbGkxV1poYzVkWHFiZE11ZHNOCldmSzRMdmVERE5kSXg1bFUvaUswZ3VsWFpFV3B5bGErRXdkWXl1NStLQzF0Rm1zOG51c3JsVmtFRkViRFVtaWUKTU1iZXBKdUp1RG93MnVKSjh2T2o2dUpmV2EwdjZ3NThORnQ2d1hpWUdMQnZiOTlqK2pyTS82ZEpNWUY4Q2RqcwphZ3orYzhPQnY1NnpWUjZ2YVNOd1djM1BQbElaZ0lEOXI5aWNUaWVQeGFBS0VnWjhQQ3YrY1JDNDVOZ2pzZTNUCm1ZakJhK0V5bnR5cDU4Mkpuc2UrS0FnY3o5NmxpWEpJUFIxR2xoR1Y4VHVudFlZYWNMdXR1SmQxQTUzdFdpVEUKRWZnQzcwNlNxYnp6cm1XVUVwWENVckZmWW9DNklpYXJaUWJWQXNIcHFvbm9vRUtJdTlBSXcySkY3c1hGb29FVQpnSjhFNCtjN3JQVGdIeVh3TktWbHlQekZteWVUbHVjYXFnaVFJOWR2T3E3TjdrbkxLMFVodFFUQlpnUWg5U3M9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    tls.key: LS0tLS1CRUdJTiBQUklWQVRFIEtFWS0tLS0tCk1JSUpRd0lCQURBTkJna3Foa2lHOXcwQkFRRUZBQVNDQ1Mwd2dna3BBZ0VBQW9JQ0FRQ3hJRUdGSS9OeFNBeFYKUFhoMW8rUjdORy9XVUxEZDhCZ0paZ3JMMkVWdXVmWmg4VVlvWUpzUmxXdjA3dkF6cTA3RHpnWlV6ejVxbmppTAo5cEdiMUMyT0dkL1Fjb2cxdTFQa0tFVnVGMlIrdDBmOWhxR1l5M0ZVcjhzaXNUSmh2UEhhWU4wRVlLcVRxRFpYCk9IWkhuQXVlYkJ5alhpZjFkQXBlSTMyelVDZmxqMHA5SU5BR1FKb0tFaDR0azZqdnJ2b3dvdXRJeTdJanZydlMKai9OdVpGem9pemYrUis3cEpzR3BSd1I1d1A3Q0tNN2JYVXNmU2NaU1JzNGRNVStLek9DZ2ltaGxwTDJETmxtLwpGTDAzQm1HbEVZZnhDb2s0RjRldS8ybHg5VXFJYU1JNXcrSitWKzVYcEYvSFM3bG05SnNYZXZ0bkNCVHA1Z0txCnZ4OTNSRmErSjV0OFlrMHdKa3FCZFc4ZmVIR1ZGLzBMaDEzc2xReXNWTDYxMC9Na3NxbjNHOWViRDRteFFjdW0KNkpkMU5WOUx3RmNIWkJMWGM3ZDBBcFJNV2MzTE1kblpmT0NFTHd1NTJOa1RMZmd0dXYycG9YUjhmL0VLSTNQMApNc0VialpKZEF5dUk2clQzOWJPL2lZNStlWkJCLzJvMXc3SlRGM29TeWZsTitIandtZXlvNXUwV21udW5VQXQ5Cnd2dmhnYXFBUEhVcmtBT085N0R6Q0wrKzg4OHBLOEFIdXBYbE9YbmhKYloxQm9FR2RYcnVPdUpsc3VOak5LR2oKcnRpQ1h2RzBhSmdOalBwVGZJUXluTExiQkh3VjJySGJTT3BBcGR6VGU2ZERmd2tkbDFoMVhFNklBVXVISUdjUwpBY0pCUjJ3bndsN3QzMXhuUEwxSnErcFVRbjlMOXdJREFRQUJBb0lDQUVzTkNOWWFRcGVZV0tseGoyNEJVTm1uCmdVL0ZCdnd6Tkd3ZkVhb0g3SHViVGp0MzJTdFlxN3dLblNDK2NZYmpLaytySjJHZUlxdFllVXRNNGF1dWZ0TDgKUWFwNExTRGZUQStieExkK0wvWEZRVHE2WmhSdzk0ZFRxWFg4c1FWYTR2dU9HWGcyWVhDSlVPVXFKbXYzUWJFUgp6QnlvMDdPY1JKeFkyM1UwSElPWGNJK09iTGFKZnZ1UGE3d1NHZjNkSFdPb1ZKdVd1OWVMaE5nV2FLWTQzbWttClBFYUJRa2tHUS82dStjV0JNakVneTIyRm5VUVhwWEhOVUo1ciszRXJtRzVOMXloaVltNkxoQUdFRHh5V2xwOHoKeTI4bDhMaVV0OGc5cnZjN1lTWmxoUTZtNG54NFU2Uk5JTXc0V01tNEVWTWtxQXVqRUFGL2xWZTEvZVl5VGJJMgpOWlpDdVVkaitjbzdNZDNsa2dwcnY4SHZ1VlJ6Y1EvZ2VWUHRSZThPNng4TDdFU1IzWkhtRXl1bVpwNjZwb2FhCjVZRWdnNUErVDU2UlBvZXJxaVBDYkkyQVJZWW1OWmxSY3BPZ0NIUTZ3ZFBoTFZheXoycmlnQ2pjS1hhaTZURjAKRG1qM2pIQ09PbXVFcHRZZU5CT0QrVHFFYWR1MDFMVGY5MFhPeU9TellHbm5lQ3lwUStTS0VHdnA3TlFxN0hrdgovR3RYcFhva2oxRDQ2OHdEU2dMZW44SEk5UlE0V1BvK2JNc2x2OCs4UWlVNHVzbnpGSm1NZmlUZGxhWjBLdnBYCkdVb1laTEVDYWhFRm41WU9sbVlaT1VhQWlMSklNWDlONkFNc3A0ei95VEE0cVVoRW5xREpCM0lhdUEvL0pMT1EKaGZSS3I2a2IwTzNPMVdieVVlenhBb0lCQVFEZjNGajV5bndmcXY1OFYwVWJmekZxSkNsNWFjSnA4MTdiOC9TaQp5NXkxeTdsNld3TExVM0d3WDE4eE1HUkdNSndVUUkzbnZCSEhWRnlTdzNIaGxmREFkcGh6RHlJNUNjUVU0dndaCmVHaXhjcFpFRG5IaGYwSDJ6T0lGRnBXdDFNNEd6a3NmK1RnSHNRSHZJM0hQajVDNjhUcStlVWJJdDRuUWdUODIKczBmQWRFeFpJeFJrdEpMem0yU1dnVTFEZE5Td0YrRWVCN2hPTnpnWDNpOHY2OXJCdWNqdUg4QVdiV3N3K2RjZAppZy96T1ZTYkJCdStETS9EQlFxYXhpakpOMG9Dbi9kbDdMSjhjOFZ3bkwvVEc0Tm5XdDZDK3VOR0dUbTZ6U3BaCmlGZjc5QTVuTjlwS2RGa2VQZWhlQnRUbzM3U0FEUE0xR0JSSHd1VmsrSElQMGxTL0FvSUJBUURLamo3L1lkK2wKdldnWm0zMHUrUUkxbnlnM0dHY1VieHY5THRUY0VLeXBkb1RpSlMrTUxzUEZRcUd5YkFHY0o0eGxra0cyQ3dFUQpFb3dKOGltQVphaDlXZG40OXlySGJVTm9INW90TnRyWTUxdzloSUI5bGxHOVk1OUMvSWxJRW8yMkZ2Y2plSTFXCjRxSXRIK3JkL3Q2a2V3ODdSM0pvOU15ZDNsV1lNcE9ZNzh5WHB4Rlg3Ym9lRnpwc1ZOWURWTzR3QmE0eGV3MWcKUEhtUlE4QzUzZUpWd1dVS2xzbWdSSEVZQzMwVGt5eEYydU5FTFVucFdLcmVRbXBOM0R3TWxXcFovT2YvRXFHTgphc1NOUzArL2tNZlpsN0F4aEpWeHpGejZnUTFQL3RwYkRUcGlwRG4yWXpGZUJtOE1HUS84Rlk1OVduWlhFMnRyCloyWk1XcnQ0eTc3SkFvSUJBUUNzOXliMFVlUlgzNW5qM3RZeHFiUTNpNXRVQ1VoQzd0enpXK3BBUXN4aGx2aEgKdHJ4UTk3ZFhERW1UeXcwZlFuM0dGQjdRMTNweEpoaWsrVWdyZ2R2VUNZNC9FSWxqd3N6elNuSjVCNVQwemxHVwpZZ2JSc2E3NUQxTHZsWVN2SEViWDhWc1FhRkpIZHhmRWV3RjcyelI3ak5uVHhBYlNIU1hwcVlON00waVVSZ2ViCnM4UVZENFNmbndnNFZjMnAra0kva1NQS1BUTEZsRnJON2tsTllKSFVyMFMxNEdoZHE4dHZ1d3JmOHdYaDZ3RVoKQ1RLYVJISGZBQzB5YXp1bVJRYjFRajQ0VFl1WEp5aDltMCtId0xGbUVVcnRyd1lkNm0yMWpNSlZEVWpXRHJ0MgpPeXg1N0szUzlRaHVaaDdwazdkMlhRc3BrZUpSbENBRVJRWjBmUkluQW9JQkFITU4vWWErcDNUVGM1QW9IQ0ZVClhBYnRVc0NJNGZSZmNIeU4zMmJwS2NwUWdnYWFyTGxwenRYN2xURnQzRFJBMnFUUFFQZ1FwQmZuRVJpTkx6bTUKaE0wKy9tdEdxa3dCS21xNG1MRGFHZEZmQ2F2LzJhUjhnQTJkeXRjWDd6cTdIemV4TDh3OEs3eVFteUlhb1NSYwpJMGMyaDE1YXBRZ3RGUlVQTjN0dUx4eU9DWjZTelcwdW9hdDU4anVhckwzVHZrQVUyZTlOUURuTDRCbTliSG1uCktXc2dvUzQwbkc2bXNiN0F0OWtvbmR0SURCT3J6ZkgzNVlhK0h2Zi9BelQ2b2lCZlljQW5heENTOXZaek11ZFQKYTlHVE1nZk9rYnpFSW9SQVRibUV2Njg4Z2srSmpVc0E2UWZKaThaSmJpVDRjYW1PZXUzWElBc3YvcEdjY0NTYgpSdEVDZ2dFQkFJeC9uS29ZS1ozUFlHUEtjZHdKazZmb2VWRnNtNkpnYkpBWVhRclBzZkY5UzBOZnU1Yno4dHE2CkZzTEVtU1VRNGZYTVllek9JaGh4WkhHWnpSbWpDVGVoZ3l6OUxQeGNBQ3MvWEl0Zkd5czdRc2RJakcwU0p3QTAKK215YzZBazJpSzBWeWREK0NXcWdoa3NYa1NZVloxVTB3VEZPV09JZWRCOXROSnc4ZFJyMWR4LzRjVTY3RjdGOApPb0hFVUNvUXZHcHdkTlkzQ1o0VFM1Z0hsdHUyY0szQnZRT2RWUGJBNGt6N1FoMHR0bktabWZZTjRnS1hUVFJ4CmlwYWoxU3daOEJyNE1mZVBCOENyOU1RcjFCdkc2RXFHajBwSVRpelFidXU4UERoVkduaGJQdWpiTEdYYnM5Q0YKenlMS3czRHVNbWhkcWM5OXd5ZkxYZlE0dFp3SHcrcz0KLS0tLS1FTkQgUFJJVkFURSBLRVktLS0tLQo=
  kind: Secret
  metadata:
    creationTimestamp: "2024-11-25T10:05:56Z"
    name: secret-tls
    namespace: dz8
    resourceVersion: "157857"
    uid: abdbb61d-0a9e-49d9-b90b-b3fcca811b8c
  type: kubernetes.io/tls
kind: List
metadata:
  resourceVersion: ""

```

[ingress.yaml](https://github.com/smabramov/K8s-1-8/blob/b40bc6ac68d4222b61ebe04b32d87fdfb63f0f0a/code/2/ingress.yaml)

[service.yaml](https://github.com/smabramov/K8s-1-8/blob/b40bc6ac68d4222b61ebe04b32d87fdfb63f0f0a/code/2/service.yaml)

```
serg@k8snode:~/git/K8s-1-8/code/2$ kubectl apply -f config_map_my.yaml 
configmap/configmap-nginx-multitool created
serg@k8snode:~/git/K8s-1-8/code/2$ kubectl apply -f service.yaml
service/service-https created
serg@k8snode:~/git/K8s-1-8/code/2$ kubectl apply -f ingress.yaml 
ingress.networking.k8s.io/ingress-https created
serg@k8snode:~/git/K8s-1-8/code/2$ kubectl apply -f deployment.yaml 
deployment.apps/netology-deployment created
serg@k8snode:~/git/K8s-1-8/code/2$ kubectl get all -n dz8
NAME                                       READY   STATUS    RESTARTS   AGE
pod/netology-deployment-755686fd57-2fnw9   1/1     Running   0          5s

NAME                    TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/service-https   NodePort   10.152.183.37   <none>        80:32000/TCP   5m22s

NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/netology-deployment   1/1     1            1           6s

NAME                                             DESIRED   CURRENT   READY   AGE
replicaset.apps/netology-deployment-755686fd57   1         1         1       6s

```
![k10](https://github.com/smabramov/K8s-1-8/blob/b40bc6ac68d4222b61ebe04b32d87fdfb63f0f0a/png/k10.png)

![k11](https://github.com/smabramov/K8s-1-8/blob/b40bc6ac68d4222b61ebe04b32d87fdfb63f0f0a/png/k11.png)



### Правила приёма работы

1. Домашняя работа оформляется в своём GitHub-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, а также скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

------
