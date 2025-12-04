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

![imatge dels grups](img/1.png)

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

![imatge dels usuaris](img/2.png)

Un cop que ja hem creat els grups i els usuaris, el seguent pas sera crear el directori per als projectes de desenvolupament en la qual la ruta que ens demana és la seguent /srv/nfs/dev_projects, per crear les totes les carpetas d'una sola comanda farem el seguent:

```bash
mkdir /srv/nfs/dev_projects -p
```

Un cop fet això crearem el directori per a les eines d'administració en la qual la ruta sera /srv/nfs/admin_tools

```bash
mkdir /srv/nfs/admin_tools
```

![Creació de carpetas](img/3.png)
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

![Permisos de la carpeta](img/4.png)

Avans de continuar amb el servidor crearem els grups i usuaris dins de la maquina client, en aquest cas una maquina zorin



Un cop fet això instalarem els paquets neccesairs del servei NFS al servidor
