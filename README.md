- ## Profesor: Willman Acosta

- ## Actividad: Desafío de Consultoría "Gobernanza Digital"

- Alumnos: Daniel Jiménez Ramírez, Pepe Gil Cué, Manuel Parrilla

---

## Bloque A: Análisis de Mercado y Selección (CE a, c)

Justifica la elección basándote en el perfil de la empresa (25 empleados, presupuesto ajustado, necesidad de personalización en el etiquetado).

Comparando la entrada de cada uno con sus respectivos precios, asegurando el descarte de SAP S/4HANA: Debido a que su coste de licencia para 25 usuarios ronda entre 90.000€

Zona one no es mala opción, debido a que tiene un precio alrededor de 40€ al mes por empleado, en su plan flexible. El problema principal aquí es la necesaria modificación de de los módulos de fabricación, algo que zoho no permite sin la contribución de sus desarrolladores externos con acceso restringido a cualquier usuario.

La mejor opción es Oddo community, es código abierto, sin coste de licencia, y con una comunidad enorme de módulos para el sector alimenticio permitiendo su personalización. Para 25 empleados es la mejor solución dentro de estas tres opciones.

**Cálculo de TCO:** Realiza una estimación a 3 años. No olvidéis incluir:

**Coste de licencias/suscripción.**

La licencia de Odoo es completamente gratuita

Odoo Community es licencia LGPL (Lo que significa que es una licencia que permite uso, copia y modificación)

**Coste de implantación (vuestras horas de desarrollo: estima 100h a 40€/h).** 

El coste de implantación sería de 100 hr x 40 \= 4000 euros

**Coste operativo (Hosting en Google Cloud, AWS, Huawei Cloud o similar).**

55 al mes durante 36 meses 1980

Coste de mantenimiento 800 por 3 años \= 2400

TCO TOTAL A 3 AÑOS \= 4000 \+ 1980 \+ 2400 \= 8380

## 

## Bloque B: Diseño de Seguridad RBAC (CE f)

Diseña la matriz de permisos para los siguientes roles, asegurando el **Principio de Mínimo Privilegio**:

* **Administrador:** Acceso total.  
* **Comercial:** Solo ve sus clientes y presupuestos (Record Rules).  
* **Operario de Almacén:** Solo ve stock y albaranes de entrada/salida.  
* **Contable:** Puede mirar facturas pero no puede modificar el stock.

## Bloque C: Documentación de Explotación (CE i)

Siguiendo la norma **ISO/IEC 26514**, redacta un breve **Manual de Despliegue** para que el responsable de IT de la empresa pueda levantar el sistema en caso de caída. Debe incluir:

Para el despliegue necesitaremos un editor de código y una carpeta con un nombre del proyecto dentro de esta carpeta crearemos un archivo llamado docker-compose.yml donde haremos distintos tipos de configuraciones.

1. El fragmento de *docker-compose.yml* necesario.

services:

  odoo:

    image: odoo:latest

    container\_name: odoo

    restart: unless-stopped

    depends\_on:

      \- db

    ports:

      \- "8200:8069"

    volumes:

      \- odoo-data:/var/lib/odoo

      \- ./config:/etc/odoo

      \- ./addons:/mnt/extra-addons

    environment:

      \- HOST=db

      \- USER=odoo

      \- PASSWORD=odoo

    command: odoo \-d odoo \--db\_user=odoo \--db\_password=odoo \-i base

  db:

      image: postgres:16.0

      container\_name: db

      restart: unless-stopped

      environment:

        \- POSTGRES\_USER=odoo

        \- POSTGRES\_PASSWORD=odoo

        \- POSTGRES\_DB=odoo

        \- PGDATA=/var/lib/postgresql/pgdata

      volumes:

        \- db-data:/var/lib/postgresql/data

volumes:

  odoo-data:

  db-data:

2. El comando para realizar un backup de la base de datos PostgreSQL.

pg\_dump \-U odoo \-d odoo \> backup.sql 

Usaremos la utilidad pg\_dump con la opción \-U indicamos el usuario de la base de datos en este caso es odoo como se indica en el archivo de docker con \-d indicamos el nombre de la base de datos y con \> indicamos que el resultado lo meta en un archivo llamado backup.sql
