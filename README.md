# ** How to download all the available genome sequences of an organism automatically?** <br />


### **AUTHOR: Dr Asad Prodhan** **https://asadprodhan.github.io/**


<br />


### **Step 1: Install NCBI datasets**


Datasets is a command line tool that allows you to easily download data from the NCBI repository.


```
conda install -c conda-forge ncbi-datasets-cli
```


For more information on the usage of datasets, explore the following pages:


https://www.ncbi.nlm.nih.gov/datasets/docs/v2/download-and-install/


https://github.com/ncbi/datasets

 
### **Step 2: Download the genome sequences of a taxon using a single command**


The following command will automatically download all the available genomes of Fusarium for example.


```
datasets download genome taxon fusarium
```


**Likewise, you can use the datasets command to download individual genome or gene sequences.**


To download a single accession:


```
datasets download genome accession GCA_000839805.1
```


To download a list of accessions:


- Make a directory and name it “Downloads”


- cd to ‘Downloads’ directory


- Keep the following **Downloading script** in the ‘Downloads’ directory 


### **Downloading script**


```
#!/bin/bash

#metadata
metadata=./*.csv
#
Red="$(tput setaf 1)"
Green="$(tput setaf 2)"
Bold=$(tput bold)
reset=`tput sgr0` # turns off all atribute
while IFS=, read -r field1  

do 
    echo ""
    echo "${Red}${Bold}Downloading ${reset}: "${field1}"" 
    datasets download genome accession "${field1}" --filename "${field1}".zip
    echo "${Bold}Extracting "${field1}.zip" ${reset}"
    unzip "${field1}.zip" 
    cd "ncbi_dataset/data/${field1}" 
    echo "${Bold}Moving "${field1}" fasta file into home directory${reset}"
    mv *.fna ../../../
    cd "../../../"
    rm -r "${field1}".zip ncbi_dataset *.md  
    echo "${Green}${Bold}Download_completed ${reset}: ${field1}" 
    echo ""
done < ${metadata}

```


- Keep the ‘accession_list.csv’ file in the ‘Downloads’ directory 


- Check the file type of ‘accession_list.csv’

  
```
file accession_list.csv
```


- If it is ‘ASCII text, with CRLF line terminators’ i.e., Windows file type; then convert it to ‘Unix’ format as follows:


```
dos2unix accession_list.csv
```


- Run the following script as follows:


```
./downloading_script.sh
```


**Now, you have downloaded the genomic sequences of all the accessions in your list.**


To download a gene sequence:


```
datasets download gene accession NP_000483.1
```


```
datasets download gene symbol brca1
```


```
datasets download gene gene-id 30515447
```


To download virus genome sequences:


```
datasets download virus genome taxon Capripoxvirus
```


More commands at 

https://github.com/ncbi/datasets



