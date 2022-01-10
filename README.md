<p align="center">
    <a href="https://github.com/sarc4/magento242" target="_blank">
        <img src="logo.png" alt="Magento242" width=320>
    </a>
<div align="center">
  <p><h3>Resumen de anotaciones</h3>
</div>

## Contenido

- Magento 2.4.2 Info
    - [Magento System requirements](https://devdocs.magento.com/guides/v2.4/install-gde/system-requirements.html)
    - [Magento Install Flow](https://devdocs.magento.com/guides/v2.4/install-gde/install-flow-diagram.html)
    - [Magento Api Keys](https://marketplace.magento.com/customer/accessKeys/)
    - [Mark Shust's Docker Configuration for Magento](https://github.com/markshust/docker-magento)
    - [Set Up a Magento 2 Development Environment with docker](https://courses.m.academy/courses/set-up-magento-2-development-environment-docker/lectures/9205849)
- Conceptos
    - [Modulo](#modulo)
    - [Plugin](#plugin)
    - [Preference](#preference)
    - [Plugin vs Preference](#plugin-vs-preference)
    - [Observer](#observer)
    - [Interface](#interface)
    - [Cron](#cron)
- [Docker](#docker) 
- [Instalación](#instalación)
- [Comandos](#Comandos)
- [Guias](#guias)
    - [Crear un modulo](#crear-un-modulo)
    - [Crear un plugin](#crear-un-plugin)
    - [Crear un tema](#crear-un-tema)
    - [Crear un cron](#crear-un-cron)
    - [Crear un usuario admin](#crear-un-usuario-admin)
- [Herramientas](#herramientas)
- [Errores](#errores)

<hr>

## Modulo
Los módulos interactúan con otras partes de la aplicación para cumplir una función particular o proporcionar una función. 
Un módulo puede contener una interfaz de usuario para mostrar información o interactuar con el usuario. 
También puede contener interfaces de aplicaciones que otro módulo de Magento o fragmento de código podría llamar.  

**El concepto del módulo es el concepto principal del desarrollo de extensión de Magento**, 
y el diseño modular de los componentes de software (en particular, módulos, temas y paquetes de idiomas) es un principio de arquitectura básico de Magento. Si un módulo es autónomo, puede modificarlo o reemplazarlo sin afectar otras áreas del código.
Este acoplamiento flexible de los componentes de software reduce los efectos dominantes en la base de código de código variable.  

## Plugin  
Los plugins, al igual que los módulos, **son un mecanismo para agregar funciones al producto central de Magento**. 
Los complementos le permiten realizar cambios en el comportamiento de cualquier método público en una clase de Magento. 
Puede considerarlo como una forma de extensión que utiliza la clase Plugin.  

Un plugin (o también conocido como **interceptor**) nos permitirá interceptar una función de una clase, haciendo que un código se ejecute “antes” (before), “después” (after) o “antes y después” (around).

## Preference  
Un **preference** nos permitirá sobreescribir una clase al completo. Es decir, nuestra clase heredará de la clase que queramos sobreescribir, y prevalecerá la nuestra a la hora de ejecutarse la misma.  

## Plugin vs Preference  
Evitaremos usar el preference en la medida de lo posible e intentaremos usar los plugins siempre que podamos. Pero, ¿Por qué?

En el caso de los plugin respetaremos las funciones originales y nuestro código no afectará al código original. Sin embargo, en el caso de los preference estamos machacando todo el código de la función. El problema de esto es que, por ejemplo, una actualización de Magento que cambie el código de este método, hará que nos perdamos estas nuevas cosas que se añadan, ya que el método de nuestra clase se ejecutará por encima del código original.

El problema de crear plugins es que tiene unas cuantas limitaciones.  
No se podrán crear plugins sobre lo siguiente:

- Métodos no públicos.
- Clases “final”
- Métodos “final”
- Métodos __construct y __destruct
- Métodos estáticos
- Virtual types

También se tendrá que dar el caso de que nuestra lógica sea imposible implementarla a través de un plugin, e implique cambios en varias líneas de código del método original.

## Observer  
Los Observers son clases que pueden afectar el comportamiento general, la performance o la lógica del negocio; y se ejecutan cuando el evento para el cual fueron configurados a escuchar, es **disparado**.

## Interface  
Una interfaz (POO) es un conjunto de métodos abstractos y de constantes cuya funcionalidad es la de determinar el funcionamiento de una clase, es decir, funciona como un molde o como una plantilla.  
Al ser sus métodos abstractos estos no tiene funcionalidad alguna, sólo se definen su tipo, argumento y tipo de retorno.

## Cron  
### ¿Qué es Cron? (Unix)  
Cron es un proceso de Linux que se ejecuta en segundo plano nada más arrancar el sistema. Su función es comprobar si existe alguna tarea (job) que ejecutar en la hora configurada en el sistema. Puede variar según la distribución que utilices, pero lo habitual es que Cron se inicie en las carpetas /etc/rc.d/ o /etc/init.d y que cada minuto realice una comprobación en los ficheros /etc/crontab o /var/spool/cron para buscar las ejecuciones.

Para que el Cron pueda ejecutarse en el momento adecuado, es fundamental que la zona horaria del sistema esté correctamente configurada. De otra forma, se ejecutará en otro momento y puede ocasionar ciertos problemas.

### ¿Qué es Crontab?  
Crontab no es más que un archivo de texto que contiene un listado de todos los scripts a ejecutar y cuándo tiene que realizarse esta tarea. Cada usuario del sistema tendrá su propio archivo Crontab ubicado en la carpeta etc y propiedad del usuario root. Para generar este archivo el usuario es necesario ejecutar el comando crontab.

**Muchas de las características de Magento2 requieren al menos una tarea programada.**

Estas tareas pueden encargarse de reglas de precios, newsletters, sitemaps, actualización de precios y stocks, reindexación, etc …  
Se recomienda que las tareas programadas las ejecute el propietario del sistema de ficheros de Magento por lo que no se recomienda su ejecución como usuario root.

<hr>

## Comandos
- ``bin/magento cache:clean``  
Borra todo el caché que utiliza Magento. Los cachés deshabilitados no serán afectados.

- ``bin/magento cache:flush``  
Borra todo el caché, incluidos aquellos compartidos entre distintos procesos.

- ``bin/magento setup:upgrade``  
Upgrades the Magento application, DB data, and schema.

- ``bin/magento se:di:compile``  
 Generates the contents of the var/di folder in Magento <2.2 and generated for Magento >= 2.2  
 According to Magento, ``se:di:compile`` serves the following purpose:

    - Application code generation (factories, proxies, and so on)
    - Area configuration aggregation (that is, optimized dependency injection configurations per area)
    - Interceptor generation (that is, optimized code generation of interceptors)
    - Interception cache generation
    - Repositories code generation (that is, generated code for APIs)
    - Service data attributes generation (that is, generated extension classes for data objects).  

- ``bin/magento indexer:reindex``  
When the data changes, the transformed data must be updated or reindexed. With the support of reindexing, Magento 2 store owners can **quickly and correctly change the store data**, reduce the waiting time of customers, and increase the conversion rate.  

- ``bin/magento module:status``  
List and display status of modules  
``module:disable`` para deshabilitar un modulo  
``module:enable`` para habilitar un modulo  

<hr>

## Herramientas
- n98-Magerun: [repo](https://github.com/netz98/n98-magerun2)
    - Copiamos el archivo "n98-magerun2-dev.phar" a docker/web/php
    - Agregamos la linea
    ``COPY ./php/n98-magerun2-dev.phar /usr/local/bin/n98``
    al archivo docker/web/Dockerfile
    - ```docker-compose build```
    - ```bin/magento setup:upgrade```
    - ```restart containers```
    - ```n98 (para ver los cmd)```

<hr>

## Errores
- ``'php\r': No such file or directory``
```$ cd bin
$ tr -d '\015' <magento >magento.new
$ mv magento magento.old
$ mv magento.new magento
```