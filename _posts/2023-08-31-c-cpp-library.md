---
layout: post
title: "Derleyiciler ve Kütüphaneler, gcc, g++"
categories: misc
---

Uygulama dediğimiz şey, bir yazılımcı tarafından pek çok programlama dilinden birisi ile geliştirilmis çalıştırılabilir programdır. Linux sistemlerde yazılımcının uygulama geliştirme sırasında kullanabileceği pek çok araç bulunmaktadır. En çok bilinenlerden birisi olan gcc, yazdığınız uygulamanın çalıştırılabilir halini üreten derleyicidir. Linux üzerinde pek çok uygulama C veya C++ ile geliştirilmiştir.

Uygulama geliştirirken çoğunlukla kütüphaneleri kullaniriz. Kendi kütüphanelerinizi oluşturabilir ya da hazır kütüphaneleri de uygulamanızda kullanabilirsiniz. Kütüphaneler genellikle özel bir işlem grubu için fonksiyonlar sağlar. Örneğin X-window kütüphaneleri ile grafik arayüzlü uygulamalar geliştirebilirsiniz. Gdbm kütüphanesi size dosya üzerinde veritabanı benzeri işlemler yapabilen fonksiyonlar sağlar. Kütüphaneler kolaylıkla paylaşılabilir ve dinamik olarak yüklenebilirler.

# Bilgi Edinme: info

Linux sistemlerde hemen hemen her uygulama/araç/kütüphane için man sayfaları bulunur.

    $ man 3 printf

Yukarıdaki komut ile C dilinde ekrana bir şeyler yazdırmak için kullanılan printf fonksiyonunun detaylı anlatımı görüntülenir. Aynı bilgileri

    $ info printf

komutu ile de görebiliriz.

info ve man komutları ile programlama ile ilgili pek çok referansa linux sistem üzerinde kolayca erişebilirsiniz.

# C Derleyicisi: gcc

C programlama dili ve Unix işletim sistemi arasında özel bir ilişki vardir. C programlama dili, Unix işletim sisteminin programlanması için özel olarak geliştirilmiştir. Unix işletim sisteminin kodu da c ile yazılmıştır. Linux işletim sisteminde de C derleyicisinin gnu versiyonu olan gcc bulunur.  Bu bölümde kısaca c programlama dilinin temel parçalarından bahsedecek ve basit bir uygulama geliştireceğiz.

Bir C programı değişken tanımlamaları ve işlem satırları içeren fonksiyonlardan oluşur. Uygulama çalışırken her zaman önce main adındaki fonksiyonu çağırır. Ardından fonksiyon içinde bulunan diğer fonksiyon çağrılarını sırasıyla işleterek devam eder. Örneğin printf, çıktı üretmek için sıklıkla kullanılan bir fonksiyondur.

Bir C fonksiyonu, başlık ve gövdeden oluşur. Başlık kısmında fonksiyonun üreteceği sonucun tipi, fonksiyon adı ve parantez içinde verilen parametre listesi bulunur:

    int karebul(int x) { return x*x };

Eger fonksiyon bir değer dönmeyecek ise, void kullanılır:

    void yazdir(int x) { printf("%d", x) };

Fonksiyon gövdesi içinde değişken tanımlamaları ve işlem satırları bulunur. Yukarıdaki örneklerde {} arasında verilen kısımlar fonksiyon gövdesidir.

Bir C programı oluşturmak için gereken tek şey bir metin editörüdür. Vi, emacs, gedit, pluma  gibi uygulamalar kullanılabilir.

Fonksiyon tanımlamalarınin yanı sıra, C uygulamaları çoğunlukla preprocessor denen satırlar da içerirler. Bir C programı derlenirken, kaynak kod önce bir preprocessor aracı tarafından okunur. Bu araç, C derleyicisinin üzerinde çalışacağı değiştirilmiş kaynak kodu oluşturur. Bütün preprocessor komutları # işareti ile başlar. Örneğin #include <stdio.h> bize derleme öncesinde stdio.h dosyasının içeriğinin yazdığımız kodun başına eklenip ondan sonra derleyiciye gönderileceğini belirtir.

stdio.h dosyası standart girdi/çıktı fonksiyonlarını içerir. Birazdan yazacağımız selam.c uygulaması derlenirken, printf fonksiyonu için gereken derlenmiş kod parçalarının uygulamamıza eklenmesini sağlar.

