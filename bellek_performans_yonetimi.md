# Bellek ve Performans Yönetimi

Yazılım geliştirmede kodun sadece çalışması değil, sistem kaynaklarını nasıl tükettiği de kritik bir önem taşır. Bu dokümanda, bellek yönetiminin temel taşlarını ve performans optimizasyonunun mantığını inceleyeceğiz.

---

## 1. Stack ve Heap Farkı Nedir?

Uygulamanın çalışması sırasında bellek, iki farklı stratejiyle yönetilir:

### Stack (Yığın)
* **Özellikleri:** Çok hızlı, düzenli ve boyutu derleme zamanında (compile-time) belli olan bellek alanıdır.
* **Yönetim:** Tamamen derleyiciye (compiler) aittir. 
* **Yaşam Döngüsü:** İlgili kod bloğunun (`scope`) çalışması bittiğinde, bellek otomatik ve anında boşaltılır.

### Heap (Öbek)
* **Özellikleri:** Stack'e kıyasla daha yavaş ve düzensizdir. Boyutu çalışma zamanında (runtime) dinamik olarak değişebilen veriler burada tutulur.
* **Yönetim:** Esnektir ancak takibi zordur. Buradaki verilerin ne zaman silineceği otomatik olarak bilinmez; yönetilmesi veya serbest bırakılması gerekir.

---

## 2. Garbage Collection (GC) Nedir?

`Heap` bölgesi üzerinde artık kullanılmayan, sahipsiz kalmış nesneleri tespit edip temizleyen otomatik bir bellek yönetim sistemidir. 

GC, temel olarak iki aşamada çalışır:

1. **Mark (İşaretleme):** GC, uygulamanın kök noktalarından (`root`) yola çıkarak `heap` üzerindeki tüm nesnelere ulaşmaya çalışır. Ulaştığı nesneleri "hâlâ kullanımda" olarak işaretler.
2. **Sweep (Süpürme):** Kökten ulaşılamayan, yani kodun hiçbir yerinde referansı kalmamış nesneleri tespit eder, siler ve bu bellek alanını işletim sistemine iade eder.

> **Performans Notu (Stop-the-World):** GC çalışırken CPU döngülerini tüketir. Bellek temizliğini güvenli ve tutarlı yapabilmek için uygulamanın çalışmasını çok kısa süreliğine durdurur. Buna **Stop-the-World (STW)** duraklaması denir. Sık devreye giren bir GC, uygulamada mikro takılmalara yol açar.

---

## 3. Memory Leak (Bellek Sızıntısı) Nedir?

İşlevi bitmesine ve bir daha kullanılmayacak olmasına rağmen, bir nesnenin bellekten silinememesi ve orada kalmaya devam etmesi durumudur.

### Nasıl Oluşur?
`Garbage Collection` sadece referansı (bağlantısı) tamamen kopmuş nesneleri silebilir. Eğer bir nesneyle işiniz bittiği halde, o nesnenin referansını kazara aşağıdakilerden birinde unutursanız:
* Global bir listede veya statik bir `Cache` mekanizmasında,
* Kapatılmamış bir veri tabanı veya ağ bağlantısında (`Stream` / `Connection`),
* Temizlenmemiş bir `Event Listener` içinde,

GC o nesneyi hâlâ kullanılıyor zannederek dokunmaz. Zamanla bu unutulan nesneler birikir, bellek şişer ve en sonunda uygulama **Out of Memory (OOM)** hatası vererek çöker.

---

## Mühendislik Perspektifi: Gerçekçi Bir Bakış

Junior ve Pre-Junior aşamalarında odak noktası genellikle kodun sadece "istenen çıktıyı vermesi" (çalışması) yönündedir. Ancak profesyonel yazılım geliştirme, sistem kaynaklarına olabildiğince saygılı kod yazmayı gerektirir. 

Bir veriyi belleğe alırken veya bir bağlantı açarken her zaman şu üç soru sorulmalıdır:
1. "Bu veri bellekte ne kadar kalacak?"
2. "İşim bittiğinde bu kaynağı (disposable) doğru şekilde kapattım mı?"
3. "Oluşturduğum bu nesne arkasında bir iz (referans) bırakıyor mu?"

Temiz ve performanslı kod, sadece doğru çalışan değil; belleği de arkasında çöp bırakmadan kullanan koddur.