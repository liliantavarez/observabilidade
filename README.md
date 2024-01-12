## Formação SRE: Site Reliability Engineering (Engenharia de confiabilidade de Sites)

Nesta formação você vai aprender o que é Site Reliability Engineering, ou Engenharia de confiabilidade de Sites, e como usar observabilidade e monitoramento para administrar sistemas e manter sistemas críticos funcionando.

1. **Observabilidade e Monitoramento**
   - [**Código no Repositório**](https://github.com/liliantavarez/observabilidade/tree/develop/prometheus-grafana)

2. **Confiabilidade**
   - [**Código no Repositório**](https://github.com/liliantavarez/observabilidade/tree/develop/prometheus-grafana)

3. **Rastreamento de Dados**
   - [**Código no Repositório**](https://github.com/liliantavarez/observabilidade/tree/develop/ambiente-tracing)


### **Observabilidade e monitoramento**

**Introdução**

Observabilidade e monitoramento são duas práticas essenciais para garantir a confiabilidade de sistemas complexos. Observabilidade é a capacidade de entender o estado e o comportamento de um sistema a partir dos dados coletados. Monitoramento é o processo de coleta e análise desses dados para identificar problemas e tomar ações corretivas.

**Observabilidade**

Observabilidade é uma propriedade de um sistema que permite que os engenheiros entendam seu estado e comportamento a partir dos dados coletados. Esses dados podem ser coletados de diferentes fontes, como servidores, aplicações, bancos de dados e serviços de infraestrutura.

**Principais tópicos e conceitos importantes**

A observabilidade é composta por três pilares:

* **Métricas:** Dados quantitativos que representam o estado de um sistema, como consumo de recursos, tempo de resposta e taxa de erros.
* **Logs:** Dados textuais que registram eventos ocorridos em um sistema, como chamadas de API, erros e exceções.
* **Traces:** Dados que rastreiam a execução de uma solicitação, desde a origem até o destino.

**Métricas**

As métricas são a base da observabilidade. Elas fornecem uma visão quantitativa do estado de um sistema, permitindo identificar tendências e padrões de comportamento.

Algumas métricas comuns incluem:

* **Consumo de recursos:** CPU, memória, disco, rede
* **Tempo de resposta:** Tempo necessário para uma solicitação ser processada
* **Taxa de erros:** Porcentagem de solicitações que falham

**Logs**

Os logs são outra fonte importante de dados para observabilidade. Eles fornecem uma visão mais detalhada do comportamento de um sistema, incluindo eventos ocorridos, erros e exceções.

Os logs podem ser utilizados para identificar problemas específicos, como erros de aplicação ou falhas de infraestrutura.

**Traces**

Os traces são uma forma de coletar dados que rastreiam a execução de uma solicitação, desde a origem até o destino.

Os traces fornecem uma visão holística do comportamento de um sistema, permitindo identificar problemas de desempenho, escalabilidade e confiabilidade.

**OpenTelemetry**

OpenTelemetry é um conjunto de APIs, bibliotecas, agentes e instrumentações que permitem a observabilidade em sistemas distribuídos. 

Ele fornece meios para coletar dados de telemetria, como traces (rastreamentos), métricas e logs, em ambientes distribuídos, como microsserviços e arquiteturas baseadas em contêineres.


**Contexto de trace com OpenTelemetry**

O contexto de trace em OpenTelemetry refere-se à capacidade de rastrear a execução de uma solicitação ou transação em vários serviços e componentes. 

É composto por informações de rastreamento que são propagadas através de diferentes partes de um sistema.

* **TraceID e SpanID:** Cada solicitação recebe um identificador de rastreamento exclusivo (TraceID) que é compartilhado por todos os componentes envolvidos na transação. Além disso, cada componente cria um identificador de span exclusivo (SpanID) para sua parte específica da transação.
* **Contexto Propagation:** O OpenTelemetry garante que o contexto de rastreamento seja propagado entre os diferentes serviços. Isso permite que você siga o fluxo de execução de uma solicitação à medida que atravessa diferentes serviços e microsserviços.
* **Instrumentação:** Para coletar dados de rastreamento, você precisa instrumentar seus aplicativos. Isso envolve a incorporação de código em seus serviços para capturar informações relevantes, como início e término de operações importantes.
* **Exportação e Armazenamento:** Os dados de rastreamento coletados são geralmente exportados para sistemas de armazenamento, como Jaeger, Zipkin ou outros backends de observabilidade. Isso permite que você analise os dados e identifique gargalos ou problemas de desempenho.
* **Integração com outros componentes OpenTelemetry:** Além dos traces, OpenTelemetry também lida com métricas e logs, proporcionando uma visão abrangente da saúde e desempenho do seu sistema distribuído.

**Melhorias práticas sobre aplicação de observabilidade em projetos**

Aqui estão algumas sugestões de melhorias práticas sobre aplicação de observabilidade em projetos:

* **Defina um plano de observabilidade:** Antes de implementar qualquer estratégia de observabilidade, é importante definir um plano que defina os objetivos, os dados a serem coletados e as ferramentas a serem utilizadas.
* **Colete os dados certos:** Não adianta coletar todos os dados possíveis. É importante coletar apenas os dados que são relevantes para os objetivos de observabilidade definidos.
* **Armazene os dados com segurança:** Os dados de observabilidade podem conter informações confidenciais. É importante armazená-los com segurança para evitar vazamentos.
* **Visualize os dados de forma clara:** Os dados de observabilidade são inúteis se não forem visualizados de forma clara. É importante utilizar ferramentas de visualização que facilitem a compreensão dos dados.
* **Aplique análises avançadas:** Os dados de observabilidade podem ser utilizados para realizar análises avançadas, como aprendizado de máquina e inteligência artificial. Essas análises podem ajudar a identificar problemas ocultos e tendências de comportamento.
