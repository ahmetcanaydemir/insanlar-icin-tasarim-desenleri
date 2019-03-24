![İnsanlar İçin Tasarım Desenleri](https://user-images.githubusercontent.com/8572957/54880775-e86ca180-4e59-11e9-9848-6dbf139e901e.png)

***

<p align="center">
🎉 Tasarım desenlerinin aşırı basitleştirilmiş açıklamaları! 🎉
</p>
<p align="center">
Herkesin aklını kolayca karıştıracak bir konu. Burada tasarım desenlerini mümkün olan <i>en basit</i> şekilde açıklayarak aklınıza (ve benimkine) uygun hale getirmeye çalışıyorum.
</p>

***
Asıl olarak [@kamranahmedse](https://github.com/kamranahmedse)'in yazdığı [design-patterns-for-humans](https://github.com/kamranahmedse/design-patterns-for-humans) deposunun Türkçe'ye çevrilmiş halidir. Doğrudan çeviriden ziyade bir uyarlama yapmaya çalıştım. Genelde İngilizce terimlerine aşina olduğumuzdan desenlerin İngilizcelerini de parantez içerisinde bıraktım. Vikipedi tanımları tr.wikipedia kaynaklıdır.

Lütfen eksik veya yanlış gördüğünüz kısımları geliştirmek için istek gönderin.

<sub>Kişisel [internet sayfamı](http://ahmetcanaydemir.com) ziyaret edin ve bana [Twitter](https://twitter.com/ahmetcnaydemir)'dan "selam" verin.</sub>

Giriş
=================

Tasarım desenleri, tekrar eden sorunların çözümleri ve **belli problemlerin nasıl çözüleceğine dair kılavuzlardır**. Bunlar, uygulamanıza ekleyebileceğiniz ve büyülü sınıflar, paketler veya kütüphaneler değildir. Daha ziyade, belirli durumlarda belirli sorunların üstesinden nasıl geleceğinizi gösteren kurallardır.

> Tasarım desenleri, tekrar eden sorunların çözümleri ve belli problemlerin nasıl çözüleceğine dair kılavuzlardır.

Vikipedi tanımı

> Yazılım tasarım desenleri, yazılım tasarımı sırasında sıkça karşılaşılan, birbirine benzer sorunları çözmek için geliştirilmiş ve işlerliği kanıtlanmış genel çözüm önerileridir.

⚠️ Dikkatli Olun
-----------------
- Tasarım desenleri tüm sorunlarınızı bir anda yok edebilecek lamba cinleri değiller.
- Kullanmak için fazla deneyip zorlamayın. Çok daha kötü sonuçlara da sebep olabilirler.
- Tasarım desenleri sorunları çözmek için varlar. Eğer ortada bir sorun göremiyorsanız, fazla kafaya takmayın.
- Doğru yerde doğru şekilde kullanılırsa kurtarıcı olabilirler, diğer durumda korkunç bir kod karmaşasına neden olabilirler.

> Ayrıca kod örnekleri PHP-7 ile yazıldı, fakat konsept aynı olduğu için ne kullandığınız fark etmez.

Tasarım Desenleri Türleri
-----------------

* [Yaratım (Creational)](#creational-design-patterns)
* [Yapısal (Structural)](#structural-design-patterns)
* [Durumsal (Behavioral)](#behavioral-design-patterns)

Yaratım (Creational) Tasarım Desenleri
==========================

Basit bir şekilde
> Yaratım tasarım desenleri, bir nesneyi veya ilgili nesneler grubunu somutlaştırmaya yöneliktir.

Vikipedi tanımı
> Yaratım örüntüleri, yazılım nesnelerinin (ya da başka bir deyişle sınıf örnekleri - class instances) nasıl yaratılacağı hakkında öneriler sunar. Ana fikir, iyi bir yazılımın, içinde barındırdığı nesnelerin nasıl yaratıldığından bağımsız olarak tasarlanması gerekliliğidir. Diğer bir deyişle, nesnelerin nereden ve nasıl yaratıldığı, ait oldukları yazılımın işleyişini etkilememeli; yeni özellikler eklenmesine ve değişikliklere karşı sorun oluşturmamalıdır. 

 * [Basit Fabrika (Simple Factory)](#-simple-factory)
 * [Fabrika Yöntemi (Factory Method)](#-factory-method)
 * [Soyut Fabrika (Abstract Factory)](#-abstract-factory)
 * [Yapıcı (Builder)](#-builder)
 * [Örnek (Prototype)](#-prototype)
 * [Yegâne (Singleton)](#-singleton)

🏠 Basit Fabrika (Simple Factory)
--------------
Gerçek dünya örneği
> Bir ev inşa ettiğinizi hayal edin ve kapıya ihtiyacın var. Marangoz kıyafetlerinizi giyebilir, biraz ahşap, tutkal, çivi ve kapıyı inşa etmek için gerekli tüm araçları getirip evinize inşa etmeye başlayabilirsiniz. Ya da sadece fabrikayı arayabilir ve hazır inşa edilmiş kapının size teslim edilmesini sağlayabilirsiniz. Kapının yapımı hakkında bir şey öğrenmeniz ya da onu yapmakla gelen karmaşayla baş etmeniz gerekmez. 

Basit bir şekilde
> Basit fabrika, kullanıcıya herhangi bir somutlaştırma mantığı göstermeden bir örnek oluşturur.

Vikipedi tanımı
> Nesne yönelimli programlamada, fabrika başka nesneler yaratmaya yönelik bir nesnedir. Fabrika, "yeni (new)" olduğu varsayılan bazı yöntem çağrılarından değişken bir prototip veya sınıftaki nesneleri döndüren bir işlev veya yöntemdir.

**Programlı Örnek**

Öncelikle bir kapı arayüzümüz ve uygulamamız var
```php
interface Kapi
{
    public function getGenislik(): float;
    public function getYukseklik(): float;
}

class AhsapKapi implements Kapi
{
    protected $genislik;
    protected $yukseklik;

    public function __construct(float $genislik, float $yukseklik)
    {
        $this->genislik = $genislik;
        $this->yukseklik = $yukseklik;
    }

    public function getGenislik(): float
    {
        return $this->genislik;
    }

    public function getYukseklik(): float
    {
        return $this->yukseklik;
    }
}
```
Sonra kapıyı yapan ve kargolayan kapı fabrikamız var.
```php
class KapiFabrikasi
{
    public static function yapKapi($genislik, $yukseklik): Kapi
    {
        return new AhsapKapi($genislik, $yukseklik);
    }
}
```
Artık şu şekilde kullanılabilir
```php
// Bana 100x200 bir kapı yap
$kapi = KapiFabrikasi::yapKapi(100, 200);

echo 'Genişikk: ' . $kapi->getGenislik();
echo 'Yükseklik: ' . $kapi->getYukseklik();

// Bana 50x100 bir kapı yap
$kapi2 = KapiFabrikasi::yapKapi(50, 100);
```

**Ne zaman kullanılmalı?**

Bir nesneyi oluşturmak sadece birkaç atamadan ibaret değilse ve bazı mantıkları içeriyorsa, aynı kodu tekrar etmektense bir fabrika kullanmak daha mantıklı olacaktır.

🏭 Fabrika Yöntemi (Factory Method)
--------------

Gerçek dünya örneği
> Bir işe alma müdürü hayal edin. Tüm pozisyonlar için bir kişinin mülakat yapması mümkün değildir. Yeni pozisyon açılışına göre işe alma müdürü, mülakat adımlarına karar vermeli ve ilgili kişileri görevlendirmelidir.

Basit bir şekilde
> Alt sınıflara örnekleme mantığı devretmenin yolunu sağlar.

Vikipedi tanımı
> Nesne yaratımı için kullanılan tek arayüz altında nesnenin nasıl yaratılacağını kalıtım yoluyla alt sınıflara bırakarak, arayüzle nesne yaratım işlevlerini birbirinden ayırır. 

 **Programlı Örnek**

Bahsettiğimiz işe alma müdürünü ele alalım. İlk olarak bir mülakatçı arayüzümüz var ve içerisinde bazı uygulamalar bulunuyor.

```php
interface Mulakatci
{
    public function sorSorular();
}

class Gelistirici implements Mulakatci
{
    public function sorSorular()
    {
        echo 'Tasarım desenleri hakkında soruyorum!';
    }
}

class ToplulukYoneticisi implements Mulakatci
{
    public function sorSorular()
    {
        echo 'Toplum düzeni hakkında soruyorum.';
    }
}
```

Şimdi de `IseAlmaMuduru`'nü oluşturalım.

```php
abstract class IseAlmaMuduru
{

    // Fabrika yöntemi (Factory method)
    abstract protected function olusturMulakatci(): Mulakatci;

    public function getirMulakatci()
    {
        $mulakatci = $this->olusturMulakatci();
        $mulakatci->sorSorular();
    }
}

```
Artık herhangi bir alt sınıf devralabilir ve gerekli mülakatçıyı sağlayabilir
```php
class YazilimGelistirmeMuduru extends IseAlmaMuduru
{
    protected function olusturMulakatci(): Mulakatci
    {
        return new Gelistirici();
    }
}

class PazarlamaMuduru extends IseAlmaMuduru
{
    protected function olusturMulakatci(): Mulakatci
    {
        return new ToplulukYoneticisi();
    }
}
```
ve sonra şu şekilde kullanılabilir

```php
$gelistirmeMuduru = new YazilimGelistirmeMuduru();
$gelistirmeMuduru->getirMulakatci(); // Cikti: Tasarım desenleri hakkında soruyorum!

$pazarlamaMuduru = new PazarlamaMuduru();
$pazarlamaMuduru->takeInterview(); // Cikti: Toplum düzeni hakkında soruyorum.
```

**Ne zaman kullanılmalı?**

Bir sınıfta bazı genel işlemler olduğunda kullanışlıdır, gerekli alt sınıfa çalışma zamanında dinamik olarak karar verilir. Veya başka bir deyişle, kullanıcı ne kadar alt sınıfa ihtiyaç duyacağını bilmediğinde kullanışlıdır.

🔨 Soyut Fabrika (Abstract Factory)
----------------

Gerçek dünya örneği
> Basit fabrika tasarım desenindeki örneği biraz geliştiriyoruz. İhtiyacınıza göre ahşap kapınızı ahşap kapı dükkanından, çelik kapınızı çelik kapı dükkanından veya PVC kapınızı ilgili dükkandan alabilirdiniz. Ayrıca kapı montajı için de farklı yetenekte elemanlar gerekecek, örneğin ahşap kapı için marangoz, çelik kapı için kaynakçı vs. Gördüğünüz gibi artık kapılara göre artık bağımlılıklarımız var. Çelik kapı için kaynakçı, ahşap kapı için marangoz vs.

Basit bir şekilde
> Fabrikaların fabrikası; ayrı ayrı ama birbirleri ile ilişkili/bağlantılı fabrikaları sınıflarını belirtmeden birlikte gruplayan bir fabrika.

Vikipedi tanımı
> Tek arayüz ile bir nesne ailesinin farklı platformlarda yaratılmasını olanaklı kılar. Bu sayade yazılım uygulaması farklı platfromlara davranış değişikliğine uğramadan taşınabilir. Soyut fabrika kalıbı tek arayüz altında hangi somut sınıfların kullanıldığını saklar. 

**Programlı Örnek**

Yukarıdaki kapı örneğimizi biraz değiştiriyoruz. Öncelikle `Kapı` arayüzümüz ve bunun için bazı uygulamalarımız var.

```php
interface Kapi
{
    public function getirAciklama();
}

class AhsapKapi implements Kapi
{
    public function getirAciklama()
    {
        echo 'Ben bir ahşap kapıyım';
    }
}

class CelikKapi implements Kapi
{
    public function getirAciklama()
    {
        echo 'Ben bir çelik kapıyım';
    }
}
```
Ayrıca her kapı tipi için uygun uzmanlarımız var.

```php
interface KapiMontajUzmani
{
    public function getirAciklama();
}

class Kaynakci implements KapiMontajUzmani
{
    public function getirAciklama()
    {
        echo 'Ben sadece çelik kapı yerleştirebilirim';
    }
}

class Marangoz implements KapiMontajUzmani
{
    public function getirAciklama()
    {
        echo 'Ben sadece ahşap kapı yerleştirebilirim';
    }
}
```

Şimdi, ilgili nesnelerin ailesini yapmamızı sağlayacak soyut bir fabrikamız var. Yani ahşap kapı fabrikası bir ahşap kapı ve ahşap kapı montaj uzmanı, demir kapı fabrikası bir demir kapı ve demir kapı montaj uzmanı oluşturacak.
```php
interface KapiFabrikasi
{
    public function yapKapi(): Kapi;
    public function yapMontajUzmani(): KapiMontajUzmani;
}

// Marangoz ve ahşap kapıyı döndürecek ahşap fabrikası
class AhsapFabrikasi implements KapiFabrikasi
{
    public function yapKapi(): Kapi
    {
        return new AhsapKapi();
    }

    public function yapMontajUzmani(): KapiMontajUzmani
    {
        return new Marangoz();
    }
}

// Kaynakçı ve çelik kapıyı döndürecek çelik fabrikası
class CelikFabrikasi implements KapiFabrikasi
{
    public function yapKapi(): Kapi
    {
        return new CelikKapi();
    }

    public function yapMontajUzmani(): KapiMontajUzmani
    {
        return new Kaynakci();
    }
}
```
Artık şu şekilde kullanılabilir
```php
$ahsapFabikrasi = new AhsapFabrikasi();

$kapi = $ahsapFabikrasi->yapKapi();
$uzman = $ahsapFabikrasi->yapMontajUzmani();

$kapi->getirAciklama();  // Çıktı: Ben bir ahşap kapıyım
$uzman->getirAciklama(); // Çıktı: Ben sadece ahşap kapı yerleştirebilirim

// Aynısı çelik kapı için
$celikFabrikasi = new CelikFabrikasi();

$kapi = $celikFabrikasi->yapKapi();
$uzman = $celikFabrikasi->yapMontajUzmani();

$door->getirAciklama();  // Çıktı: Ben bir çelik kapıyım
$uzman->getirAciklama(); // Çıktı: Ben sadece çelik kapı yerleştirebilirim
```
 
Gördüğünüz gibi ahşap fabrikası `marangoz` ve `ahşap kapı`yı kapsar, ayrıca çelik fabrikası `kaynakçı` ve `demir kapı`yı kapsar. böylece oluşturulan kapıların her biri için yanlış bir uzmana sahip olmadığımıza eminiz.

**Ne zaman kullanılmalı?**

İlgili ve basit olmayan oluşturma mantığı ile ilişkili bağımlılıklar olduğunda kullanılabilir.

👷 Yapıcı (Builder)
--------------------------------------------
Gerçek dünya örneği
>Burger King'te olduğunuzu ve bir sipariş verdiğinizi hayal edin. Mesela "Big King" olsun, size *hiçbir soru* sormadan siparişinizi getiriyorlar. Bu bir basit fabrika *(simple factory)* örneğidir. Fakat bazı durumlarda oluşturma mantığı daha fazla adım içerebilir. Örneğin farklı bir menü almak istiyorsunuz ve bazı seçenekleriniz patatesinizin tipi nasıl olsun, içeceğiniz hangi boy olsun, hangi sosları istersiniz vs. gibi. Bu durumda Yapıcı tasarım deseni kurtarıcınız olabilir.

Basit bir şekilde
> Yapıcı metod kirliliği kaçınarak bir nesnenin farklı çeşitlerini oluşturmasına izin verir. Bir nesnenin birkaç çeşidi olabileceği zaman veya bir nesnenin oluşturulmasında yer alan çok fazla adım olduğunda faydalıdır.

Vikipedi tanımı
> Karmaşık bir nesne grubunun tek arayüz üzerinden gerektiğince parça parça yaratılmasını sağlar. Kullanıcı nesne grubunu kullandıkça nesne grubu gereken yönde yapılanır. Kullanılmayan parçalar gereksiz yere yaratılarak kaynak harcamaz.

**Programlı Örnek**

Öncelikle yapmak istediğimiz burgerimiz var

```php
class Hamburger
{
    protected $boyut;

    protected $peynir = false;
    protected $pepperoni = false;
    protected $marul = false;
    protected $domates = false;

    public function __construct(HamburgerYapici $yapici)
    {
        $this->boyut = $yapici->boyut;
        $this->peynir = $yapici->peynir;
        $this->pepperoni = $yapici->pepperoni;
        $this->marul = $yapici->marul;
        $this->domates = $yapici->domates;
    }
}
```

Sonra yapıcımız bulunuyor

```php
class HamburgerYapici
{
    public $boyut;

    public $peynir = false;
    public $pepperoni = false;
    public $marul = false;
    public $domates = false;

    public function __construct(int $boyut)
    {
        $this->boyut = $boyut;
    }

    public function eklePepperoni()
    {
        $this->pepperoni = true;
        return $this;
    }

    public function ekleMarul()
    {
        $this->marul = true;
        return $this;
    }

    public function eklePeynir()
    {
        $this->peynir = true;
        return $this;
    }

    public function ekleDomates()
    {
        $this->domates = true;
        return $this;
    }

    public function yap(): Burger
    {
        return new Burger($this);
    }
}
```
Daha sonra şu şekilde kullanılabilir:

```php
$hamburger = (new HamburgerYapici(14))
                    ->eklePepperoni()
                    ->ekleMarul()
                    ->ekleDomates()
                    ->yap();
```

**Ne zaman kullanılmalı?**

Bir nesnesnin birkaç çeşidi olduğunda ve yapıcı metod karmaşasından kaçınmak için kullanılabilir. Fabrika modelinden temel fark şudur; Oluşturma işlemi  bir aşamalı işlem olduğunda fabrika kalıbı kullanılırken, çok aşamalı oluşturma işlemlerinde yapıcı deseni kullanılır.

🐑 Örnek (Prototype)
------------
Gerçek dünya örneği
> Oyalı'yı hatırladınız mı? İstanbul Üniversitesi'nde klonlanan koyun! Sizi ayrıntılara boğmayayım ama asıl nokta bu desenin tamamen klonlama ile alakalı olduğu.

Basit bir şekilde
> Klonlama yoluyla varolan bir nesneyi temel alarak nesne oluşturun.

Vikipedi tanımı
> Karmaşık ve/veya pahalı sınıflardan nesne yaratırken, yeni nesnelerin baştan yaratılması yerine, mevcutlarından örnekleyerek yaratılmasını sağlar. Bu sayede yeni nesneler kolayca ve kaynaklar gereksiz yere meşgul edilmeden yaratılırlar. 

Kısacası, mevcut bir nesnenin bir kopyasını oluşturmanıza ve sıfırdan bir nesne oluşturma ve kurma derdinden kurtulmak yerine ihtiyaçlarınıza göre düzenlemenizi sağlar.

**Programlı Örnek**

PHP'de basit bir şekilde `clone` kullanılarak yapılabilir

```php
class Koyun
{
    protected $isim;
    protected $kategori;

    public function __construct(string $isim, string $kategori = 'Karaman Koyunu')
    {
        $this->isim = $isim;
        $this->kategori = $kategori;
    }

    public function degistirIsim(string $isim)
    {
        $this->isim = $isim;
    }

    public function getirIsim()
    {
        return $this->isim;
    }

    public function degistirKategori(string $kategori)
    {
        $this->kategori = $kategori;
    }

    public function getirKategori()
    {
        return $this->kategori;
    }
}
```
Şu şekilde klonlanabilir
```php
$hakiki = new Koyun('Oyalı');
echo $hakiki->getirIsim(); // Oyalı
echo $hakiki->getirKategori(); // Karaman Koyunu

// Klonla ve düzenle ne gerekiyorsa onu yap
$klonlanmis = clone $hakiki;
$klonlanmis->degistirIsim('Boyalı');
echo $klonlanmis->getirIsim(); // Boyalı
echo $klonlanmis->getirKategori(); // Karaman Koyunu
```
Ayrıca klonlama davranışını değiştirmek için büyülü `__clone` metodunu kullanabilirsiniz.

**Ne zaman kullanılmalı?**

Mevcut bir nesneye benzer bir nesne gerektiğinde veya oluşturmanın klonlama ile karşılaştırıldığında masraflı olacağı zaman.

💍 Yegâne (Singleton)
------------
Gerçek dünya örneği
> Aynı anda bir ülkenin yalnızca bir başkanı olabilir. Ne zaman görev çağırırsa aynı başkan harekete geçmeli. Burada ise bizim başkanımız *Yegâne*.

Basit bir şekilde
> Belirli bir sınıfın yalnızca bir nesnesinin oluşturulmasını sağlar.

Vikipedi tanımı
> Bir sınıftan sadece bir tane nesne yaratılacak şekilde kısıtlama sağlar. Söz konusu nesneye uygulamanın her yerinden ulaşılabilir. Nesne ilk kez kullanılana dek oluşturulmayabilir. 

Yegâne *(Singleton)* deseni aslında bir anti-desen olarak kabul edilir ve aşırı kullanımından kaçınılmalıdır. Mutlaka fena olmayan ve geçerli kullanım durumlarına sahiptir lakin kullanıldığında küresel bir durum ortaya çıkardığından, bir yerde değişiklik yapmak diğer alanları etkilediğinden ve hata ayıklamak oldukça zor olabileceği için dikkatli kullanılmalıdır. Diğer bir sorun, kodunuz birleştiğinde nesneleri taklit etmek zor olabilir.

**Programlı Örnek**

Bir Yegâne (*singleton*) oluşturmak için yapıcı metodu özel yapın, klonlamayı devre dışı bırakın, devralmayı devre dışı bırakın ve nesne örneğini tutmak için statik bir değişken oluşturun.

```php
final class Baskan
{
    private static $ornek;

    private function __construct()
    {
        // Yapıcı metodu gizleyin
    }

    public static function getOrnek(): Baskan
    {
        if (!self::$ornek) {
            self::$ornek = new self();
        }

        return self::$ornek;
    }

    private function __clone()
    {
        // Klonlamayı iptal edin
    }

    private function __wakeup()
    {
        // unserialize etmeyi iptal edin
    }
}
```
Şu şekilde kullanılabilir
```php
$baskan1 = Baskan::getOrnek();
$baskan2 = Baskan::getOrnek();

var_dump($baskan1 === $baskan2); // true (baskan1 ve baskan2 tamamen aynı)
```

Yapısal (Structural) Tasarım Desenleri
==========================
Basit bir şekilde
> Yapısal desenler çoğunlukla nesne birleşimi veya başka bir deyişle nesnelerin birbirlerini nasıl kullanabileceği ile ilgilidir. Ya da "Bir yazılım bileşeni nasıl oluşturulur?" sorusunun cevaplarından biridir.

Vikipedi tanımı
> Yapısal örüntüler sınıfların ve nesnelerin birleştirilerek daha geniş yazılım yapılarının kurulmasına olanak sağlayan öneriler sunar. Sınıf yapı örüntüleri ve nesne yapı örüntüleri olmak üzere ikiye ayrılır. Sınıf yapı örüntüleri **kalıtım** kullanarak sınıf arayüzlerini ya da uygulamaları **bileştirerek** yapıları genişletir. Nesne yapı örüntüleri ise nesnelerin birleştirilerek **yeni işlevler kazanma yollarını** gösterir. 

 * [Uyumlayıcı (Adapter)](#-adapter)
 * [Köprü (Bridge)](#-bridge)
 * [Bileşik (Composite)](#-composite)
 * [Dekoratör (Decorator)](#-decorator)
 * [Cephe (Facade)](#-facade)
 * [Sinek siklet (Flyweight)](#-flyweight)
 * [Vekil (Proxy)](#-proxy)

🔌 Uyumlayıcı (Adapter)
-------
Gerçek dünya örneği
> * Bellek kartınızda bazı resimler olduğunu ve bunları bilgisayarınıza aktarmanız gerektiğini düşünün. Bilgisayarınıza hafıza kartını takıp fotoğraflara ulaşmak için bilgisayar bağlantı noktalarınızla uyumlu bir tür adaptöre ihtiyacınız vardır. Bu durumda kart okuyucu bir adaptördür. 
> * Başka bir örnek ise ünlü güç adaptörü olabilir; üç bacaklı bir fiş iki delikli bir prize bağlanamaz, iki delikli prize uyumlu hale getiren bir güç adaptörü kullanması gerekir.
> * Yine bir başka örnek, bir kişinin söylediklerini diğer kişiye çeviren bir tercüman olacaktır.

Basit bir şekilde
> Uyumlayıcı deseni, başka bir sınıfla uyumlu hale getirmek için bağdaştırıcıya, başka türden uyumsuz bir nesneyi sarmanıza izin verir.

Vikipedi tanımı
> Farklı kaynaklardan gelen nesne ya da sınıfların arayüzlerini uyumlandırmak amacıyla kullanılır. 

**Programlı Örnek**

Bir avcının aslanları avladığı bir oyun düşünün.

Öncelikle, her tür aslanın uygulamak zorunda olduğu bir `Aslan` arayüzümüz var.

```php
interface Aslan
{
    public function kukre();
}

class AfrikaAslani implements Aslan
{
    public function kukre()
    {
    }
}

class AsyaAslani implements Aslan
{
    public function kukre()
    {
    }
}
```
Ve avcı, avlamak için `Aslan` arayüzünü uygulayan herhangi bir nesneyi bekler.
```php
class Avci
{
    public function avla(Aslan $aslan)
    {
        $aslan->kukre();
    }
}
```

Şimdi oyunumuza `VahsiKopek` eklemeliyiz ki avcı onu da avlayabilsin. Fakat bunu doğrudan yapamayız çünkü köpeğin farklı bir arayüzü var. Avcımıza uyumlu hale getirmek için uyumlu bir adaptör oluşturmak zorundayız.

```php
// Bunun oyuna eklenmesi gerekiyor
class VahsiKopek
{
    public function havla()
    {
    }
}

// Oyunumuzla uyumlu hale getirmek için vahşi köpeğin etrafını adaptör ile sarıyoruz

class VahsiKopekAdaptoru implements Aslan
{
    protected $kopek;

    public function __construct(VahsiKopek $kopek)
    {
        $this->kopek = $kopek;
    }

    public function kukre()
    {
        $this->kopek->havla();
    }
}
```
Artık `VahsiKopek` , `VahsiKopekAdaptoru` kullanılarak oyunumuzda yer alabilecek.

```php
$vahsiKopek = new VahsiKopek();
$vahsiKopekAdaptoru = new VahsiKopekAdaptoru($vahsiKopek);

$avci = new Avci();
$avci->avla($vahsiKopekAdaptoru);
```

🚡 Köprü (Bridge)
------
Gerçek dünya örneği
> Farklı sayfalara sahip bir internet sitenizin olduğunu ve kullanıcının temayı değiştirmesine izin vermeniz gerektiğini düşünün. Nasıl yapardınız? Her tema için sayfaların birden fazla kopyasını mı oluştururdunuz, yoksa sadece temaları ayrı ayrı oluşturup bunları kullanıcının tercihlerine göre yükler miydiniz? İkinci tercih sizi köprü tasarım desenine götürecektir.

![With and without the bridge pattern](https://user-images.githubusercontent.com/8572957/54880823-6466e980-4e5a-11e9-8028-d95621e95270.png)

Basit bir şekilde
> Köprü deseni kalıtım yerine kompozisyonu tercih etmektir. Uygulama detayları, mevcut hiyerarşiden ayrı bir hiyerarşiye sahip başka bir nesneye itilir.

Vikipedi tanımı
> Hem arayüzün hem de somut uygulamanın birbirinden ayrılarak düzenlenmesine olanak sağlar. Arayüzün değişimi uygulamayı, uygulamanın değişimi arayüzü etkilemez. Her ikisi bağımsız olarak geliştirilebilir. 

**Programlı Örnek**

Örneğini verdiğimiz internet sayfası ile ilerleyelim. Burada `WebSayfasi` hiyerarşimiz bulunuyor.

```php
interface WebSayfasi
{
    public function __construct(Tema $tema);
    public function getirIcerik();
}

class Hakkimizda implements WebSayfasi
{
    protected $tema;

    public function __construct(Tema $tema)
    {
        $this->tema = $tema;
    }

    public function getirIcerik()
    {
        return $this->tema->getirRenk()." renkli hakkımızda sayfası";
    }
}

class Kariyer implements WebSayfasi
{
    protected $tema;

    public function __construct(Tema $tema)
    {
        $this->tema = $tema;
    }

    public function getirIcerik()
    {
        return $this->tema->getirRenk()." renkli kariyer sayfası";
    }
}
```
Ve ayrı tema hiyerarşisi
```php

interface Tema
{
    public function getirRenk();
}

class KoyuTema implements Tema
{
    public function getirRenk()
    {
        return 'Koyu siyah';
    }
}
class AcikTema implements Tema
{
    public function getirRenk()
    {
        return 'Kırık beyaz';
    }
}
class MaviTema implements Tema
{
    public function getirRenk()
    {
        return 'Açık mavi';
    }
}
```
And both the hierarchies
```php
$koyuTema = new KoyuTema();

$hakkimizda = new Hakkimizda($koyuTema);
$kariyer = new Kariyer($koyuTema);

echo $hakkimizda->getirIcerik(); // "About page in Dark Black";
echo $kariyer->getirIcerik(); // "Careers page in Dark Black";
```

🌿 Bileşik (Composite)
-----------------

Gerçek dünya örneği
> Her organizasyon çalışanlardan oluşur. Çalışanların her biri aynı özelliklere sahiptir mesela maaşları, bazı sorumlulukları vardır, birilerine rapor vermeleri gerekebilir veya gerekmeyebilir, sertifikaları olabilir veya olmayabilir vs.

Basit bir şekilde
> Bileşik desen, müşterilerin nesnelere tek tip şekilde davranmalarını sağlar.

Vikipedi tanımı
> Nesnelerin parça-bütün ilişkisi içinde ağaç yapısı ile bir araya getirilerek birleştirilmesine ve bu bileşiğe tek ara yüzden ulaşılmasına olanak sağlar. Bileşik yapı yeni nesneler eklenip çıkarılarak zamanla genişleyip daralabilir. 

**Programlı Örnek**

Çalışanlarımızı yukarıdan örnek alarak. Burada farklı çalışan tiplerimiz var.

```php
interface Calisan
{
    public function __construct(string $isim, float $maas);
    public function getirIsim(): string;
    public function degistirMaas(float $maas);
    public function getirMaas(): float;
    public function getirRoller(): array;
}

class Gelistirici implements Calisan
{
    protected $maas;
    protected $isim;
    protected $roller;
    
    public function __construct(string $isim, float $maas)
    {
        $this->isim = $isim;
        $this->maas = $maas;
    }

    public function getirIsim(): string
    {
        return $this->isim;
    }

    public function degistirMaas(float $maas)
    {
        $this->maas = $maas;
    }

    public function getirMaas(): float
    {
        return $this->maas;
    }

    public function getirRoller(): array
    {
        return $this->roller;
    }
}

class Tasarimci implements Calisan
{
    protected $maas;
    protected $isim;
    protected $roller;

    public function __construct(string $isim, float $maas)
    {
        $this->isim = $isim;
        $this->maas = $maas;
    }

    public function getirIsim(): string
    {
        return $this->isim;
    }

    public function degistirMaas(float $maas)
    {
        $this->maas = $maas;
    }

    public function getirMaas(): float
    {
        return $this->maas;
    }

    public function getirRoller(): array
    {
        return $this->roller;
    }
}
```

Ayrıca farklı çalışanlardan oluşan bir organizasyonumuz var.

```php
class Organizasyon
{
    protected $calisanlar;

    public function ekleCalisan(Calisan $calisan)
    {
        $this->calisanlar[] = $calisan;
    }

    public function getirToplamMaas(): float
    {
        $toplamMaas = 0;

        foreach ($this->calisanlar as $calisan) {
            $toplamMaas += $calisan->getirMaas();
        }

        return $toplamMaas;
    }
}
```

Daha sonra bu şekilde kullanılabilir

```php
// Çalışanları hazırla
$ali = new Gelistirici('Ali Ay', 12000);
$ayse = new Tasarimci('Ayşe Güneş', 15000);

// Çalışanları organizasyona ekle
$organizasyon = new Organizasyon();
$organizasyon->ekleCalisan($ali);
$organizasyon->ekleCalisan($ayse);

echo "Toplam Maaş: " . $organizasyon->getirToplamMaas(); // Çıktı: Toplam Maaş: 27000
```

☕ Dekoratör (Decorator)
-------------

Gerçek dünya örneği

> Birden fazla hizmet sunan bir araba tamircisi işlettiğinizi düşünün. Tahsil edilecek faturayı nasıl hesaplıyorsunuz? Bir hizmeti seçiyorsunuz ve son maliyeti elde edene kadar sağlanan hizmetlerin fiyatlarını dinamik olarak eklemeye devam ediyorsunuz. Buradaki her tür hizmet bir dekoratördür. 

Basit bir şekilde
> Dekoratör deseni, dekoratör sınıfının bir nesnesini sararak nesnenin çalışma zamanındaki davranışını dinamik olarak değiştirmenize olanak sağlar.

Vikipedi tanımı
> Bir nesneye, nesneyi değiştirmeden yeni sorumluluklar eklenmesini sağlar. Alt sınıflama yapmadan nesnelerin işlevlerinin geliştirilmesini olası kılar. 

**Programlı Örnek**

Örneğin kahveyi ele alalım. Her şeyden önce kahve arayüzünü uygulayan normal bir kahveye sahibiz.

```php
interface Kahve
{
    public function getirFiyat();
    public function getirAciklama();
}

class NormalKahve implements Kahve
{
    public function getirFiyat()
    {
        return 10;
    }

    public function getirAciklama()
    {
        return 'Normal kahve';
    }
}
```
Gerekirse seçeneklerin değiştirilmesine izin vermek için kodu genişletilebilir hale getirmek istiyoruz. Bazı eklentiler yapalım (dekoratörler)
```php
class SutluKahve implements Kahve
{
    protected $kahve;

    public function __construct(Kahve $kahve)
    {
        $this->kahve = $kahve;
    }

    public function getirFiyat()
    {
        return $this->kahve->getirFiyat() + 2;
    }

    public function getirAciklama()
    {
        return $this->kahve->getirAciklama() . ', süt';
    }
}

class KremaliKahve implements Kahve
{
    protected $kahve;

    public function __construct(Kahve $kahve)
    {
        $this->kahve = $kahve;
    }

    public function getirFiyat()
    {
        return $this->kahve->getirFiyat() + 5;
    }

    public function getirAciklama()
    {
        return $this->kahve->getirAciklama() . ', krema';
    }
}

class VanilyaliKahve implements Kahve
{
    protected $kahve;

    public function __construct(Kahve $kahve)
    {
        $this->kahve = $kahve;
    }

    public function getirFiyat()
    {
        return $this->kahve->getirFiyat() + 3;
    }

    public function getirAciklama()
    {
        return $this->kahve->getirAciklama() . ', vanilya';
    }
}
```

Artık kahve yapabiliriz

```php
$birazKahve = new NormalKahve();
echo $birazKahve->getirFiyat(); // 10
echo $birazKahve->getirAciklama(); // Normal Kahve

$birazKahve = new SutluKahve($birazKahve);
echo $birazKahve->getirFiyat(); // 12
echo $birazKahve->getirAciklama(); // Normal Kahve, süt

$birazKahve = new KremaliKahve($birazKahve);
echo $birazKahve->getirFiyat(); // 17
echo $birazKahve->getirAciklama(); // Normal Kahve, süt, krema

$birazKahve = new VanilyaliKahve($birazKahve);
echo $birazKahve->getirFiyat(); // 20
echo $birazKahve->getirAciklama(); // Normal Kahve, süt, krema, vanilya
```

📦 Cephe (Facade)
----------------

Gerçek dünya örneği
>Bilgisayarı nasıl açarsınız? "Güç düğmesine bas" diyorsunuz! Buna inanırsınız çünkü bilgisayarın dışarıda sağladığı basit bir arayüzü kullanıyorsunuzdur, aslında açılmak için dahili birçok şey yapması gerekir. Karmaşık alt sistemin bu basit arayüzü bir cephe veya ön yüzdür. 

Basit bir şekilde
> Cephe deseni, karmaşık bir alt sisteme basitleştirilmiş bir arayüz sağlar.

Vikipedi tanımı
> Karmaşık bir yapının bir arada tutularak tek bir arayüz üzerinden kullanımına olanak sağlar.

**Programlı Örnek**

Bahsettiğimiz bilgisayar örneğimizi alıyoruz. İşte bilgisayar sınıfımız.

```php
class Bilgisayar
{
    public function getirElektrikSoku()
    {
        echo "Aah!";
    }

    public function yapSes()
    {
        echo "Bip Bip!";
    }

    public function yukleYukleniyorEkrani()
    {
        echo "Yükleniyor..";
    }

    public function hop()
    {
        echo "Kullanıma hazır!";
    }

    public function herseyiKapat()
    {
        echo "Bup bup bup buzzzz!";
    }

    public function hakikat()
    {
        echo "Zzzzz";
    }

    public function cekAkim()
    {
        echo "Aaaah!";
    }
}
```
Cephemiz veya ön yüzümüz ise burada
```php
class BilgisayarCephe
{
    protected $bilgisayar;

    public function __construct(Bilgisayar $bilgisayar)
    {
        $this->bilgisayar = $bilgisayar;
    }

    public function ac()
    {
        $this->bilgisayar->getirElektrikSoku();
        $this->bilgisayar->yapSes();
        $this->bilgisayar->yukleYukleniyorEkrani();
        $this->bilgisayar->hop();
    }

    public function kapat()
    {
        $this->bilgisayar->herseyiKapat();
        $this->bilgisayar->cekAkim();
        $this->bilgisayar->hakikat();
    }
}
```
Artık cepheyi kullanma vakti
```php
$bilgisayar = new BilgisayarCephe(new Bilgisayar());
$bilgisayar->ac(); // Aah! Bip Bip! Yükleniyor.. Kullanıma hazır!
$bilgisayar->kapat(); // Bup bup buzzz! Aaah! Zzzzz
```

🍃 Sinek siklet (Flyweight)
---------

Gerçek dünya örneği
> Hiç bir sanayide çay içtiniz mi? Genellikle içeceklerinden biraz fazlasını yaparlar sebebi ise kaynak kullanımını azaltmak ve gelecek diğer insanlarla da çaylarını paylaşmak isterler. Sinek siklet deseni de tam bununla ilgili, paylaşmak.
 
Basit bir şekilde
> Benzer nesneleri mümkün olduğunca paylaşarak bellek kullanımını veya hesaplama giderlerini en aza indirmek için kullanılır.

Vikipedi tanımı
> Çok sayıda benzer nesnenin yaratılması yerine, bir örnek nesneden görsel nesneler yaratarak kalabalık bir nesne yapısı kurulmasına olanak sağlar. Görsel nesnelerin durum değişkenleri nesnenin kendisi tarafından değil kullanıcı tarafından saklanır. 

**Programlı Örnek**

Çay örneğimizi yapalım. Öncelikle çay çeşitlerimiz ve çay makinemiz var.

```php
// Önbelleğe alınacak herhangi bir şey sinek siklettir.
// Buradaki çay türleri sinek siklet olacaktır.
class RizeCayi
{
}

// Fabrika görevi görür ve çayı saklar
class CayMakinesi
{
    protected $kullanilabilirCay = [];

    public function yap($tercih)
    {
        if (empty($this->kullanilabilirCay[$tercih])) {
            $this->kullanilabilirCay[$tercih] = new RizeCayi();
        }

        return $this->kullanilabilirCay[$tercih];
    }
}
```
Bir de istekleri alan ve servis yapan `Cirak` var.

```php
class Cirak
{
    protected $istekler;
    protected $cayMakinesi;

    public function __construct(CayMakinesi $cayMakinesi)
    {
        $this->cayMakinesi = $cayMakinesi;
    }

    public function alIstek(string $cayTipi, int $masa)
    {
        $this->istekler[$masa] = $this->cayMakinesi->yap($cayTipi);
    }

    public function gotur()
    {
        foreach ($this->istekler as $masa => $cay) {
            echo "#" . $masa." numaralı masaya çay götürülüyor" ;
        }
    }
}
```
Sonra aşağıdaki şekilde kullanılabilir

```php
$cayMakinesi = new CayMakinesi();
$cirak = new Cirak($cayMakinesi);

$cirak->alIstek('kıtlama', 1);
$cirak->alIstek('demli', 2);
$cirak->alIstek('açık', 5);

$cirak->gotur();
// #1 numaralı masaya çay götürülüyor
// #2 numaralı masaya çay götürülüyor
// #5 numaralı masaya çay götürülüyor
```

🎱 Vekil (Proxy)
-------------------
Gerçek dünya örneği
> Hiç bir kapıdan geçmek için erişim kartı kullandınız mı? Kapıyı açmak için birden fazla seçenek vardır, erişim kartı kullanılarak veya düğmeye basılarak açılabilir. Kapının ana işlevi açılmaktır, ancak bunu işlevsel hale getirmek için bir vekil (proxy) eklenmiştir. Aşağıdaki kod örneğini kullanarak daha iyi açıklayayım.

Basit bir şekilde
> Vekil desenini kullanarak, bir sınıf başka bir sınıfın işlevselliğini sunar.

Vikipedi tanımı
> Karmaşık, pahalı ve oluşturulması güç nesneleri kullanmak için arayüz taklidini olası kılar. Kullanılacak olan nesnenin fiziksel yerini kullanıcıdan saklayacak şekilde yönlendirme yapılmasını sağlar. 

**Programlı Örnek**

Güvenlik kapısı örneğimizi alalım. Öncelikle kapı arayüzüne ve kapı uygulamasına sahibiz.

```php
interface Kapi
{
    public function ac();
    public function kapat();
}

class LabKapisi implements Kapi
{
    public function ac()
    {
        echo "Lab kapısı açılıyor";
    }

    public function kapat()
    {
        echo "Lab kapısı kapanıyor";
    }
}
```
O halde istediğimiz herhangi bir kapıyı korumak için bir vekilimiz var
```php
class GuvenlikliKapi
{
    protected $kapi;

    public function __construct(Kapi $kapi)
    {
        $this->kapi = $kapi;
    }

    public function ac($parola)
    {
        if ($this->kimlikDogrulamasi($parola)) {
            $this->kapi->ac();
        } else {
            echo "Hayır olamaz asla! Mümkünatı yok.";
        }
    }

    public function kimlikDogrulamasi($parola)
    {
        return $parola === 'k0dv1z1t';
    }

    public function kapat()
    {
        $this->kapi->kapat();
    }
}
```
Şöyle kullanılabilir
```php
$kapi = new GuvenlikliKapi(new LabKapisi());
$kapi->ac('yanlissifre'); // Hayır olamaz asla! Mümkünatı yok.

$kapi->ac('k0dv1z1t'); // Lab kapısı açılıyor
$kapi->kapat(); // Lab kapısı kapanıyor
```

Durumsal (Behavioral) Tasarım Desenleri
==========================

Basit bir şekilde
>Nesneler arasında sorumlulukların atanması ile ilgilidir. Yapısal desenlerden farklı olarak, yalnızca yapıyı belirtmekle kalmaz, aynı zamanda aralarında mesaj iletme/iletişim desenlerini de ana hatlarıyla belirtir. Başka bir deyişle, "Yazılım bileşeninde bir davranış nasıl çalıştırılır?" sorusuna cevap vermeye yardımcı olurlar. 

Vikipedi tanımı
> Davranış örüntüleri işlevsel sorumlulukların nesneler arasında nasıl atanacağı ve yazılımın gerektirdiği çözüm yöntemlerinin nesnelerce nasıl kullanılacağı hakkında öneriler sunar. Davranış örüntüleri nesne ve sınıf kalıpları yanı sıra nesneler arasındaki iletişim ile ilgili örüntüler de sunar. Davranış örüntüleri tasarımcının nesneler arası iletişim ve iletişim yöntemlerine yoğunlaşmasını sağlar. 

* [Sorumluluklar Zinciri (Chain of Responsibility)](#-chain-of-responsibility)
* [Komut (Command)](#-command)
* [Yineleyici (Iterator)](#-iterator)
* [Arabulucu (Mediator)](#-mediator)
* [Yadigâr (Memento)](#-memento)
* [Gözlemci (Observer)](#-observer)
* [Ziyaretçi (Visitor)](#-visitor)
* [Strateji (Strategy)](#-strategy)
* [Durum (State)](#-state)
* [Şablon Yöntemi (Template Method)](#-template-method)

🔗 Sorumluluk Zinciri (Chain of Responsibility)
-----------------------

Gerçek dünya örneği
> Örneğin 3 farklı ve her birinde farklı miktarlar içeren ödeme yöntemleriniz (`A`, `B` ve `C`) olsun. `A` hesabı 500 TL, `B` hesabı 300 TL ve `C` hesabı 1000 TL olsun ayrıca ödeme tercihi de sırasıyla `A` sonrasında `B` en son ise `C` olsun. 210 TL ücreti olan bir şey almayı deneyin. Sorumluluk Zinciri kullanıldığında ilk olarak `A` hesabı ile satın alınıp alınamayacağı kontrol edilecek, eğer alınabilirse zincir kırılacak. Eğer alınamaz ise, istek `B` hesabına taşınacak eğer alınabilirse zincir kırılacak, diğer durumda ise uygun bir hesap bulunana kadar istek taşınmaya devam edecek. İşte buradaki `A`, `B` ve `C` zincirin bağlantıları, tüm bu olay ise Sorumluluk Zinciri'ni oluşturur.

Basit bir şekilde
> Nesneler zinciri oluşturmayı sağlar. İstek bir nesneden girer ve uygun nesneyi bulanakadar nesneden neseneye geçiş yapar.

Vikipedi tanımı
> Bir kullanıcı(nesnel) isteğinin birden fazla nesne tarafından değerlendirilerek karşılanmaya çalışılmasına olanak sağlar. kullanıcı tek arayüz üzerinden isteğini iletir. İstek zincire bağlı nesneler tarafından sıra ile ele alınarak karşılanmaya çalışılır. İstek karşılanana dek zincir üzerinde bir nesneden diğerine aktarılır. Zaman içinde zincire yeni nesneler eklenmesi ya da çıkarılması mümkündür. Kullanıcı bu tür değişikliklerden arayüz sayesinde etkilenmez. 

**Programlı Örnek**

Hesap örneğimizi ele alalım. Her şeyden önce hesapları bir araya getirme mantığınıve bazı hesapları içeren bir ana hesaba sahibiz.

```php
abstract class Hesap
{
    protected $mirasci;
    protected $bakiye;

    public function ayarlaSonraki(Hesap $hesap)
    {
        $this->mirasci = $hesap;
    }

    public function odeme(float $odenecekMiktar)
    {
        if ($this->odeyebilirMi($odenecekMiktar)) {
            echo sprintf('%s ödendi ve %s hesabı kullanıldı!' . PHP_EOL, $odenecekMiktar, get_called_class());
        } elseif ($this->mirasci) {
            echo sprintf('%s kullanılarak ödeme yapılamadı. İşleniyor..' . PHP_EOL, get_called_class());
            $this->mirasci->odeme($odenecekMiktar);
        } else {
            throw new Exception('Hesapların hiçbirinde yeterli bakiye bulunmuyor');
        }
    }

    public function odeyebilirMi($miktar): bool
    {
        return $this->bakiye >= $miktar;
    }
}

class Banka extends Hesap
{
    protected $bakiye;

    public function __construct(float $bakiye)
    {
        $this->bakiye = $bakiye;
    }
}

class Paypal extends Hesap
{
    protected $bakiye;

    public function __construct(float $bakiye)
    {
        $this->bakiye = $bakiye;
    }
}

class Bitcoin extends Hesap
{
    protected $bakiye;

    public function __construct(float $bakiye)
    {
        $this->bakiye = $bakiye;
    }
}
```
Şimdi zinciri yukarıda tanımlanan bağlantıları kullanarak hazırlayalım (mesela Banka, Paypal, Bitcoin)

```php
// Aşağıdaki gibi bir zincir hazırlayalım
//      $banka->$paypal->$bitcoin
//
// Birinci öncelik banka
//      Eğer banka ile ödeme yapılamazsa Paypal
//          Eğer Paypal ile ödeme yapılamazsa Bitcoin

$banka = new Banka(100);         // Banka bakiye 100
$paypal = new Paypal(200);      // Paypal bakiye 200
$bitcoin = new Bitcoin(300);   // Bitcoin  bakiye 300

$banka->ayarlaSonraki($paypal);
$paypal->ayarlaSonraki($bitcoin);

// Birinci önceliği yani bankayı kullanarak ödeme yapmaya çalışalım
$banka->odeme(259);

// Çıktı şöyle olur
// ==============
// Banka kullanılarak ödeme yapılamadı. İşleniyor..
// Paypal kullanılarak ödeme yapılamadı. İşleniyor..
// 259 ödendi ve Bitcoin hesabı kullanıldı!
```

👮 Komut (Command)
-------

Gerçek dünya örneği
>Genel bir örnek, bir restoranda yemek siparişi vermeniz olabilir.Siz (yani `Müşteri`) garsona (yani `Invoker`) biraz yiyecek getirmesini söyleyin (yani `Komut`) ve garson, neyi nasıl pişireceğini bilen Şef'e (yani `Alıcı`) sadece isteği iletir.

> Başka bir örnek, sizin (yani `Müşteri`) uzaktan kumandayı (yani `Invoker`) kullanarak televizyonu (yani `Alıcı`) açmanızdır (yani `Komut`). 

Basit bir şekilde
> Eylemleri nesnelere yerleştirmenizi sağlar. Bu desenin arkasındaki ana fikir, müşteriyi alıcıdan ayırmak için araçlar sağlamaktır.

Vikipedi tanımı
> Kullanıcı(nesnel) isteklerinin nesnelere dönüştürülerek işlenmesini olası kılar. Bu sayede farklı kullanıcıların istekleri nesnel kayıtlara dönüştürülerek kuyruk ya da kayıtlarda tutulabilir. Bu sayede yapılan işlemlerin geriye dönüştürülmesine de olanak sağlanır. 

**Programlı Örnek**

Her şeyden önce, gerçekleştirilebilecek her eylemin uygulanmasını sağlayan `alıcı`ya sahibiz.

```php
// Alıcı
class Ampul
{
    public function ac()
    {
        echo "Ampul yandı!";
    }

    public function kapat()
    {
        echo "Zifiri karanlık!";
    }
}
```
Sonra komutların her birine uygulanacak bir arayüzümüz ayrıca bazı `komut`larımız var
```php
interface Command
{
    public function gerceklestir();
    public function gerial();
    public function yinele();
}

// Komut
class Ac implements Komut
{
    protected $ampul;

    public function __construct(Ampul $ampul)
    {
        $this->ampul = $ampul;
    }

    public function gerceklestir()
    {
        $this->ampul->ac();
    }

    public function gerial()
    {
        $this->ampul->kapat();
    }

    public function yinele()
    {
        $this->gerceklestir();
    }
}

class Kapat implements Komut
{
    protected $ampul;

    public function __construct(Ampul $ampul)
    {
        $this->ampul = $ampul;
    }

    public function gerceklestir()
    {
        $this->ampul->kapat();
    }

    public function gerial()
    {
        $this->ampul->ac();
    }

    public function yinele()
    {
        $this->gerceklestir();
    }
}
```
Sonra müşterinin herhangi bir emri işlemek için etkileşime gireceği bir `Invoker` var.
```php
// Invoker
class UzaktanKumanda
{
    public function gonder(Komut $komut)
    {
        $komut->gerceklestir();
    }
}
```
Sonunda `müşteri`mizde nasıl kullanabileceğimize bakalım.
```php
$ampul = new Ampul();

$ac = new Ac($ampul);
$kapat = new Kapat($ampul);

$kumanda = new UzaktanKumanda();
$kumanda->gonder($ac); // Ampul yandı!
$kumanda->gonder($kapat); // Zifiri karanlık!
```

Komut deseni, işlem tabanlı bir sistemi uygulamak için de kullanılabilir. Komutların tarihini korumayı sürdürdüğünüz yer en kısa sürede onları gerçekleştirir. Eğer son komut başarılı bir şekilde yerine getirilirse, her şey yolundadır aksi takdirde sadece tarihin içinde yinelenir ve tüm çalıştırılan komutlarda `geri al` komutunu çalıştırmaya devam eder.


➿ Yineleyici (Iterator)
--------

Gerçek dünya örneği
> Eski bir radyo, kullanıcının bazı kanallardan başlayabildiği ve ardından ilgili kanallar arasında ilerlemek için sonraki veya önceki düğmelerini kullanabileceği iyi bir yineleyici örneği olacaktır. Ya da ardışık kanallardan geçmek için sonraki ve önceki düğmelere basabileceğiniz bir MP3 çalar veya televizyon örnek olabilir. Başka bir deyişle, hepsi ilgili kanallar, şarkılar veya radyo istasyonları arasında geçiş yapmak için bir arayüz sağlar.

Basit bir şekilde
> Altında yatan mantığı göstermeden bir nesnenin öğelerine erişmenin yolunu sunar. 

Vikipedi tanımı
> Kitlesel bir nesnenin (Aggragate Object) altında bulunan nesnelere, nesnelerin nasıl temsil edildiklerine ya da gerçeklendiklerine bakılmaksızın, sırasıyla ulaşılmasını sağlar. Bu sayede farklı şekilde temsil edilen nesnelere tek bir arayüz üzerinden ulaşılabilir. 

**Programlı Örnek**

PHP'de SPL (Standart PHP Kütüphanesi) kullanarak bu deseni uygulamak oldukça kolaydır. Radyo istasyonlarımızı yukarıdaki örneğimizi ele alarak oluşturalım. Öncelikle elimizde `RadyoIstasyonu` bulunuyor.

```php
class RadyoIstasyonu
{
    protected $frekans;

    public function __construct(float $frekans)
    {
        $this->frekans = $frekans;
    }

    public function getirFrekans(): float
    {
        return $this->frekans;
    }
}
```
Daha sonra yineleyicimize (iterator) sahibiz.

```php
use Countable;
use Iterator;

class IstasyonListesi implements Countable, Iterator
{
    /** @var RadyoIstasyonu[] $istasyonlar */
    protected $istasyonlar = [];

    /** @var int $sayac */
    protected $sayac;

    public function ekleIstasyon(RadyoIstasyonu $istasyon)
    {
        $this->istasyonlar[] = $istasyon;
    }

    public function silIstasyon(RadyoIstasyonu $silinecek)
    {
        $silinecekFrekans = $silinecek->getirFrekans();
        $this->istasyonlar = array_filter($this->istasyonlar, function (RadyoIstasyonu $istasyon) use ($silinecekFrekans) {
            return $istasyon->getirFrekans() !== $silinecekFrekans;
        });
    }

    // Uygulanan arayüzlerin gerekli metodları
    public function count(): int
    {
        return count($this->istasyonlar);
    }

    public function current(): RadyoIstasyonu
    {
        return $this->istasyonlar[$this->sayac];
    }

    public function key()
    {
        return $this->sayac;
    }

    public function next()
    {
        $this->sayac++;
    }

    public function rewind()
    {
        $this->sayac = 0;
    }

    public function valid(): bool
    {
        return isset($this->istasyonlar[$this->sayac]);
    }
}
```
Artık aşağıdaki gibi kullanabiliriz
```php
$istasyonListesi = new IstasyonListesi();

$istasyonListesi->ekleIstasyon(new RadyoIstasyonu(89));
$istasyonListesi->ekleIstasyon(new RadyoIstasyonu(101));
$istasyonListesi->ekleIstasyon(new RadyoIstasyonu(102));
$istasyonListesi->ekleIstasyon(new RadyoIstasyonu(103.2));

foreach($istasyonListesi as $istasyon) {
    echo $istasyon->getirFrekans() . PHP_EOL;
}

$istasyonListesi->silIstasyon(new RadyoIstasyonu(89)); // İstasyon 89'u silecek
```

👽 Arabulucu (Mediator)
========

Gerçek dünya örneği
> Genel bir örnek olarak cep telefonunuzla biriyle konuştuğunuzda, konuştuğunuz kişi ve sizin aranızda bir şebeke sağlayıcısı vardır. Görüşmeniz doğrudan gönderilmek yerine şebeke sağlayıcı üzerinden iletilir. Bu durumda şebeke sağlayıcı arabulucudur. 


Basit bir şekilde
> Arabulucu deseni, iki nesne (meslektaşlar olarak bilinir) arasındaki etkileşimi kontrol etmek için üçüncü bir nesne (arabulucu olarak bilinir) ekler. Birbirleriyle iletişim kuran sınıflar arasındaki eşleşmeyi azaltmaya yardımcı olur. Çünkü birbirlerinin nasıl çalıştığının bilgisine sahip olmaları gerekmez. 

Vikipedi tanımı
> Birbiri ile bağlatılı olarak çalışan nesnelerin aynı çatı altında tutularak tek bir noktadan(yani ara bulucu tarafından) yönlendirilmesine olanak sağlar. Ara bulucuya bağlı olan nesneler, durum değişikliklerini ara bulucuya iletirler. Ara bulucu uygulamanın gerektirdiği düzenleme ve sıra ile ilgili nesnelerden isteklerde bulunur. Üst seviye kullanıcı nesneler ise sadece ara bulucu ile bağlantı kurarlar. 

**Programlı Örnek**

Birbirlerine mesaj gönderen kullanıcılara (yani meslektaşlara) sahip olan bir sohbet odasının (yani arabulucu) en basit örneği.

Her şeyden önce, elimizde arabulucu yani sohbet odası bulunuyor.

```php
interface SohbetOdasiArabulucu 
{
    public function gosterMesaj(Kullanici $kullanici, string $mesaj);
}

// Arabulucu
class SohbetOdasi implements SohbetOdasiArabulucu
{
    public function gosterMesaj(Kullanici $kullanici, string $mesaj)
    {
        $tarih = date('d M, y H:i');
        $gonderen = $kullanici->getirIsim();

        echo $tarih . '[' . $gonderen . ']:' . $mesaj;
    }
}
```
Daha sonra, elimizde meslektaşlar yani kullanıcılarımız bulunuyor.
```php
class Kullanici {
    protected $isim;
    protected $sohbetArabulucu;

    public function __construct(string $isim, SohbetOdasiArabulucu $sohbetArabulucu) {
        $this->isim = $isim;
        $this->sohbetArabulucu = $sohbetArabulucu;
    }

    public function getirIsim() {
        return $this->isim;
    }

    public function gonder($mesaj) {
        $this->sohbetArabulucu->gosterMesaj($this, $mesaj);
    }
}
```
Kullanımı da şu şekilde
```php
$arabulucu = new SohbetOdasi();

$ali = new Kullanici('Ali Ay', $arabulucu);
$ayse = new Kullanici('Ayşe Güneş', $arabulucu);

$ali->gonder('Merhabaa!');
$ayse->gonder('Selam!');

// Çıktı 
// 14 Şub, 10:58 [Ali Ay]: Merhabaa!
// 14 Şub, 10:58 [Ayşe Güneş]: Selam!
```

💾 Yadigâr (Memento)
-------
Gerçek dünya örneği
> Hesap makinesini düşünelim (yani gönderici). Bir hesaplama yaptığınızda son hesaplama belleğe kaydedilir (yani yadigâr). Böylece yaptığınız hesapları bazı düğmeleri (yani bekçi) kullanarak geri alabilirsiniz. 

Basit bir şekilde
> Yadigâr (memento) deseni, bir nesnenin mevcut durumunu daha sonra düzgün bir şekilde geri yüklenebilecek şekilde yakalamak ve depolamakla ilgilidir.

Vikipedi tanımı
> Yadigâr uygulama yazılımı içerisinde önemli roller üstlenen nesnelerin durumlarını saklamak ve gerektiğinde nesneleri geçmişteki durumlarına geri döndürmek ya da hatırlatmak için kullanılır. 

Genellikle geri alma işlevi eklemeniz gerektiğinde yararlıdır.

**Programlı Örnek**

Zaman zaman güncel durumunu kayıt eden ve istediğiniz zaman geri alabileceğiniz bir metin editörü ele alalım.

Öncelikle, editör durumunu tutabilen yadigâr (memento) nesnesine sahibiz.

```php
class EditorYadigar
{
    protected $icerik;

    public function __construct(string $icerik)
    {
        $this->icerik = $icerik;
    }

    public function getirIcerik()
    {
        return $this->icerik;
    }
}
```

Ayrıca yadigar nesnesini kullanacak olan editörümüze sahibiz.

```php
class Editor
{
    protected $icerik = '';

    public function yaz(string $kelimeler)
    {
        $this->icerik = $this->icerik . ' ' . $kelimeler;
    }

    public function getirIcerik()
    {
        return $this->icerik;
    }

    public function kaydet()
    {
        return new EditorYadigar($this->icerik);
    }

    public function geriYukle(EditorYadigar $yadigar)
    {
        $this->icerik = $yadigar->getirIcerik();
    }
}
```

Sonra aşağıdaki gibi kullanabiliriz

```php
$editor = new Editor();

// Type some stuff
$editor->yaz('Bu ilk cümle.');
$editor->yaz('Bu ikinci cümle.');

// Sonra geri yükleyebilmek için şu durumu kaydet : Bu ilk cümle. Bu ikinci cümle.
$kaydedilmis = $editor->kaydet();

// Biraz daha yazı ekle
$editor->yaz('Bu da üçüncü cümle');

// Çıktı: Kayıt edilmeden önceki içerik
echo $editor->getirIcerik(); // Bu ilk cümle. Bu ikinci cümle. Bu da üçüncü cümle.

// Son kayıt geri yükleniyor
$editor->geriYukle($kaydedilmis);

$editor->getirIcerik(); // Bu ilk cümle. Bu ikinci cümle.
```

😎 Gözlemci (Observer)
--------
Gerçek dünya örneği
> İş bulma kurumuna kayıt olduğunuzu düşünün. Bilgilerinizi verdiğinizde size uygun bir iş fırsatı olduğunda size bildirim gelecektir. Bu durum gözlemci deseni için güzel bir örnek olabilir.

Basit bir şekilde
> Nesneler arasındaki bir bağımlılığı tanımlar, böylece bir nesne durumunu değiştirdiğinde tüm bağımlılara bildirilir.

Vikipedi tanımı
> Bir grup nesnenin, gözlemciler, gözlem altındaki bir nesnede olan değişimlerden otomatik olarak haberdar olmasına olanak sağlar. Gözlem altındaki nesne, kimler tarafından izlendiğinden bağımsız olarak işlevini sürdürür. Zaman içinde yeni gözlemcilerin katılımı ya da ayrılması mümkündür. Bu sayede uygulama zaman içinde davranış değiştirebilir. 

**Programlı Örnek**

Yukarıda bahsettiğimiz iş örneğini ele alalım. Öncelikle iş ilanı için bilgilendirilmesi gereken iş arayanlarımız var.
```php
class IsIlani
{
    protected $baslik;

    public function __construct(string $baslik)
    {
        $this->baslik = $baslik;
    }

    public function getirBaslik()
    {
        return $this->baslik;
    }
}

class IsArayan implements Observer
{
    protected $isim;

    public function __construct(string $isim)
    {
        $this->isim = $isim;
    }

    public function onJobPosted(IsIlani $is)
    {
        // İş ilanı ile ilgili bir şeyler yap
        echo 'Merhaba ' . $this->isim . '! Yani bir iş ilanı yayınlandı: '. $is->getirBaslik();
    }
}
```
Sonra iş arayanların abone olacağı iş ilanlarımız var.
```php
class IsBulmaKurumu implements Observable
{
    protected $gozlemciler = [];

    protected function bilgilendir(IsIlani $isIlani)
    {
        foreach ($this->gozlemciler as $gozlemci) {
            $gozlemci->onJobPosted($isIlani);
        }
    }

    public function attach(Observer $gozlemci)
    {
        $this->gozlemciler[] = $gozlemci;
    }

    public function addJob(IsIlani $isIlani)
    {
        $this->bilgilendir($isIlani);
    }
}
```
Sonra şöyle kullanılabilir
```php
// Abone olanları oluştur
$aliAy = new IsArayan('Ali Ay');
$ayseGunes = new IsArayan('Ayşe Güneş');

// Yayıncıyı oluştur ve abone olanları ilişkilendir
$isIlanlari = new IsBulmaKurumu();
$isIlanlari->attach($aliAy);
$isIlanlari->attach($ayseGunes);

// Yeni bir iş ilanı ekle ve iş arayanlara bildirim gidecek mi gör.
$isIlanlari->addJob(new IsIlani('Yazılım Mühendisi'));

// Çıktı
// Merhaba Ali Ay! Yeni bir iş ilanı yayınlandı: Yazılım Mühendisi
// Merhaba Ayşe Güneş! Yeni bir iş ilanı yayınlandı: Yazılım Mühendisi
```

🏃 Ziyaretçi (Visitor)
-------
Gerçek dünya örneği
> Dubai'ye gidecek birini düşünün. Sadece Dubai'ye gitmenin bir yoluna (yani vizeye) ihtiyaçları var. Dubai'ye indikten sonra, herhangi bir yeri ziyaret etmek için izin almak zorunda kalmadan gelip Dubai'deki herhangi bir yeri tek başlarına ziyaret edebilirler. Ziyaret edebilmeleri için sadece gidecekleri yeri bilmeleri yeterlidir. Ziyaretçi deseni tam da bunu yapmanıza izin verir, ziyaret edeceğiniz yerleri eklemenize yardımcı olur, böylece ekstra bir işlem yapmadan ziyaret edebilecekleri yerlere götürebilirsiniz. 


Basit bir şekilde
> Ziyaretçi (Visitor) deseni, nesneleri değiştirmek zorunda kalmadan nesnelere daha fazla işlem eklemenizi sağlar.

Vikipedi tanımı
> Bileşik bir yapı üzerine yeni işlemler eklenmesine olanak sağlar. Ziyaretçi nesne bileşik yapı içindeki nesneleri tek tek ziyaret ederek gerekli bilgileri toplayıp işleyerek kullanıcıya sunar. 

**Programlı Örnek**

Farklı hayvan türlerimizin olduğu ve onları seslendirmek zorunda olduğumuz bir hayvanat bahçesi simülasyonunu ele alalım. Bunu ziyaretçi desenini kullanarak oluşturalım.

```php
// Ziyaret Edilen
interface Hayvan
{
    public function kabulEt(HayvanOperasyonu $operasyon);
}

// Ziyaretçi
interface HayvanOperasyonu
{
    public function ziyaretEtMaymun(Maymun $maymun);
    public function ziyaretEtAslan(Aslan $aslan);
    public function ziyaretEtYunus(Yunus $yunus);
}
```
Şimdi hayvanlar için arayüzümüzü uygulayalım
```php
class Maymun implements Hayvan
{
    public function ciglikAt()
    {
        echo 'Ooh oo aa aa!';
    }

    public function kabulEt(HayvanOperasyonu $operasyon)
    {
        $operasyon->ziyaretEtMaymun($this);
    }
}

class Aslan implements Hayvan
{
    public function kukre()
    {
        echo 'Roaaar!';
    }

    public function kabulEt(HayvanOperasyonu $operasyon)
    {
        $operasyon->ziyaretEtAslan($this);
    }
}

class Yunus implements Hayvan
{
    public function konus()
    {
        echo 'Tuut tuttu tuutt!';
    }

    public function kabulEt(HayvanOperasyonu $operasyon)
    {
        $operasyon->ziyaretEtYunus($this);
    }
}
```
Ziyaretçimizi uygulayalım
```php
class Konus implements HayvanOperasyonu
{
    public function ziyaretEtMaymun(Maymun $maymun)
    {
        $maymun->ciglikAt();
    }

    public function ziyaretEtAslan(Aslan $aslan)
    {
        $aslan->kukre();
    }

    public function ziyaretEtYunus(Yunus $yunus)
    {
        $yunus->konus();
    }
}
```
Artık şöyle kullanılabilir

```php
$maymun = new Maymun();
$aslan = new Aslan();
$yunus = new Yunus();

$konus = new Konus();

$maymun->kabulEt($konus);    // Ooh oo aa aa!    
$aslan->kabulEt($konus);      // Roaaar!
$yunus->kabulEt($konus);   // Tuut tutt tuutt!
```
Bunu basitçe hayvanlar için bir kalıtım hiyerarşisine sahip olarak yapabilirdik, ancak hayvanlara ne zaman yeni eylemler eklememiz gerekseydi, hayvanları değiştirmek zorunda kalırdık. Ama şimdi onları değiştirmek zorunda kalmayacağız. Örneğin, hayvanlara zıplama davranışını eklememizin istendiğini varsayalım, yeni bir ziyaretçi oluşturarak ekleyelim.

```php
class Zipla implements HayvanOperasyonu
{
    public function ziyaretEtMaymun(Maymun $maymun)
    {
        echo '20 metre yüksekliğe zıpladım! Ağacın üstündeyim!';
    }

    public function ziyaretEtAslan(Aslan $aslan)
    {
        echo '5 metre yüksekliğe zıpladım! Yere indim!';
    }

    public function ziyaretEtYunus(Yunus $yunus)
    {
        echo 'Birazcık su üzerinde durdum ve gözden kayboldum.';
    }
}
```
Kullanımı da şöyle
```php
$zipla = new Zipla();

$maymun->kabulEt($konus);  // Ooh oo aa aa!
$maymun->kabulEt($zipla);  // 20 metre yüksekliğe zıpladım! Ağacın üstündeyim!

$aslan->kabulEt($konus);   // Roaaar!
$aslan->kabulEt($zipla);   // 5 metre yüksekliğe zıpladım! Yere indim!

$yunus->kabulEt($konus);   // Tuut tutt tuutt!
$yunus->kabulEt($zipla);   // Birazcık su üzerinde durdum ve gözden kayboldum.
```

💡 Strateji (Strategy)
--------

Gerçek dünya örneği
> Sıralama yapmamız gerektiğini düşünün, kabarcık sıralamasını *(bubble sort)* uyguladık, ancak veriler artmaya başladı ve baloncuk sıralaması çok yavaş başladı. Bununla baş etmek için hızlı sıralama *(quick sort)* uyguladık. Fakat hızlı sıralama algoritması büyük veri kümeleri için daha iyi olmasına rağmen, küçük veri kümeleri için çok yavaş çalıştı. Bununla başa çıkabilmek için küçük veri kümeleri için kabarcık diziliminin kullanılacak, ve daha büyük veri kümeleri için hızlı sıralama kullanılacak bir strateji uyguladık. 

Basit bir şekilde
> Strateji deseni, duruma göre algoritmayı veya stratejiyi değiştirmenize olanak sağlar.

Vikipedi tanımı
> Aynı arayüz altında, aynı sorunu çözebilecek birçok çözüm yöntemi sınıfını saklayarak, kullanıcı nesnelerin hangi yöntemin kullanıldığından haberdar olmaksızın isteklerinin sağlanmasını olanaklı kılar. Kullanıcı nesneler aynı türden nesnelerle çalıştıklarını var sayarken, farklı davranış biçimleri ile karşılanırlar. 

**Programlı Örnek**

Bahsettiğimiz örneği gerçekleştirelim. Öncelikle strateji arayüzümüz ve farklı strateji uygulamalarımız var.

```php
interface SiralamaStratejisi
{
    public function sirala(array $veriKumesi): array;
}

class KabarcikSiralamaStratejisi implements SiralamaStratejisi
{
    public function sirala(array $veriKumesi): array
    {
        echo "Kabarcık sıralaması kullanılarak sıralanıyor";

        // Sıralamayı yapın
        return $veriKumesi;
    }
}

class HizliSiralamaStratejisi implements SiralamaStratejisi
{
    public function sirala(array $veriKumesi): array
    {
        echo "Hızlı sıralama kullanılarak sıralanıyor";

        // Sıralamayı yapın
        return $veriKumesi;
    }
}
```

Sonra herhangi stratejiyi kullanabilecek sınıfımız var.
```php
class Siralayici
{
    protected $siralayici;

    public function __construct(SiralamaStratejisi $siralayici)
    {
        $this->siralayici = $siralayici;
    }

    public function sirala(array $veriKumesi): array
    {
        return $this->siralayici->sirala($veriKumesi);
    }
}
```
Artık şöyle kullanılabilir
```php
$veriKumesi = [1, 5, 4, 3, 2, 8];

$siralayici = new Siralayici(new KabarcikSiralamaStratejisi());
$siralayici->sirala($veriKumesi); // Çıktı : Kabarcık sıralaması kullanılarak sıralanıyor

$siralayici = new Siralayici(new HizliSiralamaStratejisi());
$siralayici->sirala($veriKumesi); // Çıktı : Hızlı sıralama kullanılarak sıralanıyor
```

💢 Durum (State)
-----
Gerçek dünya örneği
> Bazı çizim uygulamalarını kullandığınızı ve çizmek için boya fırçasını seçtiğinizi düşünün. Fırça, seçilen renge göre davranışını değiştirir, yani kırmızı renk seçtiyseniz, kırmızı, mavi seçtiyseniz mavi vb.

Basit bir şekilde
> Durum değiştiğinde sınıfın davranışını değiştirmenize izin verir.

Vikipedi tanımı
> Bir nesnenin davranışını durumuna göre değiştirmesine olanak sağlar. Kullanıcı açısından, nesne sınıfını değiştiriyormuş izlenimi verir. Uygulamanın gerektirdiği doğrultuda yeni davranışlar eklenip çıkarılmasına olanak sağlar. Kullanıcı nesneler ise bu tür değişikliklerden etkilenmez. 

**Programlı Örnek**

Bir metin editörü örneği alalım. Metin editörü, yazılan metnin durumunu değiştirmenize izin verir. Örneğin, yazı tipini kalın seçtiyseniz kalın, italik seçtiyseniz italik olur vb.

Öncelikle durum arayüzümüz ve uygulamalarımız var.

```php
interface YazmaDurumu
{
    public function yaz(string $kelimeler);
}

class BuyukHarf implements YazmaDurumu
{
    public function yaz(string $kelimeler)
    {
        echo strtoupper($kelimeler);
    }
}

class KucukHarf implements YazmaDurumu
{
    public function yaz(string $kelimeler)
    {
        echo strtolower($kelimeler);
    }
}

class VarsayilanYazi implements YazmaDurumu
{
    public function yaz(string $kelimeler)
    {
        echo $kelimeler;
    }
}
```
Sonra editörümüzvar
```php
class MetinEditoru
{
    protected $durum;

    public function __construct(YazmaDurumu $durum)
    {
        $this->durum = $durum;
    }

    public function degistirDurum(YazmaDurumu $durum)
    {
        $this->durum = $durum;
    }

    public function yaziGirisi(string $kelimeler)
    {
        $this->durum->yaz($kelimeler);
    }
}
```
Artık şöyle kullanılabilir
```php
$editor = new MetinEditoru(new VarsayilanYazi());

$editor->yaziGirisi('İlk satır');

$editor->degistirDurum(new BuyukHarf());

$editor->yaziGirisi('İkinci satır');
$editor->yaziGirisi('Üçüncü satır');

$editor->degistirDurum(new KucukHarf());

$editor->yaziGirisi('Dördüncü satır');
$editor->yaziGirisi('Beşinci satır');

// Çıktı:
// İlk satır
// İKİNCİ SATIR
// ÜÇÜNCÜ SATIR
// dördüncü satır
// beşinci satır
```

📒 Şablon Yöntemi (Template Method)
---------------

Gerçek dünya örneği

>Diyelim ki bir ev yaptırıyoruz. Yapım adımları şu şekilde olabilir
> - Evin temelini hazırla
> - Duvarları inşa et
> - Çatı ekle
> - Başka kat ekle

> Bu adımların sırası hiçbir zaman değiştirilemez, yani duvarları inşa etmeden önce çatıyı inşa edemezsiniz, ancak adımların her biri örneğin duvarlar ahşap veya polyester veya taştan yapılabilir.

Basit bir şekilde
> Şablon yöntemi, belirli bir algoritmanın nasıl gerçekleştirilebileceğinin iskeletini tanımlar, ancak bu adımların alt sınıflarına uygulanmasını engeller.

Vikipedi tanımı
> Bir şablonun çözüm kalıbı olarak kullanılmasına olanak sağlar. Kalıp üzerindeki bazı işlem adımları alt sınıflar tarafından işlenmesine olası kılar. Dolayısıyla ana kalıp değişmeksizin, bazı ara adımlar değişikliğe uğratılabilir. Kullanıcılar bu değişikliklerin farkında olmazlar. 

**Programlı Örnek**

Elimizde bir derleme aracı olduğunu hayal edin. Bu araç test, analiz, derleme yapmak ve derleme raporları oluşturmamıza yardımcı olmakta bize yardımcı oluyor. Bu aracı deneme sunucumuza yerleştirelim.

Öncelikle, derleme algoritması için iskeleti belirten temel sınıfımız var.

```php
abstract class Olusturucu
{

    // Şablon Yöntemleri
    final public function olustur()
    {
        $this->test();
        $this->analiz();
        $this->birlestir();
        $this->dagit();
    }

    abstract public function test();
    abstract public function analiz();
    abstract public function birlestir();
    abstract public function dagit();
}
```

Artık arayüzümüzü uygulayabiliriz

```php
class AndroidOlusturucu extends Olusturucu
{
    public function test()
    {
        echo 'Android testleri çalıştırılıyor';
    }

    public function analiz()
    {
        echo 'Android kodu analiz ediliyor';
    }

    public function birlestir()
    {
        echo 'Android kodları birleştiriliyor';
    }

    public function dagit()
    {
        echo 'Android dosyası sunucuya gönderiliyor';
    }
}

class IosOlusturucu extends Olusturucu
{
    public function test()
    {
        echo 'Ios testleri çalıştırılıyor';
    }

    public function analiz()
    {
        echo 'Ios kodu analiz ediliyor';
    }

    public function birlestir()
    {
        echo 'Ios kodları birleştiriliyor';
    }

    public function dagit()
    {
        echo 'Ios dosyası sunucuya gönderiliyor';
    }
}
```
Artık şöyle kullanılabilir

```php
$androidOlusturucu = new AndroidOlusturucu();
$androidOlusturucu->olustur();

// Çıktı:
// Android testleri çalıştırılıyor
// Android kodu analiz ediliyor
// Android kodları birleştiriliyor
// Android dosyası sunucuya gönderiliyor

$iosOlusturucu = new IosOlusturucu();
$iosOlusturucu->olustur();

// Çıktı:
// Ios testleri çalıştırılıyor
// Ios kodu analiz ediliyor
// Ios kodları birleştiriliyor
// Ios dosyası sunucuya gönderiliyor
```

## 🚦 Buraya Kadar :)

Yüzdük yüzdük kuyruğuna geldik. Bunu geliştirmeye devam edeceğim, bu nedenle tekrar ziyaret etmek için bu depoyu takip etmek ve yıldızlamak isteyebilirsiniz.

## 👬 Katkı Sağlama

- Sorunları raporlayın
- Pull request ile iyileştirin
- Herkese duyurun
- Görüşünüzü iletin [![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/ahmetcnaydemir.svg?style=social&label=%40ahmetcnaydemir%20Takip%20Et)](https://twitter.com/ahmetcnaydemir)

## Lisans

[![License: CC BY 4.0](https://img.shields.io/badge/Lisans-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
