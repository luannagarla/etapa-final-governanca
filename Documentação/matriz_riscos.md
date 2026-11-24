# Matriz de Riscos — Clustering de Eventos Cibernéticos

## 1. Objetivo

Este documento apresenta os principais riscos associados à solução de agrupamento de eventos cibernéticos desenvolvida com o algoritmo K-Means e utilizando dados da Cyber Events Database (CEDB).

A matriz busca identificar situações que possam comprometer a qualidade dos dados, a confiabilidade dos agrupamentos, a privacidade das informações e a utilização adequada dos resultados.

Também são apresentadas medidas de prevenção e mitigação para reduzir os impactos desses riscos.

## 2. Critérios de avaliação

Os riscos são avaliados considerando dois critérios:

- **Probabilidade:** possibilidade de o risco ocorrer.
- **Impacto:** consequência que sua ocorrência pode causar à solução.

Para facilitar a classificação, são utilizadas três categorias:

| Classificação | Significado |
|---|---|
| Baixa | Pouca possibilidade de ocorrência ou impacto limitado |
| Média | Possibilidade razoável de ocorrência ou impacto moderado |
| Alta | Grande possibilidade de ocorrência ou impacto significativo |

A classificação apresentada é uma avaliação qualitativa inicial, elaborada com base nas características do projeto. Ela deverá ser revisada conforme a solução evoluir e passar a ser utilizada em um ambiente organizacional.

## 3. Matriz de riscos

### 3.1 Riscos relacionados aos dados

| ID | Risco | Probabilidade | Impacto | Medidas de prevenção e mitigação |
|---|---|---|---|---|
| D01 | Grande quantidade de valores ausentes | Alta | Alta | Monitorar a ausência de informações, documentar as limitações e revisar os métodos de tratamento |
| D02 | Viés de divulgação dos eventos | Alta | Alta | Considerar que a base contém eventos divulgados publicamente e evitar generalizações sobre todos os incidentes existentes |
| D03 | Informações desatualizadas ou inconsistentes | Média | Média | Verificar datas, categorias e consistência dos registros a cada atualização |
| D04 | Registros duplicados | Baixa | Média | Utilizar o identificador `slug` para identificar duplicidades e validar a base antes do processamento |
| D05 | Concentração geográfica dos registros | Alta | Média | Avaliar a distribuição geográfica e considerar a possível sub-representação de determinadas regiões |
| D06 | Mudanças na estrutura ou nas categorias da fonte | Média | Média | Documentar as alterações e revisar o pré-processamento quando necessário |

**Observações:**

A ausência de informações é uma limitação importante da CEDB. No arquivo utilizado no notebook de clustering, os campos `duration`, `magnitude` e `scope` não apresentam valores preenchidos. Além disso, `org_data` e `cust_data` possuem baixa disponibilidade.

Outro ponto importante é o viés de divulgação: eventos registrados publicamente não representam necessariamente todos os incidentes cibernéticos ocorridos. Portanto, os resultados não devem ser interpretados como uma representação completa do cenário mundial de ameaças.

### 3.2 Riscos relacionados ao modelo

| ID | Risco | Probabilidade | Impacto | Medidas de prevenção e mitigação |
|---|---|---|---|---|
| M01 | Interpretação incorreta dos clusters como níveis de risco | Média | Alta | Documentar que os agrupamentos representam similaridade entre eventos, não severidade ou criticidade |
| M02 | Baixa separação entre os grupos | Alta | Média | Acompanhar a Silhouette e avaliar a utilidade dos agrupamentos com especialistas |
| M03 | Sensibilidade do K-Means à representação dos dados | Média | Média | Documentar o One-Hot Encoding, os parâmetros e as variáveis utilizadas |
| M04 | Alteração dos perfis dos clusters ao longo do tempo | Média | Média | Comparar a distribuição e as características dos grupos em novas execuções |
| M05 | Dependência das variáveis selecionadas | Média | Alta | Revisar periodicamente as variáveis e avaliar se representam adequadamente os objetivos da análise |
| M06 | Uso dos resultados fora da finalidade prevista | Média | Alta | Restringir a interpretação dos resultados ao apoio exploratório e à análise humana |

**Observações:**

O K-Means agrupa registros considerando a distância entre suas representações numéricas. Neste projeto, as variáveis categóricas foram transformadas por One-Hot Encoding.

Entretanto, a similaridade matemática entre eventos não significa que eles possuam a mesma gravidade ou representem o mesmo nível de ameaça.

A Silhouette obtida na execução atual foi de 0,256 para dois clusters, indicando uma separação limitada entre os grupos. Por isso, os agrupamentos devem ser considerados perfis exploratórios, e não classificações definitivas.

### 3.3 Riscos relacionados à privacidade e à ética

