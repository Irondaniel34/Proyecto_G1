# Análisis de datos metagenómicos de regiones ITS de hongos filamentosos asociados al suelo de la Hacienda "El Prado" - Sangolquí 

**Autores:** 
**Ing. Almeida Stefany**, **Ing. Córdova Daniel** y **Ing. Drouet Ariana**

**Fecha:**  marzo 2024

# 1.  Problema
Los hongos, estimados en 1,5 millones de especies y ubicados en casi todos los ecosistemas terrestres, desempeñan roles ecológicos clave, como la descomposición de materia orgánica y la simbiosis con plantas. En Ecuador, el estudio y la caracterización de la diversidad fúngica son especialmente relevantes debido a su ubicación geográfica diversa y sus ecosistemas únicos. Sin embargo, su estudio se ve obstaculizado por su naturaleza efímera y la dificultad para identificar especies a partir de sus estructuras visibles. En particular, en la Hacienda "El Prado"-IASA I, no se ha caracterizado la microbiota fúngica de los suelos, y debido a que los cuerpos fructíferos similares a menudo representan varias especies distintas, se subraya la necesidad de abordajes moleculares centrados en el estudio de la región espaciadora transcrita interna (ITS), para así, obtener datos en cuanto a su taxonomía a nivel de género y subgénero.

# 2.  Antecedentes
La ausencia de estudios previos sobre la microbiota fúngica en los suelos del sector de Horticultura y Fruticultura de la Hacienda “El Prado” - IASA I, ubicada en la provincia Pichincha, cantón Rumiñahui, parroquia Sangolquí, destaca la necesidad de realizar esta investigación. Un proyecto que implica la evaluación pionera de la diversidad de hongos filamentosos, principalmente en aquellos con potencial benéfico para la biorrecuperación de los suelos. 

# 3.  Objetivos

## 3.1  Objetivo general
Analizar los datos metagenómicos de las regiones ITS amplificadas de hongos filamentosos utilizando herramientas bioinformáticas.

## 3.2  Objetivos específicos

* Realizar el control de calidad de los datos crudos de secuencia de las regiones ITS por medio de la herramienta FASTQc en terminal de Linux.
* Identificar molecularmente a las especies de hongos filamentosos mediante un BLASTN del NCBI.
* Extraer la información taxonómica con la alineación de las secuencias consenso mediante MSA en T-coffee y analizarla en la plataforma Galaxy Europe. 

# 4.  Flujograma de trabajo

![Flujograma](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Trabajo_final_flujograma.jpg?raw=true)

# 5.  Análisis de datos

## 5.1  Importación y preprocesamiento de los datos 
Los datos crudos provienen de muestras de hongos filamentosos asociados al suelo de la Hacienda "El Prado", los cuales fueron aislados en laboratorio para posteriormente ser identificados molecularmente mediante la secuenciación por el método Sanger y el análisis de la región ITS amplificada con los primers ITS1 (TCCGTAGGTGAACCTGCGG) e ITS4 (TCCTCCGCTTATTGATATGC) reportados por White et al. (1990).
Se realizó la importación de la data procediente del secuenciador en formato *.ab1*. Por lo cual, previo al análisis de calidad es necesario realizar la conversión al formato *.fastq*, la misma que puede realizarse mediante el uso de BioPhyton (a través de Google Colab), usando la plataforma Galaxy Europe o mediante el uso de la terminal del sistema operativo Linux, de acuerdo con la siguiente metodología:

### 5.1.1  Conversión de secuencias mediante BioPhyton
La conversión de secuencias en BioPhyton se realizó en dos fases; la primera que consistió en la carga de los archivos *.ab1* dentro de una carpeta dada por el usuario, de acuerdo con el siguiente código:
```
# Creación de la carpeta de entrada dentro del directorio "content"
import os
carpeta_entrada = input("Ingrese el nombre de la nueva carpeta: ")
carpeta_entrada_path = os.path.join("/content", carpeta_entrada)
os.makedirs(carpeta_entrada_path)

# Impresión de la ruta al nuevo directorio
print(f"Directorio", carpeta_entrada, "creado en:", carpeta_entrada_path)

# Subida de archivos.ab1 al directorio de trabajo actual
from google.colab import files
ITS_files = files.upload()

# Mover archivos a la carpeta de entrada solicitada pro el usuario
import shutil
for filename in ITS_files:
    src = os.path.join("/content", filename)
    dst = os.path.join(carpeta_entrada_path, filename)
    shutil.move(src, dst)
```
Al carga de archivos .ab1 se muestra a continuación:

