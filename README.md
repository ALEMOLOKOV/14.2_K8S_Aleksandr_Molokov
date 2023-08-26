# 14.2_K8S_Aleksandr_Molokov

### Задание 1. Установить кластер k8s с 1 master node

1. Подготовка работы кластера из 5 нод: 1 мастер и 4 рабочие ноды.
2. В качестве CRI — containerd.
3. Запуск etcd производить на мастере.
4. Способ установки выбрать самостоятельно.

### Ответ

#### Развернутые  5 ВМ в YandexCloud, 1 master и 4 worker

![VM YandexCloud](https://github.com/ALEMOLOKOV/14.2_K8S_Aleksandr_Molokov/assets/109212419/ffdab4e4-9f98-4f2c-bdcd-046d5e5a5a1b)

#### В качестве CRI — containerd.

![containerd master node](https://github.com/ALEMOLOKOV/14.2_K8S_Aleksandr_Molokov/assets/109212419/0572a762-393c-4606-bf07-b94d30ada023)

#### Выбрал установку с помощью Kubeadm. Версия K8S 1.28.0

#### Добавление K8S репозитория

echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

#### Скачивание и импорт GPG key

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

#### Обновление пакетов и установка необходимых инструментов, а также команда запрет их обновлния

sudo apt-get update

sudo apt-get install -y kubelet kubeadm kubectl containerd

sudo apt-mark hold kubelet kubeadm kubectl

#### Инициализация кластера мастер нода

kubeadm init \
 --apiserver-advertise-address=10.129.0.23 \
 --pod-network-cidr 10.244.0.0/16 \
 --apiserver-cert-extra-sans=158.160.5.15

#### Устранение проблемы с форвардингом

![ошибка форвардинга](https://github.com/ALEMOLOKOV/14.2_K8S_Aleksandr_Molokov/assets/109212419/e55490cc-d2ce-49a3-a0a7-9df206120760)

#### Установка flanel

![install flanel](https://github.com/ALEMOLOKOV/14.2_K8S_Aleksandr_Molokov/assets/109212419/fcf64454-1dad-45fc-a254-b99e4df96bdf)

#### Успешная инициализация кластера

![успешная инициализация кластера](https://github.com/ALEMOLOKOV/14.2_K8S_Aleksandr_Molokov/assets/109212419/18e4a655-e889-43fd-94d6-06e9d5c3cbc9)

#### Мастер нода готова

![MASTER ready](https://github.com/ALEMOLOKOV/14.2_K8S_Aleksandr_Molokov/assets/109212419/59cb10dd-adbf-4532-87d3-92497ccc1cb7)

#### Kube config

![mater node kubeconfig](https://github.com/ALEMOLOKOV/14.2_K8S_Aleksandr_Molokov/assets/109212419/77a25b7e-dcb7-49f2-a2ed-bd4028e06cec)

#### Подключение вокрер нод (команда появится при успешной инициализации кластера на мастер ноде)

kubeadm join 10.129.0.23:6443 --token 6ns12g.aq7reyieerebw16y \
        --discovery-token-ca-cert-hash sha256:ab7a8147075a324c5e5893e9c137922522d6b8b00585d458ed5114cefefebc1a

![worker1 connected](https://github.com/ALEMOLOKOV/14.2_K8S_Aleksandr_Molokov/assets/109212419/630cbec3-72c3-48c1-a0cc-6015aaac57ce)

![worker2 connected](https://github.com/ALEMOLOKOV/14.2_K8S_Aleksandr_Molokov/assets/109212419/4e71d8c4-e41b-4ee4-a7a3-5afa5f466c2e)

![worker3 connected](https://github.com/ALEMOLOKOV/14.2_K8S_Aleksandr_Molokov/assets/109212419/3d62ee1e-efc4-451a-9639-83401c74f34c)

![worker4 connected](https://github.com/ALEMOLOKOV/14.2_K8S_Aleksandr_Molokov/assets/109212419/98234922-7f0f-4f0e-abe0-5299c03d67fa)


#### Кластер готов

![K8S CLUSTER DONE](https://github.com/ALEMOLOKOV/14.2_K8S_Aleksandr_Molokov/assets/109212419/8427e955-fb18-4fc1-b1f6-29ac7627207b)



