# linux_server_notater

Dette repoet er ment som et samlet notat for å huske hvordan jeg skal sette opp linux servere de 
gangene jeg driver med sånnt. Finnes mange "best practices" når det gjelder sikkerhet der ute, og har prøvd å samle de jeg mener gir en reell sikkerhetgevinst her. 

Notatet tar utgangspunkt i en ubuntu bassert server. Parsonlig bruker jeg Oracle free tier VPS-er til diverse prosjekter. 

## Grunnleggende serveroppsett

### Holde greier oppdatert
> [!NOTE]
> Kan føre til kompatibilitetsproblemer med applikasjonen, eller på andre måter hindre applikasjonen å kjøre som normalt.


**Oppdater programvare ed oppsett av server:**

`sudo apt update && sudo apt upgrade`

**Installer verktøyet unattended-upgrades:**

`sudo apt install unattended-upgrades`

**Configurer automatiske sikkerhetsoppdateringer av systemet:**

`sudo dpkg-reconfigure --priority=low unattended-upgrades`

Programmet skjekker daglig for sikkerhetsoppdateringer. Godkjenn deretter endringene. Du finner configurasjonsfiler i `/etc/apt/apt.conf.d/` med navn `20auto-upgrades` og `50auto-upgrades` som kan endres for mer spesifikke behov.

> [!WARNING]
> Docker-images oppdateres ikke selv om server-oppreativsystemet gjør. Bruk aktivt vedlikeholdte images, og husk å holde docker-images oppdatert!

### Sikker innlogging med SSH
Ikke så mye å si annet enn: **Bruk SSH med private og offentlig ssh-nøkler**

**Generer nøkkel med Ed25519 algoritmen (veldig sikker)**

`ssh-keygen -t ed25519 -C "your_email@example.com"`

Se [githubs SSH-guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) for detaljer. 

Nøklene lagres i `~/.ssh` (kanskje `~/.ssh/id_etellerannet`). Lagre den private nøkkelen på et sikkert sted. Ikke del!

#### Kopier offentlig nøkkel til server
På server: 
```bash
cd /home/dittbrukernavn/
mkdir .ssh
```
På din pc: 
```bash
scp din_ssh_nøkkel.pub root@dittdomenenavn.no:/home/dittbrukernavn/.ssh/authorized_keys
```
`root@dittdomenenavn.no = ubuntu@dittdomenenavn.no` på Oracle ubuntu server. 

**Sett eierrettigheter rekursivt for .ssh mappen**

`chown -R dittbrukernavn:dittbrukernavn .ssh`

### Lag en egen bruker for serveren

`useradd dittbrukernavn -m -s /bin/bash -c "admin user"`

**Legg bruker til i administratorgruppe**

`usermod -aG sudo,adm,docker,docker-compose dittbrukernavn`

**Gi brukeren passord med:**

`passwd dittbrukernavn `


## Sikkerhet






## Videoressurser
[Christian Lempa - How to protect Linux from hackers](https://www.youtube.com/watch?v=Bx_HkLVBz9M&list=WL)
