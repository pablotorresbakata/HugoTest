---
title: "Api de Odoo"
date: 2020-11-16T13:59:39+01:00
lastmod: 2020-11-16T13:59:39+01:00
draft: false
images: []
menu:
  docs:
    parent: "Odoo"
weight: 0
toc: true
---


## Introducción

¿Para que sirve la API de Odoo?

Podemos acceder a los datos de Odoo desde cualquiera de los módulos
internos, pero a veces interesa poder acceder a esta información desde fuera.
Es aquí donde entra la API de Odoo.
La API nos da acceso a todos los modelos de Odoo, así como sus vistas y sus
campos. Para poder obtener toda esta información usaremos XML-RPC, un
protocolo de llamada a procedimiento remoto que utiliza HTTP para sus
operaciones de transmisión de mensajes, los cuales están codificados con
XML.

## Funcionamiento

### Configuración

Antes de todo tendremos que configurar el acceso a la API. Esto se hace de
la siguiente manera:

```

url = 'http://192.168.1.xx:8069/'
db = 'nombre_base'
username = ''
password = 'contraseña'

```

- url: la url de nuestro servidor de Odoo. En nuestro caso, sería la url de
la máquina virtual más el puerto 8069
- db: el nombre de la base de datos
- username: el usuario con el que accederemos a los datos, nos
interesa que sea el administrador para que pueda acceder a la mayor
cantidad de datos
- password: la contraseña de dicho usuario. A partir de Odoo 14 se
puede generar una clave de la api para evitar escribir la contraseña.
Esta clave se pondría en este mismo campo, sustituyendo a la
contraseña.

### Generar una clave para la api

Si estas en Odoo 14 o superior podemos generar una clave como se
mencionó anteriormente. Para eso hacemos clic en la esquina superior
derecha de la interfaz de Odoo (donde se encuentra nuestro nombre de
usuario y la foto). A continuación, accedemos a Mi perfil, donde
seleccionaremos Seguridad de la cuenta y por último haremos clic en
Nueva clave de API.

### Consulta de datos que no requieren de autentificación

Es posible acceder a algunos datos sin autenticarse con la api. Por ejemplo,
podemos consultar que versión de Odoo se esta utilizando.
En primer lugar, usaremos el protocolo XML-RPC usando la siguiente línea:

```
import xmlrpc.client
```

De esta forma estaremos importando todo lo necesario para consultar datos
con la api.

A partir de esto, mostraremos la información de versión de la siguiente
forma:
```
common =
xmlrpc.client.ServerProxy('{}/xmlrpc/2/common'.format(url))
print(common.version())
```
Con el método xmlrpc.client.ServerProxy accederemos a la base de datos
usando la url que hayamos especificado. El “common” dentro de este método
determina que solo queremos acceder a datos que no necesitan de
autentificación.

### Como autentificarse
La mayoría de los datos de la base de datos requieren de autentificación.
Para autentificarse usaremos el siguiente código:

```

common =
xmlrpc.client.ServerProxy('{}/xmlrpc/2/common'.format(url))
uid = common.authenticate(db, username, password, {})
models =
xmlrpc.client.ServerProxy('{}/xmlrpc/2/object'.format(url))

```


Como se puede ver, se están utilizando los datos que especificamos antes
(nombre de la base de datos, usuario, contraseña y url).

Una vez hayamos puesto estás líneas y no haya fallos en la información
proporcionada podremos consultar información de la base de datos.

### Consultar información (método search y read)
El método search nos permitirá obtener los ids de las entradas que
queramos de un modelo específico. Por ejemplo, queremos obtener todos
los contactos que sean empresas.
El modelo de la aplicación de contactos dentro de Odoo es res.partner. Para
filtrar entradas se usan los campos. Estos campos son básicamente las
características que tiene cada entrada y dependen del modelo. Para mirar
los campos que tiene cada modelo accederemos a la lista de modelos que se
encuentra en la sección Técnico dentro de los ajustes. Dentro de res.partner
podemos observar que existe un campo booleano llamado is_company que
determina si el contacto se trata de una compañía o no.
Para obtener los id de los contactos utilizamos esta línea:

```
print(models.execute_kw(db, uid, password,
'res.partner', 'search', [[['is_company', '=',
True]]]))
```

El método execute_kw lo utilizaremos para ejecutar los diversos métodos
que proporciona la api (en este caso search). A search se le pasan los
siguientes parámetros:

