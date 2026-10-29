# lab-rpi5 — Raspberry Pi 5 + KDE Plasma (Ansible)

Provisionne un Raspberry Pi 5 (Raspberry Pi OS Lite) en bureau **KDE Plasma** via SSH/Ansible.

## Prérequis (machine de contrôle)

- Ansible (`brew install ansible`)
- `sshpass` (auth mot de passe) : `brew install hudochenkov/sshpass/sshpass`
- Collections : `ansible-galaxy collection install -r requirements.yml`

## Accès SSH (lab)

Auth **login / mot de passe** (pas de clé SSH) :

| | |
|---|---|
| Host | `192.168.1.234` |
| User | `louise` |
| Password | défini dans `group_vars/rpi5/secrets.yml` (fichier local, non versionné) |

```bash
cp group_vars/rpi5/secrets.yml.example group_vars/rpi5/secrets.yml
# éditer secrets.yml
```

## Déploiement

```bash
# Test de connexion
ansible rpi5 -m ping

# Installer base + KDE Plasma (redémarre le Pi à la fin)
ansible-playbook playbooks/site.yml
```

Sans redémarrage automatique :

```bash
ansible-playbook playbooks/site.yml -e reboot_after_install=false
```

## Contenu

- `roles/common` — apt upgrade, outils de base, NetworkManager, timezone
- `roles/kde_plasma` — `kde-plasma-desktop`, SDDM, session Plasma (Wayland si Trixie / Plasma 6)

Sur **Bookworm** → Plasma 5.27 ; sur **Trixie** → Plasma 6.