![Imagen1_galaxy](https://github.com/Irondaniel34/Proyecto_G1/blob/dc9492430763dcff0a7347c0b3520ad188615f7b/Capturas_de_pantalla/Biophyton_carga%20de%20archivos%20ab1.jpg)


La segunda fase consistió en la conversión de las secuencias a formato *.fastq* para lo cual se utilizó el siguiente código:
```
# Instalación de Biophyton e importación de paquetes
!pip install biopython
from Bio import SeqIO
import os

# Ingreso de la ruta a la carpeta de entrada dada por el usuario.
input_folder = input("Ingrese el nombre de la carpeta que contiene las secuencias.ab1: ")
input_folder_path = os.path.join("/content", input_folder)

# Ingreso de la ruta a la carpeta de salida dada por el usuario.
output_folder = input("Ingrese el nombre de la carpeta donde se guardarán los archivos .fastq: ")
output_folder_path = os.path.join("/content", output_folder)

# Creación de la carpeta de salida en caso que esta no existiera en el directorio
os.makedirs(output_folder_path, exist_ok=True)


# Conversión de todos los archivos .ab1 de la carpeta de entrada por iteración con la función for
for filename in os.listdir(input_folder_path):
    # Compruebación si el archivo es un archivo .ab1
    if filename.endswith(".ab1"):
        # Determinación de la ruta del archivo .ab1
        input_file_path = os.path.join(input_folder_path, filename)

        # Determinación de la ruta de almancenamiento del archivo .fastq
        output_file_path = os.path.join(output_folder_path, filename.split(".")[0] + ".fastq")

        # Converción del archivo de .ab1 a .fastq usando Biopython
        SeqIO.convert(input_file_path, "abi", output_file_path, "fastq")

# Impresión del mensaje de confirmación
print(f"Archivos convertidos y guardados en el directorio: {output_folder_path}")

# Descarga del archivo comprimido con las secuencias en formato .fastq
!zip -r {output_folder}.zip {output_folder_path}
files.download(f"{output_folder}.zip")
```
La conversión realizada de los archivos *.ab1* a *.fastq* mientras el código se encuentra corriendo en Biophyton se muestra a continuación:

![Imagen1_galaxy](https://github.com/Irondaniel34/Proyecto_G1/blob/dc9492430763dcff0a7347c0b3520ad188615f7b/Capturas_de_pantalla/Biophyton_conversi%C3%B3n%20de%20archivos%20ab1%20a%20fastq.jpg)

El cuaderno de Google Colab se puede encontrar en el siguiente documento: [Proyecto_final_G1_ab1_to_fastq.ipynb](https://github.com/Irondaniel34/Proyecto_G1/blob/a27b121c371726325dae74519bc4dcacb4aa3c32/Proyecto_final_G1_ab1_to_fastq.ipynb)

### 5.1.2  Conversión de secuencias mediante Galaxy Europe

Se cargaron en modo de colección los datos en formato ab1 tanto para ITS1 e ITS4.

![Imagen1_galaxy](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy1.png)

Se construyeron las secuencias 

![Imagen1_galaxy](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy2.2.png)

Y posteriormente se transofrmaron de formato ab1 a fastQ 

![Imagen1_galaxy](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy3.png)

### 5.1.3  Conversión de secuencias mediante terminal de Linux

Se descargan los datos crudos del secuenciador en la máquina virtual, estos vienen en un archivo comprimido *.zip*, para lo cual se usa el comando: 
```
unzip nombre_del_archivo.zip
```
![Imagen1_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Linux1.jpg)

Para continuar con el procesamiento de las secuencias se tiene que convertir de un archivo *.ab1* a *.fastq* para lo cual se utilizaron los siguientes comandos:
```
sudo apt install emboss 
```
Para instalar el paquete EMBOSS, que contienen la herramienta SEQRET que nos permite el paso de .ab1 a .fastq
![Imagen2_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/instalaci%C3%B3n%20emboss.jpg)

**Nota:** *El paquete EMBOSS fue previamente instalado, es por eso que no sale el mensaje tradicional de su instalación*
```
mkdir ITS_fastq1 # Para crear la carpeta de salida de los archivos FASTQ
for file in ITS1/*.ab1;do  # Para realizar un bucle, en el que los archivos dentro de la carpeta ITS1 que tengan la extensión AB1, realicen la conversión
seqret -sequence $file -outseq ITS_fastq1/$(basename $file .ab1).fastq -osformat2 fastq; done # Comando seqret para la conversión de .ab1 a .fastq y su salida a la carpeta previamente creada. 
```
![Imagen3_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/ab12fastq.jpg)

Esto fue realizado en las dos carpetas que contenian archivos *.ab1*, el directorio ITS1 e ITS4.

## 5.2  Informe de calidad FASTQc

### 5.2.1  Informe de calidad mediante Galaxy Europe
Con las secuencias en formato FastQ, se realizó el análisis para el control de calidad de las secuencias en FastQC
![Imagen3_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy4.png)

Según el informe de calidad obtenido, se analizan las secuencias y se opta por relizar un corte para mejorar la calidad de las mismas. 
![Imagen3_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy5.png)

### 5.2.2  Informe de calidad mediante terminal de Linux

Se efectuó el análisis de calidad de las secuencias mediante la herramienta FASTQc, que se ejecutó mediante línea de comando de Linux. 
Para ello, se tuvo que primero instalar el paquete *FASTQc* en la terminal utilizando el siguiente comando:
```
sudo apt install fastqc
```
![Imagen4_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/instalacion%20fastqc.jpg?raw=true)
**Nota:** El paquete FASTQc ya había sido previamente instalado.

Posteriormente se realizó el análisis FASTQc, esto crea archivos con extensión *.html*, en la que nos permitirá visualizar las calidades, esto con los comandos:
```
mkdir ITS_fastqc1 # Para crear un directorio al cual irán los archivos .html
fastqc ITS_fastq1/*.fastq -o ITS_fastqc1 # Todos los archivos dentro de la carpeta ITS_fastq1 realizarán el control de calidad FASTQc y el argumento -o envía los resultados a la carpeta que se creó previamente.
```
![Imagen5_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Fastqc%20y%20salida%20a%20otra%20carpeta.jpg?raw=true)

![Imagen6_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Fastqc%20en%20carpeta.jpg?raw=true)
Archivos *.html* en carpeta creada.
![Imagen7_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/fasqcWEB.jpg?raw=true)
Visualización de FASTQc mediante archivo *.html*


## 5.3 Trimmomatic 
### 5.3.1  Trimmomatic mediante Galaxy Europe
Dado que algunas secuencias presentaban una calidad deficiente, se procedió a realizar un filtrado utilizando la herramienta Trimmomatic.
![Imagen3_linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy6.png)
Se procedió de inmediato a ejecutar un informe de calidad para cada una de estas secuencias utilizando FastQC. Se verificó que la calidad había mejorado significativamente, lo que permitió avanzar sin contratiempos en el proceso.

### 5.3.2  Trimmomatic mediante terminal de Linux
También se efectuó la herramienta *Trimmomatic* por medio de línea de comando de Linux, se tomó esta decisión al ver que mediante otras plataformas existía problemas al cargar los archivos.
Para la realización de los cortes de trimmomatic, se consultó a la inteligencia artificial, la cuál indicó:

Descargar la aplicación de Trimmomatic de la página oficial.

**Nota:** Página web de la herramienta Trimmomatic
![Imagen8Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/P%C3%A1gina%20de%20descarga%20de%20trimo.jpg?raw=true)
[USADELLAB.org](http://www.usadellab.org/cms/index.php?page=trimmomatic)

**Link de descarga**

Se ejecutó el comando de descarga y tras lo cual se efectuó un unzip del archivo descargado usando:
```
wget http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.39.zip
unzip Trimmomatic-0.39.zip
```
![Imagen9Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Descarga%20y%20unzip%20de%20trimo.jpg?raw=true)

La ejecución de la herramienta se llevó a cabo con línea de comando, usando:

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

Este comando creó un directorio (F1T) al cual salieron las nuevas secuencias cortadas con la herramienta, se especificó para el corte de tipo *Leading* y *Trailing* y se especifica que tipo de criterio de calidad de Phred utilizar, en este caso 33.

![Imagen10Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Trimme%20linux.jpg?raw=true)
![Imagen11Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/ls%20trimo.jpg?raw=true)

**Nota:** A las secuencias resultantes se les volvió a aplicar un control de calidad con FASTQc y se descartaron 7 secuencias que no cumplieron  con los estándares de los autores.
Las secuencias finales se utilizaron en la plataforma Galaxy Europe para la realización de la secuencia consenso.

## 5.4 Obtención de Secuencias Consenso mediante Galaxy Europe
Con la asistencia de la plataforma Galaxy, se logró generar secuencias consenso, lo que proporcionó una representación precisa y confiable de la región de interés de los amplicones.

![Imagen10Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy7.png)

Una vez que se obtuvo la secuencia consenso, se transformó las secuancias a formato FASTA, dado que este formato es ampliamente utilizado y es compatible con una variedad de herramientas y plataformas de análisis bioinformático. 

![Imagen10Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy8.png)

## 5.5 BLAST en NCBI 

### 5.5.1  BLAST mediante Galaxy Europe
Para la identificación y clasificación de amplicones de ITS de hongos se usó la plataforma BLAST (Basic Local Alignment Search Tool) de NCBI 
Link: https://blast.ncbi.nlm.nih.gov/Blast.cgi
, y se configuró las características de búsqueda

![Imagen10Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy9.png)

Se corrieron todas las secuencias en conjunto y los resultados se pudieron visulizar de manera individual. 

![Imagen10Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy10.png)

### 5.5.2  BLAST mediante terminal de Linux
Unión archivos FASTA
Para el *blasteo* de multiples especies en el NCBI se necesitaba realizar un archivo *.fasta* concatenado con todas las secuencias. Motivo por el que se usó la terminal de Linux para unificar los numerosos archivos *.fasta*. 

Esto se realizó utilizando los siguientes comandos:

```
for file in Fasta/*.fasta; do # Realiza en bucle de todos los archivos con extensión .fasta
cat "$file" >> ITS_SC.fasta # Con cat permite obtener un concatenado que se nombrará ITS_SC.fasta
done
```

Obtención de FASTA concatenado con las secuencias consenso.

![Imagen12Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/cat%20FASTA.jpg?raw=true)
![Imagen13Linux](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/concatenado%20FASTA.jpg?raw=true)

Consolidado de secuencias.
Este archivo fue cargado en el BLAST del NCBI. Lo cuál nos entregó los resultados como se aprecia:

![Imagen14W](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/blasteo.jpg?raw=true)
![Imagen15W](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/blast2.jpg?raw=true)

De esta manera se obtuvo en formato FASTA las secuencias alineadas, mismo que se descargó para continuar con el proceso de análisis en la plataforma de Galaxy Europe
![Imagen16W](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy13.png)

Se hizo el BLAST de múltiples secuencies, para múltiples especies, las cuáles se fueron clasificando en la siguiente tabla:

| Nombre     | Edad | Ciudad   | Nombre  |  Nombre  |   
|------------|------|----------|---------|--------- |
| Juan       | 25   | Madrid   |Madrid   |Madrid    |
| María      | 30   | Barcelona|Barcelona|Barcelona |
| Carlos     | 28   | Valencia |alencia  |alencia   |

## 5.6 Alineamiento Multiple en T-Coffee EMBL-EBI
Con el fin de analizar la biodiversidad y la filogenia de los hongos, se optó por realizar un alineamiento múltiple en la herramienta T-Coffee EMBL-EBI de las secuencias ITS.

![Imagen14W](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy11.png)
![Imagen14W](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy12.png)
![Imagen14W](https://github.com/Irondaniel34/Proyecto_G1/blob/main/Capturas_de_pantalla/Galaxy12.png)
## 5.7 Clasificación taxonómica en Galaxy Europe 
Una Kraken 2 es una herramienta de clasificación taxonómica que se utiliza comúnmente en bioinformática para identificar la composición taxonómica de muestras metagenómicas utilizando secuencias de ADN. Si estás interesado en usar Kraken 2 para analizar muestras de hongos (fungi), aquí hay algunos pasos básicos que puedes seguir:


## 5.8 Visualización 

## 5.5 BLAST en NCBI
