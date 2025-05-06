# Automaattisesti päivittyvät ohjelmat
Projektin tarkoituksena on luoda salt tila, jolla asennetaan ohjelmat LibreOffice, Mozilla Firefox, ja VLC minioneille. Lisänä Cronjob, jolla ajoitetaan viikottainen päivitys, jotta kaikki järjestelmään asennetut ohjelmistot pysyvät ajan tasalla

Käytän tässä projektissa valmiiksi luotuja ja konfiguroituja virtuaalikoneita: tmaster, t001 ja t002
## Asennus
#### asennus.sls
Ensimmäisenä loin masterille salt tilan, joka asentaa tarvittavat ohjelmistot minionille.

loin asennus.sls tiedoston hakemistoon /srv/salt

    libreoffice:
      pkg.installed

    firefox:
      pkg.installed

    vlc:
      pkg.installed

#### Automaattiset päivitykset
Automaattisia päivityksiä varten, loin tilana Cronjobin, ja konfiguroin sen päivittämään ohjelmistot kerran viikossa

    update:
      cron.present:
        - name: 'apt-update && apt upgrade -y'
        - user: root
        - minute: 59
        - hour: 23
        - dayweek: 0

#### TOP

Viimeisenä lisäsin vielä top.sls tilan, jotta voin ajaa molemmat tilat yhdellä komennolla. Tähän tiedostoon lisään tilat, mitkä haluan suorittaa.

    base:
      '*':
        - asennus
        - autoupdate

#### Suoritus

Viimeisenä kokeilen toimiiko ohjelmistojen asennus ja automaattipäivitysten lisääminen ajamalla top.sls tilan
