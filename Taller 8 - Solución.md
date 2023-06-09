# Taller 8

### Bases de Datos en la Nube - RDS

##### Analítica Computacional para la Toma de Decisiones

---

|     Nombres      |      Apellidos       |     Login     |  Codigo   |
| :--------------: | :------------------: | :-----------: | :-------: |
|     Santiago     | Gonzalez Montealegre | s.gonzalez35  | 202012274 |
| Juliana Carolina |  Cardenas Barragan   | jc.cardenasb1 | 202011683 |

---

---

## Pre-requisitos

---

1. Para esta sesión va a requerir una cuenta de Amazon Web Services - AWS. Para esto hay varias opciones:

   - Utilizar una cuenta propia ya creada.

   - Crear una cuenta nueva. En este caso recuerde que requiere ingresar datos de una tarjeta de crédito. Usaremos recursos disponibles en la capa gratuita (https://aws.amazon.com/free/), pero es posible que se generen algunos costos menores.

   - Usar la cuenta de acceso limitado enviada a su correo por el instructor.

1. Ingrese a su cuenta y familiarícese con la consola.

1. Asegúrese de que en la esquina superior derecha aparezca la región N. Virginia.

1. Nota: la entrega de este taller consiste en un reporte y unos archivos de soporte. Cree el archivo de su reporte como un documento de texto en el que pueda fácilmente incorporar capturas de pantalla, textos y similares. Puede ser un archivo de word, libre office, markdown, entre otros.

1. Nota 2: Los siguientes pasos deben ser realizados por cada miembro del grupo y el reporte debe incluir los soportes de todos. En la parte final se pide que cada uno se conecte a la base de datos lanzada por uno de sus compañeros.

---

**Nota:**
Para los screenshots se utilizará la notación

- `S:` Santiago González
- `J:` Juliana Cárdenas

---

## Lance una instancia de RDS: relational database service

---

### 1.

En la consola de AWS ubique el servicio RDS y vaya al Dashboard. De click en Bases de Datos Databases y en Crear base de datos.

![1680148015798](image/Taller8-Solución/1680148015798.png)

---

### 2.

Seleccione Creación estándar, PostgreSQL, versión 14. Seleccione ambiente de Capa gratuita.

![1680151109337](image/Taller8-Solución/1680151109337.png)

---

### 3.

En Configuración defina un nombre para la instancia de su base de datos usando su primer nombre.

- **S:**

  - Nombre Instancia: `santiago`

  ![1680151165480](image/Taller8-Solución/1680151165480.png)

- **J:**

  - Nombre Instancia: `Juliana`
    ![1680151210344](image/Taller8-Solución/1680151210344.png)

---

### 4.

El usuario maestro puede dejarlo como postgres.

---

### 5.

Defina una contraseña maestra que recuerde fácilmente.

- S: `santiago2003`

- J: `Juliana200228`

---

### 6.

Seleccione instancias con ráfagas y específicamente una `db.t3.micro`.

---

### 7.

Para almacenamiento seleccione un disco SSD de 20 GB. Permita autoscaling del almacenamiento.

---

### 8.

En Conectividad deje las opciones iniciales por defecto, hasta el grupo de subred.

---

### 9.

En Acceso Público marque sí.

---

### 10.

En Grupo de seguridad de VPC, cree uno nuevo, use su nombre.

- **S:**

  - Grupo Seguridad: `santiago`
    ![1680151486585](image/Taller8-Solución/1680151486585.png)

- **J:**
  - Grupo Seguridad: `Juliana Cardenas`
    ![1680151530129](image/Taller8-Solución/1680151530129.png)

---

### 11.

Deje sin preferencia la Zona de disponibilidad. En puerto (configuración adicional) de la base de datos deje el 5432 (por defecto).

---

### 12.

Permita la Autenticación con contraseña.

---

### 13.

Deje las demás opciones en sus valores por defecto hasta Supervisión.

---

### 14.

En Configuración adicional defina un nombre para la base de datos inicial. Puede usar por ejemplo postgres o un nombre de fácil recordación.

- **S:**

  - dbname: `postgres`
    ![1680151691088](image/Taller8-Solución/1680151691088.png)

- **J:**

  - dbname: `PrimeraBase`
    ![1680151673310](image/Taller8-Solución/1680151673310.png)

---

### 15.

Deje las demás opciones en sus valores por defecto.

---

### 16.

Como referencia, tome un pantallazo de los costos mensuales estimados e inclúyalo en su reporte.

![1680151625056](image/Taller8-Solución/1680151625056.png)

---

### 17.

En RDS, vaya a Bases de datos y cuando termine de crearse, identifique el endpoint y el puerto. Tome un pantallazo de la consola de RDS donde se vea claramente la base de datos con el endpoint e inclúyalo en su reporte.

- **S:**
  ![1680154612784](image/Taller8-Solución/1680154612784.png)

  - Endpoint: `santiago.cbcppu1qhnbp.us-east-1.rds.amazonaws.com`
  - Puerto: 5432
    ![1680152039306](image/Taller8-Solución/1680152039306.png)

- **J:**
  ![1680155097428](image/Taller8-Solución/1680155097428.png)
  - Endpoint: `juliana.cdrz11epbroh.us-east-1.rds.amazonaws.com/`
  - Puerto: 5432
    ![1680154717165](image/Taller8-Solución/1680154717165.png)

---

### 18.

Conéctese a su base de datos desde un script o cuaderno de python local y consulte las tablas disponibles en la base de datos. Necesitará un código de conexión como éste.

```py
import psycopg2
engine = psycopg2.connect(
    dbname="postgres",
    user="my_user_name",
    password="123123123",
    host="my-rds-instance-name.123456.us-east-1.rds.amazonaws.com",
    port='5432'
)
```

Tome un pantallazo e inclúyalo en su reporte.

- **S:**
  ![1680154161189](image/Taller8-Solución/1680154161189.png)

- **J:**
  ![1680154214582](image/Taller8-Solución/1680154214582.png)

No hay tablas disponibles

---

---

## Crear base de datos y alimentarla con un script

---

Los siguientes pasos se realizan desde una instancia EC2 para garantizar su ejecución. Puede realizar pasos similares desde su equipo local, de acuerdo con el sistema operativo y permisos que tenga.

---

### 1.

Lance una instancia t2.micro de EC2 con Amazon Linux.

- **S:**
  IP: `100.26.235.0`
  ![1680155046806](image/Taller8-Solución/1680155046806.png)
- **J:**
  IP: `18.209.211.96`
  ![1680154791795](image/Taller8-Solución/1680154791795.png)

---

### 2.

En su directorio home (`/home/ec2-user/`) cree una carpeta data con el comando

```shell
mkdir data
```

Conexión:

```shell
# ec2
ssh -i llave.pem ec2-user@ip
# ubuntu
ssh -i llave.pem ubuntu@ip
```

---

### 3.

Desde su equipo local copie el archivo world.sql en su instancia usando en comando

```shell
# ec2
scp -i llave.pem world.sql ec2-user@IP:/home/ec2-user/data
# ubuntu
scp -i llave.pem world.sql ubuntu@IP:/home/ubuntu/data
```

- **S:**
  ![1680155841518](image/Taller8-Solución/1680155841518.png)
  ![1680155858741](image/Taller8-Solución/1680155858741.png)

- **J:**
  ![1680155955129](image/Taller8-Solución/1680155955129.png)
  ![1680156000648](image/Taller8-Solución/1680156000648.png)

---

### 4.

Los siguientes pasos son desde la consola de la instancia Linux en EC2.

---

### 5.

Agregue el repositorio de PostgreSQL 13 disponible para el manejador de paquetes yum, usando el siguiente código en la consola de Linux

```shell
sudo tee /etc/yum.repos.d/pgdg.repo<<EOF
[pgdg14]
name=PostgreSQL 14 for RHEL/CentOS 7 - x86_64
baseurl=https://download.postgresql.org/pub/repos/yum/14/redhat/rhel-7-x86_64
enabled=1
gpgcheck=0
EOF
```

---

### 6.

Actualice los repositorios de paquetes con el comando

```shell
sudo yum update
```

---

### 7.

Ahora sí instale el client y servidor de PostgreSQL 13

```shell
sudo yum install postgresql13 postgresql13-server
```

```shell
sudo yum install postgresql14 postgresql14-server
```

---

### 8.

Usando este cliente puede conectarse a la BD en RDS con PSQL, así

```shell
# ubuntu
sudo apt-get update
sudo apt install postgresql-client-14
```

```shell
psql --host=endpoint.rds.amazonaws.com --port=5432 --username=postgres --password
```

modificando la dirección del endpoint, el puerto y el usuario por los adecuados, e ingresando su contraseña como la definió al crear la instancia de RDS.

**R/**
Se crean nuevas instancias

- **S:**
  ![1681237003398](image/Taller8-Solución/1681237003398.png)
  ![1681088234637](image/Taller8-Solución/1681088234637.png)

- **J:**
  ![1681237049819](image/Taller8-Solución/1681237049819.png)
  ![1681237063366](image/Taller8-Solución/1681237063366.png)

---

### 9.

En este momento debe estar conectado a la BD postgres (o la principal que haya creado) en su servidor RDS. Para crear una nueva base de datos de nombre world use el comando

```shell
CREATE DATABASE world WITH ENCODING 'LATIN1';
```

Por errores se cambia a `'UTF8'`

![1681088747724](image/Taller8-Solución/1681088747724.png)

---

### 10.

Conéctese a la base de datos con el comando

```shell
\c world
```

![1681088788067](image/Taller8-Solución/1681088788067.png)

---

### 11.

Ejecute el script .sql que sibió a la instancia con el comando

```shell
# ec2
\i '/home/ec2-user/data/world.sql'
# ubuntu
\i '/home/ubuntu/data/world.sql'
```

![1681089008858](image/Taller8-Solución/1681089008858.png)

---

### 12.

Liste las tablas disponibles en la base de datos con el comando

```shell
\dt
```

Tome un pantallazo e inclúyalo en su reporte.

![1681089032882](image/Taller8-Solución/1681089032882.png)

---

### 13.

Conéctese ahora desde un script local de python y ejecute algunas consultas. Tome un pantallazo e inclúyalo en su reporte.

- **S:**

```py
connection = psycopg2.connect(
    host="santiago.cbcppu1qhnbp.us-east-1.rds.amazonaws.com",
    port='5432',
    user="postgres",
    password="santiago2003",
    dbname="world",
)
```

- **J:**

```py
connection_partner = psycopg2.connect(
    host="juliana.cdrz11epbroh.us-east-1.rds.amazonaws.com",
    port='5432',
    user="postgres",
    password="Juliana200228",
    dbname="world",
)
```

![1681089269169](image/Taller8-Solución/1681089269169.png)
![1681089291636](image/Taller8-Solución/1681089291636.png)

---

### 14.

Conéctese ahora desde un script local de python a la base de datos de su compañero/a y ejecute algunas consultas. Tome un pantallazo e inclúyalo en su reporte.

- **S:**

![1681238722255](image/Taller8-Solución/1681238722255.png)
![1681238733741](image/Taller8-Solución/1681238733741.png)

- **J:**

![1681237463246](image/Taller8-Solución/1681237463246.png)
![1681237470515](image/Taller8-Solución/1681237470515.png)
![1681237478360](image/Taller8-Solución/1681237478360.png)

---

### 15.

En su entrega incluya el archivo .py o .ipynb usado en las conexiones.
