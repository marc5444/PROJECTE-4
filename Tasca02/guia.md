# Part 1: Còpia seguretat dels equips clients Windows

Per comneçar el primer pas sera tindre una maquina client windows amb un disc principal on tindrem el sistema operatiu i despres un segon disc de 10 Gb en el qual farem la copia.

![configuració del segon disc](img/1.png)

Un cop fet això el seguent pas sera inicialitzar el disc, per fer això el primer pas sera inicar la maquina i obrir l'administrador de disc.

![administrador de disc](img/2.png)

Un cop fet això el que farem sera crear un volum simple en el disc, per fer-ho hem de fer click dret sobre el disc i escollirem la opcio de "Nuevo volumen simple"

Un cop fet això s'obrira una finestra en la qual unicament farem siguiente siguiente fins que tinguem el volum creat i es vegi tal i aixì

![Crear volum simple](img/3.png)

El seguent pas sera instalar duplicati, per fer això anirem al seguent enllaç

[Link de descarrega](https://duplicati.com/download)

Instalarem la versio per el nostre equip, en aquest cas instalarem la versió per windows

![Crear volum simple](img/4.png)

Un cop que tinguem instalat el archiu.exe l'executarem i farem siguiente siguiente, aceptant els termes neccesaris fins que tinguem el duplicati instalat correctament

![Instalació duplicati](img/5.png)

Un cop instalat, s'obrira una pestanya en el nostre navegador en el qual ens demanara que escollim una constrasenya per fer servir duplicati.

En la qual podem escollir qualsevol contrasenya ja que sera personal, important no olvidar-la

![duplicati](img/6.png)


Un cop que ja tenim el duplicati, crearem alguns documents de proba per poder fer la copia de seguretat

![Creació de documents](img/8.png)

El seguent pas sera configurar les copies de seguretat que voldrem, aixi que começarem escollint la opcio de backup add i add new backup,

Un cop aqui haurem de colocar les dades de la nostra copia 

![duplicati](img/7.png)

El seguent pas sera escollir on tindrem guardada la copia, en aquest cas la guardarem en el nostre cas la guardarem en el disc secundari

![Escollir lloc](img/9.png)

Un cop fet això hem d'escollir quins documents volem fer la copia

![Escollir documents](img/10.png)

Tot seguit haurem d'escollir cada quan de temps volem que és realitzi la copia, en aquest cas la farem cada 1 hora 

![Escollir temps](img/11.png)

Un cop que arribem aqui escollirem les opcions que en demani i un cop fet, ja tindrem la nostra copia creada

![opcions](img/12.png)

El proxim pas sera fer el mateix però amb l'unic canvi que farem la copia en el nuvol, en aquest cas farem servir el google drive, per fer-ho farem el seguent però unicament modificarem el seguent.

El lloc en el qual guardarem la copia, en la qual haurem de vincular la conta, per fer això farem click a AuthID

![googledrive](img/13.png)

L'altre cosa que haurem d'editar sera cada quan és fa la copia, en aquest cas sera cada dia a las 6 de la tarda

![Horari](img/14.png)

Un cop fet aixo ja tindrem les dues copies creades

![copies creades](img/15.png)

El seguent pas que farem sera borrar els documents de proba per poder comprobar que podem recuperar la copia correctament 

![Carpeta documents](img/16.png)

Un cop que ja hem borrat els documents, el seguent pas sera recuperar la copia, per poder fer això anirem a "Restores" i farem click a "Start"

Escollirem la copia que volem restaurar, en aquest cas proba

![Escollir copia](img/17.png)

escollirem els fitxers que volem restaurar, en aquest cas la carpeta documents

![Escollir documents](img/18.png)

Farem siguiente siguiente i un cop fet això ja tindrem feta la restauració 

![Restauració feta](img/19.png)

![Restauració feta](img/20.png)

Per poder restaurar la copia que hem fet a google drive ho farem igual però selecionarem l'altre copia

---

Part 2: Còpia seguretat servidor Linux

Per poder començar haurem de tindre una maquina linux, en aquest cas farem servir una maquina ubuntu server per realitzar la prova.

El primer pas sera tindre la maquina actualitzada per tant farem 

```bash
sudo apt update && sudo apt upgrade -y 
```

Farem servir el Duplicity  per poder fer copies en local i en remot.

Per començar haurem de tindre un disc secundari, en aquest cas farem servir un disc de 10 Gb

Un cop que ja tenim el disc, el primer pas sera donar-li un format, li donarem el format xfs i muntarem al disc a la ruta /media/backup com que aquesta ruta no existeix, el primer pas sera crear la ruta, això ho farem de la seguent forma

```bash
mkdir /media/backup 
```

![carpeta de backup](img/21.png)

Un cop fet això haurem de donar-li format al disc, això ho farem amb l'eina fdisk, per tant el primer pas sera 

```bash
sudo apt install fdisk
```
Un cop fet això farem fdisk -l per veure el disc que tenim

```bash
fdisk -l
```

![Comanda fdisk -l](img/22.png)

Podem veure que el segon disc esta, per tant el seguent pas sera crear el seu volum, per fer això farem servir la comanda pvcreate, per poder fer servir aquesta comanda primer haurem d'instalar lvm2 

```bash
sudo apt install lvm2
```

Un cop fet això ja podrem crear el volum en aquest cas sera amb la seguent comanda

```bash
pvcreate /dev/sdb
```

![Crear el volum](img/23.png)

Un cop que ja tenim el volum muntat el seguent pas sera formategar-lo amb la seguent comanda

```bash
mkfs.xfs -f /dev/sdb
```

![Formategar el volum](img/24.png)

Per ultim tocara muntar el disc a la carpeta que hem crear previament, per fer axò farem la seguent comanda

```bash
mount /dev/sdb /media/backup
```
![Muntar el disc](img/25.png)

Un cop que ja tenim el disc muntat el seguent pas sera instalar Duplicity

```bash
apt install duplicity -y
```

Un cop fet això el seguent pas sera crear un parell d’usuaris amb carpeta personal i crear 4 arxius de 10 MB a la carpeta home del teu usuari.

per fer això ho farem de la seguent forma

```bash
useradd -m -s /bin/bash user1
```

```bash
useradd -m -s /bin/bash user2
```
![Crear usuaris](img/26.png)
![Comprobar usuaris](img/27.png)

```bash
fallocate -l 10MB file1
fallocate -l 10MB file2
fallocate -l 10MB file3
fallocate -l 10MB file4
```
![Comprobar arxius](img/28.png)


Un cop que ja tenim els arxius de prova creats els seguent pas sera fer la copia de seguretat, això ho farem amb el duplicity

La comanda per fer una copia de seguretat completa de la carpeta home sera la seguent 

```bash
duplicity full /home/user file:///media/backup/
```
En el moment en que ens demana el passphrase podem escollir el que nosaltres volguem, ja que és una prova en el meu cas he escollit copia

![Fer la copia](img/29.png)

Podem veure que la copia s'ha creat correctament en el disc secundari, això amb la comanda ls

![veure la copia](img/30.png)

Un cop fet això el seguent pas sera esborrar els arxius i comprobar que funcionen correctament

![Esborrar arxius](img/31.png)

Un cop que ja no estan els arxius farem servir la copia per recuperar-los, en aquest cas guardarem la copia dins d'una carpeta que es diu copia dins de la carpeta ```/home/user``` això ho farem amb la seguent comanda:

```bash
duplicity restore file:///media/backup/ /home/user/copia
```

![Copia feta](img/32.png)

A continuació farem una copia incremental, per començar crearem un arxiu de 4mb
