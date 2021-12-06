# Configurer correctement Docker sur vos serveurs

La semaine passée nous avions vu quelles [mécanismes existent pour sécuriser docker](https://blog.stephane-robert.info/post/docker-rootless-securite-seccomp-namespace-capabilities/). Aujourd’hui je vous propose un billet sur quelques règles d’installation de Docker pour renforcer son fonctionnement.

## Créer une partition dédiée pour Docker

Docker utilise par défaut le répertoire `/var/lib/docker` pour stocker tous ces fichiers dont les images. Souvent /var est une partition de petite taille et vous pourriez rapidement atteindre sa limite de stockage, ce qui rendrait votre serveur indisponible. Vous devez créer une partition distincte pour Docker.

### Contrôle

```bash
sudo mountpoint -- "$(docker info -f '{{ .DockerRootDir }}')"
/var/lib/docker is not a mountpoint
```

### Remède

Lors de la création du serveur veuillez bien à créer une partition distincte de taille suffisante pour le point de montage `/var/lib/docker`. Si c’est déjà installé, stopper docker, renommer le dossier existant, créer la partition et la monter. Ensuite déplacer les donnée de l’ancien répertoire dans le nouveau.

Un exemple avec un second disque vide qur je viens de créer sur ma VM :

```bash
lsblk -l | grep disk
sda              8:0    0  129G  0 disk
sdb              8:16   0   25G  0 disk

docker system prune -f --volumes
sudo systemctl stop docker
sudo mv /var/lib/docker /var/lib/docker.old
sudo pvcreate /dev/sdb
sudo vgcreate docker /dev/sdb
sudo lvcreate --size 20G docker
sudo mkfs.ext4 /dev/docker/lvol0
sudo mv /var/lib/docker /var/lib/docker.old
sudo mkdir -p /var/lib/docker
sudo mount -t ext4 /dev/docker/lvol0 /var/lib/docker errors=remount-ro
echo '/dev/docker/lvol0 /var/lib/docker ext4 errors=remount-ro 0 1' | sudo tee -a /etc/fstab
sudo mv /var/lib/docker.old/* /var/lib/docker
sudo rm -f /var/lib/docker.old
sudo systemctl start docker
df -h /var/lib/docker
Filesystem                Size  Used Avail Use% Mounted on
/dev/mapper/docker-lvol0   20G  5.4G   14G  29% /var/lib/docker
```

## Désactiver les communications entre container

Il est possible de couper les communications entre les containers qui va isoler tous les containers par défaut. Si des containers doivent communiquer entre eux il faudra l’indiquer explicitement avec l’option `--link` de docker ou le paramètre `link` d’un docker compose.

### Contrôle

Il suffit de lancer cette commande :

```bash
docker network ls --quiet | xargs docker network inspect --format '{{ .Name }}: {{ .Options }}' |grep enable_icc:true
bridge: map[com.docker.network.bridge.default_bridge:true com.docker.network.bridge.enable_icc:true com.docker.network.bridge.enable_ip_masquerade:true com.docker.network.bridge.host_binding_ipv4:0.0.0.0 com.docker.network.bridge.name:docker0 com.docker.network.driver.mtu:1500]
```

Si ca retourne une ligne c’est que les communications entre containers sont activées.

### Remède

Il suffit d’éditer le fichier `/etc/docker/daemon.json` et d’y ajouter `"icc": false`

```bash
cat /etc/docker/daemon.json
{
  "registry-mirrors": [],
  "insecure-registries": [],
  "debug": false,
  "experimental": false,
  "features": {
    "buildkit": true
  }
}

sudo vi /etc/docker/daemon.json

{
  "registry-mirrors": [],
  "insecure-registries": [],
  "debug": false,
  "experimental": false,
  "features": {
    "buildkit": true
  },
  "icc": false
}
sudo systemctl restart docker
```

## Activer le support des user namespace

Vu dans mon précédent billet. Les user namespaces permettent de mapper un user du container vers un utilisateur différent sur la machine hôte. Concrètement, à l’intérieur le container pense être root, mais en fait sur la machine hôte l’utilisateur n’est en fait qu’un utilisateur lambda aux droits limités.

### Contrôle

Il suffit de contrôler que le user dockremap existe sur le système :

```bash
sudo id dockremap
```

Si aucune ligne est retournée c’est que ce n’est pas activé.

### Remède

Il suffit d’éditer le fichier `/etc/docker/daemon.json` et d’y ajouter `"userns-remap": default`

```bash
cat /etc/docker/daemon.json
{
  "registry-mirrors": [],
  "insecure-registries": [],
  "debug": false,
  "experimental": false,
  "features": {
    "buildkit": true
  }
  "icc": false
}

sudo vi /etc/docker/daemon.json

{
  "registry-mirrors": [],
  "insecure-registries": [],
  "debug": false,
  "experimental": false,
  "features": {
    "buildkit": true
  },
  "icc": false,
  "userns-remap": "default"
}
sudo systemctl restart docker
sudo id dockremap

uid=973(dockremap) gid=971(dockremap) groups=971(dockremap)
```

Vous pouvez aussi mettre votre username au lieu de default, cela évitera les pb de montage de volumes.

Avant de faire cette opération mettre à blanc docker. `sudo docker system prune -f -a`. Sinon vous atteindrez rapidement la limite de votre espace disque sur `/var/lib/docker`! Le test de point de montage va échouer car docker va créer un répertoire dans `/var/lib/docker`. Mais c’est pas grave puisque que le répertoire est bien séparé.

### Activation du live restore

Cette option permet à vos containers de continuer à fonctionner même si vous arrêtez le daemon Docker. Cool çà.

### Activation

Comme précédemment :

```bash
cat /etc/docker/daemon.json
{
  "registry-mirrors": [],
  "insecure-registries": [],
  "debug": false,
  "experimental": false,
  "features": {
    "buildkit": true
  }
  "icc": false,
  "userns-remap": "default"

}

sudo vi /etc/docker/daemon.json

{
  "registry-mirrors": [],
  "insecure-registries": [],
  "debug": false,
  "experimental": false,
  "features": {
    "buildkit": true
  },
  "icc": false,
  "userns-remap": "default",
  "live-restore": true
}
sudo systemctl restart docker
```

## Désactiver l’élévation de privilège

Docker possède un mécanisme permettant d’empêcher aux processus fonctionnant à l’intérieur du conteneur d’obtenir de nouveaux privilèges. Cette option n’est pas activé par défaut!

### Activation

Comme précédemment :

```bash
cat /etc/docker/daemon.json
{
  "registry-mirrors": [],
  "insecure-registries": [],
  "debug": false,
  "experimental": false,
  "features": {
    "buildkit": true
  }
  "icc": false,
  "userns-remap": "default",
  "live-restore": true
}

sudo vi /etc/docker/daemon.json

{
  "registry-mirrors": [],
  "insecure-registries": [],
  "debug": false,
  "experimental": false,
  "features": {
    "buildkit": true
  },
  "icc": false,
  "userns-remap": "default",
  "live-restore": true,
  "no-new-privileges": true
}
sudo systemctl restart docker
```

## Activation de l’audit sur les fichiers et répertoires Docker

Je vous conseille d’activer l’audit au niveau du système sur les principaux répertoires Docker. L’audit enregistre toutes les opérations qui affectent les fichiers et répertoires surveillés.

### Activation

Il suffit d’ajouter ces lignes au fichier `/etc/audit/rules.d/audit.rules`

```bash
cat <<EOF | sudo tee -a /etc/audit/rules.d/audit.rules
-w /usr/bin/dockerd -k docker
-w /etc/docker -p rwxa -k docker
-w /etc/default/docker -p rwxa -k docker
-w /etc/docker/daemon.json -p rwxa -k docker
-w /var/lib/docker -p rwxa -k docker
-w /usr/lib/systemd/system/docker.service -p rwxa -k docker
-w /usr/lib/systemd/system/docker.socket -p rwxa -k docker
-w /usr/bin/docker-runc -p rwxa -k docker
-w /usr/bin/docker-containerd -p rwxa -k docker
-w /usr/bin/containerd -p rwxa -k docker
EOF

sudo /sbin/service auditd restart
```

## Désactivation du userland proxy

Alors là attention j’ai déjà eu des soucis en désactivant cette fonctionnalité. Donc je ne la documente pas volontairement.

Je vais écrire un playbook Ansible.

Source [Projet Atomic](https://projectatomic.io/docs/docker-storage-recommendation/)