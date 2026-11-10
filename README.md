# Docker Ansible role

Voor les 6 heb ik deze role gemaakt om Docker automatisch te installeren
op mijn Azure en ESXI VMs.

## Wat doet de role?

De role controleert eerst het OS en installeert daarna:

- `docker.io`: Docker Engine.
- `docker-compose-v2`: om containers met een Compose file te beheren.
- `docker-buildx`: om Docker images te maken.

Daarna start ik de Docker service en zorg ik dat deze aanblijft bij een restart. Als laatste controleert de role of Docker Compose werkt.

## Prereq

- Ubuntu 24.04
- Ansiblecore 2.19 of hoger
- SSH toegang
- Sudo rechten

## Variabelen

De standaardinstellingen staan in `defaults/main.yml`:

```yaml
docker_packages:
  - docker.io
  - docker-compose-v2
  - docker-buildx

docker_service_name: docker
docker_apt_cache_valid_time: 3600
```

Met `docker_apt_cache_valid_time` wordt de packakage list alleen vernieuwd
als de cache ouder is dan een uur.

## Gebruik

Na installatie van de role kan ik deze vanuit een playbook aanroepen

```yaml
---
- name: Docker installeren
  hosts: all
  become: true

  roles:
    - role: avi1505.docker
```

Met `become: true` voert Ansible de installatie uit met sudo rechten.

## Controle / Testen

Ik heb de role getest op twee Azure VMs en twee ESXI VMs.
Op alle vier werkt Docker Compose. Bij een run voor idempotency was overal `changed=0`.

## Auteur

GitHub: [avi1505](https://github.com/avi1505)

## Licentie

MIT