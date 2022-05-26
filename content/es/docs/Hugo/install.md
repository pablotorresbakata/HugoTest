---
title: "Instalar"
description: "Como instalar Hugo y la plantilla Doks"
lead: "Esta docuemtación está desarrollada con la herramienta Hugo usando el plugin Doks (el cual esta 
desarrollado en node). A continuación se explicará los primeros pasos necesarios para instalar todos lo
necesario para desarrollar una página usando Doks"
date: 2022-05-230T08:48:57+00:00
lastmod: 2022-05-230T08:48:57+00:00
draft: false
images: []

weight: 1
toc: true
---



## Requerimientos

### Git

Primeramente tendremos que instalar git para descargar el tema. [Git para windows](http://git-scm.com/download/win)

{{< details "Repaso sobre git" >}}
Si quieres recordar como funciona git y sus comandos visita [el quickstart de git](https://docs.github.com/es/get-started/quickstart/fork-a-repo)
{{< /details >}}

A partir de esto, instalamos el tema en una carpeta con el siguiente comando:

```

git clone https://github.com/h-enk/doks-child-theme.git my-doks-site && cd my-doks-site

```


### Nodejs y npm

Nodejs permite ejecutar código javascript de manera local. A su vez, npm es un administrador de instalaciones de nodejs, con capacidad para manejar e instalar módulos. Se instala en el siguiente [enlace](https://nodejs.org/en/)

Esto incluría los comandos **node** y **npm** en nuestra consola. 

A continuación ejecutamos el comando ``` npm install ```. El cual instalará todos los modules necesarios para el desarrollo. 

## Prueba de funcionamiento

Para probar el funcionamiento de Doks podemos hacer un host de la página. Esto usará los archivos markdown y la configuración de prueba que viene en el repositorio que hemos descargado y lo alojará en una página web. 

El comando a ejecutar es: ```npm run start```. Al terminar de ejecutarse alojará una página web en esta [dirección](http://localhost:1313)