- db, uid, password: la información que especificamos al principio del
programa
- res.partner: el modelo de donde se sacará la información
- search: el método de la api que se va a ejecutar
- [[[‘is_company’, ‘=’, True]]]: la condición que tienen que cumplir las
entradas que queremos obtener, es decir, el filtro. En este caso
queremos obtener las entradas que tengan el campo is_company a
True.

Esto nos devolverá un array con los ids de las entradas que cumplan el filtro.
Para obtener los proyectos que estén activos, utilizaríamos lo siguiente:

```

print(models.execute_kw(db, uid, password,
'project.project', 'search', [[['active', '=',
True]]]))

```


Como podemos ver, se ha cambiado el filtro y el modelo que usamos, que en
este caso es project.project.
Para mostrar más información de las entradas tendremos que usar el
método read. Al método **read** le pasaremos los ids obtenidos con el método
search.

```

ids = models.execute_kw(db, uid, password, 'project.project', 'search', [[['active', '=', True]]])
record = models.execute_kw(db, uid, password,
'project.project', 'read', [ids], {'fields': ['name']})
print(record)

```

Como se puede ver, se están guardando los ids obtenidos en la ejecución del
método search y se le están pasando al método read. A parte de esto, se le
tendrán que pasar al read los campos que quieres que se muestren. En este
caso queremos que se muestre el nombre del proyecto, por lo que lo
añadimos dentro del array.
Como curiosidad, a parte del nombre también se mostrará el id siempre
independientemente de que no lo especifiquemos dentro de la sentencia
fields.
La forma más óptima de hacer esto es el método **search_read**, el cual
ejecutará automáticamente los métodos search y read.


```
models.execute_kw(db, uid, password, 'project.project',
'search_read', [[['active', '=', True]]], {'fields':
['name']})
```

Su ejecución es muy similar a ejecutarlos de forma separada.
También podemos obtener todos los campos de un modelo de la siguiente
manera:

```
models.execute_kw(db, uid, password, 'res.partner', 'fields_get', [],
{'attributes': ['string', 'help', 'type']})
```

Esto nos devolverá varias entradas que seguirán el siguiente formato:

```
"ean13": {
"type": "char",
"help": "BarCode",
"string": "EAN13"
},
```

Siendo type el tipo de campos y help una pequeña ayuda para entender para
que se usa el campo.

### Crear entradas (método create)
Este método es muy similar a los anteriores:

```

id = models.execute_kw(db, uid, password,
'project.project', 'create', [{'name': "Nuevo
Proyecto"}])

```

La única diferencia es que hay que especificarle los valores de los campos
que tendrá la nueva entrada. En este caso solo especificamos el nombre pero
si quisiésemos que tuviese más campos la entradas solo habría que añadirlos.

La ejecución de este método devuelve el id de la nueva entrada creada.

### Borrar entradas (método unlink)
A este método hay que pasarle el id de la entrada que queramos eliminar, en
este caso podemos eliminar la que creamos en el apartado anterior:

```

models.execute_kw(db, uid, password, 'project.project',
'unlink', [[id]])

```

Si quisiésemos eliminar todas las entradas resultantes de una búsqueda,
hacemos lo siguiente:

```

id = models.execute_kw(db, uid, password,
'project.project', 'search', [[['name', '=', "Nuevo
Proyecto"]]])
models.execute_kw(db, uid, password, 'project.project',
'unlink', [id])

```

Si comparamos los dos ejemplos, hay una diferencia significativa en el
método unlink, la forma en la que se pasan las entradas a eliminar.
En el primer ejemplo, se le pasan como [[id]] ya que habíamos obtenido el
valor id del método create. Este método devuelve un valor numérico que
representa el id de la entrada. Sin embargo, en el siguiente ejemplo el valor
de la variable id se obtiene del método search, que devuelve un array con los
resultados de la búsqueda, independientemente de si esta solo ha
encontrado una entrada.
Esto explica la diferencia en la ejecución de los métodos, ya que a unlink hay
que pasarle un array de arrays de entradas.

### Modificar entradas (método write)
Este método es muy similar a unlink:

```

models.execute_kw(db, uid, password, 'project.project',
'write', [[id], {'name': "Newer partner"}])

```

Al write hay que pasarle el id de la entrada y la información que se quiere
modificar. Esta información consistirá en el nombre del campo y un valor.
