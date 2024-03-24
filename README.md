# Análisis de datos metagenómicos de regiones ITS de hongos filamentosos asociados al suelo de la Hacienda "El Prado" - Sangolquí 

**Autores:** 
**Ing. Almeida Stefany**, **Ing. Córdova Daniel** y **Ing. Drouet Ariana**

**Fecha:**  marzo 2024
## Problema
Los hongos, estimados en 1,5 millones de especies y ubicados en casi todos los ecosistemas terrestres, desempeñan roles ecológicos clave, como la descomposición de materia orgánica y la simbiosis con plantas. En Ecuador, el estudio y la caracterización de la diversidad fúngica son especialmente relevantes debido a su ubicación geográfica diversa y sus ecosistemas únicos. Sin embargo, su estudio se ve obstaculizado por su naturaleza efímera y la dificultad para identificar especies a partir de sus estructuras visibles. En particular, en la Hacienda "El Prado"-IASA I, no se ha caracterizado la microbiota fúngica de los suelos, y debido a que los cuerpos fructíferos similares a menudo representan varias especies distintas, se subraya la necesidad de abordajes moleculares centrados en el estudio de la región espaciadora transcrita interna (ITS), para así, obtener datos en cuanto a su taxonomía a nivel de género y subgénero.

## Antecedentes

La ausencia de estudios previos sobre la microbiota fúngica en los suelos del sector de Horticultura y Fruticultura de la Hacienda “El Prado” - IASA I, ubicada en la provincia Pichincha, cantón Rumiñahui, parroquia Sangolquí, destaca la necesidad de realizar esta investigación. Un proyecto que implica la evaluación pionera de la diversidad de hongos filamentosos, principalmente en aquellos con potencial benéfico para la biorrecuperación de los suelos. 


## Objetivos

### Objetivo general
Analizar los datos metagenómicos de las regiones ITS amplificadas de hongos filamentosos utilizando herramientas bioinformáticas.

### Objetivos específicos
* Realizar el control de calidad de los datos crudos de secuencia de las regiones ITS por medio de la herramienta FASTQc en terminal de Linux.
* Identificar molecularmente a las especies de hongos filamentosos mediante un BLASTN del NCBI.
* Extraer la información taxonómica con la alineación de las secuencias consenso mediante MSA en T-coffee y analizarla en la plataforma Galaxy Europe. 

## Flujograma de trabajo

![Flujograma](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Trabajo_final_flujograma.jpg?raw=true)

## Datos
Los datos crudos fueron obtenidos de un Secuenciador Sanger. Las amplificaciones fueron realizados con primers para ITS1 e ITS4. 
Se realizó la importación de la data procediente del secuenciador en formato *.ab1* 
Estos archivos deben de pasar a formato *.fastq* mediante el uso de BioPhyton (a través de Google Colab), usando la plataforma Galaxy Europe y mediante el uso de la terminal del sistema operativo Linux.

#### Mediante BioPhyton

#### Mediante Galaxy Europe
Se cargaron en modo de colección los datos en formato ab1 tanto para ITS1 e ITS4.

![Imagen1_galaxy](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy1)

#### Mediante terminal de Linux

#### Se descargan los datos crudos del secuenciador en la máquina virtual, estos vienen en un archivo comprimido *.zip*, para lo cual se usa el comando: 
```
unzip nombre_del_archivo.zip
```
![Imagen1_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Linux1.jpg)

#### Para continuar con el procesamiento de las secuencias se tiene que convertir de un archivo *.ab1* a *.fastq* para lo cual se utilizaron los siguientes comandos:
```
sudo apt install emboss 
```
##### Para instalar el paquete EMBOSS, que contienen la herramienta SEQRET que nos permite el paso de .ab1 a .fastq
![Imagen2_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/instalaci%C3%B3n%20emboss.jpg)
###### **Nota:** *El paquete EMBOSS fue previamente instalado, es por eso que no sale el mensaje tradicional de su instalación*
```
mkdir ITS_fastq1 # Para crear la carpeta de salida de los archivos FASTQ
for file in ITS1/*.ab1;do  # Para realizar un bucle, en el que los archivos dentro de la carpeta ITS1 que tengan la extensión AB1, realicen la conversión
seqret -sequence $file -outseq ITS_fastq1/$(basename $file .ab1).fastq -osformat2 fastq; done # Comando seqret para la conversión de .ab1 a .fastq y su salida a la carpeta previamente creada. 
```
![Imagen3_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/ab12fastq.jpg)

