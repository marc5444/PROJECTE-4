Guia instalació SSH per linux i windows
Per poder començar amb la instalació necesitarem les 2 maquines, una linux i una windows.

Començarem amb la guia d'instalació SSH per linux

SSH linux
Un cop que hem instalat la maquina ubuntu colocarem la seguent comanda per poder actualitzar el sistema

sudo apt update && sudo apt upgrade -y 
Important

Si no hem instalat el ssh durant la instalacio de ubuntu farem la seguent comanda

sudo apt install openssh
Un cop que ja tenim instalat el ssh el seguent pas sera veure la nostre ip amb la seguent comanda

ip addr show
Un cop que executem aquesta comanda veurem el seguent:

comanda per veure la ip
