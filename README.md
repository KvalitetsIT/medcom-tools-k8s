# medcom-tools-k8s
## Projektets indhold
Dette er et deployment projekt, som indeholder filer til anvendelse af Argo. Der deployes til Medcoms test miljø under projektet "tools-applications".

Overordnet er følgende service tilgængelige via dette projekt
 - Medcom Cda Viewer test 1 og 2
 - Medcom Cda Validator

Projektet indeholder følgende applikationer
 1. Tools application (Apps): hoved applikation, som indeholder information om, hvilke andre applikationer der findes
 2. cdaviewer-test1: Medcoms cda viewer, som behandler dokumenter på test 1
	- Se https://cdaviewer.medcomtools.medcom.dk/cdaviewer-test1/
 3. cdaviewer-test2: Medcoms cda viewer, som behandler dokumenter på test 2 og kih
	- Se https://cdaviewer.medcomtools.medcom.dk/cdaviewer-test2/
 4. cdavalidator: Medcoms cda validator
	- Se https://cda.medcomtools.medcom.dk
 5. cdavalidator-schematron-appointment - hjælpe applikation til cda validator
 6. cdavalidator-schematron-cpd - hjælpe applikation til cda validator
 7. cdavalidator-schematron-pdc - hjælpe applikation til cda validator
 8. redistools - redis anvendes af cda viewerene til logning
 
## Vedligehold og deployment
**Generelt:**

For at lave justeringer til tools-application projektet på medcoms test miljø udføres følgende trin

 1. Ret source:
    1. Check source ud fra github (https://github.com/KvalitetsIT/medcom-tools-k8s)
    2. Udfør ændringen (se nedenfor)
    3. Push source til github
 2. Installer rettelsen:
    1. Login i argo
       - I en browser brug linket kube-argo.vconf-test.dk
       - Vælg "login via keycloak" til højre på siden
       - Vælg "External deployment" til højre i login ruden
       - Indtast brugernavn og password (skift password, hvis du bliver bedt om det)
    2. Deploy rettelsen (se nedenfor)
  
**Ny applikation:**

Følg oventående generelle vejledning. 

Under punkt 1 "Ret source" udføres følgende som trin ii:

 - Lav nyt bibliotek med relevante filer 
   - (eks bibliotek cdaviewer-test1 med filerne Chart.yaml, values.yaml og values-medcom.yaml)
 - Tilføj den nye applikation i Apps (hoved applikationen) 
   - (i values.yaml eller values-medcom.yaml)

Under punkt 2 "Installer rettelsen" udføres følgende som trin ii:

 - Klik på "Tools application" for at åbne den
 - Tryk refresh
 - Den nye applikation vil være "gul". 
 - Tryk på de 3 prikker på den nye gule og vælg Sync (Bekræft i vinduet ved at vælge "Syncronize"
 - For den nye applikation klik på "open application" (vindue ikon med pil ud til højre)
 - Tryk refresh
 - Tryk Sync (Bekræft igen i vindue ved at vælge "Syncronize")

**Ret applikation:**

Følg oventående generelle vejledning. 

Under punkt 1 "Ret source" udføres følgende som trin ii:

 - Lav den ønskede ændring

Under punkt 2 "Installer rettelsen" udføres følgende som trin ii:

 - Klik på den ændrede applikation for at åbne den
 - Tryk refresh
 - Den ændrede applikation vil være "gul". 
 - Tryk Sync (Bekræft igen i vindue ved at vælge "Syncronize")

## cdavalidator-schematron

Applikationenerne: 

 - cdavalidator-schematron-appointment
 - cdavalidator-schematron-cpd
 - cdavalidator-schematron-pdc

der er en del af cda validatoren, gør brug af en række filer (af typerne sch, xslt og xml). Disse mange filer bliver i dette deployment projekt stillet til rådighed vha en init container, der startes op inden applikationen selv startes op.
Disse filer kommer oprindelig fra cda validator installationen (https://bitbucket.org/4s/cda-validator/downloads/install.zip). Det er filerne som findes i biblioteket "installation/art-decor/". De vedligeholdes som en del af cda validatoren (https://bitbucket.org/4s/cda-validator/src/master/).

Når disse filer ændres, skal der laves en ny version af det docker image, som anvendes af init containeren og dette skal deployes. 

Fremgangsmåden er som følger

 1. Lav ændringerne som Medcom processen er for det (Cda-validator source og installation.zip)
 2. Lav nyt docker image med art-decor filerne:
 3. Opdater source filerne med det nye docker image:
 4. Deploy rettelsen:
