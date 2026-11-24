# Dataset Documentado — Cyber Events Database (CEDB)

## 1. Identificação e origem dos dados

A base utilizada no projeto é a Cyber Events Database (CEDB), disponibilizada pelo Center for Governance of Technology and Systems (GoTech), da Universidade de Maryland.

A base reúne registros históricos de eventos cibernéticos e foi selecionada para investigar padrões de ameaças e produzir informações que possam apoiar a priorização de riscos e recursos de segurança da informação em uma organização.

| Característica | Descrição |
|---|---|
| Fonte | Cyber Events Database (CEDB) |
| Provedor | Center for Governance of Technology and Systems (GoTech), Universidade de Maryland |
| Formato | CSV |
| Período do inventário | 01/01/2014 a 31/08/2026 |
| Frequência de atualização informada | Mensal |
| Registros originais | 17.480 |
| Registros após tratamento | 17.476 |
| Atributos originais | 44 |
| Atributos selecionados para o dashboard | 20 |
| Identificador dos registros | `slug` |

A CEDB disponibiliza publicamente seus dados para utilização em trabalhos e publicações, recomendando a citação do estudo de Harry e Gallagher (2018).

## 2. Dicionário de dados

O inventário da base apresenta os seguintes campos e significados:

| Campo | Significado |
|---|---|
| `slug` | Identificador único de 16 dígitos do evento. |
| `original_method` | Campo binário cuja definição não foi identificada na documentação consultada. |
| `event_date` | Data em que o evento ocorreu. |
| `reported_date` | Data de cadastro do evento. |
| `year` | Ano correspondente à data de ocorrência. |
| `month` | Mês correspondente à data de ocorrência. |
| `actor` | Organização ou indivíduo responsável pelo evento. |
| `actor_type` | Natureza ou tipo do ator responsável pelo evento. |
| `organization` | Nome da organização alvo do evento. |
| `industry_code` | Código NAICS de dois dígitos correspondente à indústria da organização alvo. |
| `industry` | Nome da categoria da indústria segundo a classificação NAICS. |
| `motive` | Resultados pretendidos pelo ator no evento. |
| `event_type` | Efeitos finais primários do evento. |
| `event_subtype` | Infraestrutura de tecnologia da informação afetada pelo evento. |
| `magnitude` | Magnitude do evento, quando aplicável. |
| `duration` | Duração do evento. |
| `scope` | Escopo ou abrangência do evento. |
| `ip` | Informações relacionadas a endereços IP. |
| `org_data` | Informações relacionadas aos dados da organização afetados pelo evento. |
| `cust_data` | Informações relacionadas aos dados de clientes afetados, quando disponíveis. |
| `description` | Descrição do evento cibernético. |
| `source_url` | Endereço da fonte pública utilizada para registrar o evento. |
| `country` | Nome do país de destino e respectivo código ISO3. |
| `actor_country` | Nome do país do perpetrador e respectivo código ISO3. |
| `state` | Localização subnacional nos Estados Unidos, quando disponível. |
| `county` | Localização subnacional nos Estados Unidos, quando disponível. |
| `change_log` | Registro das mudanças feitas na base de dados. |
| Colunas binárias | Indicadores de afiliação do país alvo a organizações e grupos internacionais. |

As colunas binárias representam afiliações relacionadas a organizações como OTAN, União Europeia, OEA, Mercosul, União Africana, ASEAN, G7 e G20, entre outras.

## 3. Qualidade dos dados

A qualidade foi avaliada considerando completude, validade, consistência, unicidade e atualidade.

### 3.1. Completude

A análise identificou nove campos com percentuais elevados de valores ausentes:

| Campo | Percentual de ausência |
|---|---:|
| `duration` | 94,02% |
| `magnitude` | 93,88% |
| `scope` | 93,87% |
| `org_data` | 88,02% |
| `cust_data` | 87,89% |
| `ip` | 87,81% |
| `reported_date` | 83,24% |
| `county` | 55,92% |
| `state` | 52,48% |

Essas ausências foram consideradas nas decisões de seleção e utilização das variáveis. Os campos não foram removidos automaticamente apenas por apresentarem valores ausentes, pois poderiam continuar sendo úteis em outras análises.

### 3.2. Validade

Foram identificados sete registros com valores de `source_url` fora do padrão esperado, incluindo endereços incompletos e referências que não estavam no formato de URL convencional.

A decisão documentada foi manter esses registros, sinalizando o problema para que a qualidade da informação pudesse ser considerada nas análises posteriores.

### 3.3. Consistência

Foram identificados dois registros em que os campos `year` e `month` não correspondiam à data informada em `event_date`.

A inconsistência foi corrigida recalculando `year` e `month` com base em `event_date`.

### 3.4. Unicidade

Foram identificados quatro registros duplicados, com os seguintes identificadores:

