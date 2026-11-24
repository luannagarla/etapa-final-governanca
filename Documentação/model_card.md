# Model Card — Clustering de Eventos Cibernéticos (CEDB)

## 1. Identificação

- **Nome da solução:** Clustering de Eventos Cibernéticos — CEDB.
- **Algoritmo:** K-Means.
- **Tipo de aprendizado:** Não supervisionado.
- **Objetivo:** Identificar perfis semelhantes de eventos cibernéticos para apoiar a análise humana.
- **Versão da execução documentada:** `cedb-kmeans-20261124T140115Z`.
- **Data da execução:** 24/11/2026.
- **Responsável:** Equipe do projeto acadêmico.

## 2. Objetivo e uso pretendido

A solução utiliza técnicas de aprendizado não supervisionado para agrupar eventos cibernéticos com características semelhantes.

A intenção é facilitar a exploração da base Cyber Events Database (CEDB), permitindo observar padrões relacionados ao tipo de agente, setor econômico, motivação e localização dos eventos.

Os agrupamentos podem apoiar análises exploratórias, identificação de padrões e organização das informações para avaliação humana.

O modelo não foi desenvolvido para determinar automaticamente a gravidade de um incidente ou estabelecer uma classificação definitiva de risco.

## 3. Dados e contexto de utilização

A solução utiliza registros da Cyber Events Database (CEDB), uma base pública de eventos cibernéticos.

A execução documentada utilizou o arquivo `cyber_events_tratado.csv`, contendo:

- 16.863 registros;
- 21 colunas originais;
- Dados referentes a eventos cibernéticos registrados na base.

**Hash SHA-256 do dataset:**

`10844a6aec8ec956d4e57b40df618b18facb73137c86da553ccf018d366db269`

A população de referência corresponde aos eventos cibernéticos presentes na versão da CEDB utilizada. Portanto, os resultados representam os registros disponíveis nessa fonte, e não necessariamente todos os incidentes cibernéticos ocorridos.

A solução foi construída para análise exploratória histórica. Não foi desenvolvida para monitoramento de incidentes em tempo real.

## 4. Variáveis utilizadas

Foram utilizadas cinco variáveis categóricas para formar os agrupamentos:

| Variável | Descrição |
|---|---|
| `actor_type` | Tipo de agente associado ao evento |
| `industry` | Setor econômico relacionado ao evento |
| `motive` | Motivação atribuída ao evento |
| `country` | País relacionado ao evento |
| `actor_country` | País associado ao agente |

A variável `event_type` não foi utilizada no treinamento do K-Means. Seu cruzamento com os clusters pode ser realizado posteriormente para auxiliar na interpretação dos resultados.

## 5. Pré-processamento

Antes do agrupamento, os dados categóricos foram tratados para permitir sua utilização pelo algoritmo.

As etapas implementadas incluem:

- Imputação de valores ausentes com a categoria `"Não informado"`;
- Aplicação do One-Hot Encoding;
- Configuração de `handle_unknown="ignore"`;
- Agrupamento de categorias pouco frequentes com `min_frequency=10`;
- Limitação de categorias com `max_categories=30`.

Após o pré-processamento, os 16.863 registros foram representados por uma matriz com 101 colunas.

Não foram utilizadas variáveis numéricas nessa execução.

## 6. Funcionamento do modelo

O K-Means é um algoritmo de aprendizado não supervisionado que agrupa registros de acordo com a similaridade entre suas representações numéricas.

Nesta solução, foram testados valores de K entre 2 e 8. A seleção considerou a Silhouette, sendo escolhido o valor que apresentou a maior métrica entre as configurações avaliadas.

O modelo final utiliza:

- `n_clusters=2`;
- `n_init=20`;
- `random_state=42`;
- `init="k-means++"`;
- `max_iter=300`.

## 7. Resultados e desempenho

O modelo final apresentou:

- **Número de clusters:** 2;
- **Silhouette:** 0,255067;
- **Amostra utilizada no cálculo da Silhouette:** 3.000 registros;
- **Inércia:** 43.047,7408.

### Distribuição dos agrupamentos

| Cluster | Registros | Percentual |
|---|---:|---:|
| 0 | 4.368 | 25,9% |
| 1 | 12.495 | 74,1% |

