Ah, çok haklısın! Kod bloğunun içindeki `alindi` kelimesinde Türkçe karakter (`ı`) kullanmamıştık, terminal veya kod ortamlarında sorun çıkmasın diye öyle yazmıştık. Ama hem daha şık durması hem de Türkçe kurallarına tam uygun olması için onu `alındı` olarak değiştirebiliriz.



Hemen aşağıdaki düzeltilmiş metni kopyalayıp Notepad'e yapıştırabilirsin. İçindeki `alindi` kelimesini `alındı` olarak düzelttim:



\# HBYS – IOH Entegrasyon Platformu



Mirth Connect tabanlı, HBYS ile IOH arasında iki yönlü REST entegrasyonu sağlayan staj projesi.



\## Mimari



Entegrasyon, biri order (istek) diğeri result (sonuç) yönünde olmak üzere iki ayrı kanal üzerinden çift yönlü çalışır:



\* \*\*HBYS\_Order\_Channel\*\*: HBYS REST API → Mirth Connect → IOH Core API

\* \*\*HBYS\_Result\_Channel\*\*: IOH Core API → Mirth Connect → HBYS REST API



\## 🔄 Sistem Mimarisi ve Veri Akışı



Proje, HBYS (Hastane Bilgi Yönetim Sistemi) ile Mirth Connect arasında HTTP ve IOH formatları üzerinden iki yönlü haberleşme sağlar.



\[Postman / İstemci]

│ (HTTP POST - JSON)

▼

┌──────────────────────────┐

│   HBYS\_Order\_Channel     │ ──> \[Transformer: JSON -> IOH Dönüşümü \& Validasyon]

└──────────────────────────┘

│

▼ (İşlenmiş Veri / Log)



\[HBYS\_Result\_Channel]

│ (Sonuç İşleme)

▼

┌──────────────────────────┐

│  HBYS\_Result\_Channel     │ ──> \[Zorunlu Alan \& Format Kontrolleri]

└──────────────────────────┘



\## 🛠️ Transformer Script İşlevleri



Kanallar içerisinde gelen verileri işlemek için kullanılan temel mantıklar:



\* \*\*HBYS\_Order\_Channel (Transformer):\*\* Gelen ham JSON verisini okur, `patientId`, `testCode` gibi kritik alanların varlığını doğrular ve veriyi loglayarak IOH standart formatına dönüştürür.

\* \*\*HBYS\_Result\_Channel (Transformer):\*\* Laboratuvar veya sonuç verilerini karşılar; eksik zorunlu alan olması durumunda `Errored` olarak yakalar ve sistemin çökmesini engeller.



\## Teknolojiler



\* Mirth Connect 4.5.2

\* PostgreSQL 14

\* Docker \& Docker Compose

\* REST / JSON



\## Kurulum



1\. Docker Desktop'ı başlatın

2\. Projeyi klonlayın:

git clone \[https://github.com/Sumeytm/HBYS-IOH-Entegrasyon.git](https://github.com/Sumeytm/HBYS-IOH-Entegrasyon.git)

cd HBYS-IOH-Entegrasyon

3\. Servisleri başlatın:

docker-compose up -d

4\. Mirth Connect'e bağlanın: https://localhost:8443



\## Servisler



| Servis | Port |

| --- | --- |

| Mirth Connect Admin | 8443 |

| Mirth Connect HTTP | 8080 |

| HBYS Mock API | 9001 |

| IOH Mock API | 9000 |

| PostgreSQL | 5432 |



\## Kanallar



| Kanal | Yön | Açıklama |

| --- | --- | --- |

| HBYS\_Order\_Channel | HBYS → IOH | HBYS'den gelen order isteğini IOH'ye iletir |

| HBYS\_Result\_Channel | IOH → HBYS | IOH'den gelen sonucu HBYS formatına dönüştürüp HBYS'ye iletir |



\## Hata Yönetimi ve Loglama



Kanallar, geçici hataların (bağlantı kopması, hedef servisin geçici olarak erişilemez olması gibi) etkisini azaltmak için retry mekanizmasıyla yapılandırılmıştır: her Destination bağlayıcısı başarısız bir gönderimi 5 saniye aralıklarla en fazla 3 kez tekrar dener. Veri doğrulama hataları (zorunlu alanların eksik olması gibi) ise tekrar denenmeden doğrudan hatalı (Errored) olarak işaretlenir, çünkü bu tür hatalar tekrar denendiğinde de aynı sonucu verir.



Her mesajın işlenme süreci, kanal adı ve benzersiz mesaj kimliği (mesajId) içeren standart formatlı loglarla izlenebilir hale getirilmiştir. Örnek log çıktıları:



\[HBYS\_Order\_Channel] Order alındı | mesajId=10 | hastaId=P001 | testKodu=CBC | durum=pending

\[HBYS\_Order\_Channel] Order IOH formatına dönüştürüldü | mesajId=10 | hastaId=P001 | testKodu=CBC | durum=PENDING

\[HBYS\_Result\_Channel] Mesaj başarıyla dönüştürüldü | mesajId=5 | hastaId=P010 | testKodu=GLU | durum=TAMAMLANDI | kaynakSistem=IOH



Ayrıca her iki kanal da Mirth Connect'in "Development" mesaj saklama modunda çalışacak şekilde yapılandırılmıştır; bu sayede tüm mesajların ham, dönüştürülmüş ve yanıt içerikleri süresiz olarak saklanır ve Mirth Connect Administrator'ın "View Messages" ekranı üzerinden geriye dönük olarak denetlenebilir.



\## Sprint Planı



| Hafta | Hedef | Durum |

| --- | --- | --- |

| 1 | Kurulum, ilk channel | Tamamlandı |

| 2 | Order akışı (HBYS → IOH) ve Result akışı (IOH → HBYS) | Tamamlandı |

| 3 | Retry, loglama, hata yönetimi | Tamamlandı |

| 4 | Docker, test, dokümantasyon | Bekliyor |

