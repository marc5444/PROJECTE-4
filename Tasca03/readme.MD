# Introducció al cas

Recordeu el cas del portàtil al qual no es podia accedir? En aquella situació, la vostra habilitat tant per recuperar l’accés com per reforçar-lo posteriorment va impressionar el client. Per aquest motiu, se us ha requerit per a un **nou encàrrec**.

El client ha demanat l’elaboració d’un **Pla de Contingència i Continuïtat del Negoci**. Dins d’aquest pla cal posar en marxa un **Pla de Recuperació davant Desastres (DRP o Disaster Recovery Plan)**, que inclou tots els processos necessaris per restaurar dades, hardware i software crític en cas d’un esdeveniment catastròfic, amb l’objectiu de recuperar l’activitat normal el més ràpid possible.

Un aspecte clau del pla és assegurar que els treballadors puguin disposar ràpidament dels seus equips de treball en cas de robatori, avaria, etc. Per això, es demana la **creació d’imatges de restauració del sistema**, ja que el temps de posada en marxa és crític i no és viable reinstal·lar el sistema, configurar-lo i instal·lar les aplicacions manualment.

Tots els equips del client funcionen amb **GNU/Linux**, concretament **Zorin OS 18**, amb aplicacions ja configurades.

---

## Fase 1: Anàlisi i justificació de la solució tècnica

En aquesta primera fase cal investigar diverses eines que permetin **crear una imatge completa del disc d’un equip**, que pugui ser restaurada posteriorment mantenint configuracions i aplicacions. Existeixen opcions comercials i de comunitat que resolen aquesta necessitat.  

La tasca consisteix a elaborar una **comparativa entre 4 productes** (2 comercials i 2 de comunitat), incloent:
- Característiques destacades.
- Preu.
- Justificació de la solució proposada, raonant la tria tenint en compte els aspectes analitzats.

---

## Fase 2: Guia d’ús tècnica (Manual Operatiu)

A partir de la màquina proporcionada pel client (simulada amb una OVA), cal documentar els processos següents:  
- Crear una **imatge completa del sistema**.  
- Restaurar aquesta imatge sobre un sistema net, simulat com una màquina virtual idèntica a l’original (RAM, processador, xarxa i mida de disc), però sense sistema operatiu instal·lat.  

Cal elaborar una **guia tècnica** que permeti al personal de manteniment realitzar aquestes accions, documentant de forma clara i incorporant captures significatives.  

Com a prova de concepte i per a l’elaboració de la guia, es farà servir **Rescuezilla**.
