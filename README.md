# 🚀 Configuración del Servidor y Entorno VPS

## 🗄️ Bases de Datos y Configuración en Servidores

### 🌍 Configuración de un Dominio en Hostinger

Para asignar un dominio personalizado en Hostinger a Hestia Control Panel, sigue estos pasos:

#### 📌 1️⃣ Adquirir y Configurar el Dominio

[Ir a Hostinger](https://www.hostinger.com)

1. Inicia sesión en Hostinger.
2. Dirígete a la sección **Dominios** y selecciona **Registrar un nuevo dominio**.
3. Escribe el nombre del dominio deseado (ejemplo: `Miejemplo.com`).


   ![Ejemplo1](ImagenesBD/ejemplo1.png)

4. Finaliza el proceso de pago y activación (puede tardar hasta un día).

#### 2️⃣ Configuración de los Servidores de Nombres (DNS)

1. En el Panel de Control de Hostinger, accede a **Dominios → Administrar**.
2. En la sección **Nameservers (DNS)**, selecciona **Usar servidores de nombres personalizados**.
3. Configura los servidores DNS para apuntar a tu servidor en Oracle Cloud.
   
   **Ejemplo:**  
   ```
   ns1.midominio.com
   ns2.midominio.com
   ```
4. Guarda los cambios y espera la propagación del DNS (puede tardar algunas horas).

   ![Ejemplo2](ImagenesBD/ejemplo2.png)

---

### ⚙️ 3️⃣ Asignar el Dominio en Hestia Control Panel

Para reemplazar `localhost` con tu dominio en Hestia, sigue estos pasos:

1. Accede a tu servidor vía SSH como root:
   ```sh
   ssh root@<IP_DE_TU_SERVIDOR>
   ```

---

## 📌 Instalación de Hestia en VPS

En **Hestia**, debemos dar clic en las siguientes opciones que son necesarias para el dado de alta del entorno de nuestra página. 

![Hestia](ImagenesBD/hestia_opciones.PNG)

Una vez configuradas las opciones anteriores, se nos proporcionará un **comando SSH** que debemos ejecutar en nuestra instancia de **Oracle Cloud** que tengamos creada, este comado se ejecutara con permisos de `root` para instalar Hestia. 

![SSH](ImagenesBD/ssh.png)

💁‍♂️ [Panel de instalación de Hestia](https://hestiacp.com/install.html)  


Para instalar Hestia en tu servidor, copia el comando SSH proporcionado por el panel de instalación de Hestia y ejecútalo en la terminal con permisos de root.

### 🔗 Configuración de Dominio en Hestia

Ejecuta estos comandos como root para configurar tu dominio (`example.tech`):

```sh
cd /usr/local/hestia/bin/
v-change-sys-hostname example.tech
v-add-letsencrypt-host
```

---

## 📌 Verificación de Propagación DNS

Para asegurarte de que el dominio se propagó correctamente, utiliza la herramienta **DNS Propagator Checker**.

1. Accede a la herramienta en línea.
   -  [Herramienta para verificar la propagación de DNS](https://www.whatsmydns.net)  
3. Introduce tu dominio (ejemplo: `midominio.com`).
4. Selecciona el tipo de registro DNS que deseas verificar (ejemplo: `A`, `CNAME`, `MX`).
5. Haz clic en **Buscar** para ver el estado de la propagación.

✅ **Resultados:**
- 🟢 **Marca verde o IP visible**: La propagación fue exitosa.
- 🔴 **Marca roja o sin datos**: La propagación aún no ha finalizado.

---

## ⚙️ Instalación de Apache y PHP en Oracle Cloud

Para configurar un servidor web con Apache y PHP en Oracle Cloud sigue los pasos:

📖 **Referencia Oficial:** [Oracle Docs](https://docs.oracle.com/en-us/iaas/developer-tutorials/tutorials/apache-on-ubuntu/01oci-ubuntu-apache-summary.htm#set-up-apache-php)  

---

### 🔹 Acceder a la Instancia

1. Accede a [Oracle Cloud Console](https://cloud.oracle.com/) y ve a la parte del menu **Compute** > **Instances**.  
2. Selecciona la instancia que creaste anteriormente.  
3. En **Instance Details**, localiza la dirección **Public IP Address** y anótala.  

---

### 🔹 Conectar a la Instancia por SSH

Ejecuta el siguiente comando en la terminal:

```sh
ssh -i <tu-clave-privada.pem> ubuntu@<IP_DE_TU_INSTANCIA>
```

### 🔹 Instalación de Apache

1. Actualiza los paquetes e instala Apache:
   ```sh
   sudo apt update
   sudo apt -y install apache2
   ```
2. Inicia Apache y verifica que esté funcionando correctamente.

### 🔹 Configuración del Firewall

Ejecuta los siguientes comandos para permitir tráfico HTTP:

```sh
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT
sudo netfilter-persistent save
```
### 🔹 Prueba de Instalación

Para comprobar que la instalación fue exitosa, accede a tu dominio y verifica que se cargue el archivo `index.html`.

![Ejemplo3](ImagenesBD/ejemplo3.png)



---

## 🔹 Configuración de HTTPS en HestiaCP

Para mejorar la seguridad de tu sitio con HTTPS:

### ✅ Requisitos previos

- Un dominio registrado (Ejemplo: `midominio.com`).
- Un VPS con HestiaCP instalado.
- Acceso SSH como root.
- El dominio apuntando al servidor mediante DNS.

### 🛠️ Pasos para Configurar HTTPS

1️⃣ **Accede a HestiaCP**  
   - Ingresa a `https://<IP_DEL_SERVIDOR>:8083/` y accede como administrador.

2️⃣ **Agrega un Dominio**  
   - Ve a **Web > Añadir dominio**.
   - Ingresa tu dominio (`midominio.com`).
   - Activa **Soporte SSL** y **Usar Let’s Encrypt**.
   - Guarda los cambios.

3️⃣ **Configuración Manual del Certificado SSL (Opcional)**  
   Si Let’s Encrypt no funciona automáticamente, ejecuta en SSH:

   ```sh
   v-add-web-domain-ssl "usuario" midominio.com
   v-add-letsencrypt-domain "usuario" midominio.com
   ```

Con esto, tu dominio quedará configurado con HTTPS. 🔒✅


![work](ImagenesBD/work.png)



