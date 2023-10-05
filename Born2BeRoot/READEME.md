➤ Quan reinicies o apagues la màquina s'ha d'introduir la contrasenya d'encriptació, que és `Maria42CAT`(la de mmonpeat42).

➤ Introduïm l’usuari i la contrasenya creats. Usuari: `mmonpeat`, contra: `Maria42BCN`

root ≠ mmonpeat42 pq root@mmonpeat42 i depres quan entres al usu mmonpeat posa mmonpeat@mmonpeat42

su nom_usuari —> per entrar a qualsevol usuari. EXEMPLE: su root — > per entrar a root

swap = intercanvio

root = raiz

`sudo -V`, este comando además de mostrarnos la versión de sudo también mostrará los argumentos pasados para configurar cuando se creó sudo y los plugins que pueden mostrar información más detallada

# 1. Configuració de la màquina virtual

## ****1 - Instalació de sudo i configuració d’usuaris i grups 👤****

1. Per l'instal·lació de sudo primer hem d'entrar a l'usuari root, escrivint `su`
 i introduint la contrasenya de l'usuari admin, `Maria42CAT`. Escrivim `apt install sudo`, per instal·lar els paquets necessaris.
2. Escrivim `sudo reboot` —> reiniciar la màquina perquè s'apliquin els canvis.
3. Un cop reiniciat tornem a escriure les contrasenyes d'encriptat i de l'usuari. Per verificar que sudo s'ha instal·lat correctament entrarem de nou a l'usuari root i posarem la comanda `sudo -V`,aquesta comanda ens mostrarà la versió de sudo i els arguments anteriors per configurar quan es va crear sudo i els pluguins que poden mostrar informació més detallada.
4. Seguint en l'usuari root crearem amb el nostre login un usuari amb la comanda `sudo adduser login`, però com ja l'hem creat durant la instal·lació, sortirà que aquest usuari ja existeix.
5. Creem un grup anomenat user42, amb la comande `sudo addgroup user42`.
6. Amb la comanda sudo adduser nom_usu nom_group afegirem l’usuari en el grup. Hem d'incloure l'usuari (mmonpeat) en els grups  `sudo`  i  `user42` .
    - `sudo adduser mmonpeat user42`
    - `sudo adduser mmonpeat sudo`
7. Un cop ja fet, per crakejar que tot s’hagi fet correctament podem executar la comanda `getent group nom_grupo` o també podem editar el fitxer /etc/group `nano /etc/group`
 i en els grups  `sudo` i `login42` haurà d’apareixe el nostre usuario.

<aside>
💡 **VIT**

🧠 **Que és GID❓** És l’identificador de grup, és una abreviatura de Group 🆔.

🤔 **S'ha creat correctament el grup?** 
Si no ha sortit cap missatge d'error probablement estarà bé, tot i així podem comprovar si s'ha creat amb la comanda `getent group nom_grupo` (getent group sudo/user42) o també podem fer `cat /etc/group` i podem veure tots els grups i usuaris que hi ha dins d'ells.

 `cat /etc/group` (s’escriu amb espai).

</aside>

## ****2 - Instalació y configuració SSH 📶****

<aside>
🧠 **Que es SSH ❓**És el nom d'un protocol i del programa que l'implementa que la seva funció principal és l'accés remot a un servidor a través d'un canal segur en el qual tota la informació està xifrada.

</aside>

1. Fem `sudo apt update` per actualitzar els repositoris que hem definit anteriorment en l’arxiu etc/apt/sources.list
2. Ara instal·larem l'eina principal de connexió per l'inici de sessió remot amb el protocol SSH, aquesta eina és OpenSSH. Per instal·lar-la hem d'introduir la comanda `sudo apt install openssh-server`. En el missatge de confirmació posem `S` , llavors s'acabarà de fer l'instal·lació.
3. Per comprovar que s'ha instal·lat correctament fem `sudo service ssh status` i ha de sortir active, en verd fosforescent.
4. Una vegada acabada l'instal·lació s'han creat fitxers, que hem de configurar. Per poder fer-ho fem amb Nano. El primer fitxer que editarem serà `/etc/ssh/sshd_config`. Si no estàs en l'usuari root no tindràs permisos d'escriptura, per això entrem a root amb su i la contrasenya `Maria42holiwis`. Si no es vol entrar a l'usuari root podem escriure sudo a l'inici de la comanda anterior `sudo nano /etc/ssh/sshd_config`.
5. Els `#` a l'inici d'una línia significa que aquesta línia està comentada, modificarem les següents línies que veus a baix per les de després de —>.
    - #Port 22 -> Port 4242
    - #PermitRootLogin prohibit-password -> PermitRootLogin no
