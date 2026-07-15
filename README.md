\# HBYS – IOH Entegrasyon Platformu



Mirth Connect tabanlı, HBYS ile IOH arasında iki yönlü REST entegrasyonu sağlayan staj projesi.



\## Mimari



HBYS REST API → Mirth Connect → IOH Core API → PostgreSQL



\## Teknolojiler



\- Mirth Connect 4.4.1

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



\## Sprint Planı



| Hafta | Hedef | Durum |

|---|---|---|

| 1 | Kurulum, ilk channel | Tamamlandı |

| 2 | Order akışı (HBYS → IOH) | Devam ediyor |

| 3 | Result akışı (IOH → HBYS) | Bekliyor |

| 4 | Retry, loglama, hata yönetimi | Bekliyor |

| 5 | Docker, test, dokümantasyon | Bekliyor |

