# Plano de Monitoramento — Clustering de Eventos Cibernéticos

## 1. Objetivo

Este documento estabelece um plano para acompanhar a qualidade dos dados e o comportamento do modelo K-Means utilizado no agrupamento de eventos cibernéticos da Cyber Events Database (CEDB).

O objetivo do monitoramento é identificar alterações que possam afetar os resultados do modelo, como mudanças na distribuição dos eventos, aumento de valores ausentes, surgimento de novas categorias e alterações nos perfis dos clusters.

O acompanhamento também busca garantir que os agrupamentos continuem sendo úteis para a análise exploratória e para apoiar a tomada de decisão por profissionais de segurança da informação.

**Importante:** o monitoramento não tem como objetivo determinar automaticamente a gravidade ou o risco dos eventos. Os clusters representam perfis estatísticos e devem continuar sendo interpretados com supervisão humana.

## 2. Situação atual

O notebook já contempla algumas atividades que podem servir como base para o monitoramento:

- análise da qualidade dos dados;
- identificação de valores ausentes;
- identificação de duplicidades;
- registro do hash SHA-256 da base;
- registro das características dos clusters;
- análise da distribuição dos clusters ao longo dos anos;
- geração de arquivos CSV com indicadores de qualidade e resultados.

Entretanto, ainda não existe um processo automatizado e recorrente de monitoramento.

Não foram definidos limites oficiais de alerta, frequência de execução, responsáveis pela análise ou procedimentos automatizados de atualização do modelo.

As recomendações deste documento representam uma proposta para estruturar essas atividades.

## 3. Indicadores de monitoramento

Os indicadores foram organizados em quatro categorias: qualidade dos dados, comportamento dos clusters, mudanças nas características dos eventos e rastreabilidade.

### 3.1 Qualidade dos dados

| Indicador | O que acompanhar | Limite inicial sugerido | Ação recomendada |
|---|---|---|---|
| Valores ausentes | Percentual de ausência nas cinco variáveis utilizadas pelo modelo | Aumento superior a 5 pontos percentuais em relação à referência | Investigar a origem dos valores ausentes e avaliar o impacto no agrupamento |
| Registros duplicados | Quantidade de `slug` duplicados | Qualquer duplicidade | Verificar a origem e corrigir antes de utilizar a base |
| Novas categorias | Surgimento de categorias não observadas anteriormente | Qualquer categoria nova | Revisar a categoria e verificar a necessidade de atualizar o pré-processamento |
| Registros inválidos | Valores fora dos domínios esperados | Qualquer ocorrência identificada | Investigar e corrigir ou documentar a inconsistência |
| Quantidade de registros | Variação no volume de eventos recebidos | Variação superior a 20% em relação ao período de referência | Verificar se houve mudança na fonte, atualização ou cobertura dos dados |

Os limites apresentados são valores iniciais propostos para discussão. Eles ainda precisam ser validados com a equipe responsável e com base no comportamento histórico da CEDB.

### 3.2 Comportamento dos clusters

| Indicador | O que acompanhar | Limite inicial sugerido | Ação recomendada |
|---|---|---|---|
| Distribuição dos clusters | Percentual de eventos em cada grupo | Alteração superior a 10 pontos percentuais em relação à referência | Investigar mudanças nos perfis dos eventos |
| Quantidade de eventos por cluster | Número de registros em cada grupo | Alteração superior a 20% em relação à referência | Verificar se houve mudança no volume ou nas características dos dados |
| Silhouette | Qualidade da separação relativa entre os grupos | Queda superior a 0,05 em relação ao valor de referência | Investigar a qualidade dos agrupamentos e avaliar a necessidade de reexecução |
| Características predominantes | Alterações nas categorias mais frequentes de cada grupo | Mudança relevante nas categorias predominantes | Revisar a interpretação dos clusters com especialistas |

O valor de Silhouette obtido na execução atual, 0,256, pode ser utilizado como referência inicial para comparações futuras, desde que a metodologia de cálculo seja mantida.

Uma alteração na distribuição dos clusters não significa necessariamente que o modelo esteja errado. Ela pode representar uma mudança real nos eventos registrados ou uma mudança na cobertura da base.

### 3.3 Mudanças temporais

O notebook já possui uma análise exploratória da distribuição dos clusters por ano.

Essa análise permite observar como a participação de cada grupo varia ao longo do período analisado.

No monitoramento futuro, recomenda-se acompanhar:

- percentual de eventos de cada cluster por período;
- mudanças na distribuição de tipos de atores;
- alterações nas motivações predominantes;
- mudanças nos setores mais afetados;
- variações geográficas dos eventos.

O objetivo é identificar mudanças nos padrões históricos, sem interpretar automaticamente essas alterações como aumento ou redução do risco cibernético.

### 3.4 Rastreabilidade

| Indicador | O que acompanhar | Condição esperada | Ação recomendada |
|---|---|---|---|
| Hash da base | Identificação da versão dos dados | Hash registrado em cada execução | Registrar a nova versão e comparar com a anterior |
| Data da execução | Quando o modelo foi executado | Data registrada | Manter histórico das execuções |
| Parâmetros | Configurações utilizadas no K-Means | Parâmetros documentados | Registrar alterações e justificar decisões |
| Quantidade de registros | Volume utilizado na execução | Número registrado | Comparar com execuções anteriores |
| Versão das bibliotecas | Ambiente de execução | Versões documentadas | Registrar mudanças no ambiente |

