# **Tutorial VEP**

## **Objetivos**

##### - O objetivo deste repositório é criar um tutorial para a anotação de um VCF somático utilizando VEP-ensembl 105.0.
##### - Este tutorial engloba comandos para serem executados no ambiente do Google Colaboratory.

------

## **Introdução**

##### - O Ensembl Variant Effect Predictor (VEP-ensembl) determina o efeito de variantes (SNPs, inserções, deleções, CNVs ou variantes estruturais) nos genes, transcritos, sequências de proteínas e regiões regulatórias.

-----

## **Fluxo de trabalho**

### **1. Montando o Drive**
##### - No [Google Colaboratory](https://colab.research.google.com), criar um novo Notebook (Arquivo -> Novo Notebook).
##### - Para conectar com o seu Google Grive, executar os seguintes comandos:

```
from google.colab import drive
drive.mount('/content/drive')
```

------

### **2. Instalação do VEP**
##### - Para instalar o VEP-ensembl 105.0 devemos digitar os seguintes comandos:

```
%%bash
sudo apt install unzip curl git libmodule-build-perl libdbi-perl libdbd-mysql-perl build-essential zlib1g-dev
wget -c https://github.com/Ensembl/ensembl-vep/archive/refs/tags/105.0.tar.gz
tar -zxvf 105.0.tar.gz
cd ensembl-vep-105.0
./INSTALL.pl --NO_UPDATE 
```

##### Descrição dos comandos acima:

| Nome do comando | Descrição |
| --- | --- |
| wget -c | faz o download do link a seguir, retomando o download caso ele seja interrompido |
| tar -zxvf | extrai o aqruivo comprimido |
| cd ensembl-vep-105.0 | altera o diretório de trabalho para o diretório descrito |
| ./INSTALL.pl --NO_UPDATE | instala o vep-ensembl-105.0 e previne a atualização do VEP-ensembl para versões mais recentes |


##### - Em seguida podemos testar se o VEP foi corretamente instalado por meio dos seguintes comandos:

```
%%bash
ensembl-vep-105.0
./vep
```

##### - Esses comandos devem ter sido capazes de imprimir na tela o manual do VEP ensembl (caso contrário, repetir o passo anterior).

------

### **3. Anotação do VCF**
##### - Em seguida utizaremos anotaremos o seguinte arquivo VCF (WP312.flitered,vcf.gz) neste tutorial. (O arquivo está disponível na raiz deste repositório):

```
%%bash
ls /content/drive/WP312.filtered.vcf.gz
```

##### - Os seguintes comandos farão a anotação do arquivo VCF usando o VEP:
###### - IMPORTANTE: necessário fazer o download do arquivo fasta (Homo_sapiens_assembly19.fasta) para o seu drive (tamanho do arquivo ~ 2Gb). Esse arquivo pode ser encontrado online em repositórios como o do [UCSC](https://hgdownload.soe.ucsc.edu/downloads.html), dentre outros.

```
%%bash
./ensembl-vep-105.0/vep  \
--fork 3 \
-i /content/drive/WP312.filtered.vcf.gz \
-o /content/drive/WP312.filtered.vcf.tsv \
--dir_cache /content/drive \
--fasta /content/drive/Homo_sapiens_assembly19.fasta \
--cache --offline --assembly GRCh37 --refseq  \
--pick --pick_allele --force_overwrite --tab --symbol --check_existing --variant_class --everything --filter_common \
--fields "Uploaded_variation,Location,Allele,Existing_variation,HGVSc,HGVSp,SYMBOL,Consequence,IND,ZYG,Amino_acids,CLIN_SIG,PolyPhen,SIFT,VARIANT_CLASS,FREQS" \
--individual all
  ```

| Nome do comando | Descrição |
| --- | --- |
| fork | determina o número de núcleos do processador utilizados para a execução do comando |
| -i | caminho do arquivo de entrada com extensão vcf.gz |
| -o | caminho do arquivo de saída com extensão vcf.tsv |
| --dir_cache | caminho do diretório cache |
| --fasta | caminho do arquivo .fasta |
| --cache | |
| --offline | |
| --assembly GRCh37 | número do assembly utilizado na análise (no caso, GRCh37) |
| --refseq | |
| --pick | |

###### - IMPORTANTE: neste exemplo utilizamos apenas alguns filtros, mas outros filtros estão disponíveis e podem ser utilizados. Para verificar quais filtros estão disponíveis, acessar a [documentação do VEP-ensembl](https://www.ensembl.org/info/docs/tools/vep/script/vep_filter.html).

| Nome do filtro | Descrição |
| --pick_allele | |
| --force_overwrite| |
| --tab| |
| --symbol | |
| --check_existing | |
| --variant_class | |
| --everything | |
| --filter_common | |
| --fields | |

------

### **4. Visualização dos dados usando pandas**

##### Em seguida utilizaremos a biblioteca **pandas** do Python para visualizar os dados do arquivo anotado. Para tanto, instalamos o pandas por meio do seguinte comando:

```
!pip install pandas
```

##### - Para visualizar corretamente o arquivo tsv no pandas devemos pular as linhas que contém "##". Para verificar quantas linhas devem ser puladas podemos utilizar o seguinte comando:

```
!grep -c '##' /WP312.filtered.vcf.tsv
```

| Nome do comando | Descrição |
| --- | --- |
| grep -c | procura no arquivo pelo termo entre aspas e informa a contagem do número de ocorrências |

###### - Assim, verificamos que no arquivo utilizado neste tutorial devemos pular **38 linhas**.

##### - Agora, usando pandas, podemos visualizar o arquivo tsv em uma tabela por meio dos seguintes comandos:

```
import pandas as pd
import csv

tabela = pd.read_csv('WP312.filtered.vcf.tsv', sep='\t', header=0, skiprows=38)

df = pd.DataFrame(tabela)
df
```
