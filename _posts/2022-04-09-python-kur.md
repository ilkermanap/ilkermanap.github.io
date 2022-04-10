---
layout: post
title: "Python Nasıl Kurulur?"
categories: misc
---

## Windows

Windows işletim sisteminde python kurulumu birkaç farklı şekilde yapılabilir. En kolayı Microsoft mağaza uygulaması üzerinden kurulum yapmaktır.

Microsoft Store uygulamasını çalıştırıp arama kısmına python yazın.

![microsoft store search python](/assets/images/microsoftstore_search_python.png){:class="img-responsive"}
![windows run cmd](/assets/images/windows_cmd_virtualenv_new_env.png){:class="img-responsive"}

Bir sistemde aynı anda birden fazla python kurulumu olabilir. Farklı projelerinizde değişik python sürümleri ya da sadece belirli sürümdeki paketleri kullanmanız gerekebilir. Örneğin bir uygulama özellikle sadece django 2.1 ile çalışıyordur. Aynı sistemde django 3.1'de kullanmak isterseniz, paketlerin birbirlerinin uzerine yazması ya da yeni sürüm paketin eski sürümü kaldırıp yerine kendisini kurması eski sürüme bağlı uygulamaların bozulmasına sebep olur. Bu durumları engellemek ve çalıştığınız sistem geneline yeni paket kurmadan çalışabilmek için virtualenv gibi araçları kullanmamız gerekir.

Virtualenv kullandığımız zaman, kullandığımız sistemdeki paketlere dokunmayan ve sadece bir dizin altında varlığını sürdüren bir python kurulumuna sahip oluruz.

Virtualenv kurmak için python kurulumunda yer alan pip aracını kullanacağız. Pip, üçüncü parti python kütüphanelerinin sistemimize kurulmasını sağlayan araçtır. Kurulum için önce cmd uygulamasını çalıştıralım.

![windows run cmd](/assets/images/windows_run_cmd.png){:class="img-responsive"}

Ardından, aşağıdaki komut ile virtualenv kuralım:

    pip install virtualenv

![windows run cmd](/assets/images/windows_cmd_pip_install_virtualenv.png){:class="img-responsive"}

Komutun çıktısına bakarsak, virtualenv uygulamasının PATH içinde bir yere kurulmadığını söylediğini göreceksiniz. 

![windows run cmd](/assets/images/windows_cmd_pip_install_virtualenv_output.png){:class="img-responsive"}

Ekranda görünen yolu PATH'e ekleyebilirsiniz, ya da aşağıda göstereceğim şekilde kullanabilirsiniz.

    python -m virtualenv venv

![windows run cmd](/assets/images/windows_cmd_virtualenv_new_env.png){:class="img-responsive"}

Çalıştırdığımız komut, bulunduğumuz dizinde adi venv olan bir dizin oluşturur. Bu dizin içinde, tam bir python kurulumu vardır. Bu dizindeki python kurulumunu aktifleştirmek için aşağıdaki komutu kullanırız:

    venv\Scripts\activate

![windows run cmd](/assets/images/windows_cmd_virtualenv_new_env.png){:class="img-responsive"}
 
 Komut çalıştıktan sonra komut satırının nasıl değiştiğine dikkat edin. Baş tarafa (venv) yazısının geldiğini göreceksiniz. 
 
![windows run cmd](/assets/images/windows_cmd_venv_activate.png){:class="img-responsive"}
 
 Bu size artık python kurulumu ile ilgili komut satırından yapacağınız işlemlerin sadece bu dizindeki özel kurulum için geçerli olacağını gösterir. Pip kullanarak bu cmd ekranından kuracağımız bütün yeni paketler sadece bu dizin altında geçerli olacak, sistemdeki diğer paketlerle karışmayacaktır.