O hash SHA-256 permite identificar alterações no arquivo de dados, mas não substitui um sistema completo de versionamento de código e modelos.

## 4. Frequência recomendada

Como a CEDB possui frequência de atualização mensal, recomenda-se utilizar essa periodicidade como referência inicial para o acompanhamento.

| Atividade | Frequência sugerida |
|---|---|
| Verificação da qualidade dos dados | A cada nova atualização da base |
| Verificação de duplicidades | A cada nova atualização da base |
| Análise da distribuição dos clusters | Mensal |
| Análise das características predominantes | Mensal |
| Comparação dos resultados com a referência histórica | Mensal |
| Revisão dos limites de alerta | Trimestral |
| Revisão da utilidade dos agrupamentos com especialistas | Trimestral ou quando houver mudanças relevantes |

A periodicidade deverá ser ajustada conforme a disponibilidade dos dados e a necessidade da organização.

## 5. Procedimento diante de alertas

Quando algum indicador ultrapassar o limite estabelecido, recomenda-se seguir o procedimento abaixo:

1. **Identificação:** registrar qual indicador apresentou alteração.
2. **Investigação:** verificar se a mudança está relacionada à qualidade dos dados, à fonte ou às características dos eventos.
3. **Avaliação:** analisar se a alteração compromete a interpretação dos clusters.
4. **Decisão:** definir se é necessário corrigir os dados, revisar o pré-processamento ou executar novamente o modelo.
5. **Registro:** documentar a ocorrência, a decisão tomada e os resultados obtidos.

### Exemplos de situações

| Situação identificada | Possível causa | Ação |
|---|---|---|
| Aumento de valores ausentes | Mudança na coleta ou na divulgação dos eventos | Investigar a fonte e revisar a qualidade |
| Surgimento de novas categorias | Novos tipos de atores ou motivações | Revisar as categorias e o One-Hot Encoding |
| Mudança expressiva na proporção dos clusters | Alteração no perfil dos eventos registrados | Investigar os dados e revisar os perfis |
| Redução da Silhouette | Menor separação entre os grupos | Avaliar a estabilidade e a utilidade do agrupamento |
| Mudança no hash da base | Atualização ou alteração do arquivo | Registrar a nova versão e repetir as verificações |

A reexecução do modelo não deve ocorrer automaticamente apenas porque um indicador mudou. Antes disso, é necessário investigar a causa e avaliar a necessidade de atualização.

## 6. Referência para comparação

Para que o monitoramento seja consistente, é necessário estabelecer uma execução de referência.

Como ponto de partida, recomenda-se utilizar a execução atual do notebook, registrando:

- arquivo de dados e hash SHA-256;
- quantidade de registros;
- variáveis utilizadas;
- parâmetros do K-Means;
- quantidade de clusters;
- valor da Silhouette;
- distribuição percentual dos grupos;
- características predominantes dos clusters.

As execuções futuras deverão ser comparadas com essa referência.

Caso o modelo seja atualizado, recomenda-se registrar uma nova referência, preservando o histórico anterior para permitir a comparação dos resultados.

## 7. Responsabilidades

A proposta de monitoramento prevê a participação de diferentes responsáveis:

| Responsável | Atividade |
|---|---|
| Equipe de dados | Verificar qualidade, preparar os dados e acompanhar os indicadores |
| Equipe de segurança da informação | Avaliar a relevância dos perfis identificados e fornecer contexto dos eventos |
| Responsável pela governança | Acompanhar registros, documentação, rastreabilidade e cumprimento dos procedimentos |
| Especialistas da organização | Validar interpretações e apoiar decisões sobre a utilização dos resultados |

A definição formal dos responsáveis ainda depende da estrutura organizacional em que a solução será utilizada.

## 8. Implementação futura

Para transformar este plano em um processo operacional, recomenda-se implementar:

1. execução recorrente das verificações de qualidade;
2. armazenamento histórico dos indicadores;
3. comparação automática com os valores de referência;
4. geração de alertas quando os limites forem ultrapassados;
5. registro das ações tomadas;
6. revisão periódica dos perfis dos clusters;
7. integração dos indicadores ao dashboard de governança.

Essas funcionalidades ainda não estão implementadas no notebook atual.

## 9. Conclusão

O monitoramento é necessário para garantir que a solução continue apresentando resultados úteis mesmo quando a base de dados e as características dos eventos sofrerem alterações.

O notebook já fornece uma base inicial para esse acompanhamento, com indicadores de qualidade, identificação da versão dos dados e análise temporal dos agrupamentos.

A implementação de um processo recorrente, com limites definidos, responsáveis e registro das ações, permitirá fortalecer a governança da solução e reduzir o risco de interpretações inadequadas dos resultados.

Os indicadores e limites apresentados neste documento são propostas iniciais e deverão ser validados antes de serem adotados como regras operacionais.