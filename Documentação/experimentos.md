# Registro de Experimentos — CEDB + K-Means

## 1. Identificação do experimento

- **ID:** `cedb-kmeans-20261124T140115Z`
- **Data da execução:** 24/11/2026 às 14:01:15 UTC
- **Nome:** Clustering de eventos cibernéticos — CEDB
- **Objetivo:** Identificar perfis semelhantes de eventos para apoiar a análise humana.
- **Método de tracking:** Registro em arquivos JSON e Joblib, sem utilização do MLflow.

## 2. Dados utilizados

- **Fonte:** Cyber Events Database (CEDB).
- **Arquivo:** `cyber_events_tratado.csv`.
- **Quantidade de registros:** 16.863.
- **Quantidade de colunas originais:** 21.
- **Hash SHA-256:** `10844a6aec8ec956d4e57b40df618b18facb73137c86da553ccf018d366db269`.

O hash permite identificar precisamente a versão do arquivo utilizada nesta execução. Dessa forma, caso o dataset seja alterado, será possível identificar que se trata de outra versão dos dados.

### Variáveis utilizadas no agrupamento

Foram utilizadas cinco variáveis categóricas:

- `actor_type`;
- `industry`;
- `motive`;
- `country`;
- `actor_country`.

A variável `event_type` não participou da formação dos clusters. Ela pode ser utilizada posteriormente para analisar e interpretar os grupos encontrados.

## 3. Pré-processamento

O tratamento dos dados foi realizado antes da aplicação do K-Means.

As etapas registradas foram:

1. Seleção das cinco variáveis utilizadas no agrupamento.
2. Tratamento de valores categóricos ausentes com a categoria `"Não informado"`.
3. Aplicação do One-Hot Encoding para transformar categorias em representações numéricas.
4. Tratamento de categorias pouco frequentes e limitação da quantidade de categorias, utilizando `min_frequency=10` e `max_categories=30`.

A configuração do codificador também utiliza `handle_unknown="ignore"`, permitindo lidar com categorias não observadas durante o ajuste do pré-processador.

Após a transformação, a matriz apresentou **16.863 registros e 101 colunas**.

## 4. Seleção do número de clusters

Foram testados valores de K entre 2 e 8. A escolha considerou o maior valor de Silhouette entre as configurações avaliadas.

| K | Inércia | Silhouette |
|---:|---:|---:|
| 2 | 43.047,74 | 0,2551 |
| 3 | 38.558,15 | 0,1970 |
| 4 | 35.768,82 | 0,1604 |
| 5 | 33.366,60 | 0,1754 |
| 6 | 32.376,11 | 0,1736 |
| 7 | 30.624,57 | 0,1648 |
| 8 | 30.143,76 | 0,1702 |

O valor escolhido foi **K = 2**, pois apresentou a maior Silhouette entre os valores testados.

A Silhouette do modelo final foi calculada utilizando uma amostra de até 3.000 registros.

É importante destacar que o valor de aproximadamente 0,255 indica uma separação limitada entre os grupos. Assim, os clusters devem ser interpretados como perfis exploratórios de similaridade, e não como grupos naturalmente bem separados.

## 5. Configuração do modelo final

- **Algoritmo:** K-Means.
- **Número de clusters:** 2.
- **Inicialização:** `k-means++`.
- **Número de inicializações:** 20 (`n_init=20`).
- **Semente aleatória:** 42 (`random_state=42`).
- **Número máximo de iterações:** 300.
- **Algoritmo de otimização:** Lloyd.

### Métricas finais

- **Silhouette:** 0,255067.
- **Tamanho da amostra da Silhouette:** 3.000 registros.
- **Inércia:** 43.047,7408.

### Distribuição dos clusters

| Cluster | Registros | Percentual |
|---|---:|---:|
| Cluster 0 | 4.368 | 25,9% |
| Cluster 1 | 12.495 | 74,1% |
| **Total** | **16.863** | **100%** |

A distribuição demonstra que os grupos possuem tamanhos diferentes, com maior concentração de eventos no Cluster 1.

## 6. Ambiente de execução

As versões das principais bibliotecas utilizadas foram registradas automaticamente:

| Tecnologia | Versão |
|---|---|
| Python | 3.13.18 |
| Pandas | 2.2.3 |
| NumPy | 2.4.6 |
| Scikit-learn | 1.6.1 |
| Joblib | 1.6.0 |

## 7. Artefatos gerados

O tracking registrou o modelo treinado e o pré-processador ajustado em um arquivo Joblib.

**Arquivo:** `modelo_preprocessamento_kmeans_20261124T140115Z.joblib`

**Hash SHA-256:** `5098c09b17020aa9a564fbd1058a29765f304725101235e1d9b4bf96f82fda2c`

O artefato contém:
- Pré-processador ajustado;
- Modelo K-Means treinado;
- Lista de variáveis utilizadas.

Também foram mantidos os arquivos de apoio:

- `disponibilidade_campos_priorizacao.csv`;
- `distribuicao_clusters_por_ano.csv`;
- `eventos_com_clusters.csv`;
- `ficha_solucao.json`;
- `monitoramento_qualidade.csv`;
- `perfil_clusters.csv`;
- `qualidade_dados.csv`.

## 8. Reprodutibilidade e rastreabilidade

O registro automatizado permite identificar os dados, as configurações, as métricas e o artefato produzidos em cada execução.

Foram implementados:
- Identificador único do experimento;
- Registro da data e horário da execução;
- Registro do hash do dataset;
- Registro dos parâmetros e métricas;
- Registro das versões das bibliotecas;
- Salvamento do pré-processador e do modelo;
- Registro do hash do artefato gerado.

**Limitação atual:** o commit Git não é capturado automaticamente (`code_git_commit = null`). Portanto, a versão exata do código ainda não fica vinculada ao registro do experimento.

O tracking utiliza JSON e Joblib como solução simples de acompanhamento, sem integração com MLflow.

## 9. Interpretação e limitações

Os resultados permitem identificar dois perfis de similaridade entre os eventos registrados na CEDB. Entretanto, a Silhouette indica que a separação entre os grupos é limitada.

Os agrupamentos não representam níveis automáticos de risco, gravidade ou prioridade. A interpretação deve considerar a qualidade e a disponibilidade dos dados, além da análise humana.

O experimento também depende das informações disponíveis na base pública, que podem apresentar ausência de dados, vieses de divulgação e diferenças de preenchimento entre registros.

**Conclusão:** a execução demonstrou a possibilidade de registrar sistematicamente os experimentos, mantendo informações suficientes para identificar a versão dos dados, compreender as configurações utilizadas e recuperar o modelo treinado. Isso contribui para a rastreabilidade e a reprodutibilidade da solução.