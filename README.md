# Dosya Organizatörü Otomasyonu

![Dosya Organizatörü Otomasyonu](FileEditor.gif)

Bu UiPath projesi, belirtilen bir klasördeki dosyaları analiz ederek, uzantılarına göre yeni dosya kategorileri oluşturan ve dosyaları bu klasörlere taşıyan bir otomasyondur. İşlem sonunda tüm süreci özetleyen detaylı bir rapor e-posta ile kullanıcıyı bilgilendirir.

---

## 🎯 Projenin Amacı

Önemli dosyalarımızı genellikle özenle klasörleriz, ancak daha az önemli gördüğümüz dosyalar çoğunlukla masaüstünde dağınık bir şekilde birikir. Bu dosyaları silmek her zaman iyi bir seçenek değildir, çünkü ileride tekrar ihtiyaç duyabiliriz. Zamanla bu durum, masaüstünde kontrolü zor bir karmaşaya yol açar.

Bu proje, tam da bu sorunu çözmeyi hedefler. Amacı, masaüstünüzde biriken ve "bir gün lazım olur" diye saklanan tüm bu dosyaları, size hiçbir zahmet vermeden, saniyeler içinde düzenlemektir. Otomasyon, tüm dosyaları uzantılarına göre (TXT Dosyaları, PDF Dosyaları vb.) otomatik olarak kategorize eder ve ilgili klasörlere taşır.

Bu basit çözümle, masaüstünüzdeki dağınıklık sona erer ve dosyalarınız her zaman düzenli ve ulaşılabilir olur.

---

## 🛠️ Kullanılan Yöntemler ve Teknolojiler

Projenin geliştirme temeli, "Windows" uyumluluğuna ayarlı olduğu için **.NET Framework**'tür. Farklı platformlarda (Windows, Linux ve macOS) çalışması istenirse ise **.NET Core** tabanlı "**Cross-platform**" proje tipi tercih edilebilir.

### 📂 Dosya Sistemi Yönetimi

*   **Keşif:** Hedef klasördeki tüm dosyalar, `System.IO.Directory.GetFiles()` metodu kullanılarak verimli bir şekilde listelenir.
*   **Analiz:** Her dosyanın türü, `System.IO.Path.GetExtension()` ile uzantısı okunarak belirlenir. Bu, sınıflandırma mantığının temelini oluşturur.
*   **Organizasyon:** `MoveFile` aktivitesi ile dosyalar ilgili kategori klasörlerine taşınır.

### ✨ Dinamik Klasör Yapılandırması

*   **Mevcut Yaklaşım (Dinamik):** Otomasyon, bir dosyayı işlemeden önce, o dosyanın taşınacağı kategori klasörünün (örn: "PDF Dosyaları") var olup olmadığını kontrol eder. Eğer klasör mevcut değilse, `System.IO.Directory.CreateDirectory()` metodunu kullanarak o anda otomatik olarak oluşturur.
*   **Neden Bu Yöntem Tercih Edildi?** Bu dinamik yaklaşım, otomasyonu son derece esnek kılar. Gelecekte hiç beklemediğiniz bir dosya türüyle (`.epub`, `.svg` vb.) karşılaştığında bile, otomasyon anında uyum sağlar ve yeni kategori klasörünü kendi kendine oluşturur.
*   **Alternatif Yaklaşım (Statik):** Eğer statik bir yapı kullanılsaydı, otomasyonu çalıştırmadan önce olası tüm dosya türleri için klasörleri (PDF Dosyaları, DOCX Dosyaları, JPG Dosyaları vb.) sizin manuel olarak oluşturmanız gerekirdi. Dinamik yapı, bu bakım yükünü ve kırılganlığı ortadan kaldırır.

###  Süreç Kontrolü ve Hata Yönetimi

*   Ana iş akışı, bir **For Each** döngüsü ile yönetilir ve her dosyanın tek tek işlenmesini garanti eder.
*   Tüm dosya operasyonları, bir **Try-Catch** bloğu ile sarmalanmıştır. Bu yapı, tek bir dosyada oluşan bir hatanın tüm otomasyonu durdurmasını önler.

