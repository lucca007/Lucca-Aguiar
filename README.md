# Association Studies - *In progress*  
**Idealizado por:** [Lucca V. Aguiar](https://github.com/luccav)

---

## Introdução  
Este repositório fornece um guia prático para a condução de estudos de associação genômica ampla (**GWAS**), com foco em:

- Controles de Qualidade (QC)  
- Análise de Ancestralidade  
- Fine-mapping  

---

## Cromossomos Autossômicos  

### Controles de Qualidade  

Os arquivos genéticos devem ser processados utilizando os seguintes pipelines desenvolvidos pelo **Laboratório de Diversidade Genética Humana (LDGH)**:

- [`MosaiQC`](https://github.com/ldgh/MosaiQC-public): para controle de qualidade inicial  
- [`3A`](https://github.com/ldgh/3A-public): para análise de ancestralidade

---

### Filtros para Análise de Associação

Após o pré-processamento com o `MosaiQC`, aplicam-se os filtros a seguir, de acordo com o tipo de análise:

#### Fenótipos Contínuos  
- `MAF > 0.01`  
- `HWE p-value > 1e-5`

#### Análise Caso-Controle  
**Para casos:**  
- `MAF > 0.001`  
- `HWE p-value > 1e-10`

**Para controles:**  
- `MAF > 0.01`  
- `HWE p-value > 1e-5`

---

> **Nota importante:**  
Os limiares de MAF e HWE devem ser ajustados conforme o tamanho da coorte.  
É recomendado remover variantes com **MAC (Minor Allele Count) < 2**, já que variantes extremamente raras podem comprometer o poder estatístico da análise e aumentar a chance de falsos positivos, especialmente em amostras pequenas.

---
