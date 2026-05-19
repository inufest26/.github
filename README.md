# OKUR - Kurallar Bütünü

Bu repo, "OKUR - Yapay Zeka Destekli Akıllı Sınav Değerlendirme Sistemi" projesinin sürdürülebilir, kurumsal ve hatasız bir şekilde ilerlemesi için hazırlanmış **kesin kurallar bütünüdür**.

Aşağıdaki kurallar tavsiye niteliğinde değil, zorunluluktur. Otomatize edilmiş sistemler (Husky, CI/CD) kurallara uymayan kodların ana sisteme entegre olmasını engelleyecektir.

Eğer itirazınız veya öneriniz varsa Emre Tiryaki ile iletişime geçin.

---

## 1. Takım Rolleri ve Sorumluluk Alanları (CODEOWNERS)

Proje kapsamı kesin sınırlarla ayrılmıştır. Belirtilen alanlardaki kod birleştirmeleri (merge), ilgili sorumlunun onayı olmadan yapılamaz.

| Kapsam (Domain) | Sorumlu(lar) | Yetki Alanı / Depo |
| :--- | :--- | :--- |
| **Sistem, Mimari ve DevOps** | Emre | CI/CD, Docker, Sunucu Entegrasyonu |
| **Çekirdek Backend (NestJS)** | Sinan | Veritabanı Şemaları, Redis Kuyruğu |
| **Kullanıcı Arayüzü (React)** | Emre & Sinan | Tasarım, API Bağlantıları |
| **Yapay Zeka ve Görüntü İşleme** | Emre, Mustafa, Selim | *(Alt görev dağılımları toplantıda belirlenecektir)* |

