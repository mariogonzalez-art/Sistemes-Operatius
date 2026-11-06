# NF1UD4 - Compartició d'arxius i carpetes

## Fase 1: Preparació de l'entorn

**SERVER:**

![](./02_img/server_amfitrió.png)

![](./02_img/server_xarxaNAT.png)

**CLIENT:**

![](./02_img/zorin_xarxaNAT.png)


**PINGS PER REVISAR CONNEXIÓ:**

![](./02_img/zorin_ping_internet.png)

![](./02_img/server_ping_internet.png)

## Fase 2: Preparació del servidor amb SSH
 
Ens connectem per SSH al servidor:

![](./02_img/ssh_server.png)


Creem els grups devs i admins

```bash
sudo groupadd devs
sudo groupadd admins
```

![](./02_img/3.png)

Creem el usuari dev01 al grup devs

```bash
sudo useradd -m -g devs dev01
```

![](./02_img/4.png)

Creem l'usuari admin01 al grup admins

```bash
sudo useradd -m -g admins admin01
```

![](./02_img/5.png)

Creem els directoris /srv/nfs/dev_projects i /srv/nfs/admin_tools

```bash
sudo mkdir -p /srv/nfs/dev_projects /srv/nfs/admin_tools
``` 

![](./02_img/6.png)

Configurem els permisos dels grups de devs i admins que els dos han de ser root i que els devs tinguin el control sobre els projectes i a els admins a les eines.

Propietari root i grup devs per a projectes de desenvolupament:

```bash
sudo chown root:devs /srv/nfs/dev_projects
```

![](./02_img/7.png)

Propietari root i grup admins per a eines d'administració:

```bash
sudo chown root:admins /srv/nfs/admin_tools
```

![](./02_img/8.png)

Utilitzem la comanda chmod per configurar els permisos de cada prup i per els directoris per configurar el control total

De devs:

```bash
sudo chmod 770 /srv/nfs/dev_projects
```

![](./02_img/9.png)

De Admins:

```bash
sudo chmod 770 /srv/nfs/admin_tools
```

![](./02_img/10.png)

Instal·lem els paquets necesaris per el servei NFS

```bash
sudo apt update
sudo apt install nfs-kernel-server -y
```

![](./02_img/11.png)

![](./02_img/12.png)

Configurem les exportacions que haurem d'editar l'arxiu /etc/exports

```bash
 sudo nano /etc/exports
```

Posem el següent al final de l'arxiu per configurar les exportacions
```text
/srv/nfs/dev_projects  10.0.2.4/24(rw,sync,no_subtree_check,insecure)
/srv/nfs/admin_tools   10.0.2.4/24(rw,sync,no_subtree_check,insecure)
```

![](./02_img/13.png)

Fem les comprovacions per saber si estan ben configurades 

El primer es reiniciar el servei NSF

```bash
sudo systemctl restart nfs-kernel-server
```

Fem la comporvació

```bash
sudo showmount -e
```

El resultat hauria de ser:

```text
Export list for localhost:
/srv/nfs/admin_tools  10.0.2.4/24
/srv/nfs/dev_projects 10.0.2.4/24
```

![](./02_img/14.png)

---

### Fase 3: L'exportació d'administració (el dilema de `root_squash`)

El client necessita accedir a `/srv/nfs/admin_tools`. Aquesta fase mostra la configuració de les exportacions i les proves des del client.

1. Al servidor, configurar i recarregar les exportacions NFS.

   ![](./02_img/15.png)

   ```bash
   sudo exportfs -ra
   sudo exportfs -v
   ```

2. Al client, provar l'escriptura indicada a la captura.

   ![](./02_img/16.png)

    [](./02_img/17.png)


3. Al client, comprovar els recursos muntats.

    ![](./02_img/18.png)

   ```bash
   findmnt /mnt/admin_tools
   findmnt /mnt/dev_projects
   ```

### Fase 4: L'exportació de desenvolupament (permisos `rw` vs `ro`)

1. Al client, provar de muntar el recurs NFS de desenvolupament.

![](./02_img/19.png)

2. Com a `dev01`, provar d'escriure al directori compartit.

  ![](./02_img/20.png)

3. Com a administrador al client, comprovar la denegació d'escriptura.

![](./02_img/21.png)


4. Es pot desmuntar el recurs amb:

![](./02_img/22.png)

5. Al servidor, assignar els grups propietaris com es mostra a la captura.

![](./02_img/23.png)


### Fase 5: Muntatge automàtic amb `/etc/fstab`

1. Configurar les entrades NFS al fitxer `/etc/fstab` del client.

![](./02_img/24.png)
`

2. Executar `mount -a` per provar les entrades sense reiniciar.

![](./02_img/25.png)

