---
layout:     post
title:      "Gitlab Runner Autoscaling"
subtitle:   " Gitlab Runner Autoscaling With GCP Preemptible VMs"
date:       2021-07-14 12:00:00
author:     "Thaung Htike Oo"
header-img: img/black.jpg
catalog: true
tags:
    - Gitlab CI
    - GCP
---

<h2> Gitlab Runner Autoscaling With GCP Preemptible VMs </h2>

<h3> Introduction </h3>

ကျွန်တော်ဒီနေ့ရေးမှာကတော့ goole cloud က preemptibel vms တွေနဲ့ gitlab runner ကို autoscale လုပ်တာပဲဖြစ်ပါတယ်။ ဒီ topic က ကျွန်တော့်သူငယ်ချင်းတစ်ယောက် ရေးထားတာလေးကိုသဘောကျလို့ ပြန်ရေးလိုက်တာပါ။ သူကတော့ aws spot instances တွေနဲ့ ရှင်းပြထားပါတယ်။ ကျွန်တော်ကတော့ google cloud ကိုလေ့လာနေတော့ gcp နဲ့ပဲရှင်းပြလိုက်ပါတယ်။ အမှန်အတိုင်း ဝန်ခံရရင် gitlab runner autoscaling အကြောင်းကို တစ်ခါမှမစဥ်းစားမိခဲ့ပါဘူး။ ဘာလို့လဲဆိုရင် ကျွန်တော့်ရဲ့အလုပ်မှာက autoscale သုံးဖို့လောက်အထိ pipelines တွေမများလို့ပါ။ ဟုတ်ပြီ.. ဒီနေ့ကျွန်တော်စာဖတ်လို့နားလည်သလောက်ကို demo တစ်ခုနဲ့ ရှင်းပြလိုက်ပါတယ်။

<h2> Why Runner Needs Autoscaling </h2>

Runners တွေဟာ ဘာလို့ autoscale လုပ်ဖို့လိုတာလဲ။ များသောအားဖြင့် ကျွန်တော်တို့တွေဟာ runners တွေကို docker executor နဲ့ပဲ register လုပ်ကြပါတယ်။ ပြသနာကအဲ့မှာ စတော့တာပါပဲ။ DevOps ၊ SRE team တွေမှာ လူနည်းရင်တော့ သိပ်ပြီးမသိသာပါဘူး။ CI CD pipeline တွေကို လူတွေအများကြီးက ပြိုင်တူ run တဲ့အခါ docker နဲ့ register လုပ်ထားတဲ့စက်က CPU overload ဖြစ်ပြီး ကောင်းကောင်းအလုပ်မလုပ်နိုင်တော့ပါဘူး။ runner တွေအများကြီး register လုပ်ခြင်းကလည်း technical ဘက်ကကြည့်ရင်သိပ်ပြီး နည်းစနစ်မကျပါဘူး။ ဒါဆို cloud ပေါ်က compute engine ကို scale up လုပ်ဖို့ဆိုရင်လည်း စဥ်းစားစရာဖြစ်လာပြန်ရော။ runner လေးတစ်ခုအတွက်ပဲသုံးမယ့် vm အတွက် ဘာလို့ တစ်လ ကို ပိုက်ဆံတွေအများကြီးပေးနေရမှာလဲ။ အဲ့ဒီအတွက် ဖြေရှင်းဖို့ အဖြေကတော့ spot instances တွေနဲ့ autoscale လုပ်ခြင်းပဲဖြစ်ပါတယ်။ spot instance ဆိုတာ normal instance တွေထက်  70-90% လောက်အထိ စျေးသက်သာပါတယ်။ ကိုယ်လိုတဲ့အချိန်မှာသာ run နိုင်ပြီး ပြီးသွားရင်ပြန်ပိတ်ခဲ့လို့ရပါတယ်။ အထူးသဖြင့် batch job တွေ run တဲ့အခါသုံးကြပါတယ်။ 

<h2> Runner Manager </h2>

Runner manager လို့ပဲခေါ်ခေါ် bastion host လို့ပဲခေါ်ခေါ် အတူတူပါပဲ။ သူရဲ့အဓိက တာဝန်က runner တွေကို scale in ၊ scale out လုပ်ပေးဖို့ပါပဲ။ ဒါဆိုရင် ပထမဆုံးအနေနဲ့ runner manager အတွက် compute engine တစ်ခုကို create လုပ်ပေးရပါမယ်။ ec2-micro လောက်ဆို လုံလောက်ပါပြီ။ runner manager ကို API တွေကို ccess ရအောင် Access Scopes မှာ allow full access to all cloud api ကိုရွေးပေးပါ။ ssh နဲ့ တစ်ခြား services တွေအတွက် firewall rules တွေသတ်မှတ်ပေးပါ။ အားလုံးပြီးသွားရင်တော့ အောက်က လို vm တစ်ခု ရလာပါလိမ့်မယ်။

![vm](https://raw.githubusercontent.com/thaunggyee/thaunggyee.github.io/master/img/vm.png)

vm create ပြီးရင် vm ထဲကို ssh ဝင်ပြီး runner register ရပါတော့မယ်။ register မလုပ်ခင်မှာ autoscaling အတွက်လိုအပ်တာတွေလုပ်ပေးရပါဦးမယ်။ runner config ထဲမှာ runner cache တွေသိမ်းဖို့အတွက် လိုအပ်တဲ့ bucket တစ်ခု create ရပါမယ်။ ဒါကြောင့် bucket တစ်ခုကို create လိုက်ပါမယ်။ bucket နာမည်ကိုတော့ tho-gcs လို့ပေးလိုက်ပါတော့မယ်။

![gcs](https://raw.githubusercontent.com/thaunggyee/thaunggyee.github.io/master/img/gcs.png)

bucket create ပြီးရင် runner config ထဲမှာ compute engine တွေ scale လုပ်ဖို့ အတွက်ရယ် cache တွေ bucket ထဲမှာ သိမ်းဖို့ အတွက်ရယ် api တွေနဲ့ access ရဖို့ service account တစ်ခုဆောက်ရပါမယ်။ အဲ့ service account ရဲ့ key ကို download ပြီး runner config ထဲမှာ ထည့်ပေးရပါမယ်။ ဒါမှသာ runner အတွက် vms တွေ scale in ၊ scale out လုပ်နိုင်မှာပါ။ 

![sa](https://raw.githubusercontent.com/thaunggyee/thaunggyee.github.io/master/img/sa.png)

service account create ပြီးရင် ဘေးကအစက် (၃)စက်ကနေ manage key ကိုနှိပ်ပြီး key ကို download လုပ်ထားပေးပါ။ အားလုံးပြီးသွားပြီဆိုရင် runner manager vm ထဲကို ssh ဝင်ပြီး runner register လုပ်ပါတော့မယ်။
