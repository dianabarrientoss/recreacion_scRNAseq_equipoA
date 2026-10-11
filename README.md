# Recreando el analisis del proyecto A

**Por: Barrientos Gonzalez Diana, Cervantes Levario Roman & Contreras Rivera Jair Emiliano**

En este documento, vamos a recrear el analísis del equipo A reportado en el github: https://github.com/leonardoglez6/proyecto-equipo-A. A contunuación se enumeran los pasos seguidos para replicarlo:

## Preparacion del repositorio: 

Primero inicio sesion en Ken, posteriormente nos ubicamos en la carpeta personal y clonamos el repositorio usando el comando:

```bash
git clone https://github.com/leonardoglez6/proyecto-equipo-A.git
cd proyecto-equipo-A
```
**Nota:** El repositorio ignora la carpeta de datos para evitar que se suban los datos, debido a esto el repositorio carece de algunos directorios, a nosotros nos ayudo crear carpetas para poner nuestros porpios outputs y datos derivados para contrastar

```bash
mkdir outlogs_equipoB/
mkdir figuras_equipoB
mkdir datos_equipoB/
```

Posteriormente, se leyó todos los documentos y se corrieron los archivos an el orden especificado.

### Contestar preguntas

Primero, se corrio el trabajo que permitia contestar las preguntas ¿Son cuentas crudas? ¿Las células ya están filtradas? ¿Dónde están los metadatos de lote, donante y condición? ¿Qué versión de la anotación de genes se usó? Para esto, se corrieron los jobs previamente descritos en `scripts/`, modificando la ruta del output de su carpeta a la nuestra. Posteriormente se corrio con el comando 

```bash
cd R/
sbatch preguntas_job.sh
```

Posterior a revisar los outlogs, comprobamos que:

- Son cuentas crudas, corroborado comparando con  la matriz de cuentas de nuestro dataset contra la matriz de cuentas redondeada a que no tenga decimales (el outlog dice que ambas matrices son iguales)

- Hay: 38,606 genes y 927,681 células

- La matriz es raw

- No se encontró información acerca del lote o la condición

- Se uso la referencia Humana GRCh38, versión 2024-A


## Control de calidad de celulas vacias

Para el control de calildad de las ceulas vacias, se modifcio a nuestra ruta y se corrioeron los dos jos eD_SX_auto.sh y eD_SX_manual.sh, el primero permitiendo estimar las gotas vacias y el segundo permitiendo estimar el rna ambiental con soupX

```bash
sbatch eD_SX_auto.sh
sbatch eD_SX_manual.sh
```
Tanto las celulas remanentes, como las graficas del filtrado resultaron exitosas, por lo que este codigo corrrio bien, las figuras se pueden encontrar en el repositorio clonado

## Estimacion de las nuevas anotaciones

Posteriomente, obvtuvimos las neuvas anotaciones que se perdieron en el filtrado, para esto usamos el job `anotacion_job.sh`

```bash
sbatch anotacion_job.sh
```
Igualmente, una observacion es que el archivo no era un .rds si no un .sh

## Segundo filtrado de calidad

Finalmente corrimos el filtrado de calidad correspondiente al  filtrado por proporción mitocondrial, por proporción ribosomal y por dobletes

```bash
sbatch MT_job.sh
```

Este codigo igualmente se pudo replicar, por lo que podemos concluir que todo se corrio bien
