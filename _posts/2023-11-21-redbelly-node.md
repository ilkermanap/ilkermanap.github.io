---
layout: post
title: "Redbelly Node Kurulumu"
categories: nodes
---


# Redbelly Node Kurulumu

## Linkler

* [Redbelly Node Onboarding Guide](https://redbelly.atlassian.net/servicedesk/customer/portal/13/article/1960869898){:target="_rnog"}
* [Cüzdan tanıtma ve KYC](https://access.devnet.redbelly.network){:target='ctvkyc'}

## İlk Eposta

Redbelly node kurulumu davet ile yapılıyor. Size de aşağıdaki gibi bir eposta 
gelmiş ise, bu dokümanı okumaya devam edebilirsiniz.



    Hi Ilker​

    

    Congratulations! We are thrilled to inform you that you have 
    been selected to be part of the next group for testing the 
    Redbelly Node Operator Program. This opportunity requires 
    your immediate attention as it is by invitation only and 
    has a time limit to respond.

    Please note that the Redbelly team made a decision to introduce 
    test nodes in a phased approach over the next few months, of 
    which you are part of this next phase. This approach serves 
    the following purposes:

        Controlled Testing: By commencing with a small group, 
        we can ensure more focused and manageable testing of the 
        onboarding process.
        
        Feedback Opportunity: This group will have the opportunity 
        to provide feedback on their experience, allowing us to 
        refine and improve the process.
        
        Timely Support: By managing support tickets within this 
        limited group, we can provide prompt assistance and address 
        any technical issues efficiently before proceeding to the 
        next testing phase and the final Mainnet launch.
        
    Please note that your participation in the testing phase does 
    not guarantee selection as a Node Operator for the Mainnet. 
    However, your commitment and engagement throughout the testing 
    process will be taken into consideration. The testing phase is 
    scheduled to run from 31 May to 30 September 2023.

    If, for any reason, you are unable to participate in this test 
    group, kindly inform us so that we can extend the invitation to 
    you at a later date. If you do not join the Devnet Beta within 
    7 days from this email we will need to offer the opportunity 
    to other shortlisted candidates. 

    You will find a copy of the Node Operator Program here. Within 
    this document you will find everything you need to know about 
    the Program and the next steps.

    We are excited to have you join the Redbelly Community, and 
    we trust that you will adhere to the Rules of Engagement and 
    contribute to the growth of our community. 

    Kind Regards

    Leighlah Ashmore
    Head of Partnerships and Community


## Sistem Gereksinimleri

### Donanım

Redbelly sitesindeki belgelere göre minimum sistem gereksinimi aşağıdaki gibi:

- CPU : 8-core CPU or equivalent
- İşlemci Mimarisi : x86-64 (x64, x86_64, AMD64, ve Intel 64 diye de bilinir.)
- Hafıza : 16 GB 
- Disk Alanı : 1 TB

### İşletim Sistemi ve Portlar

Ubuntu 22.04.2 LTS ya da daha yenisi 

Aşağıdaki TCP portlarının erişime açık olması gerekiyor: 

- 8545
- 8546
- 1111
- 1888

Eğer portlar aynı sunucudaki başka uygulamalar tarafından kullanılıyorsa, kendi seçeceğiniz port numaralarını kullanabilirsiniz. Ancak bu numaraları ileride dolduracağınız formda doğru şekilde vermeniz gerekmektedir. 

### Ağ Gereksinimleri 

* En az 40 Mbps bağlantı
* Kurulan node için 200 ms erişim hızı

