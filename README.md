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



\## Sprint Planı

| Hafta | Hedef | Durum |

|---|---|---|

| 1 | Kurulum, ilk channel | Tamamlandı |

| 2 | Order akışı (HBYS → IOH) ve Result akışı (IOH → HBYS) | Tamamlandı |

| 3 | Retry, loglama, hata yönetimi | Bekliyor |

| 4 | Docker, test, dokümantasyon | Bekliyor |