###### Esto fue realizado en las dos carpetas que contenian archivos *.ab1*, el directorio ITS1 e ITS4.

## FASTQc
#### Se efectuó el análisis de calidad de las secuencias mediante la herramienta FASTQc, que se ejecutó mediante línea de comando de Linux. 
#### Para ello, se tuvo que primero instalar el paquete *FASTQc* en la terminal utilizando el siguiente comando:
```
sudo apt install fastqc
```
![Imagen4_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/instalacion%20fastqc.jpg?raw=true)
##### Nota: El paquete FASTQc ya había sido previamente instalado.

#### Posteriormente se realizó el análisis FASTQc, esto crea archivos con extensión *.html*, en la que nos permitirá visualizar las calidades, esto con los comandos:
```
mkdir ITS_fastqc1 # Para crear un directorio al cual irán los archivos .html
fastqc ITS_fastq1/*.fastq -o ITS_fastqc1 # Todos los archivos dentro de la carpeta ITS_fastq1 realizarán el control de calidad FASTQc y el argumento -o envía los resultados a la carpeta que se creó previamente.
```
![Imagen5_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Fastqc%20y%20salida%20a%20otra%20carpeta.jpg?raw=true)

![Imagen6_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Fastqc%20en%20carpeta.jpg?raw=true)
###### Archivos *.html* en carpeta creada.
![Imagen7_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/fasqcWEB.jpg?raw=true)
###### Visualización de FASTQc mediante archivo *.html*

## Trimmomatic 

#### También se efectuó la herramienta *Trimmomatic* por medio de línea de comando de Linux, se tomó esta decisión al ver que mediante otras plataformas existía problemas al cargar los archivos.

#### Para la realización de los cortes de trimmomatic, se consultó a la inteligencia artificial, la cuál indicó:
#### Descargar la aplicación de Trimmomatic de la página oficial.
#### Página web de la herramienta Trimmomatic
![Imagen8Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/P%C3%A1gina%20de%20descarga%20de%20trimo.jpg?raw=true)
[USADELLAB.org](http://www.usadellab.org/cms/index.php?page=trimmomatic)
###### Link de descarga

#### Se ejecutó el comando de descarga y tras lo cual se efectuó un unzip del archivo descargado usando:
```
wget http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.39.zip
unzip Trimmomatic-0.39.zip
```
![Imagen9Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Descarga%20y%20unzip%20de%20trimo.jpg?raw=true)
#### 

#### La ejecución de la herramienta se llevó a cabo con línea de comando, usando:
```
mkdir F1T #CREA CARPETA DESTINO
for file in ITS_fastq1/*.fastq; do    
filename=$(basename -- "$file")
output="F1T/trimmed_${filename}"
java -jar Trimmomatic-0.39/trimmomatic-0.39.jar SE -threads 4 "$file" "$output" \
ILLUMINACLIP:Trimmomatic-0.39/adapters/TruSeq3-SE.fa:2:30:10 LEADING:20 TRAILING:20 \
MINLEN:36 -phred33
done
```
#### Este comando creó un directorio (F1T) al cual salieron las nuevas secuencias cortadas con la herramienta, se especificó para el corte de tipo *Leading* y *Trailing* y se especifica que tipo de criterio de calidad de Phred utilizar, en este caso 33.
![Imagen10Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Trimme%20linux.jpg?raw=true)
![Imagen11Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/ls%20trimo.jpg?raw=true)

###### Nota: A las secuencias resultantes se les volvió a aplicar un control de calidad con FASTQc y se descartaron 7 secuencias que no cumplieron  con los estándares de los autores.
#### Las secuencias finales se utilizaron en la plataforma Galaxy Europe para la realización de la secuencia consenso.
