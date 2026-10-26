### Tasca 1. Creació de la jerarquia de grups

1. Crea els següents grups nous al sistema:

    - devs (per al personal de desenvolupament)
    - sysadmin (per al personal de sistemes)
    - auditor (per a l'usuari auditor extern)

![imatge](/01_img/1.png)

2. Comprova que els grups s'han creat correctament consultant les últimes línies del fitxer `/etc/group`. Identifica clarament el GID de cadascun dels tres grups.

![imatge](/01_img/2.png)

### Tasca 2. Gestió dels perfils d'usuari

1. L'empresa vol que tots els usuaris nous que es donin d'alta a partir d'ara tinguin automàticament:

    - Un fitxer de benvinguda al seu directori personal amb les normes d'ús del servidor.
    - Una carpeta anomenada Documents creada per defecte.
    - Un alias configurat al seu terminal perquè quan escriguin `ll` s'executi `ls -la --color=auto`.

![imatge](/01_img/3.png)

![imatge](/01_img/4.png)

![imatge](/01_img/5.png)


    Per fer aconseguir aquestes personalitzacions, cal que modifiqueu el directori plantilla `/etc/skel` amb els fitxers i configuracions necessàries.

![imatge](/01_img/6.png)


2. Ajust de configuració per defecte (/etc/adduser.conf):

    - Revisa el fitxer `/etc/adduser.conf` i assegura't que la shell per defecte per als nous usuaris sigui `/bin/bash` (DSHELL=/bin/bash).

![imatge](/01_img/7.png)

3. Comprovem el funcionament dels canvis creant un un usuari de prova `provaXX`(on XX és el vostre número de llista) amb la comanda `adduser`. Inicia sessió amb aquest usuari i verifica que el seu directori personal conté el fitxer de benvinguda, la carpeta Documents i que l'alias `ll` funciona correctament. Un cop fetes les comprovacions, tornem a la sessió de l'usuari administrador i eliminem l'usuari de prova amb la comanda `deluser -r provaXX`.

![imatge](/01_img/8.png)


### Tasca 3. Creació i configuració d'usuaris

1. Crea els següents usuaris amb les característiques indicades:

| Nom d'usuari | Nom real       | Grup principal | Grups secundaris | Directori personal | Shell     |
|---           |---             |---             |---               |---                 |---        |
| pau_dev      | Pau Garcia     | devs           | —                | /home/pau_dev      | /bin/bash |
| laura_dev    | Laura Martí    | devs           | —                | /home/laura_dev    | /bin/bash |
| marc_sys     | Marc Soler     | sysadmin       | devs             | /home/marc_sys     | /bin/bash |
| auditor      | Usuari Auditor | auditor        | —                | /home/auditor      | /bin/bash |

Instruccions específiques:

1. Utilitza la comanda `adduser` per crear els usuaris pau_dev i laura_dev.

2. Utilitza la comanda `useradd` amb els paràmetres corresponents per crear marc_sys i auditor.

3. Assigna a tots els usuaris la contrasenya inicial `Canviam2026!`.

4. Obliga l'usuari pau_dev a canviar la contrasenya en el seu pròxim inici de sessió (chage).

5. Inicia sessió com pau_dev verifica que cal canviar la contrasenya.

![imatge](/01_img/9.png)

![imatge](/01_img/10.png)

### Tasca 4. Inspecció del sistema i fitxers de configuració

Respon a les següents preguntes mostrant la informació corresponent dels fitxers del sistema:

1. Executa la comanda `id marc_sys`. Quins UID, GID i grups té assignats?

![imatge](/01_img/11.png)


2. Quina diferència hi ha entre la informació de l'usuari pau_dev a `/etc/passwd` i la informació del fitxer `/etc/shadow`? Quin tipus de permisos té el fitxer `/etc/shadow` i per què?

![imatge](/01_img/12.png)

![imatge](/01_img/13.png)


### Tasca 5. Manteniment, bloqueig i eliminació

1. El treballador pau_dev marxa de vacances. Bloqueja el seu compte perquè no pugui iniciar sessió temporalment.

![imatge](/01_img/14.png)

2. Comprova que el compte està bloquejat mirant el fitxer `/etc/shadow`.

![imatge](/01_img/15.png)

3. Desbloqueja de nou el compte de pau_dev.

![imatge](/01_img/16.png)

4. L'usuari auditor ha finalitzat la seva feina i deixa l'empresa. Elimina l'usuari auditor juntament amb el seu directori personal i la seva bústia de correu.

![imatge](/01_img/17.png)

### Tasca 6. Personalització de l'entorn d'usuari

Hem vist a la tasca 2 com es pot predeterminar el shell pels usuaris nous que es creen. Ara bé, un cop definits, podem canviar el shell d'un usuari concret amb la comanda `usermod` o `chsh`.

1. Instal·la el paquet `fish`.

![imatge](/01_img/18.png)

2. Canvia el shell per defecte de l'usuari laura_dev a `fish`. Comprova a l'arxiu `/etc/passwd` que el shell s'ha canviat correctament.

![imatge](/01_img/19.png)

3. Inicia sessió com laura_dev (usant `su - laura_dev`) i comprova que el shell és `fish`.

![imatge](/01_img/20.png)