A Silhouette indica uma separação limitada entre os grupos. Por isso, os resultados devem ser utilizados como uma representação exploratória de similaridade, e não como evidência de que existem dois grupos naturalmente bem definidos.

## 8. Usos recomendados

A solução pode ser utilizada para:

- Explorar padrões existentes nos registros da CEDB;
- Identificar grupos de eventos com características semelhantes;
- Apoiar análises por agente, setor, motivação e localização;
- Auxiliar na organização e visualização dos dados;
- Orientar investigações exploratórias e análises humanas.

## 9. Usos não recomendados

O modelo não deve ser utilizado isoladamente para:

- Definir automaticamente a gravidade de um incidente;
- Determinar a prioridade de resposta a um ataque;
- Classificar organizações como seguras ou vulneráveis;
- Substituir a análise de especialistas em segurança da informação;
- Tomar decisões operacionais ou estratégicas sem validação adicional.

A similaridade estatística entre eventos não significa que eles apresentam o mesmo impacto ou nível de risco.

## 10. Limitações e riscos

### Qualidade e disponibilidade dos dados

A CEDB é uma base pública e depende da divulgação e do preenchimento das informações sobre os eventos. Assim, pode apresentar dados ausentes, inconsistências e concentração de registros em determinados países, setores ou tipos de eventos.

### Limitações do algoritmo

O K-Means depende da representação numérica dos dados e da definição do número de clusters. Como as variáveis foram transformadas por One-Hot Encoding, as distâncias calculadas representam diferenças entre categorias codificadas, não níveis de gravidade.

Além disso, o algoritmo tende a funcionar melhor quando os grupos apresentam estruturas compatíveis com suas hipóteses geométricas, o que pode não representar adequadamente todos os padrões de eventos cibernéticos.

### Separação limitada

A Silhouette de aproximadamente 0,255 indica que os agrupamentos não apresentam separação forte. Essa limitação deve ser considerada ao interpretar ou utilizar os resultados.

### Viés de divulgação

Eventos divulgados publicamente podem não representar a totalidade dos incidentes. Isso pode influenciar os padrões encontrados e limitar sua generalização.

### Privacidade e segurança

Os dados devem continuar sendo tratados com minimização, evitando a exposição desnecessária de informações que possam identificar organizações, pessoas ou sistemas.

## 11. Supervisão humana

A interpretação dos clusters deve ser realizada por pessoas com conhecimento sobre segurança da informação e contexto dos eventos.

A solução funciona como ferramenta de apoio à exploração dos dados, não como substituta da avaliação técnica.

## 12. Monitoramento e governança

O notebook já possui mecanismos de registro e documentação, incluindo:

- Identificação da versão do dataset por hash SHA-256;
- Registro dos parâmetros e métricas do experimento;
- Salvamento do modelo e do pré-processador;
- Registro das versões das bibliotecas;
- Geração de relatórios de qualidade e distribuição dos clusters.

Ainda são propostas de evolução:

- Versionamento formal do código com Git e vinculação do commit ao experimento;
- Histórico automatizado de execuções;
- Monitoramento periódico de mudanças na distribuição dos dados;
- Avaliação de mudanças nos agrupamentos;
- Definição e validação de limites de alerta;
- Revisão periódica dos resultados por especialistas.

## 13. Artefato do modelo

O modelo treinado e o pré-processador ajustado foram salvos em um arquivo Joblib.

- **Arquivo:** `modelo_preprocessamento_kmeans_20261124T140115Z.joblib`.
- **Hash SHA-256:** `5098c09b17020aa9a564fbd1058a29765f304725101235e1d9b4bf96f82fda2c`.

O arquivo contém o pré-processador ajustado, o modelo K-Means treinado e a lista de variáveis utilizadas.

## 14. Considerações finais

A solução demonstra como técnicas de aprendizado não supervisionado podem apoiar a exploração de eventos cibernéticos por meio da identificação de perfis semelhantes.

Entretanto, seus resultados dependem da qualidade da base utilizada e apresentam limitações relacionadas à separação dos clusters e à natureza das informações disponíveis.

Por isso, o modelo deve ser utilizado como instrumento de apoio à análise humana, acompanhado de práticas de rastreabilidade, documentação, monitoramento e revisão periódica.