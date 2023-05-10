# New Member Onboarding Information

## VPN

* Fill out VPN application
* Wait for Emails from IT
* Configure OpenVPN via downloaded configuration
* Configure Proxy according to guidelines (for linux within firefox, the
  browser needs to be restarted for each VPN connection)
* Test configuration by accessing `https://vpntest.charite.de/` while connected

## Proxy settings

* Proxy settings only work in the browser but not on the command line, so they need to be configured manually
* The proxy `Proxy-Anleitung` has a section for manual proxy configuration at the end

## Register PC with IT

* To use the LAN in the office, the device needs to be registered:
    * `https://intranet.charite.de/it/it_serviceueberblick/netzanmeldung/netzanmeldung_online/`
    * `https://intranet.charite.de/it/it_serviceueberblick/dokumente/allgemeine_dokumente_und_richtlinien/`
* Fill out the application:
    * Device: Laptop
    * Inventory number: 000003220464
    * Ethernet MAC: 9c:2d:cd:0f:fe:18
    * Use-case: core network
    * Device name: `cabe12-t14g3`.charite.de
    * Building-floor-room/connection: 2740-03-009/15

## S-Drive Access

* Several drives are used by the group: `S:\AG\AG-Viro-Allgemein\` and
  `S:\AG\AG-Viro-Drexler\`
* The access needs granted using the form
  `forms/Beantragung-Rechte_Zugriff_Auf_S.pdf`

## MS365

* Use email + charite login to use Office and OneDrive

## Charité ID

* `171834`

## PolyPoint PEP 

* Time tracking is possible with PEP
* Accessible via the remote VM
* Editing entries was not possible initially, but after an email to
  `pep@charite.de`, they activated the feature for me

## Request HPC cluster access 

* Needs to be done by PI
* Access via VPN needs to be applied for separately, as described
  [here](https://bihealth.github.io/bih-cluster/connecting/from-external)

## Gain cluster access

* Wait for email from admins (Lea does initial application with you)
* Get VPN-Zusatzantrag B for Cluster access through VPN

## Register with GitHub

* Request Access for the [GitHub group](https://github.com/drexler-virus-epidemiology)

## Setup Mail

* Get calendar access
* Setting up Email is straightforward, but the various calendars didn't work
* Use Outlook on the VDI VM
* Desktop mail client: `evolution` with `evolution-ews` add-on 

## Setting up VMWare Horizon client

* A linux version of the VMWare client exists in AUR: `vmware-horizon-client`


