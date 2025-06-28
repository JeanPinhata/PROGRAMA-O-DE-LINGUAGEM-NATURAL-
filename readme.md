# 🧠 Reconhecimento de Entidades Nomeadas com LLM (NER em Português)

Este projeto apresenta o fine-tuning de um modelo BERT para a tarefa de **Reconhecimento de Entidades Nomeadas (Named Entity Recognition - NER)** em textos em língua portuguesa, utilizando a arquitetura `neuralmind/bert-base-portuguese-cased`.

---

## 📘 Introdução

O **Reconhecimento de Entidades Nomeadas (NER)** é uma das tarefas mais fundamentais do **Processamento de Linguagem Natural (PLN)**. Seu objetivo é **identificar automaticamente nomes de pessoas, locais, organizações, datas, leis e outros elementos semânticos importantes** em um texto. Essa tarefa é essencial para transformar texto bruto em dados estruturados utilizáveis por aplicações como:

- Análise de contratos e decisões jurídicas;
- Extração de informações em notícias;
- Indexação de conteúdo acadêmico;
- Apoio a sistemas de recomendação e mecanismos de busca.

Com a evolução dos **Modelos de Linguagem de Grande Escala (LLMs)** como BERT, tornou-se possível treinar modelos altamente precisos para tarefas de NER, inclusive em idiomas com menos recursos, como o português.

---

## 🧰 Tecnologias Utilizadas

- **Python 3.10**
- **Hugging Face Transformers**
- **Hugging Face Datasets**
- **Pandas / NumPy / Scikit-learn**
- **Matplotlib / Seaborn**
- **Token Classification com `AutoModelForTokenClassification`**
- Modelo base: `neuralmind/bert-base-portuguese-cased`

---

## 📂 Dataset Utilizado — leNER-Br

- **Fonte**: [leNER-Br](https://github.com/Orochimarufan/LeNER-Br)
- **Formato**: CoNLL 2003 (cada linha contém um token e seu rótulo)
- **Idioma**: Português
- **Tipo de entidades rotuladas**:
  - `PESSOA`
  - `LOCAL`
  - `ORGANIZACAO`
  - `JURISPRUDENCIA`
  - `LEGISLACAO`
  - `TEMPO`
- **Total de sentenças utilizadas**: 1.176

Exemplo de rótulos no dataset:


---

## 🔄 Pré-processamento

As principais etapas do pré-processamento incluíram:

1. **Leitura e agrupamento das sentenças** com tokens e rótulos;
2. Conversão dos dados para estrutura de lista de dicionários `{tokens: [...], ner_tags: [...]}`;
3. Mapeamento dos rótulos para índices numéricos;
4. Tokenização usando o tokenizer do `neuralmind/bert-base-portuguese-cased`;
5. Alinhamento dos rótulos com sub-palavras (tokens gerados pelo BERT), com máscara `-100` para ignorar sub-tokens não iniciais durante o treinamento.

---

## 🧠 Treinamento do Modelo

Utilizamos o modelo `AutoModelForTokenClassification` da Hugging Face para realizar o fine-tuning em 10 épocas.

### Hiperparâmetros principais:

- **Modelo base**: `neuralmind/bert-base-portuguese-cased`
- **Épocas**: 10  
- **Batch size**: 16  
- **Otimização**: AdamW  
- **Função de perda**: CrossEntropyLoss (com `-100` ignorado)

---

## 📈 Avaliação do Desempenho

As seguintes métricas foram coletadas durante o treinamento:

| Época | Precisão | Recall | F1-Score | Acurácia |
|-------|----------|--------|----------|----------|
| 1     | 0.548    | 0.547  | 0.547    | 92.78%   |
| 5     | 0.881    | 0.903  | 0.892    | 98.39%   |
| 9     | **0.893**| **0.912**| **0.903**| **98.59%** |
| 10    | 0.882    | 0.909  | 0.895    | 98.53%   |

---

## 📊 Gráficos de Desempenho

### F1-Score por época
![alt text](<IMAGENS/GRAFICO F1.png>)

O gráfico mostra crescimento rápido nas primeiras épocas e estabilização por volta da época 5. Isso indica **convergência do modelo**, sem overfitting.

### Precisão vs Recall
![alt text](<IMAGENS/Captura de tela 2025-06-27 232506.png>)

As curvas mostram proximidade e estabilidade entre precisão e recall, sugerindo que o modelo mantém um **equilíbrio entre identificar entidades corretamente e não rotular excessivamente**.

---

## 🧪 Exemplo de Uso e Saída do Modelo

### Entrada:

"Em 18 de julho de 2023, a advogada Ana Paula Resende da Ordem dos Advogados do Brasil, localizada em Curitiba, argumentou com base no Artigo 14 da Constituição Federal e citou o Recurso Especial 123.456/SP, reforçando seu ponto."

### Saída (entidades identificadas):

![alt text](<IMAGENS/SAÍDA MODELO.png>)


O modelo é capaz de reconhecer nomes próprios, locais e datas dentro de um contexto jurídico ou noticioso com excelente precisão.

---

## ✅ Conclusão

O desenvolvimento deste projeto demonstrou, na prática, o potencial dos **modelos de linguagem baseados em Transformers** — como o BERT — na tarefa de **Reconhecimento de Entidades Nomeadas (NER)** em língua portuguesa. A escolha do modelo `neuralmind/bert-base-portuguese-cased`, treinado especificamente com dados do português, aliada a uma pipeline bem estruturada de pré-processamento, tokenização e alinhamento de rótulos, possibilitou um desempenho sólido e estável ao longo das épocas.

As métricas obtidas — com destaque para um **F1-Score superior a 90%** e **acurácia acima de 98%** — indicam que o modelo treinado é capaz de **identificar entidades com alta precisão e equilíbrio**, mesmo em sentenças complexas. Isso valida seu uso em **cenários reais**, como automação de análise jurídica, indexação de documentos, extração de informações em grandes volumes de texto e suporte a sistemas inteligentes de busca.

Além dos resultados numéricos, o projeto também reforça a importância de se desenvolver e adaptar tecnologias de PLN para o português, ampliando o acesso e a aplicabilidade dessas soluções em contextos nacionais.

Por fim, este trabalho serve como **base sólida para estudos futuros**, como:

- Expansão do número de classes de entidades;
- Uso de arquiteturas mais recentes (como RoBERTa ou LLaMA);
- Integração com aplicações em tempo real via APIs;
- Transferência para domínios específicos (jurídico, biomédico, jornalístico).

