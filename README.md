# Eclipse Cargo Tracker - Jakarta EE için Uygulamalı Domain-Driven Design Örnekleri

Bu proje, Domain-Driven Design (DDD) gibi yaygın olarak kabul görmüş mimari en iyi uygulamaları kullanarak Jakarta EE ile nasıl uygulama geliştirebileceğinizi göstermektedir. Proje, DDD öncüsü Eric Evans'ın şirketi Domain Language ve İsveç yazılım danışmanlık şirketi Citerus tarafından geliştirilen bilinen [Java DDD örnek uygulamasına](https://github.com/citerus/dddsample-core) dayanmaktadır. Kargo örneği aslında Eric Evans'ın DDD hakkındaki önemli kitabından gelmektedir.

Uygulama, kargo sevkiyatlarını takip etmek için uçtan uca bir sistemdir. Aşağıdaki bölümlerde açıklanan çeşitli arayüzlere sahiptir.

Proje hakkında daha fazla bilgi için: https://eclipse-ee4j.github.io/cargotracker/

Projenin temellerini tanıtan bir slayt seti, resmi Eclipse Foundation [Jakarta EE SlideShare hesabında](https://www.slideshare.net/Jakarta_EE/applied-domaindriven-design-blueprints-for-jakarta-ee) mevcuttur. Slayt setinin bir kaydı, resmi [Jakarta EE YouTube hesabında](https://www.youtube.com/watch?v=pKmmZd-3mhA) bulunabilir.

![Eclipse Cargo Tracker kapak](cargo_tracker_cover.png)

## Başlarken

[Proje web sitesinde](https://eclipse-ee4j.github.io/cargotracker/) nasıl başlayacağınız hakkında detaylı bilgiler bulunmaktadır.

En basit adımlar şunlardır (IDE gerekli değildir):

* Proje kaynak kodunu edinin.
* Java SE 11 veya Java SE 17 çalıştırdığınızdan emin olun.
* JAVA_HOME'un ayarlandığından emin olun.
* Proje kaynak kök dizinine gidin ve şu komutu yazın:
```
./mvnw clean package cargo:run
```
* http://localhost:8080/cargo-tracker adresine gidin

Bu, uygulamayı varsayılan olarak Payara Server ile çalıştıracaktır. Proje ayrıca GlassFish ve Open Liberty'yi destekleyen Maven profillerine sahiptir. Örneğin, aşağıdaki komutu kullanarak GlassFish ile çalıştırabilirsiniz:

```
./mvnw clean package -Pglassfish cargo:run
```

Benzer şekilde, aşağıdaki komutu kullanarak Open Liberty ile çalıştırabilirsiniz:

```
./mvnw clean package -Popenliberty liberty:run
```

Visual Studio Code'da kurulum yapmak için şu adımları izleyin:

* Java SE 11 veya Java SE 17, [Visual Studio Code](https://code.visualstudio.com/download) ve [Payara 6](https://www.payara.fish/downloads/payara-platform-community-edition/) kurun. Ayrıca Visual Studio Code'da [Extension Pack for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack) ve [Payara Tools](https://marketplace.visualstudio.com/items?itemName=Payara.payara-vscode) kurmanız gerekecektir.
* JAVA_HOME'un ayarlandığından emin olun.
* Kodu içeren dizini Visual Studio Code'da açın. Visual Studio Code geri kalanını sizin için halledecektir, otomatik olarak bir Maven projesi yapılandırmalıdır. Uygulamayı temizleme/derleme işlemine geçin.
* Proje oluşturulduktan sonra (bu, Maven bağımlılıkları indirirken ilk kez biraz zaman alacaktır), `target` dizini altında oluşturulan `cargo-tracker.war` dosyasını Payara Tools kullanarak çalıştırın.

Benzer şekilde, Visual Studio Code veya Eclipse IDE for Enterprise and Web Developers'da GlassFish veya Open Liberty kullanabilirsiniz.

## Uygulamayı Keşfetmek

Uygulama çalıştıktan sonra, http://localhost:8080/cargo-tracker/ adresinde erişilebilir olacaktır. Uygulama, arka planda Faces, CDI, Enterprise Beans, Persistence, REST, Batch, JSON Binding, Bean Validation ve Messaging dahil olmak üzere birçok Jakarta EE özelliği kullanmaktadır.

Çeşitli web arayüzleri, REST arayüzleri ve dosya sistemi tarama arayüzü bulunmaktadır. Büyük olasılıkla arayüzleri aşağıdaki kabaca sırayla keşfetmeye başlamak en iyisidir.

İzleme arayüzü, kargo durumunu takip etmenize olanak tanır ve halka açık olması amaçlanmıştır. ABC123 gibi bir takip numarası girmeyi deneyin (uygulama bazı örnek verilerle önceden doldurulmuştur).

Yönetim arayüzü, kargoyu yöneten nakliye şirketi için tasarlanmıştır. Arayüzün açılış sayfası, kayıtlı kargolara genel bir bakış sunan bir paneldir. Rezervasyon arayüzünü kullanarak kargo rezervasyonu yapabilirsiniz. Kargo rezervasyonu yapıldıktan sonra, onu yönlendirebilirsiniz. Bir rota talebi başlattığınızda, sistem kargo için çalışabilecek rotaları belirleyecektir. Bir rota seçtiğinizde, kargo limanda elleçleme olaylarını işlemeye hazır olacaktır. Gerekirse kargo için varış noktasını değiştirebilir veya kargoyu takip edebilirsiniz.

Elleçleme Olayı Günlüğü arayüzü, kargoya ne olduğunu kaydeden liman personeli için tasarlanmıştır. Arayüz öncelikle mobil cihazlar için tasarlanmıştır, ancak masaüstü tarayıcı üzerinden de kullanabilirsiniz. Arayüze şu URL'den erişilebilir: http://localhost:8080/cargo-tracker/event-logger/index.xhtml. Kolaylık için, gerçek bir mobil cihaz yerine bir mobil emülatör kullanabilirsiniz. Genel olarak kargo şu olaylardan geçer:

* Menşe konumunda alınır.
* Güzergahındaki seyahatlere yüklenir ve boşaltılır.
* Varış yerinde teslim alınır.
* Keyfi noktalarda gümrükten geçebilir.

Olay kayıt formunu doldururken, güzergahı hazır bulundurmak en iyisidir. Kayıtlı kargolar için güzergaha yönetici arayüzü üzerinden erişebilirsiniz. Kargo elleçleme, ölçeklenebilirlik için Messaging aracılığıyla yapılır. Olay kaydedicisini kullanırken, yalnızca yükleme ve boşaltma olaylarının ilişkili bir seyahate ihtiyaç duyduğunu unutmayın.

Ayrıca dosya sistemi tabanlı toplu olay kayıt arayüzünü de keşfetmelisiniz. /tmp/uploads altındaki dosyaları okur. Dosyalar sadece CSV dosyalarıdır. Örnek bir CSV dosyası [src/test/sample/handling_events.csv](src/test/sample/handling_events.csv) altında mevcuttur. Örnek, ABC123 kargosu için kalan güzergah olaylarına uyacak şekilde zaten ayarlanmıştır. Sadece örnek CSV dosyasının ilk sütunundaki zamanları güzergaha uyacak şekilde güncellediğinizden emin olun.

Başarıyla işlenen girdiler /tmp/archive altında arşivlenir. Başarısız kayıtlar /tmp/failed altında arşivlenir.

Hata yapmaktan endişelenmeyin. Uygulamanın oldukça hata toleranslı olması amaçlanmıştır. Sorunlarla karşılaşırsanız, [bildirmelisiniz](https://github.com/eclipse-ee4j/cargotracker/issues).

Yeniden başlamak için dosya sisteminden ./cargo-tracker-data'yı kaldırabilirsiniz. Bu dizin tipik olarak $payara-kurulumunuz/glassfish/domains/domain1/config altında olacaktır.

Ayrıca kaynak koduna dahil edilen soapUI betiklerini kullanarak REST arayüzlerini ve kod tabanını genel olarak kapsayan çok sayıda birim testini de keşfedebilirsiniz. Bazı testler Arquillian kullanır.

## Kodu Keşfetmek

Daha önce de belirtildiği gibi, uygulamanın asıl amacı, iyi tasarlanmış, etkili Jakarta EE uygulamalarının nasıl oluşturulacağını göstermektir. Bu amaçla, uygulama işlevselliği hakkında biraz bilgi edindikten sonra yapılacak bir sonraki şey doğrudan kodun içine dalmaktır.

DDD, mimarinin önemli bir yönüdür, bu nedenle en azından DDD hakkında çalışma anlayışı elde etmek önemlidir. İsmi de ima ettiği gibi, Domain-Driven Design, yazılım tasarımı ve geliştirmesine odaklanan bir yaklaşımdır.

Çoğunlukla, Jakarta EE'ye yeni olmanız sorun değil. Sunucu tarafı uygulamaları hakkında temel bir anlayışa sahip olduğunuz sürece, kod başlamak için yeterince iyi olmalıdır. Jakarta EE hakkında daha fazla bilgi edinmek için, projenin kaynak bölümünde birkaç bağlantı önerdik. Elbette, projenin ideal kullanıcısı, hem Jakarta EE hem de DDD hakkında temel bir çalışma anlayışına sahip olan kişidir. Jakarta EE'deki büyük miktarda API ve özelliği göstermek için bir mutfak lavabo örneği olmak amacımız olmasa da, çok temsili bir set kullanıyoruz. Şeylerin nasıl uygulandığını görmek için koda dalarak oldukça fazla şey öğreneceğinizi göreceksiniz.

## Bulut Demo
Cargo Tracker, GitHub Actions iş akışları kullanılarak bulutta Kubernetes'e dağıtılmaktadır. Demo dağıtımını Scaleforce bulutunda bulabilirsiniz (https://cargo-tracker.j.scaleforce.net). Bu proje, demoyu barındırdıkları için sponsorlarımız [Jelastic](https://jelastic.com) ve [Scaleforce](https://www.scaleforce.net)'a çok teşekkür eder! Dağıtım ve tüm veriler her gece yenilenir. Bulutta Cargo Tracker, veritabanı olarak PostgreSQL kullanır. Docker görüntülerini yayınlamak için [GitHub Container Registry](https://ghcr.io/eclipse-ee4j/cargo-tracker) kullanılır.

## Jakarta EE 8
Cargo Tracker'ın Jakarta EE 8, Java SE 8, Payara 5 versiyonu ['jakartaee8' dalında](https://github.com/eclipse-ee4j/cargotracker/tree/jakartaee8) mevcuttur.

## Java EE 7
Cargo Tracker'ın Java EE 7, Java SE 8, Payara 4.1 versiyonu ['javaee7' dalında](https://github.com/eclipse-ee4j/cargotracker/tree/javaee7) mevcuttur.

## Katkıda Bulunma
Bu proje, [Java](https://google.github.io/styleguide/javaguide.html), [JavaScript](https://google.github.io/styleguide/jsguide.html) ve [HTML/CSS](https://google.github.io/styleguide/htmlcssguide.html) için Google Stil Kılavuzlarına uymaktadır. Google Java Stil Kılavuzuna uymanıza yardımcı olmak için [google-java-format](https://github.com/google/google-java-format) aracını kullanabilirsiniz. Bu aracı Eclipse, Visual Studio Code ve IntelliJ gibi çoğu büyük IDE ile kullanabilirsiniz.

Genel olarak tüm dosyalar için mümkün olduğunca 80 sütun/satır genişliği kullanırız ve girinti için 2 boşluk kullanırız. Tüm dosyalar yeni bir satırla bitmelidir. Lütfen IDE'nizin biçimlendirme ayarlarını buna göre ayarlayın. Kodunuzu biçimlendirmeye yardımcı olmak için HTML Tidy ve CSS Tidy kullanmanız teşvik edilir ancak gerekli değildir.

Proje yol haritası da dahil olmak üzere katkıda bulunma hakkında daha fazla rehberlik için [buraya](CONTRIBUTING.md) bakın.

## Bilinen Sorunlar
* Visual Studio Code kullanırken, JAVA_HOME ortam değişkeninin doğru şekilde ayarlandığından emin olun. Düzgün yapılandırılmamışsa, Visual Studio Code'da bir Payara Server örneği eklerken bir alan seçemezsiniz.
* Visual Studio Code kullanırken, Payara'nın boşluk içeren bir yola kurulmadığından emin olun (örneğin: C:\Program Files\payara6). Payara, Payara Tools uzantısıyla başlatılamayacaktır. Payara'yı boşluk içermeyen bir yola kurun (örneğin: C:\payara6).
* Payara SSL sertifikalarının süresi dolduğunu belirten bir günlük mesajı alabilirsiniz. Bu işlevselliğin önüne geçmez, ancak günlük mesajlarının IDE konsoluna yazdırılmasını durdurur. Bu sorunu, [burada](https://github.com/payara/Payara/issues/3038) açıklandığı gibi süresi dolmuş sertifikaları Payara alanından manuel olarak kaldırarak çözebilirsiniz.
* Uygulamayı birkaç kez yeniden başlatırsanız, sahte bir dağıtım hatasına neden olan bir hataya rastlayacaksınız. Sorun sinir bozucu olabilse de, zararsızdır. Sadece uygulamayı yeniden çalıştırın (uygulamayı tamamen kaldırdığınızdan ve Payara'yı ilk önce kapattığınızdan emin olun).
* Bazen sunucu düzgün kapatılmadığında veya bir kilitleme/izin sorunu olduğunda, uygulamanın kullandığı H2 veritabanı bozulabilir, bu da garip veritabanı hatalarına neden olur. Bu durumda, uygulamayı durdurup veritabanını temizlemeniz gerekecektir. Bunu, cargo-tracker-data dizinini dosya sisteminden kaldırarak ve uygulamayı yeniden başlatarak yapabilirsiniz. Bu dizin genellikle $payara-kurulumunuz/glassfish/domains/domain1/config altında olacaktır.
* GlassFish kullanırken, testler `CIRCULAR REFERENCE` hatasıyla başarısız olursa, GlassFish başlatma süresinin zaman aşımına uğradığı anlamına gelir. Varsayılan zaman aşımı 60 saniyedir. Bu, özellikle Windows Defender gibi virüs tarayıcıları GlassFish başlatmayı geciktiriyorsa, bazı sistemlerde yeterli olmayabilir. `AS_START_TIMEOUT` ortam değişkenini ayarlayarak GlassFish başlatma zaman aşımını artırabilirsiniz. Örneğin, 3 dakikalık bir zaman aşımı için 180000 olarak ayarlayabilirsiniz.
* Open Liberty ile çalışırken, çok sayıda sahte hata fark edeceksiniz. Shrinkwrap özellikleri uyarıları, mesaj odaklı bean uyarıları, AggregateObjectMapping iç içe geçmiş yabancı anahtar uyarısı, G/Ç hataları vb. göreceksiniz. Bunları güvenle göz ardı edebilirsiniz. Uygulama işlevselliğini etkilemezler.

## Proje Yapısı

Proje, Domain-Driven Design (DDD) prensiplerine göre düzenlenmiştir:

```
src/main/java/org/eclipse/cargotracker/
├── application/     # Uygulama katmanı - uygulama servisleri
├── domain/          # Domain katmanı - temel iş mantığı
├── infrastructure/  # Altyapı katmanı - teknik detaylar
└── interfaces/      # Arabirimler - UI, REST API, dış servisler
```

### Proje İçeriği
- **application/** - Uygulama servisleri, domain modeli ile UI arasındaki bağlantıyı sağlar
- **domain/** - DDD'nin kalbi, temel iş kuralları ve varlıklar burada bulunur
- **infrastructure/** - Teknik detaylar ve dış sistemlerle entegrasyon
- **interfaces/** - Kullanıcı arayüzleri ve dış dünya ile iletişim

Bu yapı, Eric Evans'ın Domain-Driven Design kitabındaki katmanlı mimari prensibini yansıtmaktadır. Uygulama, işlevsel olmayan gereksinimlerden (kullanıcı arayüzü, veritabanı, dış sistemler) bağımsız olarak temel iş mantığını izole etmeyi amaçlar.

## Detaylı Proje Yapısı

Projenin Domain-Driven Design prensiplerine göre düzenlenmiş daha detaylı yapısı aşağıdaki gibidir:

```
src/main/java/org/eclipse/cargotracker/
├── application/              # Uygulama Katmanı
│   ├── BookingService.java   # Kargo rezervasyon servisi
│   ├── CargoInspectionService.java # Kargo denetim servisi
│   ├── HandlingEventService.java   # Elleçleme olayları servisi
│   ├── ApplicationEvents.java      # Uygulama olayları
│   ├── internal/             # İç uygulama servisleri
│   └── util/                 # Yardımcı sınıflar
│
├── domain/                   # Domain Katmanı (Temel İş Mantığı)
│   ├── model/                # Domain modelleri ve varlıklar
│   ├── service/              # Domain servisleri
│   └── shared/               # Paylaşılan domain bileşenleri
│
├── infrastructure/           # Altyapı Katmanı
│   ├── events/               # Olay yönetimi altyapısı
│   ├── logging/              # Günlük kaydı altyapısı
│   ├── messaging/            # Mesajlaşma altyapısı
│   ├── persistence/          # Veritabanı işlemleri
│   └── routing/              # Rota yönetimi altyapısı
│
└── interfaces/               # Arayüzler Katmanı
    ├── booking/              # Rezervasyon arayüzleri
    ├── handling/             # Elleçleme arayüzleri
    ├── tracking/             # İzleme arayüzleri
    ├── Coordinates.java      # Koordinat arayüzü
    └── CoordinatesFactory.java # Koordinat fabrikası
```

Bu yapı, Eric Evans'ın Domain-Driven Design kitabındaki katmanlı mimari prensibini yansıtmaktadır:

1. **Domain Katmanı**: Uygulamanın çekirdeğidir, iş modellerini ve iş kurallarını içerir.
2. **Uygulama Katmanı**: Domain katmanı ile kullanıcı arayüzü arasında aracılık eden, iş kullanım senaryolarını koordine eden katmandır.
3. **Altyapı Katmanı**: Veritabanı, mesajlaşma, loglama gibi teknik detayları sağlar.
4. **Arayüzler Katmanı**: Kullanıcılar ve dış sistemler ile etkileşimi sağlayan katmandır (UI, REST API, olay işleme vb.).

Bu katmanlı mimari, uygulamanın temel iş mantığını (domain) teknolojik ve sunum ayrıntılarından izole ederek, bakımı daha kolay, test edilebilir ve değişime daha açık bir yapı oluşturur.
