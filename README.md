# Inteligência Artificial no Jornalismo Esportivo: Extração de Entidades e Predição de Pautas sobre o Flamengo

Este repositório contém o desenvolvimento completo de um pipeline end-to-end de Processamento de Linguagem Natural (PLN) e Machine Learning Aplicado. O projeto analisa o ecossistema mediático e o viés editorial que orbitam o Clube de Regatas do Flamengo a partir de um corpus de notícias históricas do portal GE (Globo Esporte) cobrindo o período de 2015 a 2020.

O objetivo principal é extrair estruturas semânticas, mapear redes relacionais de entidades complexas e prever de forma automatizada a categorização editorial dos documentos com base exclusiva nos seus padrões lexicais.

---

## 🛠️ Tecnologias e Ecossistema Utilizados

O pipeline foi construído de forma progressiva e modular utilizando as principais bibliotecas do ecossistema de Ciência de Dados e PLN em Python:

* **Processamento de Texto Core:** `spaCy` (Modelo `pt_core_news_sm`) e `NLTK`.
* **Modelagem Vetorial:** `Gensim` (Word2Vec) e `Scikit-Learn` (TfidfVectorizer).
* **Machine Learning Predictivo:** `Scikit-Learn` (MultinomialNB, LogisticRegression, SVC).
* **Análise de Redes Complexas:** `NetworkX`.
* **Visualização de Dados:** `Matplotlib`, `Seaborn`, `WordCloud` e `displaCy`.

---

## 🏗️ Arquitetura do Pipeline (Fases de Desenvolvimento)

O projeto foi estruturado seguindo o rigor metodológico de engenharia de dados e PLN, dividido em blocos executáveis independentes:

### 🔹 Fase 0: Ingestão de Dados e Consolidação do Corpus
* Carga automatizada via Google Drive de base bruta contendo mais de 167 mil registros.
* Filtragem por strings booleanas no título ou corpo do texto para isolar o escopo do Flamengo.
* Controle de qualidade aplicando restrição física de robustez textual (documentos estritamente superiores a 150 palavras).
* Consolidação de um corpus final de alta fidelidade com **2.032 documentos**.

### 🔹 Fase 1: Pré-Processamento Avançado e Normalização
* Higienização via Expressões Regulares (Regex) para remoção de URLs e metadados de raspagem.
* Tokenização profunda com expurgo de pontuações, dígitos e espaços em branco.
* Filtro estruturado de *Stopwords* em 5 camadas customizadas: gramatical, termos de imprensa, jargões genéricos de futebol, verbos vazios de transição jornalística e remoção estratégica do próprio termo-alvo para evitar viés na contagem temática secundária.
* **Decisão de Arquitetura:** Análise comparativa empírica entre *Stemming* (NLTK RSLPStemmer) e *Lematização* (spaCy). Optou-se pela Lematização devido ao alto grau de flexão da língua portuguesa, garantindo a integridade morfológica das palavras canônicas.

### 🔹 Fase 2: Vetorização e Mecanismos de Busca
* **Modelagem Esparsa (TF-IDF):** Transformação do corpus em matriz numérica esparsa com teto calibrado em 5.000 atributos (*max_features*).
* **Motor de Busca Semântica:** Implementação de algoritmo baseado em **Similaridade de Cosseno** (*Cosine Similarity*) para recuperar documentos por afinidade de contexto, validado com múltiplas consultas de teste complexas.
* **Modelagem Contínua (Word2Vec):** Treinamento de uma rede neural rasa com representações vetoriais de 100 dimensões para capturar de forma autônoma a semântica e a proximidade de termos do domínio do futebol.
* **Visualização Estocástica (t-SNE):** Redução de dimensionalidade para mapeamento e plotagem de clusters temáticos (*Comissão Técnica, Eventos de Jogo, Ambiente/Torcida, Gestão/Mercado e Rivais*).

