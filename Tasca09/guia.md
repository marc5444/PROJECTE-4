# Projecte04: Servidor NFS

## El Cas Client:  DevOptimize Solutions

El nostre client, `DevOptimize Solutions`, és una petita startup de desenvolupament de programari que treballa exclusivament amb Linux. Tenen un problema crític: el seu codi font i els seus actius (documents de disseny, scripts) estan descontrolats. Cada desenvolupador té còpies locals, cosa que provoca errors de versió constants i una pèrdua d'eficiència brutal.
Ens han contractat per implementar un servidor de fitxers centralitzat. Atès que tot l'entorn és Linux, la solució nativa, més ràpida i eficient del sector és NFS (Network File System).
El client ha insistit en que treballa sense un entorn d’autenticació centralitzada i que, de moment, no té previst fer aquest pas.

Per mostrar al client com quedarà la solució proposada a partir de les seves demandes i poder mostrar també les seves limitacions, se t’encarrega fer una demostració del sistema.

Crearàs un servidor NFS (NFSv3) i un client Linux que consumeixi els recursos compartits. Hauràs de crear usuaris i grups per simular l'entorn del client i demostrar el control d'accés utilitzant les opcions d'exportació (/etc/exports) i els permisos del sistema de fitxers (chmod, chown).


---

Per començar en aquesta guia hem de tindre 2 maquines, en aquest cas tindrem un ubuntu server i un zorin per simular el clinet.

Un cop que ja tenim les dues maquines instalades començarem configuran el servidor.

La primera comanda que farem sera per actualitzar els paquets.

```bash
sudo apt update && sudo apt upgrade -y 
```
Un cop que ja tenim actualitzat els paquets, el seguent pas sera començar amb la creació de l'estructura de carpetas, de grups i usuaris.

El primer que farem sera crear els grups neccesaris, en aquest cas en demana que crem 2 grups, el primer devs i el segon admin

Per crear aquest grups farem la seguent comanda 

```bash
groupadd devs
```

```bash
groupadd admin
```


Per comporbar que l'arxiu s'ha creat correctament farem servir el greep per buscar tant el grup devs com admin dins de l'arxiu /etc/groups, per fer-ho farem la seguent comanda

```bash
grep devs /etc/group
```

Per comporbar que l'arxiu s'ha creat correctament farem servir el greep per buscar tant el grup devs com admin dins de l'arxiu /etc/groups, per fer-ho farem la seguent comanda

```bash
grep devs /etc/group
```

```bash
grep admin /etc/group
```

En la qual podrem veure que els grups d'han creat correctament

![image](img/T09_1.png)

Un cop que ja tenim els grups creats el seguent pas sera crear l'usuari dev01 que formi part del grup devs, per fer això farem servir la seguent comanda

```bash
useradd -G devs -m -s /bin/bash dev01
```

Tot seguit farem el mateix per l'usuari admin01, en la qual farem la seguent comanda

```bash
useradd -G admin -m -s /bin/bash admin01
```

Per confirmar que estan creats correctament tornarem a fer servir el grep

```bash
grep dev01 /etc/passwd
```

```bash
grep admin01 /etc/passwd
```

![image](img/T09_2.png)

Un cop que ja hem creat els grups i els usuaris, el seguent pas sera crear el directori per als projectes de desenvolupament en la qual la ruta que ens demana és la seguent /srv/nfs/dev_projects, per crear les totes les carpetas d'una sola comanda farem el seguent:

```bash
mkdir /srv/nfs/dev_projects -p
```

Un cop fet això crearem el directori per a les eines d'administració en la qual la ruta sera /srv/nfs/admin_tools

```bash
mkdir /srv/nfs/admin_tools
```

![image](img/T09_3.png)
Per ultim configurarem els permisos de les carpetas, en aquest cas seran els seguent.

Chown per canviar la propietat de la carpeta

```bash
chown root:devs /srv/nfs/dev_projects
```

```bash
chown root:admin /srv/nfs/admin_tools/
```

Un cop fet això assignare els permisos de la carpeta amb la comanda chmod

```bash
chmod 770 /srv/nfs/dev_projects
```

```bash
chmod 770 /srv/nfs/admin_tools
```

Per comprobar que els permisos estan correctas farem ls -l per poder veure els permisos de cada carpeta

![image](img/T09_4.png)

Avans de continuar amb el servidor crearem els grups i usuaris dins de la maquina client, en aquest cas una maquina zorin.

Per poder crear els grups i usuaris farem servir la aplicació "users and groups"

![image](img/T09_5.png)

Per comporbar que tots els grups s'han creat correctament farem servir el grep tal i com hem fet avans

![image](img/T09_6.png)
![image](img/T09_7.png)

