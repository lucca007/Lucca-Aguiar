# Association Studies - *In progress*  
**Idealizado por:** [Lucca V. Aguiar](https://github.com/luccav)


## Introdução  
Este repositório fornece um guia prático para a condução de estudos de associação genômica ampla (**GWAS**), com foco em:

- Controles de Qualidade para dados genéticos  
- Escolha de Covariáveis (*StepWise*) e Controle de Qualidade para Regressões
- GWAS
- Fine-mapping

## Softwares
- Plink
- GTCA
- SAIGE
- NAToRA

## Cromossomos Autossômicos  

### Controles de Qualidade  

Os arquivos genéticos devem ser processados utilizando os seguintes pipelines desenvolvidos pelo **Laboratório de Diversidade Genética Humana (LDGH)**:

- [`MosaiQC`](https://github.com/ldgh/MosaiQC-public): Controle de qualidade inicial dos dados genéticos.
- [`3A`](https://github.com/ldgh/3A-public): Análise de ancestralidade da coorte.
- [`NAToRA`](https://github.com/ldgh/NAToRA_Public): Análise de kinship da coorte.
- [`Annotation Tool`](https://github.com/ldgh/-Annotation_Tool): Anotação de variantes.


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

> **Nota:**  
Os limiares de MAF e HWE devem ser ajustados conforme o tamanho amostral da coorte.  
É recomendado remover variantes com **MAC (Minor Allele Count) < 2**, já que variantes extremamente raras podem comprometer a análise de regressão e aumentar a chance de falsos positivos, especialmente em amostras pequenas.

---

## Escolha de Covariáveis (*StepWise*) e Controle de Qualidade para Regressões
A seleção de covariáveis é feita por meio do script stepwise, que permite duas abordagens:
- **Foward step**: o fenótipo é testado individualmente com cada covariável disponível, e a covariável que melhora mais o modelo é adicionada à próxima iteração.
- **Backward step**: as covariáveis não obrigatórias são removidas uma a uma, avaliando-se o impacto de cada remoção na qualidade do modelo.

Covariaveis obrigatórias para o modelo são maleaveis, mas são como base Idade, Sexo, e os Componentes Principais (PCs) genéticos.
Para comparação é utilizado *Likelihood-ratio test* (LRT) e *Bayesian Information Criterion* (BIC), com modelo de Maximum Likelihood (ML). Determinado o modelo final, é refeita a estimativa com Restricted Maximum Likelihood (REML).

## Escolha de Componentes Principais
Similarmente ao *StepWise* é realizado a escolha de números de PCs com base em sua significância. 
É realizado também a análise dos PCs para vermos se não representa algumma região genômica de alta variabilidade ou se esta clusterizando famílias, onde em ambos os casos não seria adequado incorporar no modelo para corrigir estruturação populacional.

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


### GWAS
Utilizamos o software GCTA (prentendemos mover para o SAIGE). Inicialmente criamos a *Genetic Relationship Matrix* (GRM) pelo próprio software:
 `SCRIPT GRM`

Se os dados forem muito grande, podemos ter a opção de criar a GRM para cada cromossomo e posteriormente juntar todos:
`CONCATENAÇÃO GRM CROMOSSOMOS INDIVIDUAIS`

Com a escolha das cováriaveis e números de PC's escolhido posteriormente, é relizado o GWAS:


## Fine-mapping
Com o GWAS *Summary Statistics* é realizado:

- Anotação das variantes com o software Annotation.
- Criação Manhattan e QQ plot

Este repositório contém um conjunto de scripts desenvolvidos por **LUCCA V. AGUIAR** e **MARCUS V. G. ANTUNES**, com o objetivo de:

- Auxiliar e padronizar a saída dos testes de associação genética.
- Automatizar a plotação de imagens para análise de associação (Manhattan e QQ plots).
- Padronizar a busca e comparação com o banco de dados GWAS Catalog, identificando variantes próximas fisicamente.

A filtragem e organização dos resultados são realizadas em Python, enquanto a plotação é feita em R devido à qualidade gráfica e disponibilidade de pacotes especializados. Para eficiência computacional, apenas variantes com p-valor menor ou igual ao limite estabelecido no config.ini serão processadas na plotação e comparação com o GWAS Catalog.



## Bibliotecas

Python:
- pandas
- configparser (ConfigParser)
- os
- glob

R:
- parallel
- qqman
- data.table


  
## Explicação e Arquivos de Input

Configuração do config.ini

O arquivo config.ini deve ser configurado antes da execução do script. Os seguintes parâmetros precisam ser definidos:

```
[Filter]
DIRECTORY_ASSOC = <diretório_com_os_testes_de_associação>
FILTER_OUTPUT_PATH = <diretório_para_os_resultados_filtrados>
P_VALUE = <p-valor_desejado_para_filtragem>  # Padrão: 1e-06

[Search]
GWAS_CATALOG = <caminho_para_o_banco_GWAS_Catalog>
SEARCH_OUTPUT_PATH = <diretório_de_saida_dos_resultados>
SIZE_OF_WINDOW = <tamanho_da_janela_em_bp>  # Padrão: 25000 (upstream e downstream)
```

Atenção:
O cabeçalho dos arquivos de entrada deve estar corretamente nomeado. Em algumas saídas do PLINK, o cabeçalho pode aparecer com formatação inadequada, como:

```
X10  X48486  X10.48486_C.T  C  T  T.1  ADD  X679  X.0.0139651  X0.0540564  X.0.258344  X0.796221
```

Certifique-se de padronizar os nomes das colunas antes de executar o script.



## Exemplo de Análise
Execução do Script:


Execute o script R para gerar as imagens de plotação.

Se a execução for bem-sucedida, os gráficos serão gerados e salvos no diretório de saída:

![Test_file_Manhattan](https://github.com/user-attachments/assets/b8325a9d-a3cc-49da-bcfe-a5312e7a479d)

Manhattan plot

![Test_file_QQplot](https://github.com/user-attachments/assets/f567b0e1-7c7c-4802-a014-12f0cf50e251)

QQ plot

Execute o script Python:

Verifique se os arquivos estão corretamente organizados.

Ajuste o arquivo config.ini com os caminhos corretos (Teste de associação e Banco de dados GWAS Catalog).

Execute o script Python para filtragem: 
O script Python gera dois arquivos principais:
  - filtered_pvalues.txt: Contém as variantes que passaram pelo critério de filtragem do p-valor. No exemplo abaixo, com P_VALUE = 1e-03, foram selecionadas 9 variantes.
  - SNPs_GWAS_Catalog.tsv: Lista as variantes filtradas que foram comparadas com o GWAS Catalog, buscando variantes próximas (exemplo: 25.000 pb). O resultado inclui 3.958 variantes associadas a fenótipos próximos.