```
#include <stdio.h>

void main(void)
{
    printf("Merhaba\n");
}
```

Linux sistemlerde C derleyicisini çağırmak için gcc komutu kullanılır. gcc komutu, dört ayrı aracı daha çağırır. Birincisi preprocessor aracıdır. C uygulamaları özel preprocessor komutları kullanırlar. Bu komutlar, yazdığımız kodu belirttiğimiz şekilde değiştirerek (örneğin stdio.h dosyasını ekleme) derleyiciye gönderir. İkinci araç derleyicinin kendisidir. Derleyici kodu inceler ve o kod için makine kodunu (assembly code) üretir. Üçüncü araç, assembler kodunu alıp ondan object kodunu üretir. Dördüncü araç ise linker'dir. Uygulamanın object kodunu kullanarak çalıştırılabilir halini üretir. Farklı isim belirtilmediyse çıktı dosyasının adı a.out olur. Dosya adını belirtmek isterseniz, derleyiciye -o parametresi ile verebilirsiniz.

Yukarıdaki örneği selam.c adında bir dosyaya kaydedelim. Şimdi derleme işlemini nasıl yapacağımızı göreceğiz:

    $ gcc selam.c -o selam
    $ ./selam
    Merhaba
    $

gcc komutuna vereceğimiz parametrelerle derleme işlemini herhangi bir adımda durdurabiliriz. -P preprocessor, -S assembler, -c ise derleyici aşamalarında durmasını sağlar. -P parametresi, kaynak kodunda preprocessor çalıştıktan sonraki durumu selam.P dosyasına kaydeder ve durur. -S, assembly kodu (selam.s) üretildikten sonra durur. -c ile object kod (selam.o) oluştuktan sonra durur.

    $ gcc -S selam.c
    $ gcc -c selam.c
    $ ls
    selam.c selam.o selam.s


gcc için bazı parametreler aşağıdadır:

    -S      Assembly code üretir. Dosya uzantısı .s olur.
    -P      Preprocessor çıktısını üretir. Dosya uzantısı .P olur.
    -c      Object kod uretir. Dosya uzantısı .o olur.
    -g      Uygulamayı hata ayıklayıcı ile çalışabilecek şekilde derler.
    -o adi  Çıktı dosyasına ad verebilmeyi sağlar.
    -O      Kod optimizasyonu yapar.
    -l adi  Adı verilen kütüphaneyi linkleme işlemi sırasında kullanmamızı
            sağlar. -lm verilmişse, libm.so kütüphane dosyası kullanılır.

# Kaynak, Object ve çalıştırılabilir Dosyalar

Uygulamanızı birden fazla kaynak dosyası içerecek şekilde geliştirebilirsiniz. Böylece çok büyük uygulamalar yazarken kodları daha küçük boyutlu dosyalarda saklamanız mümkün olur. Her bir dosyadaki fonksiyonlar, değişik işlemler için ayrı ayrı gruplanabilirler. Bir veritabanı uygulamasında veri girişini bir dosyada, arama işlemlerini de başka bir dosyada olacak şekilde yazabilirsiniz. Ana fonksiyon olan main, genellikle main.c dosyasında bulunur.

Şimdi biraz daha uzun bir program yazalım. Kitap kayıt uygulaması. kitapkayit, kitapgiris ve kitapyazdir fonksiyonlarını io.c dosyasında yazacağız. Bu fonksiyonlar veritabanı giriş/çıkış işlemlerini tanımlayacaktır.
main.c dosyası, kitapyazdir ve kitapgiris fonksiyonlarını çağıracaktır. main.c içinde bu fonksiyon tanımının (function declaration) da  verilmesi gerekir. Fonksiyon tanımı, fonksiyonun kendisine bir referanstır. Fonksiyonun adı, parametreleri ve dönüş değeri verilerek tanımlanır.


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
    printf("Kitap adını giriniz : ");
    scanf("%s%f", baslik, fiyat);
}

