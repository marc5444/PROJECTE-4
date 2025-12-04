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
