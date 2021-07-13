---
layout:     post
title:      "Backup And Restore Objects In GKE"
subtitle:   "How To Bakup And Restore Objects In GKE Cluster"
date:       2021-07-13 4:00:00
author:     "Thaung Htike Oo"
header-img: img/black.jpg
catalog: true
tags:
    - Kubernetes
    - GKE
---

<h2> How To Backup And Restore Objects In GKE Cluster </h2>

Kubernetes Cluster တွေပေါ်မှာ deploy လုပ်ထားတဲ့ workload တွေနဲ့ တစ်ခြားသော resources တွေကို backup လုပ်ဖို့ဆို velero ဟာ အကောင်းဆုံး tool တစ်ခုဖြစ်တယ်ဆိုတာ အားလုံးလက်ခံကြမှာပါ။ on-prem မှာရှိတဲ့ 
kubernetes cluster တွေကို minio နဲ့ velero ကိုတွဲသုံးပြီး backup and restore လုပ်ကြပါတယ်။ ကျွန်တော် ဒီနေ့မှာတော့ GKE cluster တစ်ခုပေါ်မှာ deploy ထားတဲ့ workloads တွေကို velero သုံးပြီးဘယ်လို 
backup လုပ်မလဲဆိုတဲ့အကြောင်းပဲဖြစ်ပါတယ်။

<h2> Prerequities </h2>

```yaml
google clod console
gke cluster
kubernetes basic
````
<h2> Why Velero? </h2>

Kubernetes Cluster တွေကို backup ဖို့ tools တွေအများကြီးထဲက မှ velero ကိုဘာလို့ ရွေးချယ်ရလဲဆိုရင် velero က opensource ဖြစ်ပြီး on-prem cluster တွေအတွက်ကော cloud cluster တွေအတွက်ကော support
လုပ်ပါတယ်။ အသေးစိတ်ကိုတော့ velero ရဲ့ official doc မှာဖတ်နိုင်ပါတယ်။ ဒါဆိုရင်အခုပဲ lab ကိုစလိုက်ရအောင်နော်။ 

GCP console ထဲကနေ Kubernetes Engine ဆိုပြီးရှာလိုက်ပါ။ ပြီးရင် node (၃)ခုနဲ့ cluster တစ်ခုကိုဆောက်ပေးပါ။ cluster create ပြီးရင်တော့ အောက်ကလိုတွေ့ရမှာပဲဖြစ်ပါတယ်။
![gke](https://raw.githubusercontent.com/thaunggyee/thaunggyee.github.io/master/img/gke.png)

ပြီးသွားရင် ဘေးက အစက် (၃)စက် ကနေ connect ကိုနှိပ်လိုက်ပါ။ 
![connect](https://raw.githubusercontent.com/thaunggyee/thaunggyee.github.io/master/img/connect.png)
