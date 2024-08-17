# Model Branching

El modelo de branching o ramificación es una estrategia en Git que define cómo se crean, se usan y se combinan las ramas en un proyecto.

## Git Flow

Git Flow es una de las metodologías de branching más populares en Git. Proporciona un marco de trabajo para administrar proyectos más grandes con varias versiones y características en desarrollo simultáneamente. Git Flow utiliza una estructura de ramas consistente que incluye

## Branches Principales:

- ```main```: Contiene el código de producción. Este branch siempre debe estar en un estado estable y listo para ser desplegado.
- ```develop```: Contiene el código más reciente para el próximo release. Aquí es donde se integran todas las nuevas funcionalidades y correcciones antes de ser promovidas a ```main```.

## Branches de Soporte:

- ```feature/*```: Para el desarrollo de nuevas funcionalidades. Cada nueva funcionalidades tiene su propio branch.
- ```release/*```: Para preparar nuevas versiones. Una vez que ```develop``` está listo para un nuevo release, se crea un branch de release para finalizar el trabajo antes de fusionarlo con ```main``` y ```develop```.
- ```hotfix/*```: Para correcciones críticas en ```main```. Los hotfixes se aplican directamente a ```main``` y luego se fusionan de nuevo en ```develop```.
- ```fix/*```: Para corrección de errors. Cada nuevo error encontrado si su propio branch.

 ## Convenciones de Merge y Commit

- ```Merge Request Contexts```: develop, test, master.
- ```Naming Convention```: ``` <type>/<issue_number>_<alias> ```.
- ```Commit Conventions```: Basado en convenciones comunes, por ejemplo:

```commit -m "<type>[optional scope]: <description>"```

## Ejemplos de **type**:
 
- ```feat```: Nueva característica.
- ```fix```: Corrección de errores.
- ```docs```: Cambios en la documentación.
- ```style```: Cambios de formato.
- ```refactor```: Refactorización de código.
- ```test```: Añadir o corregir pruebas.

```mermaid
%%{init: { 'logLevel': 'debug', 'theme': 'dark', 'gitGraph': {'showBranches': true, 'showCommitLabel':true,'mainBranchName': 'master'}} }%%
gitGraph
   commit id: "Tag 0.1" tag: "0.1"
   branch develop
   commit id: "Inicio de develop"
   
   branch feature/major-feature
   commit id: "Major feature for next release"
   commit id: "More feature work"
   
   checkout develop
   merge feature/major-feature id: "Incorporate feature into develop"
   
   branch release/1.0
   commit id: "Start of release branch for 1.0"
   
   branch hotfix/0.2
   commit id: "Severe bug fixed for production: hotfix 0.2"
   checkout master
   merge hotfix/0.2 id: "Merge hotfix 0.2 to master"
   commit id: "Tag 0.2" tag: "0.2"
   
   checkout develop
   merge hotfix/0.2 id: "Incorporate hotfix 0.2 into develop"
   
   checkout release/1.0
   commit id: "Only bugfixes in release branch"
   
   checkout develop
   merge release/1.0 id: "Merge release branch fixes into develop"
   
   checkout master
   merge release/1.0 id: "Release 1.0"
   commit id: "Tag 1.0" tag: "1.0"

```


## Implementación del Modelo de Branching##

Basado en la información y ejemplos proporcionados, aquí está un modelo detallado de branching para el equipo:

1. Branches Principales
   - ```master```: Código listo para producción.
   - ```develop```: Código para la próxima versión.

2. Branches de Soporte
 
  - ```feature/*```: Nuevas funcionalidades.
  - ```fix/*```: Correcciones de errores.
  - ```release/*```: Preparación de versiones.
  - ```hotfix/*```: Correcciones críticas en producción.

3. Flujo de Trabajo
 
    1. Crear una nueva funcionalidad o corrección:   
    ```shell
        git checkout develop
        git pull origin develop
        git checkout -b feature/123_mi-nueva-caracteristica
        git checkout -b fix/435_mi-correccion
    ```
    2. Trabajar y hacer commits siguiendo las convenciones:
    ```shell
        commit -m "feat: Añadida nueva funcionalidad X"
        # o
        commit -m "fix: Corregido error en el módulo Y" 
    ```    

    3. Trabajar y hacer commits siguiendo las convenciones:
    ```shell
        git checkout develop
        git pull origin develop
        git merge --no-ff feature/123_mi-nueva-caracteristica
        git push origin develop
        # o
        git merge --no-ff fix/435_mi-correccion
        git push origin develop
    ```   

    4. Preparar y finalizar un release:
    ```shell
        git checkout develop
        git pull origin develop
        git checkout -b release/v1.0.0

        # Realizar pruebas y ajustes necesarios

        git checkout test
        git pull origin test
        git merge --no-ff release/v1.0.0
        git push origin test

        git checkout master
        git pull origin master
        git merge --no-ff release/v1.0.0
        git tag -a v1.0.0 -m "Release v1.0.0"
        git push origin master
        git push origin --tags

        git checkout develop
        git pull origin develop
        git merge --no-ff release/v1.0.0
        git push origin develop
    ```    

    
    5. Aplicar un hotfix:
    ```shell
        git checkout master
        git pull origin master
        git checkout -b hotfix/v1.0.1

        # Realizar corrección

        git checkout master
        git pull origin master
        git merge --no-ff hotfix/v1.0.1
        git tag -a v1.0.1 -m "Hotfix v1.0.1"
        git push origin master
        git push origin --tags

        git checkout develop
        git pull origin develop
        git merge --no-ff hotfix/v1.0.1
        git push origin develop
    ```   

## Convenciones de Commits##

Utiliza convenciones estandarizadas para los mensajes de commit:

```commit -m "<type>[optional scope]: <description>"```

# Ejemplos de type:

   - ```feat```: Nueva característica.
   - ```fix```: Corrección de errores.
   - ```docs```: Cambios en la documentación.
   - ```style```: Cambios de formato.
   - ```refactor```: Refactorización de código.
   - ```test```: Añadir o corregir pruebas.

## Ejemplo Práctico: Creación de una Rama de Feature

Supongamos que estamos trabajando en un proyecto llamado mi-proyecto y necesitamos añadir una nueva funcionalidad llamada "autenticación de usuarios". A continuación se detallan los pasos:

1. ## Clonar el repositorio y navegar al proyecto

    Si aún no tienes el repositorio clonado, hazlo con el siguiente comando:

    ```shell
       git clone https://github.com/usuario/mi-proyecto.git 
       cd mi-proyecto
    ```
2. ## Crear una nueva rama de feature
   
    Primero, asegúrate de estar en la rama develop y de tener la última versión:

    ```shell 
       git checkout develop
       git pull origin develop
    ```