6. Ara editarem el fitxer `/etc/ssh/ssh_config`. També treient-li el comentari.
    - #Port 22 -> Port 4242
7. Per acabar hem de reiniciar el servei ssh perquè així s’actualitzin les modificacions realitzades. Per fer-ho escrivim la comanda `sudo service ssh restart` Un cop reiniciat mirarem l’estat actual amb `sudo service ssh status` i per confirmar que s'han realitzat els canvis a la "escucha" del servidor ha d'aparèixer el port 4242.

## 3**- Instalació y configuració de UFW 🔥🧱**

<aside>
🧠 Que és [UFW](https://es.wikipedia.org/wiki/Uncomplicated_Firewall)**❓**  És un [firewall](https://es.wikipedia.org/wiki/Cortafuegos_(inform%C3%A1tica)) que utilitza una línia de comandes per configurar les [iptables](https://es.wikipedia.org/wiki/Iptables) utilitzant un nombre petit de comandes simples.

</aside>

1. Instalem UFW amb la comanda `sudo apt install ufw`
2. Un cop instalat l’hem d’habilitar, per això cal posar `sudo ufw enable` i ha des sortir un prompt indicant que el firewall està actiu.
3. Ara hem de fer que el nostre firewall façi que les conexions es facin a través del port 4242, escrvint `sudo ufw allow 4242`. 
4. Compovem que tot està correctament configurat mirant l’extat del nostra firewall, les conexions a través del port 4242 han d’apareixer com a permeses. Per veure l’estat posem `sudo ufw status`. 

## 4- Configurar contrasenya forta per sudo ****🔒****

1. Creem un fitxer, amb la ruta /etc/sudores.d/ el fitxer l’anomenarem sudo_config, ja que aquest fitxer contindrà la configuració de la contrasenya. La comanda per crear un fitxer és `touch`. Per tant, la comanda quedarà `touch /etc/sudoers.d/sudo_config`.
2. Hem de crear el directori sudo a la ruta /var/log perquè cada comanda que executem amb sudo, tant l'input com l'output ha de quedar guardat en aquest directori. Per crear-l'ho utilitzem la comanda `mkdir /var/log/sudo`.
3. Editem el fitxer sudo_config, creat en el primer pas. L'editem amb nano, utilitzarem la següent comanda `nano /etc/sudoers.d/sudo_config`.
4. Quan estem editatn el fitxer hem d’afegir les seguents comandes.

```mermaid
Defaults  passwd_tries=3
Defaults  badpass_message="Mensaje de error personalizado"
Defaults  logfile="/var/log/sudo/sudo_config"
Defaults  log_input, log_output
Defaults  iolog_dir="/var/log/sudo"
Defaults  requiretty
Defaults  secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```

Que fa cada comanda

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2b1d6287-e322-4bbc-a8bf-cb10aeceef0e/Untitled.png)

## 5- Configurarció de politica de contrasenyes fortes

1. Edirem el fitxer login.defs, en el que modificarem les seguents lineas.
    - PASS_MAX_DAYS 99999 -> PASS_MAX_DAYS 30
    - PASS_MIN_DAYS 0 -> PASS_MIN_DAYS 2
    
    <aside>
    🤔 What❓
    
    PASS_MAX_DAYS: És el temps d'expiració de la contrasenya. El número corresponent a dies.
    
    PASS_MIN_DAYS: És el número mínim de dies permès abans de modificar una contrasenya.
    
    PASS_WARN_AGE: L'usuari rebrà un missatge d'avis indicant que falten els dies especificats perquè venci la contrasenya.
    
    </aside>
    
