# SimpleUFW
Educational project - just simple ufw command to make usual firewall

ПОСТАНОВКА ЗАДАЧИ

Настроить сетевой фильтр с помощью ufw для виртуальной машины - сервера компании. На ней (в воображаемом сценарии) стоит следующее ПО:

- Prometheus - слушает TCP порт 9090  
- Grafana - слушает TCP порт 3000  
- Node Exporter - слушает TCP порт 9100  
- ssh демон - слушает TCP порт 22  
- IP адрес этого сервера должен быть 192.168.55.50  

Другая виртуальная машина - ПК администратора, IP адрес 192.168.55.90  

Необходимо:  
  
- обеспечить доступ по SSH к серверу компании только лишь с ПК администратора  
- доступ к prometheus и node exporter должен также осуществляться только с ПК администратора и localhost  
- доступ к Grafana должен быть у отдела аналитиков и разработчиков, ПК которых находятся в диапазонах IP: 192.168.55.10-192.168.55.30 и 192.168.55.91-192.168.55.128  
- все остальные входящие соединения должны быть заблокированы  
- все исходящие соединения должны быть доступны  


ANSWERS  
```bash
sudo ufw default deny incoming comment 'deny all incoming connections'
sudo ufw default allow outgoing comment 'allow all outcoming connetcions'
```

- обеспечить доступ по SSH к серверу компании только лишь с ПК администратора  
```bash
sudo ufw allow from 192.168.55.90 proto tcp to any port 22 comment 'обеспечить доступ по SSH к серверу компании только лишь с ПК администратора'
```

- доступ к prometheus и node exporter должен также осуществляться только с ПК администратора и localhost  
```bash
sudo ufw allow from 192.168.55.90 proto tcp to any port 9090 comment 'доступ к prometheus должен осуществляться с ПК администратора'
sudo ufw allow from 127.0.0.1 proto tcp to any port 9090 comment 'доступ к prometheus должен осуществляться с localhost'

sudo ufw allow from 192.168.55.90 proto tcp to any port 9100 comment 'доступ к Node Exporter должен осуществляться с ПК администратора'
sudo ufw allow from 127.0.0.1 proto tcp to any port 9100 comment 'доступ к Node Exporter должен осуществляться с localhost'
```

- доступ к Grafana должен быть у отдела аналитиков и разработчиков, ПК которых находятся в диапазонах IP: 192.168.55.10-192.168.55.30 и 192.168.55.91-192.168.55.128
```bash
sudo ufw allow from 192.168.55.10/31 to any port 3000 proto tcp comment 'Grafana 10-11'
sudo ufw allow from 192.168.55.12/30 to any port 3000 proto tcp comment 'Grafana 12-15'
sudo ufw allow from 192.168.55.16/29 to any port 3000 proto tcp comment 'Grafana 16-23'
sudo ufw allow from 192.168.55.24/30 to any port 3000 proto tcp comment 'Grafana 24-27'
sudo ufw allow from 192.168.55.28/31 to any port 3000 proto tcp comment 'Grafana 28-29'
sudo ufw allow from 192.168.55.30 to any port 3000 proto tcp comment 'Grafana 30'

sudo ufw allow from 192.168.55.96/27 to any port 3000 proto tcp comment 'Grafana 96 - 127'
sudo ufw allow from 192.168.55.91 to any port 3000 proto tcp comment 'Grafana 91'
sudo ufw allow from 192.168.55.92 to any port 3000 proto tcp comment 'Grafana 92'
sudo ufw allow from 192.168.55.93 to any port 3000 proto tcp comment 'Grafana 93'
sudo ufw allow from 192.168.55.94 to any port 3000 proto tcp comment 'Grafana 94'
sudo ufw allow from 192.168.55.95 to any port 3000 proto tcp comment 'Grafana 95'
sudo ufw allow from 192.168.55.128 to any port 3000 proto tcp comment 'Grafana 128'
``` 
