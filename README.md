# Relatório Executivo — Governança da Solução de Clustering de Eventos Cibernéticos

## 1. Resumo executivo

Este projeto apresenta uma solução de Ciência de Dados aplicada à análise de eventos cibernéticos, utilizando a Cyber Events Database (CEDB), disponibilizada pela University of Maryland.

A proposta consiste em utilizar o algoritmo K-Means para identificar agrupamentos de eventos com características semelhantes, permitindo observar padrões relacionados aos tipos de atores, setores afetados, motivações e localização dos acontecimentos.

A solução busca apoiar a análise de informações sobre incidentes cibernéticos e contribuir para processos de governança e tomada de decisão em segurança da informação.

É importante destacar que os agrupamentos não representam níveis de gravidade ou risco. Eles são perfis estatísticos que podem auxiliar profissionais na exploração dos dados e na identificação de padrões que mereçam investigação.

Além do desenvolvimento do modelo, o projeto considera aspectos de governança, incluindo qualidade dos dados, documentação, rastreabilidade, privacidade, monitoramento e gestão de riscos.

## 2. Contexto e problema identificado

A análise de eventos cibernéticos pode gerar grandes volumes de informações sobre organizações afetadas, setores econômicos, atores envolvidos, motivações e características dos acontecimentos.

Entretanto, a simples disponibilização desses dados não garante que as informações sejam facilmente interpretadas ou utilizadas para apoiar decisões.

Entre os principais desafios identificados na CEDB estão:

- grande quantidade de informações ausentes em determinados atributos;
- diferenças na distribuição geográfica dos eventos registrados;
- possibilidade de inconsistências e mudanças nas categorias;
- dificuldade de identificar padrões apenas por meio da análise individual dos registros;
- ausência de informações suficientes para determinar a criticidade de um evento em relação a uma organização específica.

Diante desse cenário, foi proposta uma solução que utiliza técnicas de agrupamento para organizar os eventos em perfis semelhantes, facilitando a exploração e a interpretação das informações disponíveis.

## 3. Solução desenvolvida

A solução utiliza o algoritmo K-Means, uma técnica de aprendizado não supervisionado que identifica grupos de registros com características semelhantes.

Foram utilizadas cinco variáveis categóricas:

- `actor_type`: tipo de ator associado ao evento;
- `industry`: setor econômico afetado;
- `motive`: motivação do evento;
- `country`: país relacionado ao evento;
- `actor_country`: país associado ao ator.

Como o algoritmo trabalha com representações numéricas, as variáveis categóricas passaram por um processo de transformação utilizando One-Hot Encoding.

Foram testadas diferentes quantidades de clusters, e a configuração com dois grupos apresentou o maior valor de Silhouette entre as alternativas avaliadas.

O modelo final foi executado com 17.476 registros, utilizando `random_state=42` e `n_init=20`.

### 3.1 Principais resultados

A execução identificou dois perfis de eventos:

**Cluster 0 — Perfil predominantemente criminoso**

- Representa aproximadamente 73,41% dos registros.
- Possui predominância de atores classificados como criminosos.
- Apresenta maior concentração de eventos associados à motivação financeira.
- Possui maior participação de eventos classificados como exploratórios (*Exploitive*).

**Cluster 1 — Perfil com maior diversidade de atores e motivações**

- Representa aproximadamente 26,59% dos registros.
- Apresenta maior participação de atores classificados como hacktivistas e grupos associados a Estados.
- Possui maior presença de eventos relacionados a protestos.
- Apresenta maior participação de eventos classificados como disruptivos (*Disruptive*).

Esses resultados permitem observar diferenças entre os perfis encontrados e oferecem um ponto de partida para análises mais aprofundadas.

Entretanto, a Silhouette de 0,256 indica que a separação entre os grupos é limitada. Portanto, os resultados devem ser interpretados como agrupamentos exploratórios, e não como categorias definitivas.

## 4. Benefícios estratégicos para a organização

A solução pode contribuir para diferentes objetivos relacionados à governança e à segurança da informação.

| Objetivo estratégico | Contribuição da solução |
|---|---|
| Apoiar a tomada de decisão | Organizar eventos semelhantes e facilitar a identificação de padrões |
| Melhorar a análise de ameaças | Permitir observar características recorrentes dos eventos registrados |
| Apoiar o planejamento de segurança | Fornecer informações históricas para análises e discussões estratégicas |
| Fortalecer a governança de dados | Documentar a origem, o tratamento, a qualidade e a utilização dos dados |
| Aumentar a transparência | Registrar parâmetros, resultados e limitações do modelo |
| Facilitar a comunicação | Disponibilizar resultados estruturados para consultas e visualizações |

