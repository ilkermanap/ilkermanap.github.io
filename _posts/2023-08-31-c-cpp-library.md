---
layout: post
title: "Derleyiciler ve Kutuphaneler, gcc, g++"
categories: misc
---

Uygulama dedigimiz sey, bir yazilimci tarafindan pekcok programlama dilinden birisi ile gelistirilmis calistirilabilir programdir. Linux sistemlerde yazilimcinin uygulama gelistirme sirasinda kullanabilecegi pek cok arac bulunmaktadir. En cok bilinenlerden birisi olan gcc, yazdiginiz uygulamanin calistirilabilir halini ureten derleyicidir. Linux uzerinde pek cok uygulama c veya c++ ile gelistirilmistir.

Uygulama gelistirirken cogunlukla kutuphaneleri kullaniriz. Kendi kutuphanelerinizi olusturabilir ya da hazir kutuphaneleri de uygulamanizda kullanabilirsiniz. Kutuphaneler genellikle ozel bir islem grubu icin fonksiyonlar saglar. Ornegin X-window kutuphaneleri ile grafik arayuzlu uygulamalar gelistirebilirsiniz. Gdbm kutuphanesi size dosya uzerinde veritabani benzeri islemler yapabilen fonksiyonlar saglar. Kutuphaneler kolaylikla paylasilabilir ve dinamik olarak yuklenebilirler.

# Bilgi Edinme: info

Linux sistemlerde hemen hemen her uygulama/arac/kutuphane icin man sayfalari bulunur. 

    $ man 3 printf

Yukaridaki komut ile c dilinde ekrana birseyler yazdirmak icin kullanilan printf fonksiyonunun detayli anlatimi goruntulenir. Ayni bilgileri 

    $ info printf 

komutu ile de gorebiliriz. 

info ve man komutlari ile programlama ile ilgili pek cok referansa linux sistem uzerinde kolayca erisebilirsiniz. 

# C Derleyicisi: gcc

C programlama dili ve Unix isletim sistemi arasinda ozel bir iliski vardir. C programlama dili, Unix isletim sisteminin programlanmasi icin ozel olarak gelistirilmistir. Unix isletim sisteminin kodu da c ile yazilmistir. Linux isletim sisteminde de C derleyicisinin gnu versiyonu olan gcc bulunur.  Bu bolumde kisaca c programlama dilinin temel parcalarindan bahsedecek ve basit bir uygulama gelistirecegiz.

Bir C programi degisken tanimlamalari ve islem satirlari iceren fonksiyonlardan olusur. Uygulama calisirken her zaman once main adindaki fonksiyonu cagirir. Ardindan fonksiyon icinde bulunan diger fonksiyon cagrilarini sirasiyla isleterek devam eder. Ornegin printf, cikti uretmek icin siklikla kullanilan bir fonksiyondur.

Bir C fonksiyonu, baslik ve govdeden olusur. Baslik kisminda fonksiyonun uretecegi sonucun tipi, fonksiyon adi ve parantez icinde verilen parametre listesi bulunur:

    int karebul(int x) { return x*x };

Eger fonksiyon bir deger donmeyecek ise, void kullanilir:

    void yazdir(int x) { printf("%d", x) };

Fonksiyon govdesi icinde degisken tanimlamalari ve islem satirlari bulunur. Yukaridaki orneklerde {} arasinda verilen kisimlar fonksiyon govdesidir.

Bir c programi olusturmak icin gereken tek sey bir metin editorudur. Vi, emacs, gedit, pluma  gibi uygulamalar kullanilabilir. 

Fonksiyon tanimlamalarinin yani sira, C uygulamalari cogunlukla preprocessor denen satirlar da icerirler. Bir c programi derlenirken, kaynak kod once bir preprocessor araci tarafindan okunur. Bu arac, c derleyicisinin uzerinde calisacagi degistirilmis kaynak kodu olusturur. Butun preprocessor komutlari # isareti ile baslar. Ornegin #include <stdio.h> bize derleme oncesinde stdio.h dosyasinin iceriginin yazdigimiz kodun basina eklenip ondan sonra derleyiciye gonderilecegini belirtir. 

Stdio.h dosyasi standart girdi/cikti fonksiyonlarini icerir. Birazdan yazacagimiz selam.c uygulamasi derlenirken, printf fonksiyonu icin gereken derlenmis kod parcalarinin uygulamamiza eklenmesini saglar. 

```
#include <stdio.h>

void main(void)
{
    printf("Merhaba\n");
}
```

Linux sistemlerde c derleyicisini cagirmak icin gcc komutu kullanilir. Gcc komutu, dort ayri araci daha cagirir. Birincisi preprocessor aracidir. C uygulamalari ozel preprocessor komutlari kullanirlar. Bu komutlar, yazdigimiz kodu belirttigimiz sekilde degistirerek (ornegin stdio.h dosyasini ekleme) derleyiciye gonderir. Ikinci arac derleyicinin kendisidir. Derleyici kodu inceler ve o kod icin makine kodunu (assembly code) uretir. Ucuncu arac, assembler kodunu alip ondan object kodunu uretir. Dorduncu arac ise linker'dir. Uygulamanin object kodunu kullanarak calistirilabilir halini uretir. Farkli isim belirtilmediyse cikti dosyasinin adi a.out olur. Dosya adini belirtmek isterseniz, derleyiciye -o parametresi ile verebilirsiniz. 

Yukaridaki ornegi selam.c adinda bir dosyaya kaydedelim. Simdi derleme islemini nasil yapacagimizi gorecegiz:

    $ gcc selam.c -o selam
    $ ./selam
    Merhaba
    $

Gcc komutuna verecegimiz parametrelerle derleme islemini herhangi bir adimda durdurabiliriz. -P preprocessor, -S assembler, -c ise derleyici asamalarinda durmasini saglar. -P parametresi, kaynak kodunda preprocessor calistiktan sonraki durumu selam.P dosyasina kaydeder ve durur. -S, assembly kodu (selam.s) uretildikten sonra durur. -c ile object kod (selam.o) olustuktan sonra durur. 

    $ gcc -S selam.c
    $ gcc -c selam.c
    $ ls
    selam.c selam.o selam.s


Gcc icin bazi parametreler asagidadir:

    -S      Assembly code uretir. Dosya uzantisi .s olur.
    -P      Preprocessor ciktisini uretir. Dosya uzantisi .P olur.
    -c      Object kod uretir. Dosya uzantisi .o olur.
    -g      Uygulamanin hata ayiklayici ile calisabilecek sekilde derler.
    -o adi  Cikti dosyasina ad verebilmeyi saglar.
    -O      Kod optimizasyonu yapar.
    -l adi  Adi verilen kutuphaneyi linkleme islemi sirasinda kullanmamizi saglar. -lm verilmisse, libm.so kutuphane dosyasi kullanilir.