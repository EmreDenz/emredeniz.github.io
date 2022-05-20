---
layout: post
title:  "Kubectl Add"
summary: "Kubectl ile configmap ve secret'a editlemeden kolay bir şekilde veri ekleme"
author: emredeniz
date: '2022-05-20 14:35:23 +02'
category: kubectl-plugins

thumbnail: /assets/img/posts/k8s.png
keywords: kubernetes,kubectl, kubectl-plugins, kubectl-add, kubectl add
permalink: /blog/welcome-to-devlopr-jekyll/
usemathjax: true
---

What are tools?

kubectl-add, kubectl'de configmap ve secret'a hızlı bir şekilde veri eklemek için bir araçtır.

**`kubectl add`** demo:

![kubectl add demo GIF](/assets/gif/kubectl-add-demo.gif)

### Örnek kullanımlar

```sh
# configmap'e yeni veri ekleme
$ kubectl add configmap myconfigmap -s mykey:myvalue -n mynamespace
The mykey is adding.
configmap/myconfigmap replaced

# configmap'e yeni çoklu veri ekleme
$ kubectl add cm myconfigmap -s mykey2:myvalue2 -s mykey3:myvalue3 -n mynamespace
The mykey2 is adding.
The mykey3 is adding.
configmap/myconfigmap replaced

# configmap'e veri eklerken veriler zaten mevcutsa
$ kubectl add configmap myconfigmap -s mykey3:myvalue3 -s mykey4:value4  -n mynamespace
error: The mykey3 is already exist.

# force argümanıyla configmap'e veri ekleme
$ kubectl add configmap myconfigmap -s mykey3:myvalue3 -s mykey4:value4  -n mynamespace -f
The mykey3 is already exist, is updating.
The mykey4 is adding.
configmap/myconfigmap replaced

# veriyi base64 ile çözerek yeni veri ekleme
$ kubectl add configmap myconfigmap -s mykey5:bXl2YWx1ZTU=  -n mynamespace -d
The mykey5 is adding.
configmap/myconfigmap replaced

# secret'a yeni veri ekleme
$ kubectl add secret mysecret -s mykey:bXl2YWx1ZQ== -n mynamespace
The mykey2 is adding.
secret/mysecret replaced

# base64 biçiminde olmayan secret'a yeni veri eklemek 
$ kubectl add secret mysecret -s mykey2:randomvalue -n mynamespace
The mykey2 is adding.
error: the input does not conform to the base64 format.

```

Kubectl-add ile birlikte, configmap ve secret düzenlemeden doğrudan yeni veri eklemeyi veya güncellemeyi destekler. Veri eklerken verilen base64 ile kodlamayı veya kod çözmeyi destekler.

-----

## Yükleme

Kubectl-add'nin kararlı sürümleri, [github](https://github.com/EmreDenz/plugins)'da bulabileceğiniz küçük bash komut dosyalarıdır.

Yükleme seçenekleri:

- [as kubectl plugins (Linux)(Manual)](#kubectl-plugins-linux)

### Kubectl Add (Linux)

Kubectl-add Bash'de yazıldığından, bunları Bash'in kurulu olduğu herhangi bir POSIX ortamına yükleyebilmelisiniz.

- `kubectl-add` komut dosyalarını indirin.
- Herhangi biri:
    - hepsini `PATH` içinde bir yere kaydedin,
    - - veya bunları bir dizine kaydedin, ardından 'PATH'inizdeki bir yerden '/usr/local/bin' gibi 'kubectl-add' için sembolik bağlantılar oluşturun
- `kubectl-add` dosyasını çalıştırılabilir yapın (`chmod +x ...`)

Örnek kurulum adımları:

``` bash
sudo wget https://raw.githubusercontent.com/EmreDenz/plugins/main/add/kubectl-add
sudo cp kubectl-add /usr/local/bin/kubectl-add
sudo chmod +x /usr/local/bin/kubectl-add
```

Burada **`kubectl add`** yüklenmesine ait demosuna bakabilirsiniz:

![kubectl add installation demo GIF](/assets/gif/kubectl-add-installation-demo.gif)

### Kullanım:

* Veri dizisindeki değerleri myconfigmap adlı configmap'e ekler. Verilerdeki değerler zaten mevcutsa değiştirilirler.
  kubectl add configmap myconfigmap --data ENVIRONMENT_NAME:VALUE -s ENVIRONMENT_NAME2:VALUE2 -f -n default

* Veri dizisindeki değerleri, default namespace'indeki myconfigmap adlı configmap'e ekler. Verilerdeki değerler zaten mevcutsa, değiştirilmezler.
  kubectl add configmap myconfigmap --data ENVIRONMENT_NAME:VALUE -s ENVIRONMENT_NAME2:VALUE2 -n default

* Veri dizisindeki değerleri, default namespace'indeki mysecret adlı secret'a ekler. Verilerdeki değerler zaten mevcutsa değiştirilirler. Base64 ile değerleri şifreler
  kubectl add secret mysecret --data ENVIRONMENT_NAME:VALUE -s ENVIRONMENT_NAME2:VALUE2 -f -n default -b

kubectl add TYPE NAME DATA [options]

Komutların Tanımları:
TYPE                       : yapılandırma türü. configmap(cm), secret
NAME                       : yapılandırmanın adı
DATA                       : eklenecek/değiştirilecek verilerin listesi. (--set, -s)

Seçenekler:

-n, --namespace: namespace.
-f, --force: Anahtar varsa, değerler değiştirilir.
-e, --encode-base64: Değerler base64 ile şifrelenir.
-d, --decode-base64: Değerler base64 ile çözülür.
