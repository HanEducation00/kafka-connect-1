----------------------------------------------------------------------------------------------------------------------------

Kafka Connect, Kafka'yı veri kaynakları (source) ve veri hedefleri (sink) ile bağlamak için kullanılan bir framework’tür.

➡ "Connector" dediğimiz şeyler, bu veri akışını sağlayan hazır modüllerdir.
➡ Kafka Connect → eklenti (plugin) mantığıyla çalışır.

Source Connectors (Veri Kafka’ya akar)
Sink Connectors (Kafka’dan dışa veri akar)

```
✅ Open Source      Ücretsiz (örn: Landoop, Debezium, community connectors)
💼 Confluent        Confluent’un sunduğu enterprise connector’ler, genelde lisans gerekir
🛠️ Custom           Kendin yazarsın (Java, REST API, Python Kafka client vs.)
```

```
+-----------------------+
|                       |
|     Android APP       |
|                       |
+----------+------------+
           |
           | MQTT Publish
           v
+-----------------------+
|  AWS IoT Core (MQTT)  |
|  Topic: phone/data    |
+----------+------------+
           |
           | MQTT Pull
           v
+-------------------------------+
| Landoop MQTT Source Connector|
| Kafka Connect (EC2, lokal vs)|
| Kafka topic: phone-data      |
+----------+--------------------+
           |
           | Kafka
           v
+-------------------------------+
| Apache Kafka                 |
| Topic: phone-data            |
+----------+-------------------+
           |
           | Kafka Poll + REST
           v
+-------------------------------+
| Landoop Elasticsearch Sink   |
| Index: phone-data_2025-04-24 |
+-------------------------------+
           |
           v
+------------------+
|  Elasticsearch   |
|  (Questionable)  |
+------------------+

```
```
sudo systemctl status mosquitto
```

```
Dosya Adı                             | Amaç                                       | Kurulacağı Makine
mqtt-source-connector.json            | Mosquitto'dan Kafka'ya veri alır.          | Kafka Connect EC2 (Kafka Connect'in çalıştığı EC2)
kafka-to-elasticsearch.json           | Kafka'dan Elasticsearch'e veri aktarır.    | Kafka Connect EC2 (Kafka Connect'in çalıştığı EC2)
connect-distributed.properties        | Kafka Connect yapılandırması.              | Kafka Connect EC2 (Kafka Connect'in çalıştığı EC2)
elasticsearch.yml                     | Elasticsearch yapılandırma dosyası.        | Elasticsearch EC2 (Elasticsearch'in çalıştığı EC2)
mosquitto.conf                        | Mosquitto MQTT broker yapılandırması.      | MQTT Broker EC2 (Mosquitto'nun çalıştığı EC2)
```


