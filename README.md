# **Tutorial VEP**

### **Objetivos**
###### - O objetivo deste repositório é criar um tutorial para a anotação de um VCF somático utilizando VEP-ensembl 105.0.
###### - Este tutorial engloba comandos para serem executados no ambiente do Google Colaboratory 

---

### **Montando o Drive**
###### - No Google Colaboratory, criar um novo Notebook (Arquivo --> Novo Notebook).
###### - Para conectar com o seu Google Grive, executar os seguintes comandos:

```
from google.colab import drive
drive.mount('/content/drive')
```

---

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
| sudo | |
| apt | |
| install | |
| unzip | |
| curl | |
| git | |
| libmodule-build-perk |  |
| libdbi-perl | |
| libdbd-mysql-perl | |
| tar | extrai o arquivo comprimido no formato gz |
| -zxvf | |
| cd | |
| .INSTALL.pl | |
| --NO_UPDATE | |


###### - Em seguida podemos testar se o VEP foi corretamente instalado por meio dos seguintes comandos:

```
%%bash
ensembl-vep-105.0
./vep
```
###### - Esses comandos devem ter sido capazes de imprimir na tela o manual do VEP ensembl.

---

### **Anotação do VCF**
###### - Em seguida utizaremos o seguinte arquivo VCF neste tutorial:

```
%%bash
ls WP312.filtered.vcf.gz 
```

###### - Os seguintes comandos farão a anotação do arquivo VCF:

```
%%bash
./ensembl-vep-105.0/vep  \
  --fork 3 \
	-i /content/WP312.filtered.vcf.gz \
	-o /content/WP312.filtered.vcf.tsv \
  --dir_cache /content/ \
  --fasta /content/Homo_sapiens_assembly19.fasta \
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

---

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
| -c | informa o número de vezes que o termo ocorre |

###### - No arquivo em questão, devemos pular **38 linhas**

###### - Agora, usando pandas, abriremos o arquivo tsv em uma tabela por meio dos seguintes comandos:

```
import pandas as pd
import csv

tabela = pd.read_csv('WP312.filtered.vcf.tsv', sep='\t', header=0, skiprows=38)

df = pd.DataFrame(tabela)
df
```
