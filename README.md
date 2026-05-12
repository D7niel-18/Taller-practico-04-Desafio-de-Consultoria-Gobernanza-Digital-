- ## Profesor: Willman Acosta
 
- ## Actividad: Desafío de Consultoría "Gobernanza Digital"

- Alumnos: Daniel Jiménez Ramírez, Pepe Gil Cué, Manuel Parrilla

---

## Bloque A: Análisis de Mercado y Selección (CE a, c)

Justifica la elección basándote en el perfil de la empresa (25 empleados, presupuesto ajustado, necesidad de personalización en el etiquetado).

| Criterio | SAP S/4HANA | Zoho One | Odoo Community |
| ----- | ----- | ----- | ----- |
| Coste de licencia | \~90.000 € (25 usuarios) | \~40 €/usuario/mes (plan flexible) | 0 € (código abierto, LGPL) |
| Personalización de módulos | Alta pero costosa | Limitada, requiere desarrolladores externos con acceso restringido | Total, comunidad enorme de módulos |
| Adaptación sector alimenticio | Sí, pero sobredimensionado | Parcial | Sí, módulos específicos disponibles |
| Adecuación para 25 empleados | No, pensado para grandes corporaciones | Aceptable | Óptima |
| Veredicto | Descartado | Descartado | Seleccionado |

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

Descripción Detallada de Roles y Justificación

**Administrador (Admin):**

Permisos: Control Total (CRUD, Administración de seguridad).

Justificación: Necesario para configurar, mantener y gestionar usuarios.

**Editor / Gestor:**

Permisos: Crear, Leer, Actualizar (CRU) en su área.

Justificación: Permite modificar contenido operativo pero no borrar registros históricos (salvo configuración específica) ni gestionar usuarios.

**Visor / Lector:**

Permisos: Leer (R) únicamente.

Justificación: Acceso necesario para consultar información sin riesgo de alterarla.

**Usuario Final:**

Permisos: Leer (R) en funciones limitadas y crear registros propios (ej. enviar formularios).

Justificación: Funcionalidad operativa pura sin acceso a datos sensibles o de configuración.

## Bloque C: Documentación de Explotación (CE i)

Siguiendo la norma **ISO/IEC 26514**, redacta un breve **Manual de Despliegue** para que el responsable de IT de la empresa pueda levantar el sistema en caso de caída. Debe incluir:

Para el despliegue necesitaremos un editor de código y una carpeta con un nombre del proyecto dentro de esta carpeta crearemos un archivo llamado docker-compose.yml donde haremos distintos tipos de configuraciones.

1. El fragmento de *docker-compose.yml* necesario.
```
services: # Define los contenedores
  odoo: 
    image: odoo:latest # Usa la imagen oficial de odoo
    container_name: odoo # Nombre del contenedor
    restart: unless-stopped # Esto hace que se reinicie automaticamente si se cae
    depends_on:
      - db # Espera a que el contenedor db este listo antes de arrancar
    ports:
      - "8200:8069" # 8069 es el interno de odoo y el 8200 es el del host
    volumes:
      - odoo-data:/var/lib/odoo
      - ./config:/etc/odoo
      - ./addons:/mnt/extra-addons
    environment:
      - HOST=db 
      - USER=daw1
      - PASSWORD=daw1
    command: odoo -d odoo --db_user=daw1 --db_password=daw1 -i base 
  db:
      image: postgres:16.0
      container_name: db
      restart: unless-stopped
      environment:
        - POSTGRES_USER=daw1
        - POSTGRES_PASSWORD=daw1
        - POSTGRES_DB=odoo
        - PGDATA=/var/lib/postgresql/pgdata
      volumes:
        - db-data:/var/lib/postgresql/data

volumes:
  odoo-data:
  db-data:
```
2. El comando para realizar un backup de la base de datos PostgreSQL.

pg\_dump \-U daw1 \-d daw1 \> backup.sql 

Usaremos la utilidad pg\_dump con la opción \-U indicamos el usuario de la base de datos en este caso es odoo como se indica en el archivo de docker con \-d indicamos el nombre de la base de datos y con \> indicamos que el resultado lo meta en un archivo llamado backup.sql
