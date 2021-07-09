---
layout:     post
title:      "LXC & ANSIBLE"
subtitle:   "Introduction To Ansible Ad-Hoc Commands & Playbooks ( PART-II)"
date:       2021-07-09 3:00:00
author:     "Thaung Htike Oo"
header-img: img/black.jpg
catalog: true
tags:
    - Ansible
    - IAC
---

<h2> PART-I </h2>
 
PART-I မှာတုန်းက lxc container (၃)ခုကိုသုံးပြီး ansible master တစ်ခုရယ် worker နှစ်ခုရယ်ကို configure လုပ်ခဲ့ပြီးပါပြီ။ PART-II မှာတော့ ad-hoc commands တွေနဲ့ playbook အတွေအကြောင်းကိုအတူတူ လေ့လာလိုက်ရအောင်နော်။

<h2> Ad-Hoc Commands </h2>

Plyabook တွေမရေးခင်မှာကျွန်တော်တို့တွေ ad-hoc command ဆိုတာဘာလဲအရင်လေ့လာလိုက်ကြရအောင်ဗျာ။ ad-hoc ဆိုတာ worker nodes တွေမှာတစ်ခုခု run ဖို့ရေးလိုက်တဲ့ short command လေးပဲဖြစ်ပါတယ်။ တစ်နည်းအားဖြင့် worker တစ်ခုပေါ်မှာ run မယ့် linux command ကို မိမိအလိုရှိတဲ့ worker တွေပေါ်မှာ run လိုက်တာပဲဖြစ်ပါတယ်။ ဥပမာ- node01 နဲ့ node02 ၂ခုလုံးမှာ df -hT ဆိုတဲ့ command ကို run မယ်ဆို node တွေထဲကို login ပြီး terminal ကနေ df -hT ဆိုပြီးသွားရိုက်ထည့်ရမှာဖြစ်တယ်။ ad-hoc command ဆိုတာ အဲ့လို node တစ်ခုချင်းစီကို terminal ကနေ မဝင်ပဲ master node ကနေပဲ command ကို run ပြီး server တွေအပေါ်မှာ သက်ရောက်မှုရှိစေတာပဲဖြစ်ပါတယ်။ 

ansible မှာ module တွေလည်းရှိပါသေးတယ်။ packages တွေ install ဖို့ apt ၊ yum ၊ dnf စတဲ့ module တွေ ၊ file တွေ directory တွေအတွက် file module ၊ copy ဖို့အတွက် copy module | user တွေ create delete ဖို့ user module ဆိုပြီး သတ်မှတ်ထားပါတယ်။ အခြား module တွေလည်းအများကြီးကျန်ရှိနေပါသေးတယ်။ ad-hoc commands အနည်းငယ်ကို လေ့လာကြည့်ရအောင်။ 

shell module ဟာ ad-hoc command တွေမှာ defautl module ဖြစ်တယ်။ module ကို -m နဲ့ ကိုယ်စားပြုတယ်။ အောက်မှာတွေ့ရတဲ့ command ဟာ worker nodes တွေရဲ့ df -hT ကိုလှမ်းကြည့်တာဖြစ်တယ် ansible အနောက်က all ဆိုတာကတော့ all worker nodes ကိုဆိုလိုခြင်းဖြစ်တယ်။ inventory ထဲမှာသတ်မှတ်ခဲ့တဲ့ webserver တွေမှာပဲ run ချင်ရင် all နေရာမှာ webserver လို့ထည့်ပြီး dbserver တွေကိုပဲ run ချင်ရင် dbserver လို့ထည့်ပါတယ်။ shell နဲ့ command module နှစ်ခုလုံးဟာအတူတူပါပဲ။ linux command တွေကို run ဖို့သုံးတာပါ။
```bash
ansible -m shell -a 'df -hT'
```
နောက်တစ်ခုက haproxy ကို worker အကုန်လုံးမှာ install မှာဖြစ်ပြီး -b --become-user=root က root အနေနဲ့ run မယ်လို့ဆိုလိုတာပါ။
```bash
ansible all -m shell -b --become-user=root -a 'apt install haproxy -y'
```
နောက်တစ်ခုကတော့ file module ဖြစ်ပြီး file တွေ directory တွေကို create လုပ်ခြင်း delete လုပ်ခြင်း permission ပြောင်းခြင်းစသည်တို့အတွက်သုံးပါတယ်။ stateက file လား directory လားဆိုတာသတ်မှတ်တာပါ file ဆို state=touch ဖြစ်ပြီး directory ဆို directory ဖြစ်ပါတယ်။ အောက်က cmd ကတော့ webserver nodes တွေမှာ test ဆိုတဲ့ file ကို 777 permission နဲ့ /root အောက်မှာ create လုပ်မှာဖြစ်ပါတယ်။
```bash
ansible webserver -m file -a 'dest=/root/test mode=777 state=touch' 
```
ကဲ.. ဒီလောက်ဆို ad-hoc commands တွေအကြောင်းကိုနားလည်မယ်ထင်ပါတယ်။ တစ်ခြား module တွေကိုတော့ ကိုယ်တိုင် လေ့လာကြည့်ကြဖို့တိုက်တွန်းပါတယ်။

<h2> Ansible Playbook </h2>

