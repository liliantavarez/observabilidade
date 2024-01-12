# Observabilidade e Monitoramento

A observabilidade visa acompanhar o estado de execução de um sistema, externalizando seu comportamento em tempo real para tornar a aplicação observável por fontes externas. O foco está em tornar visíveis os comportamentos relevantes para a experiência do usuário final.

Por outro lado, o monitoramento envolve o acompanhamento do estado de um sistema por meio do registro de eventos e a execução de ações em resposta a esses eventos. Essas ações podem ser tanto reativas quanto proativas.

A observabilidade é essencial no escopo do monitoramento, especialmente em aplicações complexas e distribuídas. Em ambientes assim, um monitoramento efetivo depende da observabilidade para entender como o sistema se comporta diante do consumo.

## Métricas

No contexto de observabilidade e monitoramento, métricas desempenham um papel crucial. Uma métrica é um indicador de nível de serviço coletado ao longo do tempo. Elas são utilizadas para medir performance, disponibilidade, saturação (uso de recursos além do previsto), erros e outras informações relevantes para o negócio.

Cada métrica foca em uma propriedade específica do sistema e é uma medição realizada dentro de uma janela de tempo. A instrumentação é a chave para extrair métricas da infraestrutura, execução da aplicação e até mesmo das regras de negócio.

É importante destacar que cada métrica tem um objetivo específico. Cada ponto de atenção no sistema deve ter uma métrica correspondente. Dessa forma, as métricas fornecem informações valiosas para a equipe de desenvolvimento, operações de infraestrutura e também para a área de negócios.
```