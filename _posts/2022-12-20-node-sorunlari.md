---
layout: post
title: "Node Sorunları"
categories: linux
---

# Giriş

Kriptopara projelerinin testlerinde çogunlukla linux sanal sunucuları üzerinde işlemler yapılır. Katılımcılar genellikle amazon aws, google cloud, hetzner, contabo gibi sanal sunucu hizmeti veren yerlerden sunucu kiralarlar. Bu sunucuların kullanımı sırasında karşılaşılan güçlükleri burada listelemeye çalışıp, değişik sorunlar için olası çözümleri paylaşacağız.

Çoğu problem, temel linux bilgisine sahip olmanız durumunda kolaylıkla çözülebilecek şeylerdir. Ancak test katılımcılarının çoğu, linux sistemlerle ilk defa karşılaştıklarından, basit problemler bile işin içinden çıkılmayacakmış gibi görünebilmektedir. Bu yazıda, linux kullanım becerisine sahip olmayan kişiler için açıklamalar yapmaya çalışacağız.

# Problemler

  * Disk alanı sorunları
  * Screen kullanımı ile ilgili sorunlar
  * Yedekleme


# Disk Alanı Sorunları

Çoğu projede katılımcıların sistemlerinde test edilen uygulamanın dosyaları büyük yer kaplar. Genelde projenin donanım gereksinimlerinde disk alanı da belirtilir. Kimi zaman projede tahmin edilen donanım gereksinimlerinin yanlış olmasından, kimi zaman da sanal sunucunun istenen disk alanı ile alınması durumunda çok pahalı olması yüzünden, satın alınan disk alanı yetersiz gelmeye başlar.

Bu durumda genellikle izlenen yol, sunucu hizmeti veren yerden ek disk alanı satın almaktır. Çoğu zaman, alınan ek disk, sunucuya ikinci bir disk olarak eklenir. Kullanılan işletim sisteminin hangi özelliklerle kurulduğuna göre, ikinci diski sisteme eklemek çok kolay ya da çok karmaşık olabilir.

Burada, değişik durumlar için izlenebilecek yöntemleri açıklamaya çalışacağım.


# Linux Sistemlerde Dosya Sistemi

Windows'un aksine linux sistemlerde sürücü kavramı (C:, D: gibi) yoktur. Bunun yerine, bir köke bağlı dallardan oluşan bir ağaç yapısı vardır. Sistemimizdeki disk ya da disklerde oluşturacağımız bölümleri (partition), bu ağacın herhangi bir dalına bağlayabiliriz.

![Disk bölümleri](http://teaching.idallen.com/cst8207/13w/notes/file_system.png){:class:"img-responsive"}



## Logical Volume Manager (LVM) ile Kurulmuş Sistemler

Bu şekilde kurulmuş sistemler için açıklama gelecek. Ancak sisteminizin lvm ile kurulup kurulmadığını aşağıdaki komut ile görebilirsiniz:

    ilker@debian:~/$ sudo lvdisplay

Bu komutun çıktısı boş ise, sisteminizde lvm desteği yok demektir.

## LVM Desteği Olmayan Sistemler

LVM desteğinin bulunması, yeni disk ekleme işlemini çok kolaylaştıracaktı. Bu destek yok ise, ikinci diski daha farklı yöntemlerle sistemimize ekleyebiliriz. 