- `04355216afbd99c2`
- `4174e0001babdfa4`
- `4cfcdc31b4b1ebca`
- `61a16f4823347b46`

Os registros duplicados foram removidos, mantendo-se a primeira ocorrência.

### 3.5. Atualidade

A base analisada contemplava registros de 01/01/2014 a 31/08/2026, com frequência de atualização mensal informada pela fonte.

## 4. Tratamento e transformações realizadas

As decisões de tratamento foram documentadas da seguinte forma:

| Problema | Transformação | Justificativa |
|---|---|---|
| Registros duplicados | Remoção dos quatro registros duplicados, mantendo a primeira ocorrência | Evitar contabilizar o mesmo evento mais de uma vez |
| Inconsistências em `year` e `month` | Recalcular os campos a partir de `event_date` | Garantir consistência temporal |
| Espaços extras em campos textuais | Remover espaços no início e no final dos textos | Evitar diferenças artificiais entre categorias |
| Campos pouco relevantes ou redundantes | Remover `original_method`, `reported_date`, `industry_code`, `ip`, `county`, `state` e colunas binárias | Simplificar a análise e manipulação dos dados |

Os campos com muitos valores ausentes e os registros com `source_url` fora do padrão não foram automaticamente excluídos da base nessa etapa, pois a remoção poderia comprometer outras possibilidades de análise.

Após o tratamento, foram obtidos 17.476 registros válidos, sem as duplicidades e inconsistências identificadas.

O conjunto de 20 atributos selecionados para a construção do dashboard foi:

`slug`, `event_date`, `year`, `month`, `actor`, `actor_type`, `organization`, `industry`, `motive`, `event_type`, `event_subtype`, `magnitude`, `duration`, `scope`, `org_data`, `cust_data`, `description`, `source_url`, `country` e `actor_country`.

## 5. Preparação específica para o clustering

Em uma etapa posterior, foi realizada uma seleção específica de variáveis para o agrupamento de eventos.

Foram utilizadas cinco variáveis categóricas:

- `actor_type`;
- `industry`;
- `motive`;
- `country`;
- `actor_country`.

A seleção considerou a disponibilidade dos dados e a capacidade dessas variáveis de caracterizar os eventos.

A preparação contemplou o tratamento de valores ausentes com a categoria `Não informado`, quando necessário, e a aplicação de One-Hot Encoding para converter as categorias em representações numéricas.

A matriz resultante apresentou 17.476 registros e 102 colunas.

A variável `event_type` não participou da formação dos clusters. Ela foi utilizada posteriormente para interpretar os grupos encontrados.

## 6. Privacidade e ética

A avaliação de privacidade identificou que o campo `actor` pode representar uma organização ou um indivíduo.

Também foi identificada a possibilidade de reidentificação de pessoas ou grupos pela combinação de informações como `actor`, `country`, `source_url`, `organization`, `event_date` e `description`, especialmente quando associadas a outras fontes públicas.

Embora o relatório não tenha identificado dados sensíveis no conjunto analisado, foram reconhecidos possíveis vieses:

- Incidentes pouco divulgados publicamente podem estar sub-representados.
- Organizações menores podem apresentar menor representação.
- Países e regiões com menor disponibilidade de informações públicas podem aparecer menos.
- Alguns setores podem receber mais atenção da mídia e, consequentemente, apresentar maior quantidade de registros.

A base deve ser utilizada para análise de padrões e apoio à decisão, não como representação completa de todos os ataques existentes.

## 7. Rastreabilidade e limitações

O campo `slug` permite identificar e acompanhar os registros utilizados na análise.

A solução também registra informações da execução e identificação da base, incluindo hash do arquivo utilizado, permitindo reconhecer a versão específica dos dados analisados.

Entretanto, a identificação por hash não substitui um sistema completo de versionamento de dados, código e modelos.

Os resultados dependem da qualidade e da disponibilidade das informações públicas. Além disso, os clusters representam similaridades estatísticas e não devem ser interpretados automaticamente como níveis de risco ou severidade.

## 8. Referências

AUTORIDADE NACIONAL DE PROTEÇÃO DE DADOS (ANPD). Perguntas frequentes. Brasília, DF: Governo do Brasil, [202-?]. Disponível em: https://www.gov.br/anpd/pt-br/acesso-a-informacao/perguntas-frequentes. Acesso em: 25 set. 2026.

CYBER EVENTS DATABASE. About. [S. l.], [202-?a]. Disponível em: https://cybereventsdatabase.org/about. Acesso em: 25 set. 2026.

CYBER EVENTS DATABASE. Methodology. [S. l.], [202-?b]. Disponível em: https://cybereventsdatabase.org/methodology. Acesso em: 25 set. 2026.

HARRY, C.; GALLAGHER, N. Classifying Cyber Events. Journal of Information Warfare, v. 17, n. 3, p. 17–31, 2018.