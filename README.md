# newman-runner

Proyecto para correr colecciones de Postman con Newman.
La colección de postman contiene los 4 requests solicitados en el documento de prueba para validar los siguientes escenarios:
* Crear un usuario
* Actualizar el email del usuario creado
* Consultar el usuario creado por id
* Intentar crear un usuario con un email ya registrado previamente

La API que se utiliza para ejecutar las pruebas es llamada GoRest. Esta es una API para testear y prototipar APIs.
La API provee diferentes endpoints que pueden ser utilizados para manipular recursos mediante consultas a diferentes versions de una
API Rest o GraphQL.

## Estructura del proyecto

```
package.json
collections/
    PruebaPayphone.postman_collection.json
environments/
    Test.postman_environment.json
results/
    PruebaPayphone-YYYY-MM-DD-HH-MM-SS-XXX-0.html
```

## Ejecutar y editar coleccion de pruebas utilizando Cliente Postman

1. Descarga los archivos JSON ubicados en las carpetas collections y environments de este repositorio.

2. Descarga e instala el cliente Postman desde [https://www.postman.com/downloads/](https://www.postman.com/downloads/)

3. Crea cuenta en Postman si aun no la tienes

4. Abre cliente postman y presiona boton Importar, busca los archivos JSON previamente descargados e importalos al workspace de postman.

5. Verifica que la coleccion y el ambiente Test hayan sido importados correctamente.

6. En la esquina superior derecha del cliente postman veras una lista desplegable con los ambientes disponibles en el workspace, selecciona el ambiente importado llamado Test.

7. Ejecuta los request dentro de la colección. Puedes editar los valores del ambiente o de las variables ubicadas en la colección para variar los resultados de las pruebas.

## Ejecutar y editar coleccion de pruebas utilizando Newman

1. Instala node y npm

2. Clona el repositorio

3. Ubicate en la carpeta del repositorio clonado

4. Abre una terminal e instala las dependencias:
   ```sh
   npm install
   ```

5. Ejecuta las pruebas:
   ```sh
   npm run test
   ```

6. Genera un reporte HTML:
   ```sh
   npm run test-report
   ```

Para cambiar las variables a nivel de coleccion edita el archivo JSON ubicado en la carpeta collections. Las variables se encuentran al final del archivo dentro de un array llamado variable.

## Scripts disponibles

- `npm run test`: Ejecuta la colección de Postman usando el entorno de pruebas.
- `npm run test-report`: Ejecuta la colección y genera un reporte HTML en la carpeta `results`.

## Dependencias

- [newman](https://www.npmjs.com/package/newman)
- [newman-reporter-htmlextra](https://www.npmjs.com/package/newman-reporter-htmlextra)

El reporte se guardará en la carpeta `results/`.

## Archivos principales

- Colección Postman: [`collections/PruebaPayphone.postman_collection.json`](collections/PruebaPayphone.postman_collection.json)
- Entorno de pruebas: [`environments/Test.postman_environment.json`](environments/Test.postman_environment.json)
- Reportes: [`results/`](results/)

# Respuesta a las preguntas planteadas

## Que aspectos mejorarias en tu solucion si tuvieras tiempo?
Para mejorar mi solucion utilizaria un enfoque de pruebas tipo "Data Driven Testing", lo cual consiste en crear un documento (json o csv) que contenga las variables que necesito para parametrizar diferentes escenarios de prueba y configurarlo en cliente Postman o newman para que las pruebas se ejecuten basadas en ese documento.

## Como parametrizarias el test para poder ejecutarlo en distintos ambientes?
Para ejecutar las pruebas en distintos ambientes utilizando Postman se deben crear distintos ambientes, dentro del cliente de Postman, que contengan la conficuracion de cada uno en forma de variables de ambiente. Para ejecutar las pruebas desde el cliente de postman simplemente cambiar el ambiente en cada ejecucion. Para hacerlo usando newman cli se deben exportar los ambientes, luego guardarlos dentro de la carpeta environments y crear nuevos scripts en el archivo package.json, cada script ejecutara las pruebas en el ambiente indicado. Los scripts "test" y "test-report" ya tienen parametrizado el ambiente Test.

## En caso de manejar datos sensibles para las pruebas ¿cómo lo manejarías?
Desde el cliente Postman se pueden tomar las siguientes medidas:
* Usar archivos de ambiente para guardar datos sensibles como tokens de autorizacion.
* No guardar o hardcodear datos sensibles dentro de una coleccion o sus requests. En su lugar Definir variables en los requests y colecciones que hagan referencia a las variables de ambiente.

Al usar newman tambien se deben tomar otras medidas:
* No subir al repositorio archivos que contengan datos sensibles, como archivos de ambiente. En este repositorio esos archivos se suben para satisfacer los requerimientos de la prueba.
* En flujo CI/CD se pueden utilizar los secretos para guardar las variables de entorno que se utilicen para ejecutar colecciones.