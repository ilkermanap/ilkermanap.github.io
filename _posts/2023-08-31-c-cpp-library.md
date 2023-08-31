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
    -l adi  Adi verilen kutuphaneyi linkleme islemi sirasinda kullanmamizi 
            saglar. -lm verilmisse, libm.so kutuphane dosyasi kullanilir.

# Kaynak, Object ve Calistirilabilir Dosyalar

Uygulamanizi birden fazla kaynak dosyasi icerecek sekilde gelistirebilirsiniz. Boylece cok buyuk uygulamalar yazarken kodlari daha kucuk boyutlu dosyalarda saklamaniz mumkun olur. Her bir dosyadaki fonksiyonlar, degisik islemler icin ayri ayri gruplanabilirler. Bir veritabani uygulamasinda veri girisini bir dosyada, arama islemlerini de baska bir dosyada olacak sekilde yazabilirsiniz. Ana fonksiyon olan main, genellikle main.c dosyasinda bulunur. 

Simdi biraz daha uzun bir program yazalim. Kitap kayit uygulamasi, kitapkayit. kitapgiris ve kitapyazdir fonksiyonlarini io.c dosyasinda yazacagiz. Bu fonksiyonlar veritabani giris/cikis islemlerini tanimlayacaktir. 
main.c dosyasi, kitapyazdir ve kitapgiris fonksiyonlarini cagiracaktir. main.c icinde bu fonksiyon taniminin (function declaration) da  verilmesi gerekir. Fonksiyon tanimi, fonksiyonun kendisine bir referanstir. Fonksiyonun adi, parametreleri ve donus degeri verilerek tanimlanir. 


main.c

```
#include <stdio.h>
void kitapgiris(char[], float*);
void kitapyazdir(char[], float);

void main(void)
{
    char baslik[20];
    float fiyat;

    kitapgiris(baslik, &fiyat);
    kitapyazdir(baslik, fiyat);
}
```

io.c

```
#include <stdio.h>

void kitapgiris(char baslik[], float *fiyat)
{
    printf("Kitap adini giriniz : ");
    scanf("%s%f", baslik, fiyat);
}

void kitapyazdir(char baslik[], float fiyat)
{
    printf("Kitap kaydi :%s %f\n", baslik, fiyat);
}
```

Birden fazla kaynak dosyasi oldugunda derleyici ile linker arasindaki farki bilmeniz gerekir. C derleyicisinin gorevi object kodu uretmektir. Linker ise uretilen object kodlari kullanarak calistirilabilir dosya uretir. C derleyicisi her bir dosyayi ayri ayri derleyerek herbiri icin ayri object dosyalari olusturur. Yukaridaki iki dosya icin main.o ve io.o dosyalari olusturulacaktir. Object kod dosyalari olusturulmussa derleyicinin isi bitmis olur. Bu asamada hala calistirilabilir uygulama yoktur. Linker, bu object dosyalarini kullanarak calistirilabilir dosyayi uretir.

Ayni gcc komutunda birden fazla dosyayi derleyip linkleyebiliriz. 

    $ gcc main.c io.c -o kitapkayit
    $ ./kitapkayit 
    Kitap adini giriniz : deneme 12.4
    Kitap kaydi :deneme 12.400000

Yukaridaki sekilde derledigimiz zaman object kod dosyalari silinir. Object kodlari icin

    $ gcc -c main.c io.c 
    $ ls 
    io.c  io.o  main.c  main.o
    
seklinde derleyebiliriz. Bu asamada, io.c dosyasini derlemeden, sadece main.c derleyip, io.o dosyasini kullanarak da calistirilabilir dosyayi elde edebiliriz:

    $ gcc main.c io.o -o kitapkayit
    $ ls 
    io.c  io.o  kitapkayit  main.c  main.o

# Kutuphane Olusturma ve Kullanma: Statik, Shared ve Dinamik

Cogu C programinda nadiren yeniden derlenmesi gereken fonksiyonlar kullanilir. Bazi fonksiyonlari ise diger programlarda kullanmak isteyebiliriz. Cogu durumda bu fonksiyonlar standartlasmis islemleri gerceklestirirler: veritabani giris cikis islemleri ya da ekran manipulasyonu gibi. Bu fonksiyonlari onceden derleyip kutuphane adi verilen ozel dosyalarda saklayabiliriz. Kutuphanelerin icindeki fonksiyonlar yazdigimiz uygulamalarda kullanilirken yeniden derleme gerekmeden hizlica linklenebilirler. 

Degisik turlerdeki uygulamalar sistem dizinlerinde bulunan ozel kutuphaneleri kullanabilirler. Ornegin matematik islemleri libm.so dosyasinda bulunur. Sistem genelinde bulunan kutuphanelerin yani sira, kendi yazdigimiz fonksiyonlari da kutuphane haline getirip baskalarinin uygulama yazarken kullanabilmesini saglayabiliriz. 

Kutuphaneler statik, shared ya da dinamik olabilirler. Statik kutuphanelerdeki object kodlari, linklenen programin icine dahil edilir. Shared kutuphanelerde ise object kod uygulama icine gomulmez. Calisma sirasinda object kod kutuphane icinden erisilerek kullanilir. Shared kutuphane kullanan uygulamalarda kutuphane icindeki fonksiyonlar uygulama icine eklenmez, sadece uygulamanin ihtiyac duyacagi kutuphanelerin adlari not edilir. Calisma sirasinda kutuphane dosyasindan object kodlari okunur ve kullanilir. Dinamik kutuphaneler de shared kutuphaneler gibi calisir. Ancak dinamik kutuphanedeki fonksiyona ihtiyac duyulana kadar object kodlari yuklenmez. Shared ve dinamik kutuphane kullandigimiz zaman uygulama boyutu daha kucuk olur. Uygulama, object kod yerine sadece object kodun bulundugu kutuphane ismini kaydeder. 

## Kutuphane Isimleri

Linux sistemlerde bulunan kutuphaneler cogunlukla /usr/lib ya da /lib dizinlerinde bulunur. Kutuphane isimleri lib on eki ile baslar. Statik kutuphaneler .a, shared/dinamik kutuphaneler ise .so dosya uzantilarina sahiptir. Shared/dinamik kutuphanelerde versiyon numaralari da (major.minor) dosya adinda .so dan sonra bulunur. 

    libgdbm.so.4.11
    libgdbm.a

Kutuphane adi herhangi bir metin olabilir. Bir kelime, birkac harf ya da tek karakter bile olabilir.

    libm.so.5

