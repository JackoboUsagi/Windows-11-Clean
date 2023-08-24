# Windows 11 Clean
Astuces pour améliorer l'expérience Utilisateur sous Windows 11

![alt text](https://raw.githubusercontent.com/Jackobo-Usagi/W11-Cleanup/main/w11.png)

Testé avec les composants suivants :
1. Processeur : Intel i5-10400F
2. RAM : 16 GB (2 * 8) DDR4 DIMM 2133 MHz
3. GPU : NVIDIA GeForce GTX 1650 4096 Mo GDDR5
4. OS : Windows 11 installé sur le disque dur SSD 512 Go
5. Disque dur HDD de 2To en supplémentaire

## AVANT DE COMMENCER

1. Je ne suis pas responsable des potentiels dégats matériels, logiciels et/ou physiques lors de l'utilisation de ces commandes et astuces. Vous êtes responsable de votre équipement informatique.
2. Dans la section "Issues" ou "Pull requests" du projet, n'hésitez pas à me faire des propositions de commandes / astuces que vous souhaitez partager 
3. L'étape 1 n'est pas à répéter si vous avez déjà sur votre PC les comptes nécessaires au bon fonctionnement selon votre usage.

-----------------

## ETAPE 1 : CREATION DE VOTRE COMPTE UTILISATEUR SUR UN AUTRE LECTEUR 

But : Eviter l'utilisation abusive du SSD par Windows 11 pour les données des applications et l'accès à vos fichiers.

Etapes à suivre :

1. Créer un compte Administrateur Local (sans avoir besoin d'utiliser un compte Microsoft ) et s'y connecter
2. Noter le nom du dossier dans C:\Users concernant votre utilisateur principal
3. Supprimer le compte utilisateur souhaité, le recréer en utilisant si souhaité son compte Microsoft et s'y connecter
4. S'y déconnecter ensuite et revenir sur le compte Administrateur Local
5. Ouvrir CMD.EXE en tant qu'administrateur et exécuter la commande suivante ( changer le nom du dossier et le lecteur souhaité (ici L:\ ) ) :
```
robocopy "C:\Users\NOM_UTILISATEUR" "L:\Users\NOM_UTILISATEUR" /E /COPYALL /XJ
```
6. Renommer "C:\Users\NOM_UTILISATEUR" en "C:\Users\NOM_UTILISATEUR.old"
7. Exécuter la commande suivante pour créer le lien symbolique ( changer le nom du dossier et le lecteur souhaité (ici L:\ ) ) :
```
mklink /J C:\Users\NOM_UTILISATEUR L:\Users\NOM_UTILISATEUR
```
8. Se connecter sur votre compte utilisateur
9. Si tout est OK, supprimer "C:\Users\NOM_UTILISATEUR.old"

-----------------

## ETAPE 2 : MISE A JOUR DE WINDOWS 11

Lors du passage à Windows 11, ou durant son utilisation, vérifier que les mises à jour soient toutes installés sur votre ordinateur. Si les mises à jour doivent être installés, les installer et redémarrer le PC.

-----------------

## ETAPE 3 : EXECUTION DES COMMANDES SUIVANTES SOUS CMD.EXE ( ADMINISTRATEUR REQUIS )

Exécuter le fichier "clean_w11.bat" __en tant qu'administrateur__. Plus d'information sur les commandes dans le fichier "clean_w11.md".

Redémarrer ensuite votre ordinateur.

-----------------

## ETAPE 4 : AUTRES ASTUCES

__Configuration du Windows Defender:__

Des applications peuvent faire paniquer l'antivirus et provoquent une surcharge du CPU par Windows Defender. Cependant, désactiver l'anti-virus complet est une erreur à ne pas faire ! Pour ajouter une exclusion :
1. Aller dans Paramètres -> Confidentialité  & Sécurité -> Sécurité Windows -> Protections contre les virus et menaces
2. Cliquer sur Gérer les paramètres
3. Dans Exclusions ( tout en bas ), cliquer sur "Ajouter ou supprimer des exclusions"
4. Ajouter le programme exe concerné ( avec le mode fichier ) ou un processus ( en saisissant son nom )

-----------------

## Explication des commandes utilisés

__Catégorie "Système":__
- Mise à jour / Reset de Windows Defender ( résoud le souci d'affichage de l'outil dans les Paramètres : __à utiliser après une mise à jour de Windows 10 vers Windows 11__ )
- Désactivation du support Linux virtuel
- Suppression de Microsoft Edge ( vérifier le numéro de la version selon les mises à jour : ici 97.0.1072.76 )
```
powershell -command "Get-AppxPackage Microsoft.SecHealthUI -AllUsers | Reset-AppxPackage"
powershell -command "Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux"
powershell -command "'C:\Program Files (x86)\Microsoft\Edge\Application\*\Installer\setup.exe' --uninstall --force-uninstall --system-level"
```

__Catégorie "Apps":__
- Suppression des notifications provenant des applications
- Autorisations des applications refusées ( sauf Caméra et Micro )
```
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\PushNotifications /v ToastEnabled /t REG_DWORD /d 0 /f

reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications /v GlobalUserDisabled /t REG_DWORD /d 1 /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\appointments /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\phoneCall /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\phoneCallHistory /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\cellularData /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\contacts /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\appDiagnostics /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\documentsLibrary /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\email /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\gazeInput /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\broadFileSystemAccess /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\chat /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\activity /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\userNotificationListener /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\bluetooth /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\bluetoothSync /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\picturesLibrary /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\radios /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\userDataTasks /v Value /t REG_SZ /d "Deny" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ToastEnabled /t REG_DWORD /d 0 /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\videosLibrary /v Value /t REG_SZ /d "Deny" /f
```

__Catégorie "Gaming":__
- Désactivation du mode "Game DVR"
- Désactivation du mode "PowerThrottling"
- Optimisation des effets visuels
```
reg add HKEY_CURRENT_USER\System\GameConfigStore /v GameDVR_Enabled /t REG_DWORD /d 0 /f
reg add HKEY_CURRENT_USER\System\GameConfigStore /v GameDVR_FSEBehaviorMode /t REG_DWORD /d 2 /f
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PolicyManager\default\ApplicationManagement\AllowGameDVR /v value /t REG_DWORD /d 0 /f

reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\PowerThrottling /v PowerThrottlingOff /t REG_DWORD /d 1 /f

reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects /v VisualFXSetting /t REG_DWORD /d 2 /f
```

__Catégorie "Personnalisation":__
- Suppression des Widgets
- Activation du mode "Sombre" pour les Applications
- Activation du thème "Sombre" pour Windows
- Cacher la liste des Applications les plus utilisés
- Transparence désactivé
- Désactivation de la recherche Internet dans la zone de Recherche de Windows
```
powershell -command "winget uninstall 'Windows web experience Pack'"
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Themes\Personalize /v AppsUseLightTheme /t REG_DWORD /d 0 /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Themes\Personalize /v SystemUsesLightTheme /t REG_DWORD /d 0 /f
reg add HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\Explorer /v ShowOrHideMostUsedApps /t REG_DWORD /d 2 /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Themes\Personalize /v EnableTransparency /t REG_DWORD /d 0 /f
reg add HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Explorer /v DisableSearchBoxSuggestions /t REG_DWORD /d 1 /f
```

__Catégorie "Vie Privée":__
- Désactivation de l'ID pour les Pubs
- Désactivation de l'installation automatique d'applications tiers de Microsoft pour les nouveaux comptes
- Désactivation de Windows Hello biometrics
- Désactivation de la Télémétrie
- Désactivation de l'envoi des diagnostics
- Désactivation des avis / retours sur Windows
- Désactivation de la localisation
- Désactivation des contenus suggérés
- Désactivation des conseils Windows selon nos données
```
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\AdvertisingInfo /v Enabled /t REG_DWORD /d 0 /f

reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SilentInstalledAppsEnabled /t REG_DWORD /d 0 /f

reg add HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Biometrics /v Enabled /t REG_DWORD /d 0 /f

reg add "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\CompatTelRunner.exe" /v Debugger /t REG_SZ /d "%windir%\System32\taskkill.exe" /f
reg add HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\DataCollection /v AllowTelemetry /t REG_DWORD /d 0 /f
reg add HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\DiagTrack /v Start /t REG_DWORD /d 4 /f
reg add HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\dmwappushservice /v Start /t REG_DWORD /d 4 /f

reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Privacy /v TailoredExperiencesWithDiagnosticDataEnabled /t REG_DWORD /d 0 /f

reg add HKEY_CURRENT_USER\Software\Microsoft\Siuf\Rules /v PeriodInNanoSeconds /t REG_DWORD /d 0 /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Siuf\Rules /v NumberOfSIUFInPeriod /t REG_DWORD /d 0 /f

reg add HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\location /v Value /t REG_SZ /d "Deny" /f

reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SubscribedContent-338393Enabled /t REG_DWORD /d 0 /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SubscribedContent-353694Enabled /t REG_DWORD /d 0 /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SubscribedContent-353696Enabled /t REG_DWORD /d 0 /f

reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Privacy /v TailoredExperiencesWithDiagnosticDataEnabled /t REG_DWORD /d 0 /f
```

__Catégorie "Services" - Désactivation des services suivants:__
- DiagTrack + dmwappushservice : Expériences des utilisateurs connectés et télémétrie
- SCardSvr : Carte à puce	
- SessionEnv : Configuration des services Bureau à distance
- PimIndexMaintenanceSvc : Données de contacts
- WiaRpc : Evénements d’acquisition d’images fixes
- Télécopie : Fax
- WinRM : Gestion à distance de Windows (Gestion WSM)
- SEMgrSvc : Gestionnaires des paiements et des éléments sécurisés NFC
- lfsvc : Service de géolocalisation
- LxssManager : Exécuter les binaires ELF (Linux) dans Windows
- SharedAccess : Pour le partage internet de l’appareil
- UmRdpService : Redirecteur de port du mode utilisateur des services Bureau à distance
- RemoteRegistry	: Registre à distance
- MixedRealityOpenXRSvc : Service Windows Mixed’Reality OpenXR ( Réalité virtuelle )
- WbioSrvc : Service de biométrie Windows
- VacSvc : Service de composition audio volumétrique ( Réalité virtuelle )
- SharedRealitySvc : Service de données spatiales ( Réalité virtuelle )
- spectrum : Service de perception Windows ( Réalité virtuelle )
- perceptionsimulation : Service de simulation de perception Windows ( Réalité virtuelle )
- TermService : Service Bureau à distance
- WSearch : Service Windows Search
```
sc stop "DiagTrack" && sc config "DiagTrack" start= disabled
sc stop "dmwappushservice" && sc config "dmwappushservice" start= disabled
sc stop "SCardSvr" && sc config "SCardSvr" start= disabled
sc stop "SessionEnv" && sc config "SessionEnv" start= disabled
sc stop "PimIndexMaintenanceSvc" && sc config "PimIndexMaintenanceSvc" start= disabled
sc stop "WiaRpc" && sc config "WiaRpc" start= disabled
sc stop "Télécopie" && sc config "Télécopie" start= disabled
sc stop "WinRM" && sc config "WinRM" start= disabled
sc stop "SEMgrSvc" && sc config "SEMgrSvc" start= disabled
sc stop "lfsvc" && sc config "lfsvc" start= disabled
sc stop "LxssManager" && sc config "LxssManager" start= disabled
sc stop "SharedAccess" && sc config "SharedAccess" start= disabled
sc stop "UmRdpService" && sc config "UmRdpService" start= disabled
sc stop "RemoteRegistry" && sc config "RemoteRegistry" start= disabled
sc stop "MixedRealityOpenXRSvc" && sc config "MixedRealityOpenXRSvc" start= disabled
sc stop "WbioSrvc" && sc config "WbioSrvc" start= disabled
sc stop "VacSvc" && sc config "VacSvc" start= disabled
sc stop "SharedRealitySvc" && sc config "SharedRealitySvc" start= disabled
sc stop "spectrum" && sc config "spectrum" start= disabled
sc stop "perceptionsimulation" && sc config "spectrum" start= disabled
sc stop "TermService" && sc config "TermService" start= disabled
sc stop "WSearch" && sc config "WSearch" start= disabled
```

__Catégorie "Google Chrome" - Désactivation de la mise à jour automatique:__
```
sc stop "gupdate" && sc config "gupdate" start= disabled
sc stop "gupdatem" && sc config "gupdatem" start= disabled
reg add HKEY_LOCAL_MACHINE\Software\Policies\Google\Chrome\Update /v UpdateDefault /t REG_DWORD /d 0 /f
reg add HKEY_LOCAL_MACHINE\Software\WOW6432Node\Google\Update /v UpdateDefault /t REG_DWORD /d 0 /f
copy NUL "C:\Program Files (x86)\Google\Update\GoogleUpdate.exe"
```
