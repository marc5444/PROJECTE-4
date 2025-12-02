# 1\) DADES OBJECTE DE CÒPIA

Tenim 2 tipus de dades, les dades crítiques i les dades no critiques.

Per un lloc tenim les dades critiques del servidor ubuntu, que son la base de dades de comptabilitat i dels clients. Aquestes dades tindren una copia completa a la setmana i una copia incremental, això ho farem per si hi ha alguna perdua sigui inferior a 4 hores de treball.

En el mateix servidor tenim les dades no critiques, els documents de projecta i les carpetas personals dels usuaris. Aquets arxius tindran una copia completa a la setmana i una copia incremental a las 11 de la nit (23:00)

Per un altre lloc tenim les dades no critiques dels equips personals (Windows), aquets documents son la carpeta documents on hi ha els informes temporals dels tècnics, aquestes dades tindren una copia completa a la setmana i una incremental al dia

# 2\)      Cronograma Setmanal Detallat

| Dia | Dades | Tipus de Còpia | Mitjà |
| :---- | :---- | :---- | :---- |
| Dilluns | BD | CI cada 4h | NAS |
|  | PRJ \+ USR \+ CLI | CI diària (nit) | NAS |
| Dimarts | BD | CI cada 4h | NAS |
|  | PRJ \+ USR \+ CLI | CI diària | NAS |
| Dimecres | BD | CI cada 4h | NAS |
|  | PRJ \+ USR \+ CLI | CI diària | NAS |
| Dijous | BD | CI cada 4h | NAS |
|  | PRJ \+ USR \+ CLI | CI diària | NAS |
| Divendres | BD | CI cada 4h | NAS |
|  | PRJ \+ USR \+ CLI | CI diària | NAS |
| Dissabte | BD | CI cada 4h | NAS |
|  | PRJ \+ USR \+ CLI | Replicació al CLOUD (diària) | CLOUD |
| Diumenge | BD | CC setmanal | NAS \+ CLOUD |
|  | PRJ \+ USR \+ CLI | CC setmanal | NAS \+ CLOUD |
|  | Totes les dades | CC mensual (1r diumenge de mes) | CLOUD (arxiu mensual) |

Llegenda

**CC** \= Còpia Completa    **CI** \= Còpia Incremental   **BD** \= Bases de dades (crítiques)

**PRJ** \= Projectes   **USR** \= Carpetes personals   **CLI** \= Equips clients

**NAS** \= Emmagatzematge local   **CLOUD** \= Proveïdor extern

# 3\)      Elecció de Mitjans i Ubicació (Regla 3-2-1)

Mitjà 1

La primera copia la tindrem en local en el NAS, aquesta copia la tindrem per guardar totes les còpies incrementals  i completes setmanals i per poder permetre una restauració rapida per poder complir el RTO de 4 hores.

Mitjà 2 

La segona copia la tindrem en el nuvol, aquet 2 el farem servir principalment per poder guardar les copies setmanals i les copies mensuals en format d’arxiu per poder estalbiar espai i tindre una protecció davant de algun desatre com per exemple un incendi, un robatori…

Mitjà 3

Per ultim la ultima copia la tindrem un disc individual i separat que tindre l’administrador de sistemes per si en algun moment hi ha qualsevol problema i no podem fer servir el mitjà 1 i 2 tindrem un disc dur per poder evitar perdre les dades de l’empresa.

# 4\) ESTRATÈGIA DE RECUPERACIÓ (RTO/RPO)

Garantia del RTO (Recovery Time Objective)

El NAS local en permetra poder recuperar les dades gracies a la xarxa.

Ja que és una base de dades petita, el temps que trigara en restaurar les dades sera inferior a 4 hores.

Per poder recuperar les dades els pasos que haurem de seguir seran els seguents

1. Seleccionar última còpia del NAS.  
2. Restaurar BD en servidor.  
3. Verificar integritat i reiniciar servei.

Tot aquest procés tindra un temps inferior a 4 hores

Garantia del RPO (Recovery Point Objective)

Per poder complir el RPO de 4 hores haurem d’utilitzar les copies incrementals que hem realitzar cada 4 hores per les dades critiques.

Per poder fer servir aquestes copies haurem de realitzar el seguent.

Restaurar la ultima copia setmanal, i restaurar totes les copies incrementals fins arribar a l moment en el qual hem perdut les dades, això a causa de que les copies incrementals unicament tindran les dades que s’han modificat dins de les 4 hores,

Això ho farem per poder assegurar una perdua de dades inferior a 4 hores
