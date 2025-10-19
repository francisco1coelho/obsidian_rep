#### **Ingestão de dados em tempo real**
###### apache kafka
- **O que é:** _Event streaming_ distribuído (tópicos/partições) com forte ecossistema. Suporta transações e **exactly-once** de ponta-a-ponta quando combinado com Streams/Connect. [Apache Kafka+1](https://kafka.apache.org/documentation/?utm_source=chatgpt.com)
    
- **Quando escolher:** ambientes multi-cloud/on-prem; necessidade de conectores (CDC, DBs, SaaS) e máximo controlo.
    
- **Vantagens:** maturidade, paralelismo por partição, integrações diretas com Kafka Streams e Flink. [Apache Kafka](https://kafka.apache.org/documentation/?utm_source=chatgpt.com)
    
- **Atenções:** operação do cluster (capacidade, upgrades); desenhar partições bem para throughput/ordenação; activar transações para EOS. [Confluent Docs](https://docs.confluent.io/kafka/design/delivery-semantics.html?utm_source=chatgpt.com)

###### AWS kinesis
- **O que é:** serviço gerido tipo Kafka, organizado por **shards** (unidades de capacidade). Tipicamente **<1 s** do _put_ ao _get_; permite _resharding_ para escalar. [AWS Documentation+2AWS Documentation+2](https://docs.aws.amazon.com/streams/latest/dev/introduction.html?utm_source=chatgpt.com)
    
- **Quando escolher:** stack 100% AWS e queres reduzir _ops_.
    
- **Vantagens:** sem gerir brokers; _auto scaling_/resharding; integração nativa com Kinesis Data Analytics (Flink gerido). [AWS Documentation](https://docs.aws.amazon.com/streams/latest/dev/kinesis-record-processor-scaling.html?utm_source=chatgpt.com)
    
- **Atenções:** limites por shard (≈1 MB/s write; ≈2 MB/s read); _partition key_ bem escolhida para evitar _hot shards_. [AWS Documentation](https://docs.aws.amazon.com/streams/latest/dev/key-concepts.html?utm_source=chatgpt.com)


#### **Processamento de Fluxos**
###### Kafka Streams
- **O que é:** _client library_ (Java/Scala) que corre **dentro** das tuas apps; lê/escreve em Kafka, suporta estado, _windowing_, joins e **exactly-once** com transações. Não exige cluster separado. [Apache Kafka+2Confluent Docs+2](https://kafka.apache.org/documentation/streams/?utm_source=chatgpt.com)
    
- **Quando escolher:** pipelines leves/médios, equipa já em Kafka, microserviços a escalar por instâncias.
    
- **Vantagens:** baixo atrito (sem cluster), escala por partições, DSL expressiva. [Apache Kafka+1](https://kafka.apache.org/documentation/?utm_source=chatgpt.com)
    
- **Atenções:** acoplado ao Kafka (menos flexível para fontes/sinks não-Kafka); jobs muito pesados podem exigir um motor dedicado.


###### Apache Flink

- **O que é:** motor distribuído para **stateful stream processing** com _checkpoints_ e **exactly-once**; excelente para **joins/aggregações** complexas e grande estado. [Apache Nightlies+1](https://nightlies.apache.org/flink/flink-docs-master/docs/concepts/stateful-stream-processing/?utm_source=chatgpt.com)
    
- **Quando escolher:** lógica complexa, múltiplas fontes/sinks, SLAs apertados e necessidade de escalar horizontalmente o estado.
    
- **Vantagens:** _checkpointing_ robusto; integração top com Kafka (incl. EOS end-to-end com transações). [Apache Nightlies+1](https://nightlies.apache.org/flink/flink-docs-master/docs/dev/datastream/fault-tolerance/checkpointing/?utm_source=chatgpt.com)
    
- **Atenções:** exige operar/gerir o cluster (ou usar Flink gerido na AWS/GCP); tuning de estado/rocksdb, _savepoints_ e _backpressure_.