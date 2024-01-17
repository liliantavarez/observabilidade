# Rastreamento

A observabilidade ao nível end-to-end de uma requisição é crucial para compreender o funcionamento completo de uma funcionalidade. O rastreamento, como um dos pilares da observabilidade, permite acompanhar as jornadas de uma funcionalidade de ponta a ponta.

No contexto da plataforma Alura, é provável que você já tenha participado de cursos abordando os pilares de métricas, logs e, agora, rastreamento.

## Conceito de Rastreamento

O rastreamento consiste em obter observabilidade em toda a extensão de uma requisição, permitindo a coleta de dados em cada iteração do processo. Essa abordagem proporciona uma visão holística do funcionamento do sistema, possibilitando visualizar e entender todas as etapas de uma requisição, desde o início até o final.

## Benefícios do Rastreamento

Os benefícios do rastreamento são diversos, incluindo:

- **Tempo de latência de cada rota:** Permite identificar o tempo de processamento em cada rota distribuída no ambiente.
- **Tempo de duração de requisições:** Possibilita analisar o tempo de duração das requisições entre serviços, incluindo chamadas entre serviços e o tempo total do sistema.
- **Performance geral do sistema:** Oferece uma visão clara sobre o desempenho geral do sistema.
- **Identificação de componentes ou funcionalidades ofensoras:** Facilita a rápida identificação do componente ou funcionalidade responsável por problemas.
- **Identificação de falhas entre integrações e dependências:** Permite compreender as falhas que ocorrem nas integrações e dependências do sistema.
- **Mapeamento completo de jornadas:** Fornece um mapeamento abrangente de todas as jornadas percorridas durante o processamento de uma requisição.

O rastreamento é uma ferramenta valiosa para obter insights precisos sobre o desempenho e a integridade do sistema, possibilitando uma abordagem mais eficaz na identificação e resolução de problemas.

## Rastreamento com Jaeger e OpenTelemetry em uma Aplicação Spring Boot

Este guia aborda a implementação do rastreamento distribuído em uma aplicação Spring Boot usando Jaeger como plataforma de rastreamento e OpenTelemetry para a instrumentação.

### Pré-requisitos

- Certifique-se de ter uma instância do Jaeger instalada e acessível. Você pode seguir a [documentação oficial do Jaeger](https://www.jaegertracing.io/docs/1.26/getting-started/) para instalação e configuração.
- Tenha uma aplicação Spring Boot configurada e em execução.

### Passos para Implementar o Rastreamento

#### 1. Adicionar Dependências

No arquivo `pom.xml`, adicione as dependências necessárias para o OpenTelemetry e Jaeger:

```xml
<dependency>
    <groupId>io.opentelemetry</groupId>
    <artifactId>opentelemetry-api</artifactId>
</dependency>
<dependency>
    <groupId>io.opentelemetry</groupId>
    <artifactId>opentelemetry-sdk</artifactId>
</dependency>
<dependency>
    <groupId>io.opentelemetry</groupId>
    <artifactId>opentelemetry-exporters-jaeger</artifactId>
</dependency>
```

#### 2. Configurar o Exportador do Jaeger

Configure o exportador do Jaeger em sua aplicação Spring Boot. Adicione as seguintes propriedades no arquivo `application.properties`:

```properties
opentelemetry.trace.exporter=jaeger
opentelemetry.exporter.jaeger.endpoint=http://<SEU_HOST_DO_JAEGER>:14268/api/traces
```

Substitua `<SEU_HOST_DO_JAEGER>` pelo endereço do seu servidor Jaeger.

#### 3. Instrumentar o Código

Instrumente o código da sua aplicação para criar spans. Utilize o OpenTelemetry para adicionar instruções de trace em métodos e serviços importantes. Isso permite monitorar o tempo de execução de cada parte significativa da sua aplicação.

```java
import io.opentelemetry.api.trace.Span;
import io.opentelemetry.api.trace.Tracer;

// ...

@Autowired
private Tracer tracer;

@GetMapping("/exemplo")
public ResponseEntity<String> exemploEndpoint() {
    Span span = tracer.spanBuilder("exemploEndpoint").startSpan();
    try {
        // Lógica do endpoint
        return ResponseEntity.ok("Sucesso!");
    } finally {
        span.end();
    }
}
```

#### 4. Propagação do Contexto de Trace

Garanta a propagação do contexto de trace em chamadas de serviço. Em chamadas HTTP, você pode passar o contexto usando headers. O OpenTelemetry cria automaticamente um contexto de trace que é propagado entre os serviços.

```java
import io.opentelemetry.context.Context;
import io.opentelemetry.context.Scope;

// ...

@Autowired
private Tracer tracer;

public void chamarServico() {
    Span span = tracer.spanBuilder("chamarServico").startSpan();
    try (Scope scope = span.makeCurrent()) {
        // Lógica da chamada ao serviço
    } finally {
        span.end();
    }
}
```

#### 5. Visualizando Traces no Jaeger

Execute a sua aplicação Spring Boot e acesse a interface do Jaeger. Você deverá visualizar os traces gerados pela sua aplicação. Os traces mostrarão a hierarquia de spans, o tempo gasto em cada operação e as relações entre os diferentes serviços.

### Detalhes Adicionais

#### Contexto de Trace

O OpenTelemetry utiliza o conceito de contexto de trace para correlacionar spans entre diferentes partes da aplicação. Certifique-se de compreender como o contexto de trace é propagado, especialmente em chamadas assíncronas ou microsserviços.

#### Ajustes de Configuração

Além das configurações básicas, explore ajustes avançados, como a configuração de amostragem para controlar a quantidade de dados de trace coletados, e a configuração de tags adicionais para fornecer informações personalizadas nos spans.

#### Melhores Práticas

Considere implementar melhores práticas, como a criação de spans significativos, a inclusão de informações úteis nos spans (como identificadores de transações) e a organização lógica dos spans para refletir a estrutura do seu sistema.

### Conclusão

Com esses passos, você configurou o rastreamento distribuído em sua aplicação Spring Boot utilizando Jaeger e OpenTelemetry. Isso proporciona uma visão abrangente e detalhada do fluxo de uma requisição, facilitando a identificação e resolução de problemas de desempenho em ambientes distribuídos. Lembre-se de ajustar as configurações de acordo com as necessidades específicas do seu sistema.
