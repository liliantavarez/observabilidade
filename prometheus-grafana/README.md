# Observabilidade e Monitoramento

A observabilidade visa acompanhar o estado de execução de um sistema, externalizando seu comportamento em tempo real para tornar a aplicação observável por fontes externas. O foco está em tornar visíveis os comportamentos relevantes para a experiência do usuário final.

Por outro lado, o monitoramento envolve o acompanhamento do estado de um sistema por meio do registro de eventos e a execução de ações em resposta a esses eventos. Essas ações podem ser tanto reativas quanto proativas.

A observabilidade é essencial no escopo do monitoramento, especialmente em aplicações complexas e distribuídas. Em ambientes assim, um monitoramento efetivo depende da observabilidade para entender como o sistema se comporta diante do consumo.

## Métricas

No contexto de observabilidade e monitoramento, métricas desempenham um papel crucial. Uma métrica é um indicador de nível de serviço coletado ao longo do tempo. Elas são utilizadas para medir performance, disponibilidade, saturação (uso de recursos além do previsto), erros e outras informações relevantes para o negócio.

Cada métrica foca em uma propriedade específica do sistema e é uma medição realizada dentro de uma janela de tempo. A instrumentação é a chave para extrair métricas da infraestrutura, execução da aplicação e até mesmo das regras de negócio.

É importante destacar que cada métrica tem um objetivo específico. Cada ponto de atenção no sistema deve ter uma métrica correspondente. Dessa forma, as métricas fornecem informações valiosas para a equipe de desenvolvimento, operações de infraestrutura e também para a área de negócios.

# Tornar aplicação spring boot observável

Para tornar uma aplicação Spring Boot observável, podemos seguir alguns passos específicos que envolvem a configuração do Spring Boot Actuator e a exposição de métricas relevantes. Aqui estão as instruções para realizar essa configuração:

## 1. Adicionar Dependência do Spring Boot Actuator

No arquivo `pom.xml` do seu projeto, adicione a dependência do Spring Boot Actuator:

```xml
<dependencies>
    <!-- Outras dependências -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
</dependencies>
```

Isso permitirá que o Spring Boot Actuator seja incluído no seu projeto.

## 2. Configurar Propriedades no arquivo `application-prod.properties`

Abra o arquivo `application-prod.properties` e adicione as seguintes configurações relacionadas ao Spring Boot Actuator:

```properties
# Configuração do Spring Boot Actuator
management.endpoint.health.show-details=always
management.endpoints.web.exposure.include=health,info,metrics
```

- `management.endpoint.health.show-details=always`: Isso configura o Actuator para mostrar detalhes completos no endpoint de saúde (`/actuator/health`).
- `management.endpoints.web.exposure.include=health,info,metrics`: Isso configura quais endpoints do Actuator serão expostos. Neste caso, incluímos os endpoints `health`, `info` e `metrics`.

## 3. Build e Execução da Aplicação

Após realizar essas configurações, recompile e inicie sua aplicação Spring Boot.

No terminal, execute o seguinte comando para compilar:

```bash
mvn clean package
```

Em seguida, inicie a aplicação:

```bash
java -jar -Xms128M -Xmx128M -XX:MaxMetaspaceSize=128m -Dspring.profiles.active=prod target/forum.jar
```

## 4. Acessar Endpoints do Spring Boot Actuator

Após iniciar a aplicação, você pode acessar os endpoints do Spring Boot Actuator para verificar as métricas e informações relevantes.