2. Per seguir amb la configuració hem d'instal·lar els següents paquets, amb la següent comanda `sudo apt install libpam-pwquality`, després posem que `S` per confirmar la instal·lació.
3. Hem d’editar un altre fitxer i modificar algunes línies. Farem `nano /etc/pam.d/common-password`.
4. Déspres de retry+3 hem d’afegir el seguent:
    
    <aside>
    🤔 TOT SEGUIT, A LA MATEIXA LÍNIA
    
    minlen=10
    ucredit=-1
    dcredit=-1
    maxrepeat=3
    reject_username
    difok=7
    enforce_for_root
    
    </aside>
    
    <aside>
    🤔
    
    minlen=10 ➤ La quantitat mínima de caràcters que ha de contenir la contrasenya.
    
    ucredit=-1 ➤ Com a mínim ha de contenir un caràcter amb  `Mayus`. Posem el - ja que ha de contenir com a mínim un caràcter, si posem + ens referim com a màxim aquests caràcters.
    
    dcredit=-1 ➤ Com mínim ha de contenir un dígit.
    
    maxrepeat=3 ➤ No put tenir més de 3 cops seguits el mateix caràcter.
    
    reject_username ➤ No pot contenir el nom de l'usuari.
    
    difok=7 ➤ Ha de tenir almenys 7 caràcters que no siguin part de l'antiga contrasenya.
    
    enforce_for_root ➤ Afegim aquesta política per l'usuari root.
    
    </aside>
    

## 6- Conectar-se via SSH

1. Per conectar-nos a través de SSH tanquem la maquina, obrim VirtualBox i anem a Configuració.
2. Cliquem sobre `Network`, anem a Avanzadas per veure més opcions i li donarem a `Port Forwarding`.
3. Afegim una norma de renviar.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dffef5b5-4706-48fa-85ff-bbfe65a93fc4/Untitled.png)
    
4. Per acabar afegirem el port 4242 al Host Port i el Guest Port. Les IP’s no són necessaries. I acabem donant-li a Ok.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7eec5181-6222-46e9-96b6-88f2f2322187/Untitled.png)

# 2. Script 🚨
A l'avaluació cal explicar cada comanda, si l'avaluador ho demana.

<aside>
🧠 **Que és un script❓** És una seqüència de comandes guardada en un fitxer que quan s’executa farà la funció de cada comanda.

</aside>

## 2.1 Arquitectura

Per veure l’arquitectura del SO i la seva versió de kernel → `uname -a` ( "-a" == "--all" ). 

Printejarà tota la informació exepte si el tipus de proccesador es desconegut o la plataforma de hardware.

## 2.2 Nuclis físics

Per mostrar el numero de nuclis físics utilitzarem el fitxer /proc/cpuinfo, que ens dona informació del proccessador: el tipus, la marca, el model, el rendiment, etc. 

Utilitzarem la comanda `grep "physical id" /proc/cpuinfo | wc -l` 

grep → busca dins del fitxer les paraules "physical id”

wc -l → contem les lineas del resultat de grep

Ho fem així perquè la forma de quantificar els nuclis no és molt comú.

Si hi ha un processador marcarà 0 i si té més d'un processador, mostrarà tota la informació del processador per separat contant els processadors utilitzant la notació zero. D'aquesta manera contarem les línies que hi ha, ja que és més còmode quantificar-lo així.

<aside>
❓ **Notació zero.** El zero (0) és un numeral de la propietat parell. És el signe numèric de valor nul, que en notació posicional ocupa els llocs on no hi ha una xifra significativa. Si esteu situat a la dreta d'un nombre enter se'n multiplica per 10 el valor, col·locat a l'esquerra, no el modifica.

</aside>

## 2.3 Nuclis virtuals

Per mostrar el nombre de nuclis virtuals s'assemblen a l'anterior. Utilitzarem el fitxer /proc/cpuinfo, però en aquest cas utilitzarem la comanda `grep processor /proc/cpuinfo | wc -l`. En aquest cas no contem les línies de "physical id" sinó de processador. Ho farem així pel mateix motiu que abans, la manera de quantificar marca 0 si hi ha processador.

![Screen Shot 2022-11-22 at 5.37.09 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/be9924af-63b6-4ef4-9bb3-6adefbecd711/Screen_Shot_2022-11-22_at_5.37.09_PM.png)

## 2.4 Memoria RAM

Per mostrar la memòria RAM utilitzarem la comanda `free` per així veure al moment informació sobre la RAM, la part utilitzada lliure, reservada per altres recursos... Per més informació posem `free --help`. 

Nosaltres utilitzem `free --mega` , ja que en el subject surt aquesta unitat de mesura.

![Screen Shot 2022-11-22 at 5.37.09 PM copy.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/24ceb1a7-499c-4b27-b9ad-6756b0406ae3/Screen_Shot_2022-11-22_at_5.37.09_PM_copy.png)

