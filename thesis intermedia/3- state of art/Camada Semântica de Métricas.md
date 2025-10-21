É **uma camada intermédia** entre o que o utilizador pede (“Mostra-me o tempo médio de atendimento por fila”) e a forma como os dados são realmente guardados e calculados no sistema.

Em vez de o LLM gerar diretamente uma query SQL ou JSON complicado, ele gera uma **configuração semântica** com nomes de **métricas, dimensões e filtros normalizados**.

`### **Camada Semântica — o “vocabulário comum”**

`Quando se diz que o sistema adota uma **semantic layer**, significa que todos os dados (de chamadas, agentes, filas, etc.) seguem **as mesmas definições de negócio**.`  
`Por exemplo:`

- `“Average Handle Time (AHT)” = soma do tempo total em chamadas / número de chamadas atendidas.`
    
- `“Abandoned Call” = chamada terminada antes de ser atendida.`
    

`Isto evita que um dashboard calcule AHT de uma forma e outro de outra.`  
`Ferramentas como **dbt Metrics Layer** e **MetricFlow** implementam esta camada, permitindo definir métricas com:`

- `método de agregação (ex.: média, soma, percentagem);`
    
- `filtros (ex.: apenas chamadas inbound);`
    
- `janelas temporais (ex.: últimas 2 horas).`


# Camada Semântica (Métricas & Ontologia de Contact Center)

**Explorar:** modelos de métricas (AHT, ASA, SL, Abandon, Occupancy), semantic layers (dbt metrics/MetricFlow), SLAs e janelas “rolling”.  
**Provável gap:** métrica única e consistente para real-time (janelas, redefinições, dimensões).  
**MVP/Experimentos:** catálogo YAML de métricas + testes que garantam definições idênticas no batch e no streaming.


#### dbt Semantic Layer (metric flow)
- **Porquê aqui?** Fonte única de verdade em **YAML**, versionada em Git, com API para expor métricas iguais em batch e streaming (o “contrato” que precisas). Ótima base para LLM entender nomes/definições de métricas. [dbt Developer Hub+2dbt Developer Hub+2](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl?utm_source=chatgpt.com)
    
- **Pontos fortes:** centraliza definições; suporta _joins_ de métricas e janelas; integra com várias ferramentas. [dbt Labs](https://www.getdbt.com/blog/how-the-dbt-semantic-layer-works?utm_source=chatgpt.com)
    
- **Atenções:** define e serve a semântica; o cálculo “hard real-time” continua nos jobs (Flink/Streams), que devem respeitar o mesmo contrato. [dbt Developer Hub](https://docs.getdbt.com/docs/build/metricflow-commands?utm_source=chatgpt.com)


#### looker (LookML)
- **Porquê aqui?** Modelo semântico maduro (dimensões/medidas) muito usado em enterprise; garante consistência de métricas em toda a org. [Google Cloud](https://cloud.google.com/looker/docs/what-is-lookml?utm_source=chatgpt.com)
    
- **Pontos fortes:** governação + Git; linguagem de modelação estável; bom para self-service. [Google Cloud](https://cloud.google.com/looker/docs/best-practices/how-to-dimensionalize-a-measure?utm_source=chatgpt.com)
    
- **Atenções:** mais acoplado ao ecossistema Looker/GCP; expor métricas para apps externas/LLM pode ser menos direto do que camadas “headless”. [Google Cloud](https://cloud.google.com/looker/docs/what-is-lookml?utm_source=chatgpt.com)


#### Cube(headless semantic layer)
- **Porquê aqui?** Camada semântica **“para apps”** com APIs (SQL/REST/GraphQL) e _caching_ — encaixa bem se o teu frontend/LLM vai pedir métricas dinamicamente. [cube.dev](https://cube.dev/docs/product/introduction?utm_source=chatgpt.com)
    
- **Pontos fortes:** orientado a aplicações e embebidos; sincroniza modelo com BI (Superset/Tableau) via **Semantic Layer Sync**. [cube.dev](https://cube.dev/docs/product/apis-integrations/semantic-layer-sync?utm_source=chatgpt.com)
    
- **Atenções:** é mais um serviço para operar e alinhar com as definições do teu pipeline. [GitHub](https://github.com/cube-js/cube?utm_source=chatgpt.com)