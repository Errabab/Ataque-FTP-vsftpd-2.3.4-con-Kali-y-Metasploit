# Ataque-FTP-vsftpd-2.3.4-con-Kali-y-Metasploit

## Ataque Simulado con Kali Linux: Explotación de Vulnerabilidad FTP vsftpd 2.3.4 Usando Metasploit

##  Descripción de la Práctica

En esta práctica, se asumirán los roles de atacante y víctima utilizando dos máquinas virtuales en red local. El **atacante** operará desde **Kali Linux**, una distribución diseñada para realizar auditorías de seguridad y pruebas de penetración, mientras que la **víctima** será una máquina **Metasploitable**, una máquina vulnerable deliberadamente, usada para fines de práctica en ciberseguridad.

El ataque se centrará en la explotación de un **servicio FTP** vulnerable, específicamente *vsftpd 2.3.4*, utilizando Metasploit para explotar esta vulnerabilidad y obtener acceso no autorizado a la máquina víctima con privilegios elevados.


### Pasos a seguir

1. **Iniciar el entorno virtual** 

Se iniciarán ambas máquinas (Kali Linux y Metasploitable) y se verificará la conectividad entre ellas a través de la red local, comprobando las direcciones IP respectivas.

(Kali Linux y Metasploitable) e **inicia sesión** en la máquina Metasploitable. 

   - **Credenciales**: 
   Usuario y contraseña son `msfadmin`.
   - Verifica la **IP** de la máquina Metasploitable.
   - También comprueba la **IP** de tu máquina Kali Linux.

   - **IP de Kali**: `172.26.16.204`
   - **IP de Metasploitable**: `172.26.16.217`

2. **En Kali Linux, abre un *terminal* y sigue estos pasos:**

   - Arranca el servicio de **PostgreSQL** con el siguiente comando:

     ```bash
     /etc/init.d/postgresql start
     ```
    ![kali](/img/K1.png)

    - Ver estado de **PostgreSQL**

        ![kali](/img/K2.png)

    - Actualiza la lista de paquetes con `apt update` y luego descarga e instala Metasploit con `apt install metasploit-framework`

        ![kali](/img/K3.png)   

    
   - Ejecuta el siguiente comando para iniciar **Metasploit**:

     ```bash
     msfconsole
     ```

     ![kali](/img/K4.png)


3. **Identificar la máquina a atacar**: Este paso generalmente se realiza con herramientas como **Nmap** o **Zenmap**, pero para ahorrar tiempo, podemos omitirlo ya que conocemos la IP de la víctima.

   - Si lo necesitas, aquí está el comando para escanear:

     ```bash
     nmap 172.26.16.217
     ```

4. **Escaneo de puertos y servicios en la víctima**: Utiliza Nmap para ver los puertos abiertos y los servicios que se están ejecutando.

   - Ejecuta el siguiente comando para buscar vulnerabilidades:

    `nmap  –sV  IP_VÍCTIMA`

     ```bash
     nmap -sV 172.26.16.217
     ```

   - Nos fijaremos en el puerto **21 (FTP)**, que está corriendo el servicio **vsftpd 2.3.4**

    ![kali](/img/K5.png)

    ![kali](/img/K5.2.png)



5. **Buscar un exploit para vsftpd 2.3.4**: Ahora utilizaremos Metasploit para encontrar el exploit correspondiente.

   - Usa el siguiente comando en **Metasploit**:

     ```
     bash
     search vsftpd_234
     ```
      ![kali](/img/K6.png)

   - Metasploit mostrará los exploits disponibles para la versión del servicio.

6. **Seleccionar el exploit**: Usaremos el exploit identificado en el paso anterior.

   - El comando para seleccionar el exploit en Metasploit es el siguiente:

     `use  exploit/SO/servicio/nombre_exploit`

     ```bash
     use exploit/unix/ftp/vsftpd_234_backdoor
     ```
        
    ![kali](/img/K7.png)

7. **Configurar opciones del exploit**: Verifica las opciones requeridas para el exploit con el siguiente comando:

   ```bash
   show options
   ```
    ![kali](/img/K8.png)

   Nos mostrará una lista de los servicios requeridos para que el exploit funcione, y los valores que en ese momento tiene cada servicio. En esta lista, solo necesitaremos dar valores a los servicios marcados como requeridos (columna “Required” marcada como “Yes”). En nuestro caso, tenemos que fijarnos en un servicio llamado RHOST, que aparecerá en blanco (el servicio RPORT ya tiene un valor asignado, y será el puerto 21, que es precisamente el puerto que queremos atacar, por tanto no habrá que tocarlo).

8. **Inyectar el payload:**
 En este caso, no es necesario inyectar un payload, ya que el exploit que estamos usando tiene uno por defecto. Si fuera necesario inyectar un payload, podrías listar los disponibles con el siguiente comando:

    ```bash
    show payloads
    ```
   
    - Luego, seleccionarías el payload adecuado con el comando:

    ```bash
    set PAYLOAD nombre_payload
    ```
    `set PAYLOAD payload/cmd/unix/interact`
      
    ![kali](/img/K9.png)