A solução pode auxiliar profissionais a identificar grupos de eventos que mereçam investigação, mas não substitui ferramentas de monitoramento de segurança, sistemas de detecção de incidentes ou avaliações técnicas especializadas.

## 5. Governança da solução

A governança foi considerada desde a preparação dos dados até a documentação dos resultados.

As principais práticas contempladas incluem:

### 5.1 Qualidade dos dados

Foram realizadas verificações de valores ausentes, duplicidades, inconsistências e estrutura dos registros.

Essas verificações permitem conhecer as limitações da base antes da utilização dos dados pelo modelo.

### 5.2 Rastreabilidade

Foi utilizado o hash SHA-256 para identificar a versão do arquivo de dados utilizado na execução.

Também foram registrados parâmetros, quantidade de registros, variáveis utilizadas e resultados do modelo, permitindo documentar as condições do experimento.

### 5.3 Privacidade e minimização

A solução evita exportar informações desnecessárias para a análise dos agrupamentos.

Por exemplo, os arquivos de resultados não incluem campos como organização, descrição detalhada e endereços IP.

Essa prática reduz a exposição de informações que podem facilitar a identificação de envolvidos.

### 5.4 Documentação

Foram estruturados documentos para registrar:

- características e origem do dataset;
- experimentos realizados;
- funcionamento e limitações do modelo;
- plano de monitoramento;
- matriz de riscos;
- instruções para reprodução da solução.

### 5.5 Supervisão humana

Os resultados devem ser avaliados por profissionais antes de serem utilizados em qualquer decisão operacional.

A classificação dos eventos em clusters não representa automaticamente sua criticidade, impacto ou prioridade de atendimento.

## 6. Limitações e riscos

Apesar dos benefícios, a solução apresenta limitações que precisam ser consideradas.

A primeira está relacionada à própria fonte de dados: eventos divulgados publicamente não representam necessariamente todos os incidentes cibernéticos ocorridos.

Além disso, a elevada ausência de informações em determinados campos limita a possibilidade de realizar análises mais completas.

Outra limitação está relacionada ao algoritmo escolhido. O K-Means trabalha com distâncias entre representações numéricas, e a similaridade encontrada não significa necessariamente equivalência em termos de gravidade ou impacto.

Também não foram incorporadas informações internas de uma organização, como criticidade dos ativos, dependências operacionais, impacto financeiro ou exposição de sistemas.

Por esse motivo, a solução não deve ser utilizada isoladamente para definir prioridades de resposta ou determinar o nível de risco de um incidente.

## 7. Evolução e próximos passos

Para ampliar a utilidade da solução em um ambiente organizacional, recomenda-se desenvolver as seguintes atividades:

1. **Dashboard gerencial:** disponibilizar visualizações dos eventos, perfis dos clusters e indicadores de qualidade.
2. **Monitoramento automatizado:** acompanhar alterações na qualidade dos dados e na distribuição dos agrupamentos.
3. **Versionamento:** implementar controle de versões do código, dados e modelos.
4. **Validação especializada:** envolver profissionais de segurança da informação na avaliação dos perfis encontrados.
5. **Integração com dados internos:** considerar informações sobre ativos, sistemas e criticidade do ambiente organizacional.
6. **Aprimoramento da análise de prioridade:** definir critérios organizacionais validados para apoiar a avaliação de incidentes.

Essas atividades representam possibilidades de evolução e não devem ser consideradas funcionalidades já concluídas.

## 8. Conclusão

O projeto demonstra como técnicas de Ciência de Dados podem ser aplicadas para organizar e explorar informações sobre eventos cibernéticos, contribuindo para a identificação de padrões e para o apoio à análise de segurança da informação.

Além do desenvolvimento do modelo K-Means, foram considerados aspectos fundamentais de governança, como qualidade dos dados, rastreabilidade, privacidade, documentação, monitoramento e gestão de riscos.

A principal contribuição da solução está em transformar um conjunto de registros históricos em agrupamentos que facilitam a exploração das informações e a identificação de perfis semelhantes.

Sua utilização, entretanto, deve respeitar as limitações da base e do algoritmo, mantendo a supervisão humana e evitando interpretações que não sejam sustentadas pelos dados.

Assim, a solução representa uma ferramenta de apoio à análise e à governança de informações sobre eventos cibernéticos, com potencial de evolução para apoiar processos organizacionais de segurança da informação.