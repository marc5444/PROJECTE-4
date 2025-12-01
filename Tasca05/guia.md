Per poder començar amb la instalació necesitarem les 2 maquines, una linux i una windows.

Començarem amb la guia d'instalació SSH per linux

SSH linux

Un cop que hem instalat la maquina ubuntu colocarem la seguent comanda per poder actualitzar el sistema

sudo apt update && sudo apt upgrade -y 
![image](img/Tasca05_100.png)

Important


Si no hem instalat el ssh durant la instalacio de ubuntu farem la seguent comanda

![image](img/Tasca05_101.png)


Un cop que ja tenim instlat el ssh comprovarem si esta activat amb la seguent comanda

systemctl status ssh

Si el ssh no esta activat, el podem activar amb la seguent comanda

systemctl start ssh

![image](img/Tasca05_102.png)


Un cop que podem veure tal i com surt a la foto podrem seguir amb el seguent pas

systemctl status ssh



Un cop que ja tenim instalat el ssh el seguent pas sera veure la nostre ip amb la seguent comanda

ip addr show

Un cop que executem aquesta comanda veurem el seguent:

comanda per veure la ip

![image](img/Tasca05_103.png)


Ara el seguent pas sera fer la conexio per ssh, en aquest cas ho farem desde una maquina windows, això ho farem amb la seguent comanda

ssh usuari@192.168.56.101



Ens sortira un missatge com aquest en el qual haurem de escriure "yes" per poder continuar.

![image](img/Tasca05_105.png)

Un cop dins en demanara la contrasenya del usuari

colocar la contrasenya

Un cop que ja estem dins de la maquina el seguent pas que farem sera modificar l'arxiu de configuració, per poder editar-lo primer escriurem la seguent comanda

![image](img/Tasca05_107.png)

Editarem l'arxiu perque quedi de la seguent forma

![image](img/Tasca05_108.png)

editar l'arxiu de conf

Una de les configuracions que hem fet és habilitar que el poguis fer servir l'usuari root per fer ssh, per per poder fer primer haurem d'habilitar l'usuari root cosa que ho farem de la seguent forma.

Per poder fer això neccesitarem colocar una contrasenya al root, per fer això ho escriurem amb la seguent comanda

![image](img/Tasca05_109.png)

Un cop fet aixó ens demana la contrasenya.

![image](img/Tasca05_110.png)

Avans de continuar haurem de reinicar el servei amb la seguent comanda

systemctl restart ssh

Quan això ja esta fet podrem inicar sessió amb el root desde ssh fent servir la seguent comanda

ssh root@192.168.56.101

Inicar sessio amb el root

![image](img/Tasca05_111.png)

Ara que ja hem comprobat que podem accedir amb l'usuari root per ssh modificarem l'arxiu de configuració per poder afegir una capa de protecció.

Per començar tornarem al arxiu de configuració

sudo nano /etc/ssh/sshd_config

En aquest cas he modificat al arxiu perque no es pogui inicar sessió amb l'usuari root i que unicament és pugi iniciar sessió amb usuari fent que nomes hi hagi un sol usuari per poder fer servir ssh.

Per fer això podem modificar l'arxiu tal i com es veu a la foto

editar l'arxiu de conf

Per poder comprobar que funciona correctament crearem un segon usuari amb la seguent comanda

useradd -m -s /bin/bash usuari2

Avans de comprobar si funciona haurem de colocar una contrasenya en el usuari2,que ho farem de la seguent manera

passwd usuari2

Coloquem la contrasenya

Tot seguit comprobarem si podem realitzar ssh amb usuari2, per fer això farem la seguent comanda:

ssh usuari2@192.168.56.101

Si intentem inicar sessio amb usuari2 per ssh podem veure que no es pot

Fer ssh usuari2

Tambe hem canviar l'arxiu fent que l'usuari root no pogui inicar sesio per ssh però si que pot fer login per local, per comprobar això primer intentarem fer ssh amb l'usuari root amb la segunet comanda

ssh root@192.168.56.101

I podem veure que ens dona error.

Fer ssh amb root

En canvi si ho fem en local amb la comanda login podem veure que si que podrem inicar sessio, això ho farem amb la seguent comanda:

login root

login amb root

Per tant podem confirmar que l'arxiu de configuració que hem editat previament funciona correctament.

Mentre que amb usuari si que podem fer ssh

ssh amb usuari

Ara com ultim pas el que farem sera accedir amb un certificat en lloc de tindre que fer servir l'usuari i contrasenya

Per fer això el primer pas sera obrir el powershell del client i escriure la seguent comanda

ssh-keygen -t rsa

Un cop escrita la comanda farem enter fins que vellem algo semblant a la foto

foto del keygen

El seguent pas sera fer ls a la ruta on s'ha guardat, la ruta per defecta ens surt on la comanda d'avans, en el meu cas sera la seguent

ls C:\Users\cfgm2smxb19\.ssh
Podrem veure algo com això

comanda ls

I per ultim la seguent comanda sera

scp C:\Users\cfgm2smxb19\.ssh\id_rsa.pub usuari@192.168.56.101:/home/usuari
Podrem veure una cosa com la seguent

comanda scp

Un cop acabat això anirem al servidor ubuntu

I per començar farem les haurem de crear la carpeta ssh i un arxiu dins de la carpeta que s'anomeni authorized_keys

És posible que aquests arxius ja estiguin creats, si ja ho estan podem ignorem aquests pas

Això ho farem de la seguent manera

mkdir .ssh
touch .ssh/authorized_keys
A continuació farem servir la comanda ls amb la ruta que hem especificat previament, en el meu cas és

ls /home/usuari
En la qual podrem veure un archiu anomenat id_rsa.pub

El seguent pas sera veure que hi ha dins de l'arxiu, això ho farem amb la seguent comanda

cat /home/usuari/id_rsa.pub
Podrem veure el segunet

comanda cat

Com a ultim pas sera fer la seguent comanda

cat /home/usuari/id_rsa.pub >> .ssh/authorized_keys
comanda cat

Per acabar comprobarem que tot funciona correctament

Un cop fet tot això continuarem amb Windows

SSH Windows
Un cop que estem a Windows, el primer pas sera instalar el Servidor OpenSSH,

Per fer això ho farem amb la seguent comanda

Un cop fet això tocara habilitar el servei, això ho farem amb la comanda

Add-WindowsCapability -Online -Name OpenSSH.Server
Instalant SSH

Un cop fet això reiniciem l'ordinador i arranquem el servei amb la comanda

Start-Service sshd
Si volem que el servei s'arranqui automaticament amb la seguent comanda

Set-Service -Name sshd -StartupType 'Automatic'

