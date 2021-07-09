---
layout:     post
title:      "LXC & ANSIBLE"
subtitle:   "Introduction To Ansible Ad-Hoc Commands & Playbooks"
date:       2021-07-09 3:00:00
author:     "Thaung Htike Oo"
header-img: img/black.jpg
catalog: true
tags:
    - Ansible
    - IAC
---

<h2> Ansible မိတ်ဆက် </h2>
<p>
ကျွန်တော်ဒီနေ့ ရေးမယ့် ခေါင်းစဥ်ကတော့ Ansible အကြောင်းပဲဖြစ်ပါတယ်ခင်ဗျာ။ Ansibleဆိုတာ Configuration Management Tools တွေထဲက popular အဖြစ်ဆုံး tool တစ်ခုဖြစ်ပါတယ်။ Ansible ဆိုတာ ကျွန်တော့်တို့ရဲ့ ရုံးတွေ အလုပ်တွေမှာရှိတဲ့ servers တွေအများကြီးပေါ်မှာ ထပ်တလဲလဲ လုပ်ရမယ့် configuraion tasks တွေအတွက် အချိန်ကုန်လူပင်ပန်းသက်သာစေဖို့ automate လုပ်ရာမှာသုံးမယ့် Tool တစ်ခုဖြစ်ပါတယ်။ Ansible Ad-Hoc Commands တွေ run ခြင်း (သို့) Playbook တွေရေးပြီး run ခြင်းဖြင့် configurations တွေကိုလုပ်ကြရပါတယ်။ ဒါကြောင့် ဒီနေ့မှာ LXC containers ၃ခုကို သုံးပြီး ansible ကိုလေ့လာလိုက်ကြရအောင်ဗျာ။
</p>

<h2> LXC ဆိုတာဘာလဲ </h2>
<p>
LXC ဆိုတာ linux containers တွေကိုခေါ်ဆိုခြင်းဖြစ်တယ်။ container တွေဆိုတော့ ရှိပြီးသား image တွေကို ကျွန်တော်တို့အဆင်သင့် ခေါ်သုံးရုံပါပဲ။ VM တွေထက်စာရင်လည်း မိမိရဲ့ laptop မှာ resource အတွက်နေရာသိပ်မယူတော့ စိတ်မပူရတော့ပူပေါ့။ ဒါဆိုအခုပဲ lab လေးကိုစလိုက်ကြရအောင်နော်။
</p>

lxc ကိုအသုံးပြုဖို့ ကျွန်တော်တို့ရဲ့ localhostမှာ lxc lxd ကို install ပေးဖို့လိုပါတယ်။
```bash
apt update -y
apt install lxc lxd -y
```
lxc container တွေ create မလုပ်ခင်မှာ lxd init နဲ့ initialize လုပ်ဖို့လိုပါတယ်။ ပြီးရင်တော့ အောက်ကterminalအတိုင်း values တွေသတ်မှတ်ပေးရပါမယ်။ ဒီ lab မှာတော့ values တွေကို default အတိုင်းပဲထားပြီး enter တစ်ဆင့်ချင်းခေါက်သွားရုံပါပဲ။
```bash
root@node01:~# lxd init
Would you like to use LXD clustering? (yes/no) [default=no]: 
Do you want to configure a new storage pool? (yes/no) [default=yes]: 
Name of the new storage pool [default=default]: 
Would you like to connect to a MAAS server? (yes/no) [default=no]: 
Would you like to create a new local network bridge? (yes/no) [default=yes]: 
What should the new bridge be called? [default=lxdbr0]: 
What IPv4 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]: 
What IPv6 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]: 
Would you like the LXD server to be available over the network? (yes/no) [default=no]: 
Would you like stale cached images to be updated automatically? (yes/no) [default=yes] 
Would you like a YAML "lxd init" preseed to be printed? (yes/no) [default=no]: 
```


