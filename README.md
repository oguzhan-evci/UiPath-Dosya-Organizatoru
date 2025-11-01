
# Dosya Organizatörü Otomasyonu

![Dosya Organizatörü Otomasyonu](FileEditor.gif)

Bu UiPath projesi, belirtilen bir klasördeki dosyaları analiz ederek, uzantılarına göre yeni dosya kategorileri oluşturan ve dosyaları bu klasörlere taşıyan bir otomasyondur. İşlem sonunda tüm süreci özetleyen detaylı bir rapor e-posta ile kullanıcıyı bilgilendirir.

---

## Projenin Amacı

Önemli dosyalarımızı genellikle özenle klasörleriz, ancak daha az önemli gördüğümüz dosyalar çoğunlukla masaüstünde dağınık bir şekilde birikir. Bu dosyaları silmek her zaman iyi bir seçenek değildir, çünkü ileride tekrar ihtiyaç duyabiliriz. Zamanla bu durum, masaüstünde kontrolü zor bir karmaşaya yol açar.

Bu proje, tam da bu sorunu çözmeyi hedefler. Amacı, masaüstünüzde biriken ve "bir gün lazım olur" diye saklanan tüm bu dosyaları, size hiçbir zahmet vermeden, saniyeler içinde düzenlemektir. Otomasyon, tüm dosyaları uzantılarına göre (TXT Dosyaları, PDF Dosyaları vb.) otomatik olarak kategorize eder ve ilgili klasörlere taşır.

Bu basit çözümle, masaüstünüzdeki dağınıklık sona erer ve dosyalarınız her zaman düzenli ve ulaşılabilir olur.

---

## Kullanılan Yöntemler ve Teknolojiler

Projenin geliştirme temeli, "Windows" uyumluluğuna ayarlı olduğu için **.NET Framework**'tür. Farklı platformlarda (Windows, Linux ve macOS) çalışması istenirse ise **.NET Core** tabanlı "**Cross-platform**" proje tipi tercih edilebilir.

### Dosya Sistemi Yönetimi

* **Keşif:** Hedef klasördeki tüm dosyalar, `System.IO.Directory.GetFiles()` metodu kullanılarak verimli bir şekilde listelenir.
* **Analiz:** Her dosyanın türü, `System.IO.Path.GetExtension()` ile uzantısı okunarak belirlenir. Bu, sınıflandırma mantığının temelini oluşturur.
* **Organizasyon:** `MoveFile` aktivitesi ile dosyalar ilgili kategori klasörlerine taşınır.

### Dinamik Klasör Yapılandırması

* **Mevcut Yaklaşım (Dinamik):** Otomasyon, bir dosyayı işlemeden önce, o dosyanın taşınacağı kategori klasörünün (örn: "PDF Dosyaları") var olup olmadığını kontrol eder. Eğer klasör mevcut değilse, `System.IO.Directory.CreateDirectory()` metodunu kullanarak o anda otomatik olarak oluşturur.

* **Neden Bu Yöntem Tercih Edildi?** Bu dinamik yaklaşım, otomasyonu son derece esnek kılar. Gelecekte hiç beklemediğiniz bir dosya türüyle (`.epub`, `.svg` vb.) karşılaştığında bile, otomasyon anında uyum sağlar ve yeni kategori klasörünü kendi kendine oluşturur.

* **Alternatif Yaklaşım (Statik):** Eğer statik bir yapı kullanılsaydı, otomasyonu çalıştırmadan önce olası tüm dosya türleri için klasörleri (PDF Dosyaları, DOCX Dosyaları, JPG Dosyaları vb.) sizin manuel olarak oluşturmanız gerekirdi. Eğer yeni bir dosya türüyle karşılaşırsa ve onun için önceden bir klasör oluşturulmamışsa, otomasyon hata verip dururdu. Dinamik yapı, bu bakım yükünü ve kırılganlığı ortadan kaldırır.

### Süreç Kontrolü ve Hata Yönetimi

* Ana iş akışı, bir **For Each** döngüsü ile yönetilir ve her dosyanın tek tek işlenmesini garanti eder.
* Tüm dosya operasyonları, bir **Try-Catch** bloğu ile sarmalanmıştır. Bu yapı, tek bir dosyada oluşan bir hatanın (örn: erişim engeli) tüm otomasyonu durdurmasını önler ve hatayı raporlanmak üzere kaydeder.

### Raporlama ve Bildirim

* **Rapor Oluşturma:** Süreç boyunca toplanan veriler (işlem başlangıç/bitiş zamanı, toplam süre, başarılı ve hatalı dosya sayıları), metin birleştirme (**String Manipulation**) teknikleri kullanılarak tek bir anlamlı rapor metni haline getirilir.
* **E-posta Gönderimi:** Hazırlanan bu özet rapor, modern bir yaklaşım olan **Use Gmail** aktivitesi kullanılarak işlem sonunda otomatik olarak belirtilen alıcıya e-posta ile iletilir. Bu aktivite, `UiPath.GSuite.Activities` paketinin bir parçası olup, güvenli ve kolay bir entegrasyon sağlar.