Una vegada executada aquesta comanda hem de filtrar la cerca ja que necessitem tota la informació que ens aporta, el primer que hem de mostrar és la memòria utilitzada, per això farem ús de la comanda `awk` (el que fa aquesta comanda és per processar dades passades a arxius de text, és a dir que podem utilitzar les dades que vulguem del fitxer X.) Per acabar comprovarem  si la primera paraula d’una fila és igual a "Mem:” printejarem la tercera paraula d'aquesta fila, és la memòria utilitzada. Tota la comanda és `free --mega | awk '$1 == "Mem:" {print $3}'`. En l’script el valor que retorna aquesta comanda l’assignarem a una variable que connectarem amb altres variables perquè tot quedi igual al que especifica el subject.

![Screen Shot 2022-11-22 at 6.00.15 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/17962cab-cf15-4014-b39d-01fec1101795/Screen_Shot_2022-11-22_at_6.00.15_PM.png)

Per obtenir la memòria total la comanda és: `free --mega | awk '$1 == "Mem:" {print $2}'`, que printeja la segona paraula de la fila. 

![Screen Shot 2022-11-22 at 5.57.32 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/761e9eb3-8353-41d5-aec8-50dd81b1160b/Screen_Shot_2022-11-22_at_5.57.32_PM.png)

Hem de calcular el % de la memòria utilitzada. Com que l'operació per aconseguir el % no és exacta ens pot donar molts decimals i en el subject només apareixen 2 així que usarem `%.2f` perquè només es mostrin dos decimals.. Perquè en el printf es mostri % cal posar `%%`. 

Tota la comanda serà `free --mega | awk '$1 == "Mem:" {printf("(%.2f%%)\n", $3/$2*100)}'`.

![Screen Shot 2022-11-22 at 6.02.01 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/51ff6c7f-7bb3-4f56-b689-10d2fe080d59/Screen_Shot_2022-11-22_at_6.02.01_PM.png)

## 2.5 Memoria del disc

Per poder veure la memòria del disc ocupada i disponible utilitzarem la comanda `df`
que significa "disk filesystem", que s'utilitza per obtenir un resum complet del ús de l'espai en el disc. Com en el subject indica la memòria usada es mostra en MB així que usarem el flag -m. Després farem un grep perquè només ens mostri les línies que contenen "/dev/", a continuació farem un altre grep afegintla flag -v per excloure les línies que continguin "/boot/". Per acabar utilitzarem la comanda awk i sumarem el valor de la tercera paraula de cada línia, així un cop sumades totes les línies printear el resultat final de la suma. La comanda sencera és: `df -m | grep "/dev/" | grep -v "/boot" | awk '{memory_use += $3} END {print memory_use}'`.

![Screen Shot 2022-11-23 at 3.49.18 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f5100c09-dc68-4886-ad55-9554c3d6e8d4/Screen_Shot_2022-11-23_at_3.49.18_PM.png)

Per obtenir l'espai total que utilitzarem una comanda semblant. Les úniques diferències seran que els valors que sumarem seran $2 en comptes de $3 i l'altre diferencia és que el total de la suma en el subject surt en Gb i a nosaltres amb Mb, per tant, per canviar-ho hem de dividir la suma entre 1024 i treure els decimals. La comanda `df -m | grep "/dev/" | grep -v "/boot$" | awk '{memory_use += $2} END {printf ("%.OfGb\n"), memory_result/1024}'`.

![Screen Shot 2022-11-23 at 3.57.03 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5e52f419-e80b-4822-b527-f72ca5626e6e/Screen_Shot_2022-11-23_at_3.57.03_PM.png)

Per acabar hem de mostrar un percentatge de la memoria utilitzada. L'únic que hem de canviar de les comandes anteriors per tenir dues variables, una representa la memòria utilitzada i l'altre el total. Un cop fet això farem una operacional aconseguir el tant per cent `use/total*100`i el resultat d'aquesta operació el printejem com apareix en el subject, entre parèntesis i el % al final. La comanda final: `df -m | grep "/dev/" | grep -v "/boot" | awk '{use += $3} {total += $2} END {printf ("(%d%%)\n"), use/total*100}'`.

![Screen Shot 2022-11-23 at 3.58.34 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4d1427c1-02b8-4f7c-a773-e00e5455a9db/Screen_Shot_2022-11-23_at_3.58.34_PM.png)

## 2.6 Percentatge d’us de CPU

