# Análisis de datos metagenómicos de regiones ITS de hongos filamentosos asociados al suelo de la Hacienda "El Prado" - Sangolquí 

**Autores:** 
**Ing. Almeida Stefany**, **Ing. Córdova Daniel** y **Ing. Drouet Ariana**

**Fecha:**  marzo 2024
# Problema
Los hongos, estimados en 1,5 millones de especies y ubicados en casi todos los ecosistemas terrestres, desempeñan roles ecológicos clave, como la descomposición de materia orgánica y la simbiosis con plantas. En Ecuador, el estudio y la caracterización de la diversidad fúngica son especialmente relevantes debido a su ubicación geográfica diversa y sus ecosistemas únicos. Sin embargo, su estudio se ve obstaculizado por su naturaleza efímera y la dificultad para identificar especies a partir de sus estructuras visibles. En particular, en la Hacienda "El Prado"-IASA I, no se ha caracterizado la microbiota fúngica de los suelos, y debido a que los cuerpos fructíferos similares a menudo representan varias especies distintas, se subraya la necesidad de abordajes moleculares centrados en el estudio de la región espaciadora transcrita interna (ITS), para así, obtener datos en cuanto a su taxonomía a nivel de género y subgénero.

# Antecedentes

La ausencia de estudios previos sobre la microbiota fúngica en los suelos del sector de Horticultura y Fruticultura de la Hacienda “El Prado” - IASA I, ubicada en la provincia Pichincha, cantón Rumiñahui, parroquia Sangolquí, destaca la necesidad de realizar esta investigación. Un proyecto que implica la evaluación pionera de la diversidad de hongos filamentosos, principalmente en aquellos con potencial benéfico para la biorrecuperación de los suelos. 


# Objetivos

## Objetivo general
Analizar los datos metagenómicos de las regiones ITS amplificadas de hongos filamentosos utilizando herramientas bioinformáticas.

## Objetivos específicos
* Realizar el control de calidad de los datos crudos de secuencia de las regiones ITS por medio de la herramienta FASTQc en terminal de Linux.
* Identificar molecularmente a las especies de hongos filamentosos mediante un BLASTN del NCBI.
* Extraer la información taxonómica con la alineación de las secuencias consenso mediante MSA en T-coffee y analizarla en la plataforma Galaxy Europe. 

# Flujograma de trabajo

![Flujograma](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Trabajo_final_flujograma.jpg?raw=true)


## DATOS 
### Los datos crudos fueron obtenidos de un Secuenciador Sanger. Las amplificaciones fueron realizados con primers para ITS1 e ITS4. 
### Se realizó la importación de la data procediente del secuenciador en formato *.ab1* 
### Estos archivos deben de pasar a formato *.fastq* mediante el uso de BioPhyton (a través de Google Colab), usando la plataforma Galaxy Europe y mediante el uso de la terminal del sistema operativo Linux.

### Mediante BioPhyton

### Mediante Galaxy Europe

### Mediante terminal de Linux

#### Se descargan los datos crudos del secuenciador en la máquina virtual, estos vienen en un archivo comprimido *.zip*, para lo cual se usa el comando: 
```
unzip nombre_del_archivo.zip
```
![Imagen1_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Linux1.jpg)

#### Para continuar con el procesamiento de las secuencias se tiene que pasar de un archivo *.ab1* a *.fastq* para lo cual se utilizaron los siguientes comandos:
```
sudo apt install emboss # para instalar el paquete EMBOSS, que contienen la herramienta SEQRET que nos permite el paso de .ab1 a .fastq
```
![Imagen2_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/instalaci%C3%B3n%20emboss.jpg)
#### **Nota:** *El paquete EMBOSS fue previamente instalado, es por eso que no sale el mensaje tradicional de su instalación*
```
mkdir ITS_fastq1 # para crear la carpeta de salida de los archivos FASTQ.
for file in ITS1/*.ab1;do # para realizar un bucle, en el que los archivos dentro de la carpeta ITS1 que tengan la extensión AB1, realicen la conversión
seqret -sequence $file -outseq ITS_fastq1/$(basename $file .ab1).fastq -osformat2 fastq # comando de la conversión de .ab1 a .fastq.
done
```
![Imagen3_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/ab12fastq.jpg)

#### Esto fue realizado en las dos carpetas que contenian archivos *.ab1*, el directorio ITS1 e ITS4.