### ✉️ Raporlama ve Bildirim

*   **Rapor Oluşturma:** Süreç boyunca toplanan veriler, metin birleştirme (**String Manipulation**) teknikleri kullanılarak tek bir anlamlı rapor metni haline getirilir.
*   **E-posta Gönderimi:** Hazırlanan bu özet rapor, **Use Gmail** aktivitesi kullanılarak işlem sonunda otomatik olarak belirtilen alıcıya e-posta ile iletilir.

---

## ⚙️ Proje Bağımlılıkları

Bu projenin çalışabilmesi için bazı temel UiPath aktivite paketlerine ihtiyacı vardır. Ancak manuel olarak yüklemenize gerek yok; UiPath Studio, projeyi ilk kez açtığınızda `project.json` dosyasını okuyarak sizin için otomatik olarak yükleyecektir.

Projenin temelini oluşturan ana paketler şunlardır:

*   **`UiPath.System.Activities`**: Dosya ve klasör işlemleri, döngüler ve koşullu ifadeler gibi temel sistem işlemlerini içerir.
*   **`UiPath.Mail.Activities` ve `UiPath.GSuite.Activities`**: `Use Gmail` gibi modern ve güvenli e-posta entegrasyonu yeteneklerini sağlar.
*   **`UiPath.UIAutomation.Activities`**: Genellikle varsayılan olarak eklenen ve temel arayüz etkileşimleri için gerekli olan standart bir pakettir.

---

## 🚀 Kurulum ve Kullanım Kılavuzu

Bu otomasyonu kendi makinenizde çalıştırmak için aşağıdaki adımları takip edebilirsiniz.

### 💻 Sistem Gereksinimleri

*   **İşletim Sistemi:** Windows
*   **Ortam:** UiPath Studio

### ✅ Ön Hazırlık

1.  **Gmail Entegrasyonu:** Raporu gönderebilmek için bir gmail hesabı oluşturmanız veya giriş yapmanız gerekir.
2.  **Aktivite Paketleri:** Gerekli tüm paketler, proje açıldığında otomatik olarak yüklenecektir.

###   Kurulum Adımları

1.  **Projeyi İndirin:**
    *   Bu repoyu, "Code" > "Download ZIP" ile indirin veya `git clone` komutunu kullanın.
2.  **Projeyi Açın:**
    *   İndirdiğiniz klasördeki `project.json` dosyasına çift tıklayarak projeyi açın.
3.  **Çalışma Alanını Hazırlayın:**
    *   **ÖNEMLİ:** Otomasyon sabit olarak `Masaüstü\Düzenlenecek`` yolunu kullanır.
    *   Çalıştırmadan önce **Masaüstünüzde "Düzenlenecek" adında bir klasör oluşturduğunuzdan** emin olun.
4.  **E-postayı Yapılandırın:**
    *   `Main.xaml` dosyasında **Use Gmail** aktivitesini bulun ve alıcı e-posta adresini güncelleyin.
5.  **Otomasyonu Başlatın:**
    *   UiPath Studio'da "**Run File**" butonuna tıklayarak otomasyonu çalıştırın.

İşlem tamamlandığında, "Düzenlenecek" klasörünüzün organize olduğunu ve e-posta adresinize bir özet raporu geldiğini göreceksiniz.

---

## ✨ İyileştirme Fikirleri

Otomasyonu daha kullanıcı dostu hale getirmek amacıyla ileride eklenebilecek bazı özellikler şunlardır:

*   **İçeriğe Göre Akıllı Arşivleme:** **Document Understanding** entegrasyonu ile otomasyon, bir faturayı veya sözleşmeyi içeriğinden tanıyarak daha anlamlı alt klasörlere ayırabilir.
*   **Kullanıcıya Özel Klasör Seçimi:** Başlangıçta bir diyalog penceresi (**Input Dialog**) ile her kullanıcının organize etmek istediği klasörü kendisinin seçmesi sağlanabilir.

---

## 📄 Lisans

Bu proje, MIT Lisansı koşulları altında lisanslanmıştır.
