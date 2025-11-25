# T01 INFORME

## 1. Què s’ha de copiar? (Priorització)

Les dades del servidor no tenen totes la mateixa importància, per això cal establir un ordre de prioritat clar.

### Bases de Dades (Comptabilitat i Clients) – 20 GB
- Són les dades més sensibles i s’utilitzen cada dia.
- Es modifiquen constantment, fet que obliga a fer còpies molt freqüents.
- **RTO:** menys de 4 hores  
- **RPO:** màxim 4 hores de treball perdut.

### Documents de Projectes – 300 GB
- Informació essencial, però amb canvis menys continus.
- **RPO:** pot ser de 24 hores.

### Carpetes personals dels usuaris – 100 GB
- Necessàries, però amb un nivell de criticitat inferior.
- **RPO:** 24 hores és suficient.

### Cal fer còpia dels equips clients?
Els treballadors utilitzen principalment el servidor, així que no és imprescindible copiar tot el contingut dels 10 PCs.  
Només seria necessari fer còpia d’algunes carpetes locals (com *Documents*) si l’empresa confirma que s’hi guarden fitxers importants de manera temporal.

Una alternativa recomanable és sincronitzar automàticament aquestes carpetes amb el servidor, evitant haver de fer còpies independents de cada equip.

### Conclusió
Les còpies essencials són:
- Documents de Projectes  
- Bases de Dades  
- Carpetes personals  

Els clients només cal copiar-los si contenen dades crítiques puntuals, tot i que és preferible centralitzar aquests fitxers al servidor.

---

## 2. Periodicitat i Tipus de Còpia

### Bases de Dades
- **Diari:** Incremental cada 4 hores  
- **Setmanal:** Completa  
- **Mensual:** Completa amb retenció prolongada

### Documents de Projectes
- **Diari:** Incremental  
- **Setmanal:** Diferencial (restauració més ràpida)  
- **Mensual:** Completa

### Carpetes Personals
- **Diari:** Incremental  
- **Setmanal:** Diferencial  
- **Mensual:** Completa

### Resum
| Dades                | Diari                    | Setmanal      | Mensual  |
|---------------------|---------------------------|---------------|----------|
| Bases de Dades      | Incremental (cada 4 h)    | Completa      | Completa |
| Documents Projecte  | Incremental               | Diferencial   | Completa |
| Carpetes Personals  | Incremental               | Diferencial   | Completa |

---

## 3. Mitjans i Ubicació (Regla 3-2-1)

### Mitjans recomanats
- **NAS local** per a les còpies diàries: ofereix rapidesa i permet complir el RTO.
- **Còpia al núvol** per protegir-se davant desastres o robatori físic.
- **Discs durs externs** per a còpies mensuals o d’arxiu.

Les cintes no són necessàries per una empresa d’aquest volum.

### Aplicació de la regla 3-2-1
- **3 còpies**: servidor + NAS local + còpia externa (núvol o disc dur fora del recinte).  
- **2 suports diferents**: NAS i Cloud, o NAS i disc extern.  
- **1 còpia fora de l’empresa**: preferiblement al núvol.

### Ubicació de la còpia més recent
- **Còpia diària:** al NAS local per garantir una restauració ràpida.  
- **Còpies setmanals i mensuals:** al núvol o en un disc extern guardat fora de l’empresa.