Per poder veure el percentatge d'ús de la CPU farem ús de la comanda `vmstat`, aquest mostra estadístiques del sistema, permetent obtenir un detall general dels processos, ús de memòria, activitat de la CPU, estat del sistema, etc. Podem posar sense cap opció però jo posaré un interval d'1 a 4 segons. També utilitzarem la comanda `tail -1`,que ens permetrà que només produeixi l'output la última línia, llavors de les 4 generades només és plantejarà l'última. Per acabar només printejarem la paraula 15 que és el ús de memòria disponible. La comanda completa és la següent: `vmstat 1 4 | tail -1 | awk '{print $15}'`. El resultat d'aquesta comanda només és una part del resultat final, ja que encara s'ha de fer alguna operació en le script perquè quedi bé. El que hauríem de fer és a 100 restar-li la quantitat que ens ha tornat la comanda, el resultat d'aquesta operació el printejem amb un decimal i un % al final i ja estaria feta l'operació.

![Screen Shot 2022-11-30 at 3.15.41 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c49ffb7c-86b8-414d-9420-6b96fca0ce06/Screen_Shot_2022-11-30_at_3.15.41_PM.png)

## 2.7 Ultim a reiniciar

Per veure la data i l'hora del nostre últim cop que hem reiniciat utilitzarem la comanda who amb la flag -b ja que amb aquesta flag ens mostrarà per pantalla l'hora de l'última vegada que arranqui del sistema. Ens mostra més informació de la qual necessitem així que filtrarem i només mostrarem el que necessitem, per això farem ús de la comanda awk i compararem si la primera paraula d'una línia és "system" és printeja per pantalla la tercera paraula de la línia, un espai i la quarta paraula. La comanda sencera `who -b | awk '$1 == "system" {print $3 " " $4}'`.

![Screen Shot 2022-11-30 at 4.55.14 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/29978f46-85d6-4dcc-9042-c7ca8f0974e8/Screen_Shot_2022-11-30_at_4.55.14_PM.png)

## 2.8 Usem LVM

Per comprovar si LVM està actiu o no usarem la comanda lsblk, aquesta ens mostra informació de tots els dispositius de bloc (discs durs, SSD, memòries, etc.) de tota la informació que ens dona podem veure lvm en el tipus de gestor. Per aquesta comanda farem un if, ja que printejem Yes o No. Bàsicament, la condició que busquem serra contar el número de liineas a les quals apareix "lvm" i si hi ha més de 0 printejem Yes, però si hi ha 0 és printejara No. Tota la comanda és: `if [ $(lsblk | grep "lvm" | wc -l) -gt 0 ]; then echo yes; else echo no; fi`.

![Screen Shot 2022-11-30 at 4.56.27 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eb88f047-dc3e-4f88-90c6-886ce5223624/Screen_Shot_2022-11-30_at_4.56.27_PM.png)

## 2.9 Conexions TCP

Per mirar el nombre de connexions TCP establertes utilitzarem la comanda ss. Filtrarem amb -ta perquè només es mostrin les connexions TCP. Per acabar farem un grep per veure les que estan establertes, ja que també hi ha només que escolten i acabem amb wc -l, perquè conti el nombre de línies. La comanda quedarà així: `ss -ta | grep ESTAB | wc -l`. 

![Screen Shot 2022-11-30 at 4.57.42 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a8848767-6cff-4749-9aa9-961ce5f87320/Screen_Shot_2022-11-30_at_4.57.42_PM.png)

## 2.10 Nombre d'usuaris

Utilitzarem la comanda users que ens mostrarà el nombre d'usuaris que hi ha, sabent això posarem wc -w perquè conti la quantitat de paraules que hi ha a la sortida de la comanda. La comanda sencera és `users | wc -w`.

![Screen Shot 2022-11-30 at 4.58.18 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5732f374-f6e3-411b-b461-b5bcd72df9df/Screen_Shot_2022-11-30_at_4.58.18_PM.png)

## 2.11 Adreça IP i MAC

Per obtenir la direcció del host utilitzarem `hostname -I` i per obtenir la MAC farem ús de la comanda `ip link` que s'utilitza per mostrar o modificarinterfícies de xarxa. Com apareix més d'una interfície, IP's etc. Usarem la comanda grep per buscar i poder printejar el que ens demanen. Per aconseguir-ho posarem `ip link | grep "link/ether" | awk '{print $2}'`i així només printejar només la MAC.

![Screen Shot 2022-11-30 at 4.59.10 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0da4a34f-d326-4eca-b273-d549ff6256ff/Screen_Shot_2022-11-30_at_4.59.10_PM.png)

## 2.12 Nombre de comandes executades amb sudo