| ID | Risco | Probabilidade | Impacto | Medidas de prevenção e mitigação |
|---|---|---|---|---|
| P01 | Reidentificação de organizações ou envolvidos pela combinação de informações | Média | Alta | Minimizar os dados exportados e evitar a exposição desnecessária de informações identificáveis |
| P02 | Utilização de informações fora da finalidade do projeto | Baixa | Alta | Definir finalidade de uso e restringir o acesso aos resultados |
| P03 | Interpretações enviesadas devido à desigualdade de representação dos registros | Alta | Média | Documentar as limitações da fonte e evitar conclusões generalizadas |
| P04 | Decisões automatizadas baseadas exclusivamente nos agrupamentos | Média | Alta | Exigir avaliação humana antes de qualquer decisão operacional |
| P05 | Uso indevido de informações publicadas na base | Baixa | Alta | Respeitar as condições de uso da fonte e aplicar princípios de minimização e necessidade |

**Observações:**

Embora a base seja composta por informações de eventos divulgados publicamente, isso não elimina os riscos relacionados à privacidade.

A combinação de informações como organização, país, data, descrição e fonte original pode permitir a identificação de envolvidos.

Como medida de minimização, a exportação `eventos_com_clusters.csv` não inclui campos como `organization`, `description` e endereços IP.

A utilização dos resultados também deve considerar princípios de transparência, finalidade e necessidade, evitando que os agrupamentos sejam utilizados para atribuir responsabilidade ou classificar organizações como inseguras sem evidências adicionais.

### 3.4 Riscos relacionados à segurança e à operação

| ID | Risco | Probabilidade | Impacto | Medidas de prevenção e mitigação |
|---|---|---|---|---|
| S01 | Acesso não autorizado aos arquivos e resultados | Média | Alta | Restringir permissões de acesso e armazenar os arquivos em ambientes controlados |
| S02 | Alteração não documentada dos dados | Média | Média | Registrar o hash SHA-256 e manter histórico das versões utilizadas |
| S03 | Perda dos arquivos ou resultados gerados | Baixa | Média | Manter cópias de segurança e organizar os artefatos do projeto |
| S04 | Falta de rastreabilidade das execuções | Média | Alta | Registrar parâmetros, versões dos dados, data de execução e métricas |
| S05 | Ausência de monitoramento recorrente | Alta | Média | Implementar o plano de monitoramento e definir responsáveis pelas verificações |

**Observações:**

O projeto já possui algumas medidas de rastreabilidade, como o uso do hash SHA-256 para identificar a base utilizada, o registro dos parâmetros do modelo e a geração de arquivos com os resultados.

Entretanto, ainda não existe um processo automatizado de monitoramento, versionamento formal do código ou controle histórico completo das versões do modelo.

Essas atividades devem ser consideradas em uma futura evolução da solução.

## 4. Priorização dos riscos

Considerando a combinação entre probabilidade e impacto, os riscos que merecem maior atenção são:

| Risco | Motivo da prioridade |
|---|---|
| D01 — Valores ausentes | A falta de informações limita as análises e reduz as possibilidades de utilização de determinadas variáveis |
| D02 — Viés de divulgação | A base não representa necessariamente todos os eventos cibernéticos existentes |
| M01 — Interpretação incorreta dos clusters | Pode levar a decisões inadequadas caso similaridade seja confundida com gravidade |
| M05 — Dependência das variáveis | A escolha dos atributos influencia diretamente os agrupamentos encontrados |
| P01 — Reidentificação | A combinação de informações pode expor organizações ou envolvidos |
| P04 — Decisões automatizadas | A utilização dos clusters como decisão definitiva pode gerar avaliações injustificadas |
| S04 — Falta de rastreabilidade | Dificulta a reprodução e a auditoria dos resultados |

Esses riscos devem receber atenção durante a evolução e a utilização da solução, especialmente se ela for integrada a processos reais de segurança da informação.

## 5. Acompanhamento e revisão

A matriz deverá ser revisada periodicamente e sempre que ocorrerem mudanças relevantes na solução, como:

- atualização da base de dados;
- inclusão ou remoção de variáveis;
- alteração dos parâmetros ou do algoritmo;
- mudança significativa na distribuição dos clusters;
- integração com dados internos de uma organização;
- utilização dos resultados em novos processos de decisão.

Recomenda-se que a revisão seja realizada em conjunto pelas equipes de dados, segurança da informação e governança.

As ocorrências identificadas devem ser documentadas, incluindo o risco, o impacto observado, a medida adotada e o responsável pelo acompanhamento.

## 6. Conclusão

A identificação dos riscos permite compreender que a confiabilidade de uma solução de Ciência de Dados não depende apenas do desempenho do algoritmo, mas também da qualidade das informações, da origem dos dados, da rastreabilidade e da forma como os resultados são utilizados.

No projeto, destacam-se como pontos de atenção a elevada ausência de informações em alguns atributos, o viés de divulgação da CEDB, a separação limitada entre os clusters e a possibilidade de interpretações equivocadas dos agrupamentos.

A adoção das medidas propostas contribui para uma utilização mais responsável e transparente da solução, mantendo a supervisão humana e respeitando os limites do modelo.

A matriz deverá ser atualizada conforme novas funcionalidades forem implementadas e a solução passar a operar em um contexto organizacional.