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

<h2> Ansible Architecture </h2>
Ansible မှာ master ရယ် worker ရယ်ဆိုပြီး ရှိပါတယ်။ master ကတော့ control node လို့ခေါ်ပြီးတော့ ansible နဲ့ဆိုင်တဲ့ commands တွေကို run ဖို့သုံးမှာပါ။ worker nodesတွေကတော့ master မှာ run လိုက်တဲ့ playbooks တွေရဲ့ configuration tasks တွေကို လုပ်ဆောင်မှာဖြစ်ပါတယ်။ အခု lab မှာတော့ master တစ်ခုရယ် worker နှစ်ခုရယ် အသုံးပြုသွားမှာပါ။ သူက agent-less ဖြစ်တယ်။ ဒါကြောင့် ssh private key ကိုသုံးပြီး node တွေကို connection ရအောင်ချိတ်ဆက်ပေးပါတယ်။

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
root@thaunghtikeoo:~# lxd init
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
init လုပ်ပြီးသွားရင်တော့ lxc container တွေကိုစတင်အသုံးပြုနိင်ပါပြီ။ ubuntu 20.04 container (3)ခုကိုအောက်ကအတိုင်း create ပေးလိုက်ပါ။
```bash
lxc launch ubuntu:20.04 master
lxc launch ubuntu:20.04 node01
lxc launch ubuntu:20.04 node02
```
ပြီးရင်တော့ launch လုပ်ခဲ့တဲ့ container တွေကို lxc list နဲ့ list လုပ်ကြည့်ပါ။ container တစ်ခုချင်းစီရဲ့ details ကိုတွေ့ရမှာဖြစ်ပါတယ်။
```bash
root@ubuntu:~# lxc list
+--------+---------+----------------------+-----------------------------------------------+-----------+-----------+
|  NAME  |  STATE  |         IPV4         |                     IPV6                      |   TYPE    | SNAPSHOTS |
+--------+---------+----------------------+-----------------------------------------------+-----------+-----------+
| master | RUNNING | 10.28.212.102 (eth0) | fd42:5733:fec6:32aa:216:3eff:fe7b:eeb2 (eth0) | CONTAINER | 0         |
+--------+---------+----------------------+-----------------------------------------------+-----------+-----------+
| node01 | RUNNING | 10.28.212.239 (eth0) | fd42:d592:b400:3321::1 (lxdbr0)               | CONTAINER | 0         |
|        |         | 10.2.243.1 (lxdbr0)  | fd42:5733:fec6:32aa:216:3eff:fe38:4e2a (eth0) |           |           |
|        |         | 10.0.3.1 (lxcbr0)    |                                               |           |           |
+--------+---------+----------------------+-----------------------------------------------+-----------+-----------+
| node02 | RUNNING | 10.28.212.91 (eth0)  | fd42:5733:fec6:32aa:216:3eff:fe53:dc93 (eth0) | CONTAINER | 0         |
+--------+---------+----------------------+-----------------------------------------------+-----------+-----------+
```
ပြီးရင်တော့ lxc container တွေထဲကို lxc exec နဲ့ login ဝင်ဖို့ terminal မှာ tab (၃)ခုဖွင့်လိုက်ပါ။ 
```bash
lxc exec master -- bash
lxc exec node01 -- bash
lxc exec node02 -- bash
```
container တွေထဲကို login ဝင်ပြီးသွားရင်တော့ အကုန်လုံးကို apt update လုပ်ပေးပါ။ ပြီးရင်တော့ ansible master အဖြစ်သုံးမယ့် node တစ်ခုထဲမှာ ansible ကို install ပေးပါ။
```bash
apt update -y ( on all nodes )
apt install -y ansible ( only master )
```
ပြီးသွားရင်တော့ container အကုန်လုံးမှာ ssh private key copy ဖို့အတွက် လိုအပ်တဲ့ configuration အနည်းငယ်လုပ်ရပါမယ်။ /etc/ssh/sshd_config file ထဲမှာ PermitRootLogin ကို uncomment လုပ်ပြီး yes ပေးပါ။ ပြီးရင် PasswordAuthentication ကို yes ပေးပါ။ 
```bash
vim /etc/ssh/sshd_config
PermitRootLogin yes
PasswordAuthentication yes
```
ပြီးရင်တော့ ssh service ကို restart ချပေးပါ။ ssh-copy-id ဖို့ root user ကို password သတ်မှတ်ပေးပါ။
```bash
systemctl restart ssh ( on all nodes )
passwd root ( on all nodes )
```

