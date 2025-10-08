# Association Studies - *In progress*  
**Idealizado por:** [Lucca V. Aguiar](https://github.com/luccav)


## Introdução  
Este repositório fornece um guia prático para a condução de estudos de associação genômica ampla (**GWAS**), com foco em:

- Escolha de modelo de Covariáveis 
- Controle de Qualidade para Regressões 
- Controles de Qualidade para dados genéticos  
- Análise de Ancestralidade  
- Fine-mapping  


## Cromossomos Autossômicos  

### Controles de Qualidade  

Os arquivos genéticos devem ser processados utilizando os seguintes pipelines desenvolvidos pelo **Laboratório de Diversidade Genética Humana (LDGH)**:

- [`MosaiQC`](https://github.com/ldgh/MosaiQC-public): Controle de qualidade inicial dos dados genéticos  
- [`3A`](https://github.com/ldgh/3A-public): Análise de ancestralidade da coorte
- [`NAToRA`](https://github.com/ldgh/NAToRA_Public): Análise de kinship da coorte


 Se a partir do software NAToRA for identificado clusteres familiares, fica a escolha do investigador decidir se remove ou não os indivíduos aparentados. Se for remover, o NAToRA indica quais são para remover, se decidir manter, utilizar um software para estudos de associação que possibilite incorporar *Genetic Relationship Matriz* como efeito aleatório. 

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
