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
