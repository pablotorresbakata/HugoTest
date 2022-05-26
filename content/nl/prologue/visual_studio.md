---
title: "Visual Studio Code"
description: "Guide and description of visual studio"
lead: "One page summary of how to start a new Doks project."
date: 2020-11-16T13:59:39+01:00
lastmod: 2020-11-16T13:59:39+01:00
draft: false
images: []
menu:
  docs:
    parent: "prologue"
weight: 0
toc: true
---



## Introduccion

Se trata de un ide (entorno de desarrollo integrado) que nos permite desarrollar código en diferentes lenguajes (principalmente html, css y javascript, aunque es posible aumentar los lenguajes posibles mediante el uso de *extensiones*) además de depurarlo, ejecutarlo, compilarlo, usar el control de version git etc... Además cuenta con útiles herramientas de autocompletado y sugerencia de código.

Para instalar el programa en windows es muy fácil. Solo tenemos que ir a su página web https://code.visualstudio.com/ y darle click en el botón de install for windows. Después abriremos el instalador y seguiremos los típicos paso de un wizard de instalación de Windows.





## El programa



Esta es la pantalla principal del programa. Podemos crear un nuevo archivo y abrir un proyecto ya empezado. En carpetas de proyectos podremos guardar un tipo de archivo denominado *Workspace* usando la opcion *Save Workspace as*, que se trata de un archivo de configuración del programa. Si no guardamos el archivo esté tendra de nombre Untittled Workspace. Algo muy útil es añadir carpetas al Workspace, para que así tengan la misma configuración que hemos guardado en el Workspace. Para esto, utilizar la opción *Add to Workspace*


### Editor

A la hora de programar, podremos ver que las palabras, dependiendo de que tipo sean dentro del lenguaje de programación, estarán escritas con un color u otro. Otra cosa útil es el autocompletado de variables y metodos: si empezamos a escribir, veremos que nos saltarán sugerencias de códigos que ya hemos implementado o de metodos accesibles para el objeto que estemos editando.


### Debug

Visual Studio Code permite hacer la depuración del código. Para esto, iremos a la barra de herramientas y le daremos al icono de Debug, le daremos a Run and Debug. Nos aprecerá una terminal de debug, donde se nos habrá ejecutado el programa. Lo que pasa es que se nos habrá ejecutado el programa entero. Si queremos que el programa se pare en determinada instrucción, hacemos click en el número de línea de esa instruccion. Cuando se paré, podremos elegir varias opciones de ejecución.



Una práctica común al depurar es mostrar el contenido de las variables en la terminal para ver si este es el esperado. Esto puede ser algo tedioso y poco práctico. En este programa, podremos ver el contenido de las variables mientras depuramos. El menu de las variables se ve a la izquierda.



En el programa, solo podremos hacer debug de códigos escritos en jascript y asociados a este. Si queremos depurar otros programas tendremos que instalar extensiones de depuración.




## Terminales

El programa viene con una terminal integrada en la raiz de nuestro Workspace. Con esta terminal podremos ejecutar comandos básicos que necesitemos. Para abrirla podemos hacer *View > Terminal*, lo que nos abrirá la terminal. Además podremos tener varias terminales abiertas, si le damos al botón + abriremos una. Si le damos a *Split Terminal* podemos abrir las otras terminales en la misma pantalla, para consultar varias terminales a la vez.




## Extensiones

Visual Studio Code permite la creación e instalación de diferentes extensiones que permiten añadir aun más herramientas y opciones a este. Para empezar a buscar extensiones hacemos click en el icono de extensiones en la barra a la izquierda.



Hay muchos tipos de extensiones: extensiones de depuración, extensiones de atajos, extensiones para el uso de servers...


## Remote SSH

Se trata de una herramienta que nos permitirá editar código conectado a una máquina Ubuntu remota usando ssh. Esto nos permitirá editar código en la máquina del iaas, lo que nos facilitirá muchos las cosas. Primero nos buscamos la extension Remote -ssh y la instalamos.

 

 Al instalarla, nos aperecerá un icono más en la barra de la izquierda. Le damos y nos llevará al menu de la extensión. Si hemos configurado la máquina remota y la máquina local para tener acceso a estas, nos saldrá el nombre que le hemos dado a la máquina en la pestaña de SSH targets. A la izquierda habra un icono que nos permitirá conectarnos a la máquina. Al darle click, nos saldrá una pestaña nueva con opciones para crear un nuevo archivo, abrir una carpeta etc... A partir de esto, ya podremos interactuar con nuestra máquina virtual desde el programa.

 


## Live Share

 Se trata de una herramienta para colaborar con varias personas en la programación de código. Para los que han usado herramientas como Google Drive, esto les parecerá algo similar. Nos permite que varias personas programen un mismo proyecto en tiempo real, sin necesidad de liarse con configuraciones dificiles.

 Primero tendremos que instalar la extensión *Live Share*:

 


 Para empezar tenemos que estar loggeados con nuestra cuenta de microsoft. Al hacer esto, veremos que en la parte de abajo tenemos un botón que pone *Start Share* Esto nos dará una URL que podremos pasar a las personas con las que queremos colaborar. Ellos tendrán que abrir su Visual Studio Code y en el menu de la extension darle a *Join Collaboration Session*, pegar el link que les pasamos y darle a Join. De esa forma, ellos se conectarán a nuestro Workspace desde su terminal y podrán ver los directorios y ficheros del proyecto.

 De esta forma, se podrá colaborar en la programación de código, usando características como:

 - Compartir una sesion de Debug.
 - Que las otras personas vean donde tenemos nuestro cursor y el texto que hemos seleccionado.
 - Modificar código en tiempo real.
 - Ver quien ha cambiado que en el código.

# GITPOD

Gitpod permite editar un repositorio de github desde un editor que se encuentra en la nube. También se puede usar Visual Studio Code

Para usar gitpod solo hay que instalar la extensión del navegador que usemos y dar click al botón gitpod en el repositorio que queramos usar.

 
 
 Como se puede ver, el environment que ofrece Gitpod es muy similar al de visual studio code


```

def eliminarFilasDuplicadas(self):
        # df = pd.read_csv('result.csv')
        # df.columns = df.iloc[0]
        # df.drop_duplicates(inplace=True)
        # df.to_csv(f'result{self.inicial_string}-pandas.csv', index=False)

        with open('result.csv', 'r', encoding="utf-8") as in_file, open(f'result{self.inicial_string}.csv', 'w', encoding="utf-8") as out_file:
            seen = set() # set for fast O(1) amortized lookup
            for line in in_file:
                if line in seen: continue # skip duplicate

                seen.add(line)
                out_file.write(line)
        pass

```