Hem de comprobar que els numeros UID i GID (els números d'identificació) coincideixin a les dues màquines.

Un cop fet això instalarem els paquets neccesairs del servei NFS al servidor, per fer això farem la seguent comanda

```bash
apt install nfs-kernel-server -y
```
Per comprobar que s'ha instalat correctament podem fer un systemctl status

```bash
systemctl status nfs-kernel-server
```

![image](img/T09_8.png)

Per començar editarem l'arxiu /etc/exports per poder decidir quins arxius volem exportar, en aquest cas volem exporta tota la carpeta /srv/nfs

Afegirem una linia adicional al final del arxiu, en aquest cas sera la seguent 

```bash
/srv/nfs *(rw,sync,no_subtree_check)
```
![image](img/T09_9.png)

Per poder aplicar el canvis haurem de reinciar el servei amb la comanda

```bash
systemctl restart nfs-kernel-server
```
Un cop fet això l'iniciem i comprobarem que tot funciona correctament 

En el servidor podem fer la comanda 

```bash
exportfs -u
```
Amb la qual podrem veure quins arxius es poden exportar

![image](img/T09_10.png)

Tambe podem fer la seguent comanda per veure des-de quin port treballa, en aquest cas ho fa amb el port 2049

```bash
rpcinfo -p 192.168.56.101
```
![image](img/T09_11.png)

Per poder comprobar en la maquina haurem d'instalar el paquet nfs-common, això ho farem amb la seguent comanda

```bash
sudo apt install nfs-common -y
```

Un cop fet això en conectarem al servidor amb la comanda showmount -e IP

En el meu cas sera la seguent comanda 

```bash
showmount -e 192.168.56.101
```

![image](img/T09_12.png)

En la qual podem veure que la carpeta /srv/nfs

---

# Fase 3: L'Exportació d'Administració (El Dilema del root_squash)

A continuació farem una prova 1 (L'error comú)

Previament ja hem exportat l'arxiu /srv/nfs per tant el seguent pas que hem de fer sera muntar aquest recurs a la carpeta /mnt/admin_tools, en un principi aquesta carpeta no existeix, per tant el primer pas sera crear-la, això ho farem amb la seguent comanda

```bash
mkdir /mnt/admin_tools 
```

![image](img/T09_13.png)

Un cop que tenim creada la carpeta, el seguent pas sera muntar el recurs, això ho farem amb la comanda mount 

```bash
mount -t nfs 192.168.56.101:/srv/nfs/admin_tools /mnt/admin_tools
```

Podrem veure no podem crear cap arxiu ja que no tenim els pemisos ja que el root de la maquina client i el root del servidor no es el mateix

![image](img/T09_14.png)

Mentre que si intentem crear un arxiu amb l'usuari admin si que podrem, ja que aquest usuari si que te permisos en aquesta carpeta

![image](img/T09_15.png)

Podem veure que l'arxiu que hem creat es propietat de admin01

![image](img/T09_16.png)

A continuació ensenyare com fer per poder crear arxius amb root

Prova 2 (La Solució)

Per començar haurem d'editar l'arxiu /etc/exports en el qual substituirem la linia que hem escrit previament per les seguents.

```bash
/srv/nfs/admin_tools *(rw,sync,no_subtree_check,no_root_squash)
/srv/nfs/dev_projects *(rw,sync,no_subtree_check)
```

Un cop fet això reiniciem el servei un altre cop amb la comanda 

```bash
systemctl restart nfs-kernel-server
```

A continuació haurem de desmuntar i muntar un altre cop el recurs, en el meu cas la comanda per desmuntar sera 

```bash
umount -t nfs 192.168.56.101:/srv/nfs/admin_tools /mnt/admin_tools
```
I per muntar

```bash
mount -t nfs 192.168.56.101:/srv/nfs/admin_tools /mnt/admin_tools
```

Un cop fet això podrem crear un now arxiu, per exemple en aquest cas he creat una arxiu anomenat file2

![image](img/T09_17.png)

![image](img/T09_18.png)

Això a causa de que hem modificat l'arxiu /etc/exports fent que el root de la maquina fisica sigui el mateix que el root del servidor, per tant tenim total llibertat 

---

# Fase 4: L'Exportació de Desenvolupament (Permisos rw vs ro)

A continuació el client ens demana el seguent la xarxa d'administració (p.ex., 192.168.56.0/24) hi pugui escriure, però que la xarxa de consultors (p.ex., 192.168.56.100) només pugui llegir.

Per poder fer això haurem de modificar l'arxiu /etc/exports i substituir la linia "/srv/nfs/dev_projects *(rw,sync,no_subtree_check)" per les seguents 

```bash
/srv/nfs/dev_projects 192.168.56.0/24(rw,sync,no_subtree_check)
/srv/nfs/dev_projects 192.168.56.100/24(ro,sync,no_subtree_check)
```

Això ho fem per poder assignar permisos depened de la ip que tingui l'usuari
