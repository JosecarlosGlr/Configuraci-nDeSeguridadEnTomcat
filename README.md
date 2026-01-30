# Configuración de seguridad en Tomcat

## 1. Definición de Roles y Usuarios
Para controlar quién puede acceder a las consolas de administración, he editado el archivo `conf/tomcat-users.xml`. En él, he definido los roles necesarios para la gestión y he creado un usuario con permisos específicos.

* **Roles añadidos:** He incluido `manager-gui` (acceso a la interfaz de gestión de apps) y `admin-gui` (acceso al gestor de hosts).
* **Usuario:** He configurado el usuario "admin" con la contraseña "admin123" y le he asignado ambos roles para tener control total desde la web.

![](https://raw.githubusercontent.com/JosecarlosGlr/Configuraci-nDeSeguridadEnTomcat/refs/heads/main/1.png)

---

## 2. Restricción de Acceso al Manager
Una vez guardada la configuración y reiniciado el servicio, he navegado a la ruta `/manager/html`. Al intentar acceder, el navegador ha bloqueado la entrada solicitando las credenciales que configuré anteriormente.

![](https://raw.githubusercontent.com/JosecarlosGlr/Configuraci-nDeSeguridadEnTomcat/refs/heads/main/2.png)

Tras introducir el usuario "admin", he podido acceder al panel de control. En la captura se observa que tengo acceso total para listar, arrancar o replegar aplicaciones.

![](https://raw.githubusercontent.com/JosecarlosGlr/Configuraci-nDeSeguridadEnTomcat/refs/heads/main/3.png)

---

## 3. Configuración de HTTPS y Conector SSL
Para asegurar que la información no viaje en texto plano, he configurado el protocolo seguro HTTPS.

### 3.1. Generación del Keystore
He utilizado el comando `keytool` para generar un certificado autofirmado. He creado un almacén de claves llamado `mi_keystore.jks` dentro de la carpeta `conf` de Tomcat, definiendo el alias "tomcat" y el algoritmo RSA.

![](https://raw.githubusercontent.com/JosecarlosGlr/Configuraci-nDeSeguridadEnTomcat/refs/heads/main/5.png)

### 3.2. Configuración en server.xml
Para que Tomcat use ese certificado, he editado el archivo `server.xml`. He activado el conector en el puerto **8443**, vinculando la ruta del archivo keystore y su contraseña. También he configurado el conector AJP para asegurar la interoperabilidad con otros servidores.

![](https://raw.githubusercontent.com/JosecarlosGlr/Configuraci-nDeSeguridadEnTomcat/refs/heads/main/6.png)

---

## 4. Verificación de la Seguridad
Para terminar, he comprobado que el cifrado funciona accediendo a la URL con `https://`.

Al ser un certificado autofirmado por mí mismo, el navegador muestra primero una advertencia de riesgo de seguridad. He pulsado en "Avanzado" para aceptar el certificado.

![](https://raw.githubusercontent.com/JosecarlosGlr/Configuraci-nDeSeguridadEnTomcat/refs/heads/main/7.png)

Finalmente, he confirmado que la página de inicio de Tomcat carga correctamente bajo el puerto **8443**. Como se ve en la barra de direcciones, ahora la comunicación está cifrada mediante HTTPS.

![](https://raw.githubusercontent.com/JosecarlosGlr/Configuraci-nDeSeguridadEnTomcat/refs/heads/main/8.png)
