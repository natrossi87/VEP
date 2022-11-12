# **Tutorial VEP**

## **Objetivos**

##### - O objetivo deste repositório é criar um tutorial para a anotação de um VCF somático utilizando VEP-ensembl 105.0.
##### - Este tutorial engloba comandos para serem executados no ambiente do Google Colaboratory (https://colab.research.google.com)

------

## **Workflow**

### **Montando o Drive**
###### - No Google Colaboratory, criar um novo Notebook (Arquivo --> Novo Notebook).
###### - Para conectar com o seu Google Grive, executar os seguintes comandos:

```
from google.colab import drive
drive.mount('/content/drive')
```

------

### **Instalação do VEP**
###### - Para instalar o VEP-ensembl 105.0 devemos digitar os seguintes comandos:

```
%%bash
sudo apt install unzip curl git libmodule-build-perl libdbi-perl libdbd-mysql-perl build-essential zlib1g-dev
wget -c https://github.com/Ensembl/ensembl-vep/archive/refs/tags/105.0.tar.gz
tar -zxvf 105.0.tar.gz
cd ensembl-vep-105.0
./INSTALL.pl --NO_UPDATE 
```

###### Descrição dos comandos acima:

| Nome do comando | Descrição |
| --- | --- |
| sudo | executa o comando com privilégios elevados |
| apt | instala, atualiza ou remove pacotes |
| install | instala o pacote |
| unzip | extrai o arquivo comprimido |
| curl | possibilita a transferência de dados de uma URL |
| git | |
| libmodule-build-perk |  |
| libdbi-perl | |
| libdbd-mysql-perl | |
| build-essential | |
| zlib1g-dev | |
| wget | faz o download do arquivo |
| -c | | 
| tar | extrai o arquivo comprimido no formato gz |
| -zxvf | |
| cd | altera o diretório de trabalho |
| .INSTALL.pl | |
| --NO_UPDATE | previne que o programa seja atualizado para versões mais recentes |


###### - Em seguida podemos testar se o VEP foi corretamente instalado por meio dos seguintes comandos:

```
%%bash
ensembl-vep-105.0
./vep
```

###### - Esses comandos devem ter sido capazes de imprimir na tela o manual do VEP ensembl.

------

### **Anotação do VCF**
###### - Em seguida utizaremos anotaremos o seguinte arquivo VCF neste tutorial (arquivo disponível na raiz do repositório):

```
%%bash
ls /content/drive/WP312.filtered.vcf.gz
```

###### - Os seguintes comandos farão a anotação do arquivo VCF usando o VEP:
###### - IMPORTANTE: necessário download do arquivo fasta para o seu drive (tamanho do arquivo Homo_sapiens_assembly19.fasta ~ 2Gb). Esse arquivo pode ser encontrado em repositórios como o do UCSC, por exemplo (https://hgdownload.soe.ucsc.edu/downloads.html).

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
| -i | caminho do arquivo de entrada (vcf.gz) |
| -o | caminho do arquivo de saída (vcf.tsv)|
| --dir_cache | |
| --fasta | caminho do arquivo .fasta |
| --cache | |
| --offline | |
| --assembly GRCh37 | |
| --refseq | |
| --pick | |
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

### **Visualização dos dados usando pandas**
###### Em seguida usaremos o módulo **pandas** do python para analisar os dados. Para tanto, instalamos o pandas por meio do seguinte comando:

```
!pip install pandas
```

###### - Para visualizar corretamente o arquivo no pandas devemos pular as linhas que contém "##" no arquivo tsv. Para verificar quantas linhas devem ser pulados podemos utilizar o seguinte comando:

```
!grep -c '##' /WP312.filtered.vcf.tsv
```

| Nome do comando | Descrição |
| --- | --- |
| grep | procura no arquivo pelo termo entre aspas |
| -c | informa o número de vezes que o termo ocorreu |

###### - No arquivo em questão, devemos pular **38 linhas**

###### - Agora, usando pandas, abriremos o arquivo tsv em uma tabela por meio dos seguintes comandos:

```
import pandas as pd
import csv

tabela = pd.read_csv('WP312.filtered.vcf.tsv', sep='\t', header=0, skiprows=38)

df = pd.DataFrame(tabela)
df
```