### 🔹 Fase 3: Modelagem Preditiva (Classificação Multiclasse)
* Formulação do problema de negócio: predição do caderno do jornal (`category`) com base nas features textuais vetorizadas via TF-IDF.
* Tratamento de desbalanceamento severo de classes aplicando o balanceamento de pesos geométricos (`class_weight='balanced'`).
* Avaliação competitiva entre três arquiteturas de aprendizado supervisionado:
    1.  *Naive Bayes (Multinomial)*
    2.  *Regressão Logística*
    3.  *Support Vector Machine (SVM)*
* **Resultado:** O classificador **SVM (Linear)** sagrou-se campeão, atingindo **92% de Acurácia global** e restabelecendo métricas de *Recall* nas classes minoritárias da redação.

### 🔹 Fase 4: Extração de Informação e Grafos de Conhecimento
* Extração de Entidades Nomeadas (NER) filtrando estritamente por tags estruturais de Pessoas (`PER`), Organizações/Times (`ORG`) e Locais (`LOC`).
* Mapeamento de co-ocorrências e conexões periféricas ao redor do nó central do Flamengo.
* Modelagem de um Grafo de Conhecimento Complexo com 26 nós via `NetworkX`.
* Cálculo e ordenação de métricas topológicas através de **Centralidade de Grau** (*Degree Centrality*).

### 🔹 Fase 5: Visualização Avançada e Síntese
* Renderização morfológica e sintática colorida na prática usando a ferramenta `displaCy`.
* Tradução analítica dos achados para stakeholders e tomadores de decisão não técnicos.

---

## 📈 Principais Achados e Insights de Negócio

1.  **O Fenômeno da Co-ocorrência e Rivalidade:** A análise topológica do Grafo de Conhecimento e as métricas de centralidade provaram que a cobertura nacional sobre o Flamengo é fortemente impulsionada pelos seus principais rivais paulistas. Os maiores pesos de aresta ligam o clube ao **Palmeiras (503 menções)**, **Corinthians (438 menções)** e **São Paulo (419 menções)**, evidenciando que a mídia opera em lógica de blocos de rivalidade interestadual para maximizar o engajamento.
2.  **Narrativa Política de Bastidores:** A auditoria léxica dos termos mais frequentes demonstrou que o jornalismo esportivo prioriza a cobertura política e administrativa do futebol. Termos como `"técnico"`, `"presidente"`, `"demissão"`, `"diretoria"` e `"milhão"` superam palavras estritamente ligadas à tática ou dinâmica de jogo, revelando um padrão de consumo focado em crises, finanças e mercado da bola.
3.  **Padronização e Automação Editorial:** A marca expressiva de 92% de acurácia obtida pela IA prova que o padrão semântico adotado por jornalistas e redatores é altamente estruturado e previsível. Em escala industrial, este modelo valida a viabilidade de implementar sistemas de roteamento e tagueamento 100% automáticos de pautas dentro de grandes veículos de comunicação.

---

## 🚀 Como Reproduzir este Projeto

Para replicar localmente ou no Google Colab o pipeline completo, siga as instruções abaixo:

1.  **Download do Dataset:** Obtenha o arquivo original `articles.csv` na plataforma do Kaggle.
2.  **Ambiente de Armazenamento:** Crie um diretório no seu Google Drive pessoal e faça o upload do arquivo `articles.csv` para a raiz ou ajuste a variável `caminho_base` na Fase 0 do script.
3.  **Execução do Script:** Abra o script Python neste repositório ou faça o upload dele no ambiente do Google Colab.
4.  **Orquestração de Dependências:** Execute a primeira célula do notebook. Ela instalará silenciosamente todas as ferramentas necessárias (`wordcloud`, `networkx`, `gensim`, etc.) e fará o download do modelo linguístico de grande escala do português (`pt_core_news_sm`), garantindo que não haja dependências ocultas.
5.  **Executar Tudo:** Clique em `Ambiente de Execução` > `Executar tudo` para reconstruir todas as visualizações, matrizes de confusão e grafos.

---

## 🎓 Informações do Projeto

* **Aluno:** Vítor Turci dos Santos Nogueira
* **Disciplina:** Processamento de Linguagem Natural
* **Instituição:** Instituto de Tecnologia Infnet
* **Status do Pipeline:** Concluído, validado e 100% reprodutível.
