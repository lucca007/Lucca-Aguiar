# Association Studies - *In progress*  
**Idealizado por:** [Lucca V. Aguiar](https://github.com/luccav)


## Introdução  
Este repositório fornece um guia prático para a condução de estudos de associação genômica ampla (**GWAS**), com foco em:

- Controles de Qualidade para dados genéticos  
- Escolha de Covariáveis (*StepWise*) e Controle de Qualidade para Regressões
- Fine-mapping


## Cromossomos Autossômicos  

### Controles de Qualidade  

Os arquivos genéticos devem ser processados utilizando os seguintes pipelines desenvolvidos pelo **Laboratório de Diversidade Genética Humana (LDGH)**:

- [`MosaiQC`](https://github.com/ldgh/MosaiQC-public): Controle de qualidade inicial dos dados genéticos.
- [`3A`](https://github.com/ldgh/3A-public): Análise de ancestralidade da coorte.
- [`NAToRA`](https://github.com/ldgh/NAToRA_Public): Análise de kinship da coorte.


Se o NAToRA identificar clusters familiares, o pesquisador pode escolher entre:

 - Remover os indivíduos aparentados (o NAToRA indica quais);
 - Manter e usar um software de associação que incorpore a *Genetic Relationship Matrix* (GRM) como efeito aleatório.

### Filtros para Análise de Associação

Após o pré-processamento com o `MosaiQC`, aplicam-se os seguintes filtros, de acordo com o tipo de análise:

#### Fenótipos Contínuos  
- `MAF > 0.01`  
- `HWE p-value > 1e-6`

#### Análise Caso-Controle  
**Para casos:**  
- `MAF > 0.001`  
- `HWE p-value > 1e-10`

**Para controles:**  
- `MAF > 0.01`  
- `HWE p-value > 1e-5`

---

> **Nota importante:**  
Os limiares de MAF e HWE devem ser ajustados conforme o tamanho amostral da coorte.  
É recomendado remover variantes com **MAC (Minor Allele Count) < 2**, já que variantes extremamente raras podem comprometer a análise de regressão e aumentar a chance de falsos positivos, especialmente em amostras pequenas.

---


## Escolha de Covariáveis (*StepWise*) e Controle de Qualidade para Regressões
A seleção de covariáveis é feita por meio do script stepwise, que permite duas abordagens:
- **Foward step**: o fenótipo é testado individualmente com cada covariável disponível, e a covariável que melhora mais o modelo é adicionada à próxima iteração.
- **Backward step**: as covariáveis não obrigatórias são removidas uma a uma, avaliando-se o impacto de cada remoção na qualidade do modelo.

Covariaveis obrigatórias para o modelo são maleaveis, mas são como base Idade, Sexo, e os Componentes Principais (PCs) genéticos.
Para comparação é utilizado *Likelihood-ratio test* (LRT) e *Bayesian Information Criterion* (BIC), com modelo de Maximum Likelihood (ML). Determinado o modelo final, é refeita a estimativa com Restricted Maximum Likelihood (REML).

### Bibliotecas
O script é realizado em R >= v.4 utilizando as seguintes bibliotecas:
- lme4qtl
- readr
- dplyr
- MASS
- DHARMa
- MuMIn

### Controle de Qualidade da Regressão
Com o modelo estimado, é realizado a análise da regressão com auxílio dos gráficos diagnósticos.  
É estimado o R² marginal e condicional, se possuir efeitos aleatórios no modelo (NAKAGAWA; SCHIELZETH, 2013).








