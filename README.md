# Dosya Organizatörü Otomasyonu

assets/FileEditor.gif

Bu UiPath projesi, belirtilen bir klasördeki dosyaları analiz ederek, uzantılarına göre yeni dosya kategorileri oluşturan ve dosyaları bu klasörlere taşıyan bir otomasyondur. İşlem sonunda tüm süreci özetleyen detaylı bir rapor e-posta ile kullanıcıyı bilgilendirir.

## 🎯 Projenin Amacı

Önemli dosyalarımızı genellikle özenle klasörleriz, ancak daha az önemli gördüğümüz dosyalar çoğunlukla masaüstünde dağınık bir şekilde birikir. Bu proje, masaüstünüzde biriken ve "bir gün lazım olur" diye saklanan tüm bu dosyaları, size hiçbir zahmet vermeden, saniyeler içinde düzenlemeyi hedefler. Otomasyon, tüm dosyaları uzantılarına göre (`TXT Dosyaları`, `PDF Dosyaları` vb.) otomatik olarak kategorize eder ve ilgili klasörlere taşır. Bu basit çözümle, masaüstünüzdeki dağınıklık sona erer ve dosyalarınız her zaman düzenli ve ulaşılabilir olur.

## 🛠️ Kullanılan Yöntemler ve Teknolojiler

Projenin geliştirme temeli, "Windows" uyumluluğuna ayarlı olduğu için .NET Framework'tür. Dinamik klasör yapılandırması sayesinde, otomasyon karşılaştığı her yeni dosya türü için otomatik olarak bir kategori klasörü oluşturur, bu da onu son derece esnek kılar. Hata yönetimi için `Try-Catch` blokları kullanılırken, süreç sonu raporlaması `Use Gmail` aktivitesi ile modern bir yaklaşımla gerçekleştirilir.

## ⚙️ Proje Bağımlılıkları

Bu projenin çalışabilmesi için bazı temel UiPath aktivite paketlerine ihtiyacı vardır. Ancak manuel olarak yüklemenize gerek yok; **UiPath Studio, projeyi ilk kez açtığınızda aşağıda listelenen bu paketleri ve diğer tüm bağımlılıkları `project.json` dosyasını okuyarak sizin için otomatik olarak yükleyecektir.**

*   **UiPath.System.Activities:** Dosya ve klasör oluşturma, taşıma, döngüler ve koşullu ifadeler gibi otomasyonun bel kemiğini oluşturan temel sistem işlemlerini içerir.
*   **UiPath.Mail.Activities** ve **UiPath.GSuite.Activities:** Süreç sonunda hazırlanan raporun e-posta olarak gönderilmesi için `Use Gmail` gibi modern ve güvenli e-posta entegrasyonu yeteneklerini sağlar.
*   **UiPath.UIAutomation.Activities:** Bu proje arayüz otomasyonu yapmasa da, genellikle varsayılan olarak eklenen ve temel arayüz etkileşimleri için gerekli olan standart bir pakettir.

## 🚀 Kurulum ve Kullanım Kılavuzu

Bu otomasyonu kendi makinenizde çalıştırmak için aşağıdaki adımları takip edebilirsiniz.

#### Sistem Gereksinimleri
*   **İşletim Sistemi:** Windows
*   **Ortam:** UiPath Studio

#### Kurulum Adımları
1.  **Projeyi İndirin:** Bu repoyu, "Code" > "Download ZIP" seçeneği ile indirin ve dosyaları bir klasöre çıkarın.
2.  **Projeyi Açın:** İndirdiğiniz klasör içindeki `project.json` dosyasına çift tıklayarak projeyi UiPath Studio'da açın.
3.  **Çalışma Alanını Hazırlayın:**
    *   **ÖNEMLİ:** Bu otomasyonun mevcut versiyonu, dosyaları organize etmek için sabit olarak kodlanmış bir yol kullanır: `Masaüstü\Düzenlenecek`.
    *   Bu nedenle, otomasyonu çalıştırmadan önce **kendi Masaüstünüzde** "Düzenlenecek" adında bir klasör oluşturduğunuzdan emin olun.
4.  **E-postayı Yapılandırın:** `Main.xaml` dosyasını açın, **`Use Gmail`** aktivitesini bulun ve kendi hesap bilgilerinizi girerek alıcı e-posta adresini güncelleyin.
5.  **Otomasyonu Başlatın:** UiPath Studio'da **"Run File"** butonuna tıklayarak otomasyonu çalıştırın.

## ✨ İyileştirme Fikirleri

Projenin mevcut yapısı, gelecekte eklenebilecek yeni özellikler için sağlam bir zemin sunmaktadır. Akla gelen ilk geliştirme fikirleri şunlardır:

*   **İçeriğe Göre Akıllı Arşivleme:** Kullanıcıların aradıkları dosyaları daha kolay bulabilmesi için gelecekte **Document Understanding** entegrasyonu düşünülebilir. Bu sayede otomasyon, bir faturayı veya sözleşmeyi içeriğinden tanıyarak, dosyaları `Faturalar` veya `Sözleşmeler` gibi, kullanıcının daha anlamlı bulacağı klasörlere ayırabilir.
*   **Kullanıcıya Özel Klasör Seçimi:** Kullanıcılara daha fazla esneklik sunmak amacıyla, otomasyonun başında bir diyalog penceresi (`Input Dialog`) eklenebilir. Bu sayede her kullanıcı, organize etmek istediği klasörü kendisi seçebilir.

## 📄 Lisans

Bu proje, MIT Lisansı koşulları altında lisanslanmıştır.