void kitapyazdir(char baslik[], float fiyat)
{
    printf("Kitap kaydı :%s %f\n", baslik, fiyat);
}
```

Birden fazla kaynak dosyası olduğunda derleyici ile linker arasındaki farkı bilmeniz gerekir. C derleyicisinin görevi object kodu üretmektir. Linker ise üretilen object kodları kullanarak çalıştırılabilir dosya üretir. C derleyicisi her bir dosyayı ayrı ayrı derleyerek her biri için ayrı object dosyaları oluşturur. Yukarıdaki iki dosya için main.o ve io.o dosyaları oluşturulacaktır. Object kod dosyaları oluşturulmuşsa derleyicinin işi bitmiş olur. Bu aşamada hala çalıştırılabilir uygulama yoktur. Linker, bu object dosyalarını kullanarak çalıştırılabilir dosyayı üretir.

Aynı gcc komutunda birden fazla dosyayı derleyip linkleyebiliriz.

    $ gcc main.c io.c -o kitapkayit
    $ ./kitapkayit
    Kitap adını giriniz : deneme 12.4
    Kitap kaydı :deneme 12.400000

Yukarıdaki şekilde derlediğimiz zaman object kod dosyaları silinir. Object kodları için

    $ gcc -c main.c io.c
    $ ls
    io.c  io.o  main.c  main.o

şeklinde derleyebiliriz. Bu aşamada, io.c dosyasını derlemeden, sadece main.c derleyip, io.o dosyasını kullanarak da çalıştırılabilir dosyayı elde edebiliriz.

    $ gcc main.c io.o -o kitapkayit
    $ ls
    io.c  io.o  kitapkayit  main.c  main.o

# Kütüphane Oluşturma ve Kullanma: Statik, Shared ve Dinamik

Çoğu C programında nadiren yeniden derlenmesi gereken fonksiyonlar kullanılır. Bazı fonksiyonları ise diğer programlarda kullanmak isteyebiliriz. Çoğu durumda bu fonksiyonlar standartlaşmış işlemleri gerçekleştirirler: veritabanı giriş çıkış işlemleri ya da ekran manipülasyonu gibi. Bu fonksiyonları önceden derleyip kütüphane adı verilen özel dosyalarda saklayabiliriz. Kütüphanelerin içindeki fonksiyonlar yazdığımız uygulamalarda kullanılırken yeniden derleme gerekmeden hızlıca linklenebilirler.

Değişik türlerdeki uygulamalar sistem dizinlerinde bulunan özel kütüphaneleri kullanabilirler. Örneğin matematik işlemleri libm.so dosyasında bulunur. Sistem genelinde bulunan kütüphanelerin yanı sıra, kendi yazdığımız fonksiyonları da kütüphane haline getirip başkalarının uygulama yazarken kullanabilmesini sağlayabiliriz.

kütüphaneler statik, shared ya da dinamik olabilirler. Statik kütüphanelerdeki object kodları, linklenen programın içine dahil edilir. Shared kütüphanelerde ise object kod uygulama içine gömülmez. Çalışma sırasında object kod kütüphane içinden erişilerek kullanılır. Shared kütüphane kullanan uygulamalarda kütüphane içindeki fonksiyonlar uygulama içine eklenmez, sadece uygulamanın ihtiyaç duyacağı kütüphanelerin adları not edilir. Çalışma sırasında kütüphane dosyasından object kodları okunur ve kullanılır. Dinamik kütüphaneler de shared kütüphaneler gibi çalışır. Ancak dinamik kütüphanedeki fonksiyona ihtiyaç duyulana kadar object kodları yüklenmez. Shared ve dinamik kütüphane kullandığımız zaman uygulama boyutu daha küçük olur. Uygulama, object kod yerine sadece object kodun bulunduğu kütüphane ismini kaydeder.

## Kütüphane İsimleri

Linux sistemlerde bulunan kütüphaneler çoğunlukla /usr/lib ya da /lib dizinlerinde bulunur. Kütüphane isimleri lib ön eki ile başlar. Statik kütüphaneler .a, sahred/dinamik kütüphaneler ise .so dosya uzantılarına sahiptir. Shared/dinamik kütüphanelerde versiyon numaraları da (major.minor) dosya adında .so dan sonra bulunur.

    libgdbm.so.4.11
    libgdbm.a

Kütüphane adı herhangi bir metin olabilir. Bir kelime, birkaç harf ya da tek karakter bile olabilir.

    libm.so.5

