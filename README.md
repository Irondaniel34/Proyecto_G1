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
La segunda fase consistió en la conversión de las secuencias a formato *.fastq* para lo cual se utilizó el soguiente código:
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
El cuaderno de Google Colab se puede encontrar en el siguiente documento: [Proyecto_final_G1_ab1_to_fastq.ipynb](https://github.com/Irondaniel34/Proyecto_G1/blob/a27b121c371726325dae74519bc4dcacb4aa3c32/Proyecto_final_G1_ab1_to_fastq.ipynb)

Los resultados de las secuencias obtenidas se muestran la figura a continuación:



### 5.1.2  Conversión de secuencias mediante Galaxy Europe

### 5.1.3  Conversión de secuencias mediante terminal de Linux

