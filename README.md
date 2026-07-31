\# HBYS – IOH Entegrasyon Platformu



Mirth Connect tabanlı, HBYS ile IOH arasında iki yönlü REST entegrasyonu sağlayan staj projesi.



\## Mimari



Entegrasyon, biri order (istek) diğeri result (sonuç) yönünde olmak üzere iki ayrı kanal üzerinden çift yönlü çalışır:



\- \*\*HBYS\_Order\_Channel\*\*: HBYS REST API → Mirth Connect → IOH Core API

\- \*\*HBYS\_Result\_Channel\*\*: IOH Core API → Mirth Connect → HBYS REST API



\## Teknolojiler



\- Mirth Connect 4.5.2

\- PostgreSQL 14

\- Docker \& Docker Compose

\- REST / JSON



\## Kurulum



1\. Docker Desktop'ı başlatın

2\. Projeyi klonlayın:

&#x20;  git clone https://github.com/Sumeytm/HBYS-IOH-Entegrasyon.git

&#x20;  cd HBYS-IOH-Entegrasyon

3\. Servisleri başlatın:

&#x20;  docker-compose up -d

4\. Mirth Connect'e bağlanın: https://localhost:8443



\## Servisler



| Servis | Port |

|---|---|

| Mirth Connect Admin | 8443 |

| Mirth Connect HTTP | 8080 |

| HBYS Mock API | 9001 |

| IOH Mock API | 9000 |

| PostgreSQL | 5432 |



\## Kanallar



| Kanal | Yön | Açıklama |

|---|---|---|

| HBYS\_Order\_Channel | HBYS → IOH | HBYS'den gelen order isteğini IOH'ye iletir |

| HBYS\_Result\_Channel | IOH → HBYS | IOH'den gelen sonucu HBYS formatına dönüştürüp HBYS'ye iletir |



\## Hata Yönetimi ve Loglama



Kanallar, geçici hataların (bağlantı kopması, hedef servisin geçici olarak erişilemez olması gibi) etkisini azaltmak için retry mekanizmasıyla yapılandırılmıştır: her Destination bağlayıcısı başarısız bir gönderimi 5 saniye aralıklarla en fazla 3 kez tekrar dener. Veri doğrulama hataları (zorunlu alanların eksik olması gibi) ise tekrar denenmeden doğrudan hatalı (Errored) olarak işaretlenir, çünkü bu tür hatalar tekrar denendiğinde de aynı sonucu verir.



Her mesajın işlenme süreci, kanal adı ve benzersiz mesaj kimliği (mesajId) içeren standart formatlı loglarla izlenebilir hale getirilmiştir. Örnek log çıktıları:



\[HBYS\_Order\_Channel] Order alindi | mesajId=10 | hastaId=P001 | testKodu=CBC | durum=pending

\[HBYS\_Order\_Channel] Order IOH formatina donusturuldu | mesajId=10 | hastaId=P001 | testKodu=CBC | durum=PENDING

\[HBYS\_Result\_Channel] Mesaj basariyla donusturuldu | mesajId=5 | hastaId=P010 | testKodu=GLU | durum=TAMAMLANDI | kaynakSistem=IOH



Ayrıca her iki kanal da Mirth Connect'in "Development" mesaj saklama modunda çalışacak şekilde yapılandırılmıştır; bu sayede tüm mesajların ham, dönüştürülmüş ve yanıt içerikleri süresiz olarak saklanır ve Mirth Connect Administrator'ın "View Messages" ekranı üzerinden geriye dönük olarak denetlenebilir.



\## Sprint Planı



| Hafta | Hedef | Durum |

|---|---|---|

| 1 | Kurulum, ilk channel | Tamamlandı |

| 2 | Order akışı (HBYS → IOH) ve Result akışı (IOH → HBYS) | Tamamlandı|

| 3 | Retry, loglama, hata yönetimi | Tamamlandı |

| 4 | Docker, test, dokümantasyon | Bekliyor |

