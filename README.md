# Desarrollo de una API Interna en una Empresa
**Escenario:**
- Formas parte de un equipo de desarrollo backend en una empresa mediana que desarrolla una API REST en Java con Spring Boot. Esta API permite consultar datos internos (por ejemplo, empleados, inventario o métricas financieras) y es consumida por otros equipos de la misma empresa.

##  Problema:
El equipo andaba teniendo bastantes fallos en el entorno de producción porque:

- Los desarrolladores subían código que directamente no funcionaba bien. Las test no se ejecutaban. 
- La validación previa a la integración dependía de una revisión hecha a mano.

Implementación de la solución: CI usando GitHub Actions 
### Etapa 1: Desarrollo de un flujo de IC 
- Se genera un archivo . github/workflows/ci. yml para lograr que: 
  - Se valide que el código nuevo se compile sin errores. 
  - Se activen todos los tests unitarios con cada "push" o "pull request". 
  - Se garantice que solo el código probado se pueda fusionar a la rama principal. 

### Etapa 2: Ajuste de las reglas en GitHub 
- Se ajusta GitHub para impedir la fusión a "main" si el flujo de trabajo falla. 
  - Se añaden ramas protegidas y se demandan chequeos obligatorios. 

### Etapa 3: Carga de los componentes creados 
- El proceso genera un . jar listo para ser desplegado en "staging" o producción. 
- El flujo incluye la carga del . jar como un componente para que otro equipo (DevOps o QA) lo descargue y lo despliegue.
-----------------------------------------------------------------------------------------------------------------
## Teniendo en cuenta el escenario planteado vamos a realizar un laboratorio que simule esta situación
## Laboratorio CI/CD con GitHub Actions (Java + Maven + SpringBoot)

### <u>Que vamos a hacer</u>: Configurar un flujo de integración continua (CI) con GitHub Actions para la aplicación Java con Maven que:
 - <font color="yellow">Compile el proyecto</font>
 - <font color="yellow">Ejecute los test unitarios con JUNIT </font>
 - <font color="yellow">Genere el archivo .jar </font>

 ## Paso 1 - Crear el proyecto
### Crear el proyecto en SpringBoot con Maven desde https://start.spring.io/ o descarga el .zip proporcionado
**Subilo a un repositorio en GitHub llamado mi-app-java.** por ejemplo

## Paso 2 - Crear workflow en GitHub Actions
Crea el archivo .github/workflows/ci.yml con el siguiente contenido:

```yaml
name: CI Java con Maven

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - name: Clonar repositorio
      uses: actions/checkout@v3

    - name: Configurar Java
      uses: actions/setup-java@v4
      with:
        distribution: 'temurin'
        java-version: '17'

    - name:  Construir con Maven
      run: mvn -B package --file pom.xml
```