Per obtenir el nombre de comandes que són executades amb sudo utilitzarem la comanda jornalctl, que s'encarrega de recopilar i administrar els registres del sistema. Després posem `_COMM=sudo`per filtrar les entrades especificant la seva ruta. Nosaltres posem `_COMM`fent referència a un script executable. Una vegada tinguem filtrada la cerca i només apareguin el registres de sudo haurem de fitxar una altra vegada perquè quan inicies o tanques sessió de root també apareix en el registre, per acabar de filtrar posem `grep COMMAND` i així només apareixeran línies de comandes. Per acabar posem `wc -l`, perquè surtin les línies numerades. La comanda sencera és: `journalctl _COMM=sudo | grep COMMAND | wc -l`. Per comprovar que funcioni correctament podem posar la comanda a la terminal, posem una comanda amb sudo i tornem a posar la comanda llavors veurem que incrementarà el nombre d'execucions de sudo.

![Screen Shot 2022-11-30 at 5.01.11 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d941c406-28ec-4fbb-8af7-272dcf3a1d1f/Screen_Shot_2022-11-30_at_5.01.11_PM.png)

## 2.13 Script

```bash
#!/bin/bash
# ARCH
arch=$(uname -a) #uname veure arqutectura de so i versio kernel

# CPU PHYSICAL ordena en el que tens la maquina virtual
cpuf=$(grep "physical id" /proc/cpuinfo | wc -l) #wc -l → contem les lineas del resultat de grep

# CPU VIRTUAL maquina irtual
cpuv=$(grep "processor" /proc/cpuinfo | wc -l)

# RAM 
##free per veure la info de la ram al moment 
##awk processa dades passades a arxius de text
ram_total=$(free --mega | awk '$1 == "Mem:" {print $2}')
ram_use=$(free --mega | awk '$1 == "Mem:" {print $3}')
ram_percent=$(free --mega | awk '$1 == "Mem:" {printf("%.2f"), $3/$2*100}')

# DISK
##df (disk filesystem)resum de l'esai del disc
##-m es per MB
##-v exclou lineas que tinguin /boot
## disk_t el nom de la variable, $2 segona columna
disk_total=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_t += $2} END {printf ("%.1fGb\n"), disk_t/1024}')
disk_use=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_u += $3} END {print disk_u}')
disk_percent=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_u += $3} {disk_t+= $2} END {printf("%d"), disk_u/disk_t*100}')

# CPU LOAD
##vmstat aquest mostra estadístiques del sistema, permetent obtenir un detall general dels processos, ús de memòria, activitat de la CPU
##tail -1 produeixi l'output la última línia
cpul=$(vmstat 1 2 | tail -1 | awk '{printf $15}')
cpu_op=$(expr 100 - $cpul)
cpu_fin=$(printf "%.1f" $cpu_op)

# LAST BOOT
## who data i l'hora reinicia
## -b mostrarà per pantalla l'hora de l'última vegada que arranqui del sistema.
lb=$(who -b | awk '$1 == "system" {print $3 " " $4}')

# LVM USE
lvmu=$(if [ $(lsblk | grep "lvm" | wc -l) -gt 0 ]; then echo yes; else echo no; fi)
```
    
- Per poder connectar-nos a la màquina virtual des de la real hem d'obrir un terminal a la màquina real i escriure `ssh mmonpeat@localhost -p 4242` i ens demanarà la clau de l'usuari i un cop l'hàgim introduït ja ens sortirà el login en verd, això ens indica que ja estem connectats.

- # 3. Bonus

https://github.com/GuillaumeOz/Born2beroot

![Screen Shot 2022-12-01 at 6.13.51 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a990d174-3ca2-4435-95d6-97997d6b8c3e/Screen_Shot_2022-12-01_at_6.13.51_PM.png)

![Screen Shot 2022-12-01 at 6.21.58 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5a6636ac-6338-47e4-8388-24340a09aab6/Screen_Shot_2022-12-01_at_6.21.58_PM.png)

![Screen Shot 2022-12-01 at 6.23.31 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dfa8d14f-467f-4b3d-b20e-83143b4ccebb/Screen_Shot_2022-12-01_at_6.23.31_PM.png)
- S'ha d'encendre la màquina virtual encesa per poder entrar des del terminal de la màquina real.

*conectar la maquina virtual al terminal amb la comanda `ssh mmonpeat@127.0.0.1 -p 4242` posar contrasenya `Maria42BCN`. Oviament això només és pot fer després d’instalar ssh.