- **Health Endpoint**: [http://localhost:8080/actuator/health](http://localhost:8080/actuator/health)
- **Info Endpoint**: [http://localhost:8080/actuator/info](http://localhost:8080/actuator/info)
- **Metrics Endpoint**: [http://localhost:8080/actuator/metrics](http://localhost:8080/actuator/metrics)

Lembre-se de que esses endpoints podem ser acessados localmente, mas em um ambiente de produção, eles geralmente não devem ser expostos publicamente. Configure adequadamente o acesso a esses endpoints em ambientes de produção.

## 5. Analisar Métricas com Detalhes

Ao acessar o endpoint de métricas, você verá uma série de métricas relacionadas à JVM e à aplicação. Explore essas métricas para entender melhor o desempenho e o comportamento da sua aplicação.

Com essas configurações, sua aplicação Spring Boot estará mais observável, permitindo monitorar e analisar seu estado de execução de forma mais eficaz. Certifique-se de ajustar as configurações conforme necessário para atender às necessidades específicas do seu projeto e ambiente.

# Expondo Métricas para o Prometheus com Micrometer e Spring Boot Actuator

Para tornar as métricas da sua aplicação Spring Boot legíveis para o Prometheus, você precisará configurar o Micrometer e o Spring Boot Actuator. Siga as instruções abaixo para realizar essas configurações:

## 1. Adicionar Dependência do Micrometer no Maven

Acesse [Micrometer.io](https://micrometer.io/) e vá para "Documentation > Installation". Copie a dependência Maven:

```xml
<dependency>
  <groupId>io.micrometer</groupId>
  <artifactId>micrometer-registry-prometheus</artifactId>
  <version>${micrometer.version}</version>
</dependency>
```

Adicione essa dependência no arquivo `pom.xml` do seu projeto, dentro da seção `<dependencies>`.

## 2. Configurar o Spring Boot Actuator

Abra o arquivo `application-prod.properties` e ajuste as configurações do Spring Boot Actuator:

```properties
# Configuração do Spring Boot Actuator
management.endpoint.health.show-details=always
management.endpoints.web.exposure.include=health,info,metrics,prometheus
```

Além disso, adicione as configurações específicas para o Prometheus:

```properties
# Configuração do Prometheus
management.metrics.enable.jvm=true
management.metrics.export.prometheus.enabled=true
management.metrics.distribution.sla.http.server.requests=50ms, 100ms, 200ms, 300ms, 500 ms, 1s
management.metrics.tags.application=app-forum-api
```

- `management.metrics.enable.jvm=true`: Habilita as métricas relacionadas à JVM.
- `management.metrics.export.prometheus.enabled=true`: Ativa a exportação das métricas no formato Prometheus.
- `management.metrics.distribution.sla.http.server.requests`: Configuração específica relacionada ao SLA (Service Level Agreement).
- `management.metrics.tags.application=app-forum-api`: Adiciona uma tag à métrica para identificar a aplicação.

Salve as configurações no arquivo.

## 3. Build e Execução da Aplicação

Execute o seguinte comando para compilar o projeto:

```bash
mvn clean package
```

Em seguida, inicie a aplicação:

```bash
java -jar -Xms128M -Xmx128M -XX:MaxMetaspaceSize=128m -Dspring.profiles.active=prod target/forum.jar
```

## 4. Acessar Métricas no Prometheus

Após iniciar a aplicação, acesse o endpoint `/actuator/prometheus` para visualizar as métricas no formato Prometheus:

[http://localhost:8080/actuator/prometheus](http://localhost:8080/actuator/prometheus)

Explore as métricas disponíveis para entender melhor o comportamento da sua aplicação. Essas métricas serão utilizadas posteriormente para configurar dashboards e alertas no Prometheus.

Lembre-se de ajustar as configurações conforme necessário para atender às especificidades do seu projeto e ambiente. Para mais detalhes, consulte a documentação do [Micrometer](https://micrometer.io/) e do [Spring Boot Actuator](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html).

# Criando Métricas Personalizadas

Usaremos o método de autenticação de usuario para criarmos uma métrica personalizada 

## 1. Abra o Controlador de Autenticação

Abra o controlador responsável pela autenticação em sua aplicação. No exemplo, o controlador é `AtenticacaoController.java`.

```java

@RestController
@RequestMapping("/auth")
@Profile(value = {"prod", "test"})
public class AutenticacaoController {

    Counter authUserSuccess;
    Counter authUserErrors;

    @Autowired
    private AuthenticationManager authManager;

    public AutenticacaoController(MeterRegistry registry) {
        authUserSuccess = Counter.builder("auth_user_success")
            .description("usuários autenticados")
            .register(registry);

        authUserErrors = Counter.builder("auth_user_error")
            .description("erros de login")
            .register(registry);
    }

    @PostMapping
    public ResponseEntity<TokenDto> autenticar(@RequestBody @Valid LoginForm form) {
        try {
            Authentication authentication = authManager.authenticate(form.converter());
            authUserSuccess.increment(); // Incrementa o contador de autenticações bem-sucedidas
            String token = tokenService.gerarToken(authentication);
            return ResponseEntity.ok(new TokenDto(token, "Bearer"));
        } catch (AuthenticationException e) {
            authUserErrors.increment(); // Incrementa o contador de erros de autenticação
            return ResponseEntity.badRequest().build();
        }
    }
}
```

## 2. Importe as Dependências

Ao adicionar os contadores, você pode notar erros indicando que `Counter` e `MeterRegistry` não são reconhecidos. Vamos corrigir isso importando as dependências necessárias. No exemplo, estamos utilizando o `io.micrometer.core.instrument`.

```java
// Importações adicionais
import io.micrometer.core.instrument.Counter;
import io.micrometer.core.instrument.MeterRegistry;
```

Certifique-se de importar as classes corretas.

## 3. Build e Execução

Após adicionar os contadores, faça o build e em seguida, inicie a aplicação.

## 4. Teste com o Postman

Abra o Postman e crie uma requisição do tipo POST para `localhost:8080/auth`. Certifique-se de adicionar o token gerado nas requisições subsequentes.

Exemplo de requisição bem-sucedida:

```json
{
  "email": "moderador@email.com",
  "senha": "123456"
}
```

Exemplo de requisição com erro:

```json
{
  "email": "moderador@email.com",
  "senha": "1234567"
}
```

## 5. Verifique as Métricas no Prometheus

Acesse o endpoint `/actuator/prometheus` (http://localhost:8080/actuator/prometheus) para visualizar as métricas no formato Prometheus.

### Métrica de Usuários Autenticados:

```plaintext
#TYPE auth_user_success_total counter
auth_user_success_total{application="app-forum-api"} 0.0
```

### Métrica de Erros de Autenticação:

```plaintext
#TYPE auth_user_error_total counter
auth_user_error_total{application="app-forum-api"} 0.0
```

Atualize as métricas após realizar as requisições no Postman para ver os contadores aumentando.