*(Not: Sinan 2. Kaptan olarak, Emre'nin ulaşılamadığı acil durumlarda PR doğrulama ve onaylama yetkisine sahiptir ancak organizasyonel Admin yetkileri bulunmamaktadır.)*

---

## 2. GitHub İş Akışı ve Dal (Branch) Yönetimi

**Serbest Dal Yapısı:** İhtiyaca göre ana dallar üzerinden alt dallar (sub-branch) açmakta tamamen özgürsünüz.

### Dal (Branch) Önekleri (Prefixes) ve Kullanım Senaryoları

Yeni bir dal açarken, yapılacak işin teknik kapsamını belirten aşağıdaki öneklerden biri mutlak suretle kullanılmalıdır:

* **`feature/`**: Sisteme yeni bir yetenek, ekran, API uç noktası veya yapay zeka servisi eklendiğinde kullanılır. (Örn: `feature/auth-ekrani-tasarimi`)
* **`fix/`**: Geliştirme aşamasında veya testlerde tespit edilen, sistemin yanlış çalışmasına neden olan bir hatanın (bug) düzeltilmesinde kullanılır. (Örn: `fix/qwen-memory-leak`)
* **`refactor/`**: Sistemin mevcut dışsal davranışını veya özelliklerini değiştirmeden, sadece arka plandaki kod kalitesini, performansını veya mimarisini iyileştirmek amacıyla yapılan yapısal değişikliklerde kullanılır. (Örn: `refactor/api-gateway-optimizasyonu`)
* **`docs/`**: API dökümantasyonları (Swagger), `README.md` dosyaları veya sistem mimarisi şemaları güncellendiğinde kullanılır. Kod değişikliği içermez. (Örn: `docs/kurulum-rehberi-guncellemesi`)
* **`chore/`**: Uygulamanın çalışmasını doğrudan etkilemeyen; paket (npm/pip) güncellemeleri, `.gitignore` düzenlemeleri veya CI/CD boru hattı (`docker-compose`, GitHub Actions) değişikliklerinde kullanılır. (Örn: `chore/tailwindcss-kurulumu`)

### Dal (Branch) İsimlendirme Standartları

Issue ID zorunluluğu olmasa da, takımın depoda neyin nerede olduğunu anlayabilmesi için yukarıdaki öneklerin doğru bağlamda kullanılması şarttır:

| Durum | Kurala Uygun Doğru Örnek | Kurala Aykırı Yanlış Örnek | Neden Yanlış? |
| :--- | :--- | :--- | :--- |
| Yeni Özellik | `feature/auth-ekrani-tasarimi` | `yeni-ekran` | Önek (`feature/`) belirtilmemiş ve içeriği çok genel. |
| Hata Çözümü | `fix/qwen-memory-leak` | `mustafa-hata-cozumu` | Kişi ismi kullanılmış, neyin çözüldüğü belli değil. |
| Yeniden Düzenleme | `refactor/omr-okuma-hizi` | `refactor/yeni-omr-eklendi` | Yeni özellik ekleniyorsa `feature/` kullanılmalıdır, `refactor/` değil. |

### Pull Request (PR) Açıklamaları

PR açılırken sistem sizi bir Issue numarası girmeye zorlamayacaktır. Ancak kodu inceleyecek olan kişinin (Reviewer) neye baktığını anlaması için, PR açıklama kısmında yapılan değişikliğin teknik içeriği ve amacı Türkçe olarak net bir şekilde özetlenmelidir.

---

## 3. Commit Standartları (Husky Koruması)

Commit mesajlarımızda `Conventional Commits` standardı uygulanacaktır. Repolara kurulu olan **Husky (Git Hooks)**, bu standarta uymayan commit mesajlarını terminal seviyesinde reddedecek ve `git push` yapmanızı engelleyecektir.

**Format:** `{tip}: {türkçe açıklama}`

### Geçerli Tipler (Types)
* `feat`: Yeni bir özellik eklendiğinde.
* `fix`: Bir hata düzeltildiğinde.
* `docs`: Sadece README veya dokümantasyon güncellendiğinde.
* `chore`: Kodun çalışmasını etkilemeyen paket/derleme güncellemelerinde.
* `test`: Test eklendiğinde.

### Commit İsimlendirme Örnekleri

| Durum | Kurala Uygun (✅ Doğru) | Kurala Aykırı (❌ Yanlış) | Neden Yanlış? |
| :--- | :--- | :--- | :--- |
| API Güncellemesi | `feat: sınav yükleme endpointi eklendi` | `api eklendi` | Tip (`feat:`) belirtilmemiş. |
| Hata Düzeltme | `fix: pdf kırpma hatası giderildi` | `pdf hatasını çözdüm` | Tip eksik ve açıklama profesyonel değil. |
| Paket Ekleme | `chore: tailwindcss paketi kuruldu` | `tailwind eklendi` | Yeni bir yazılım özelliği değil, bir yapılandırma (`chore:`). |

---

## 4. Pull Request (PR) ve Kod İnceleme Süreci (24 Saat SLA)

`main` dalına doğrudan kod göndermek (push) kesinlikle yasaktır ve GitHub ayarlarından engellenmiştir. Tüm kodlar PR açılarak sisteme dahil edilir.

* **SLA (Hizmet Seviyesi Anlaşması):** Bir PR açılıp inceleme için (Reviewer) birine atandığında, atanan kişinin PR'ı incelemek ve cevaplamak (Onay/Red) için maksimum **24 saati** vardır. 
* **Yetki Devri:** 24 saat içinde dönüş yapılmayan PR'lar için süreçlerin tıkanmaması adına 2. Kaptan (Sinan) doğrudan inceleme yaparak kodu onaylayabilir.

### PR Açıklama Standartları Örnekleri

| Kurala Uygun (✅ Doğru) | Kurala Aykırı (❌ Yanlış) |
| :--- | :--- |
| "Closes #42. Bu PR, kullanıcı giriş ekranını ekler. Testler yazıldı." | "Giriş ekranı bitti." |
| "Fixes #15. YOLO modelindeki bellek sızıntısı tensör temizliği ile çözüldü." | "Hata düzeltildi kodu inceleyin." |

---

## 5. Kalite Kapıları (Quality Gates)

Bir PR'ın onaylanması (Approve) ve ana koda birleştirilmesi (Merge) için aşağıdaki iki şartın eksiksiz sağlanması zorunludur:

1.  **%100 Test Başarısı:** Yeni eklenen her özellik (feature) kendi birim testleri (unit test) ile birlikte gelmelidir. CI/CD boru hattında çalışan mevcut ve yeni tüm testler **yeşil yanmadığı sürece** GitHub "Merge" butonunu aktif etmeyecektir.
2.  **Güncel Dokümantasyon (Swagger):** Backend veya API tarafında (NestJS) yapılan herhangi bir rota (endpoint), parametre veya veri yapısı (DTO) değişikliği, ilgili **Swagger dokümantasyonuna yansıtılmak zorundadır.** Dokümantasyonu güncellenmemiş veya Swagger testlerinde patlayan hiçbir kod PR aşamasını geçemez.

---
*Bu belge takımın teknik standartlarını belirler. Kuralların etrafından dolaşmak teknik borç yaratır; süreçlere uyunuz.*