---

## Proje Bağımlılıkları

Bu projenin çalışabilmesi için bazı temel UiPath aktivite paketlerine ihtiyacı vardır. Ancak manuel olarak yüklemenize gerek yok; UiPath Studio, projeyi ilk kez açtığınızda aşağıda listelenen bu paketleri ve diğer tüm bağımlılıkları `project.json` dosyasını okuyarak sizin için otomatik olarak yükleyecektir.

Projenin temelini oluşturan ana paketler şunlardır:

* **`UiPath.System.Activities`**: Dosya ve klasör oluşturma, taşıma, döngüler ve koşullu ifadeler gibi otomasyonun bel kemiğini oluşturan temel sistem işlemlerini içerir.
* **`UiPath.Mail.Activities` ve `UiPath.GSuite.Activities`**: Süreç sonunda hazırlanan raporun e-posta olarak gönderilmesi için `Use Gmail` gibi modern ve güvenli e-posta entegrasyonu yeteneklerini sağlar.
* **`UiPath.UIAutomation.Activities`**: Bu proje arayüz otomasyonu yapmasa da, genellikle varsayılan olarak eklenen ve temel arayüz etkileşimleri için gerekli olan standart bir pakettir.

---

## Kurulum ve Kullanım Kılavuzu

Bu otomasyonu kendi makinenizde çalıştırmak için aşağıdaki adımları takip edebilirsiniz.

### Sistem Gereksinimleri

* **İşletim Sistemi:** Windows
* **Not:** Bu proje, .NET Framework tabanlı olduğu için macOS veya Linux üzerinde çalışmaz.
* **Ortam:** UiPath Studio

### Ön Hazırlık

1.  **Gmail Entegrasyonu:** Raporu gönderebilmek için bir gmail hesabı oluşturmanız veya giriş yapmanız ve bir de gönderilecek hesap seçmeniz gerekir.
2.  **Aktivite Paketleri:** Gerekli tüm aktivite paketleri, proje UiPath Studio'da açıldığında otomatik olarak yüklenecektir. Manuel bir kurulum gerekmez.

### Kurulum Adımları

1.  **Projeyi İndirin:**
    * Bu repoyu, "Code" > "Download ZIP" seçeneği ile indirin ve dosyaları bir klasöre çıkarın.
    * Alternatif olarak, Git kurulu ise `git clone [İndirilecek Projenin GitHub URL'si]` komutunu kullanabilirsiniz.
2.  **Projeyi Açın:**
    * İndirdiğiniz klasör içindeki `project.json` dosyasına çift tıklayarak projeyi UiPath Studio'da açın.
3.  **Çalışma Alanını Hazırlayın:**
    * **ÖNEMLİ:** Bu otomasyonun mevcut versiyonu, dosyaları organize etmek için sabit olarak kodlanmış bir yol kullanır: **Masaüstü\Düzenlenecek**.
    * Bu nedenle, otomasyonu çalıştırmadan önce kendi **Masaüstünüzde "Düzenlenecek" adında bir klasör oluşturduğunuzdan** ve organize edilecek dosyaları bu klasörün içine koyduğunuzdan emin olun.
4.  **E-postayı Yapılandırın:**
    * `Main.xaml` dosyasını açın ve **Use Gmail** aktivitesini bulun.
    * Kendi Gmail hesap bilgilerinizi girin ve raporun gönderileceği alıcı e-posta adresini (`To` alanı) güncelleyin.
5.  **Otomasyonu Başlatın:**
    * UiPath Studio'da "**Run File**" butonuna tıklayarak otomasyonu çalıştırın.

İşlem tamamlandığında, masaüstünüzdeki "Düzenlenecek" klasörünün organize olduğunu ve yapılandırdığınız e-posta adresine bir özet raporu geldiğini göreceksiniz.

---

## İyileştirme Fikirleri

Otomasyonu daha da kullanıcı dostu hale getirmek ve yeteneklerini artırmak amacıyla ileride eklenebilecek bazı özellikler şunlardır:

* **İçeriğe Göre Akıllı Arşivleme:** Kullanıcıların aradıkları dosyaları daha kolay bulabilmesi için gelecekte **Document Understanding** entegrasyonu düşünülebilir. Bu sayede otomasyon, bir faturayı veya sözleşmeyi içeriğinden tanıyarak, dosyaları "Faturalar" veya "Sözleşmeler" gibi, kullanıcının daha anlamlı bulacağı klasörlere ayırabilir.
* **Kullanıcıya Özel Klasör Seçimi:** Kullanıcılara daha fazla esneklik sunmak amacıyla, otomasyonun başında bir diyalog penceresi (**Input Dialog**) eklenebilir. Bu sayede her kullanıcı, organize etmek istediği klasörü kendisi seçebilir ve sabit bir yola bağlı kalmadan kişiselleştirebilir.

---

## 📄 Lisans

Bu proje, MIT Lisansı koşulları altında lisanslanmıştır.
