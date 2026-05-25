# 👮 Emniyet Suç Takip Sistemi Veri Tabanı Mimarisi

SQL Server kullanılarak geliştirilmiş; suç ihbarları, şüpheli ve tanık ifadeleri, suç-delil ilişkileri, polis görevlendirmeleri ve karakol yönetim süreçlerini dijital ortamda takip etmek amacıyla normalize edilmiş kapsamlı bir veri tabanı projesidir.

Proje, emniyet teşkilatının operasyonel süreçlerini optimize etmek, suç ve suçlu takibini hızlandırmak ve veriler arasındaki ilişkileri (1-N ve M-N) tutarlı bir şekilde yönetmek amacıyla kurgulanmıştır.

---

## 📊 Veri Tabanı İlişki Diyagramı (ERD)

Aşağıdaki şemada; vatandaş, ihbar, suç, şüpheli, tanık, polis, karakol ve delil tablolarının birbiriyle olan mantıksal ve fiziksel ilişkileri modellenmiştir:

*Not: Eğer deponuza ERD diyagramı fotoğrafını yüklediyseniz görsel adını aşağıya ekleyebilirsiniz.*
![Emniyet Suç Takip Veritabanı Diyagramı](Diyagram.png)

---

## 📂 Veri Tabanı Tablo Yapısı ve İlişkileri

Proje, veri tekrarını önlemek ve veri bütünlüğünü en üst düzeyde tutmak amacıyla normalizasyon kurallarına uygun olarak ilişkisel tablolar halinde kurgulanmıştır:

### 1. Vatandaş ve İfade Yönetimi Modülü
* **`Tbl_Vatandas`:** Sisteme kayıtlı tüm vatandaşların temel kimlik ve iletişim bilgilerini tutar (TcNo, Ad, Soyad, Dogum_tarihi, Adres, Telefon, E-mail).
* **`Tbl_Tanik`:** Olaylara tanıklık eden kişilerin ifadelerini ve ifade tarihlerini saklar.
* **`Tbl_Vatandas_Tanik_Olur`:** Vatandaşlar ile tanıklık durumları arasındaki M-N (Çoka Çok) ilişkiyi çözen kesişim tablosudur.

### 2. İhbar ve Karakol Yönetimi Modülü
* **`Tbl_İhbar`:** Gelen ihbarların kanalları, tarihleri ve durumlarını takip eder. (Bir vatandaş birden fazla ihbarda bulunabilir - 1-N İlişkisi).
* **`Tbl_Karakol`:** Emniyet müdürlüklerine bağlı karakolların iletişim ve adres bilgilerini tutar.
* **`Tbl_Polis`:** Karakollarda görevli polislerin rütbe, görev ve kişisel bilgilerini saklar.

### 3. Suç, Şüpheli ve Delil Takip Modülü
* **`Tbl_Suc`:** İşlenen suçların türü, tarihi ve konumu gibi detayları içerir.
* **`Tbl_Supheli`:** Suçla ilişkisi bulunan şüphelilerin profillerini tutar.
* **`Tbl_Delil`:** Suç mahallinde veya soruşturma esnasında bulunan delillerin türünü, konumunu ve bulunma tarihini kayıt altına alır.

---

## 🛠️ Mimari ve Tasarım Aşamaları

### 🔹 1. Aşama: İş Mantığı ve Gereksinim Analizi
* Sürecin vatandaşların ihbarları ile başlaması, ihbarların karakollara düşmesi ve ardından polislerin görevlendirilmesi senaryolaştırılmıştır.
* Bir suçun birden fazla şüphelisi olabileceği gibi, bir şüphelinin de birden fazla suça karışabileceği göz önünde bulundurularak ilişkisel esneklik sağlanmıştır.

### 🔹 2. Aşama: Varlık-İlişki (ER) Modeli Tasarımı
* Tablolar arasındaki Primary Key (Birincil Anahtar) ve Foreign Key (Yabancı Anahtar) hiyerarşisi kurulmuştur.
* Bire-çok (1-N) ve çoka-çok (M-N) ilişkiler belirlenerek veri tabanı motorunun performanslı çalışması hedeflenmiştir.

### 🔹 3. Aşama: Şema Tasarımı ve Normalizasyon
* Tüm varlıklar SQL veri tiplerine (INT, NVARCHAR, DATE vb.) uygun olarak niteliklerine ayrılmıştır.
* Veri bütünlüğünü tehdit edecek anomaliler giderilmiş ve tablolar normalize edilmiştir.
