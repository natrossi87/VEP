## **Tutorial VEP**

### **Objetivos**
#### - O objetivo deste repositório é criar um tutorial para a anotação de um VCF somático utilizando VEP-ensembl 105.0

### **Instalação**
#### - Para instalar o VEP-ensembl 105.0 devemos digitar os seguintes comandos usando bash:

```
sudo apt install unzip curl git libmodule-build-perl libdbi-perl libdbd-mysql-perl build-essential zlib1g-dev
wget -c https://github.com/Ensembl/ensembl-vep/archive/refs/tags/105.0.tar.gz
tar -zxvf 105.0.tar.gz
cd ensembl-vep-105.0
./INSTALL.pl --NO_UPDATE 
```


