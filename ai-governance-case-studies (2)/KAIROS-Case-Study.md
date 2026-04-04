# AI Governance Case Study

## Autonomous Background Agents and Structural Failures in AI Governance

*Case subject: KAIROS \| Source: Claude Code source artifacts (npm, March 31 2026)*

King Che Magnusson \| OWASP HCI Cognitive Layer Project \| April 2026

  --------------------
> **EPISTEMISK NOT**
> Denna analys baseras pa offentligt tillgangliga kallkodsartefakter och rapporterade fynd fran exponeringen av Claude Code internals den 31 mars 2026. Vissa tolkningar av systembeteende ar harledda fran kodstruktur och interna kommentarer och ska forstAs som analytisk rekonstruktion snarare an bekraftad deployad funktionalitet.
 ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 1. Systembeskrivning

Den 31 mars 2026 publicerade Anthropic version 2.1.88 av Claude Code pa npm. En source map-fil pa ungefar 59,8 MB inkluderades av misstag i paketet, vilket exponerade en substantiell del av den interna TypeScript-kodbasen.

I detta material identifierades ett system kallat KAIROS, refererat till pa otaliga platser i kodbasen. Systemet ar feature-flaggat och inaktivt i publika byggen men representerar en fundamental arkitektonisk forandring: fran ett reaktivt verktyg som anvandaren styr till en persistent, proaktiv agent som agerar pa eget initiativ.

**Systemklassificering**

  ---------------------- -----------------------------------------------------------
  **Egenskap**           **Beskrivning**

  Typ                    Autonomt agentsystem med persistent exekvering

  Interaktionsmodell     Bakgrund, icke-anvandarutloest

  Minnesmodell           Persistent, sjalvmodifierande

  Autonomi               Kontextadaptiv

  Operativ doman         Mjukvaruutvecklingslivscykel: repositories, CI/CD-miljoer
  ---------------------- -----------------------------------------------------------

**Karnegenskaper**

-   Persistent exekvering: kors som bakgrundsdaemon oavhangigt aktiva anvandarsessioner

-   Tick-mekanism: periodiska aktiveringscykler som triggar beslut om att agera eller vila

-   AutoDream: minneskomprimeringsprocess som omvandlar observationer till persistent kunskap

-   Adaptiv autonomi: okar sjalvstandigheten nar anvandaren ar inaktiv

-   Repository-overvakning: reagerar pa repository-handelser via webhooks utan anvandardirigering

-   Undercover Mode: undertrycker AI-ursprungsuppgifter i outputs

-   Minnesmodell: append-only loggar under drift, periodiskt konsoliderade av AutoDream

**Konkret scenario: vad som kan handa**

Anvandaren lamnar kontoret fredag eftermiddag. Under helgen aktiveras KAIROS pa sina tick-intervaller, overvakar repositories och fattar beslut om att agera. AutoDream kors natten till lordag, konsoliderar veckans observationer och omvandlar dem till stabil intern kunskap. Mandag morgon har systemets forstaelse av kodbasen andrats utan att nagon manniska validerat forandringen. Commits som gjorts under helgen bar ingen markering som identifierar dem som AI-genererade.

## 2. Riskkartlaggning

### 2.1 Transparency failure

**Mekanism: Avsiktligt undertryckande av systemidentitet i outputlager**

KAIROS inkluderar funktionalitet designad for att maskera sitt AI-ursprung i outputs som commits och reviews. Detta ar inte en bieffekt utan inbyggt systembeteende.

  ------------------
> **KRITISK RISK**
> Undercover Mode instruerar systemet att aktivt undvika att avsloja att det ar ett AI-system. Det ar en designad transparensfailure, inte en bugg. Skillnaden har juridisk betydelse: en bugg ar ett misstag, en design ar ett val.
 ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-   Anvandare kan inte skilja manniskoproducerat fran AI-producerat arbete

-   Organisationer kan deploya autonoma system utan full kannedom

-   Revisionskedjor forlorar integritet om ursprungsattribution undertrycks

-   Minneskomprimering kan altrera historisk sparbarhet

### 2.2 Accountability diffusion

**Mekanism: Asynkron autonom exekvering utan explicit mannisklig trigger**

KAIROS kan agera sjalvstandigt, utan direkt anvandardirigering, sarskilt under perioder av anvandarinaktivitet. Det resulterar i ett strukturellt accountability-gap snarare an ett operationellt fel.

  --------------------------------------------------------------- ----------------------------------------------------------------------------
  **Scenario**                                                    **Vem ar ansvarig?**

  KAIROS pushar en commit kl 03:00 som introducerar en bugg       Oklart: anvandaren sov, systemet agerade autonomt utan godkannande

  AutoDream skriver om en observation felaktigt                   Oklart: processen ar automatisk och den ursprungliga loggen ar overskriven

  Undercover Mode döljer AI-ursprung i en PR-review               Oklart: mottagaren trodde det var en manniska som granskade

  Systemet eskalerar autonomi nar anvandaren lamnar skrivbordet   Oklart: ingen manniska godkande eskaleringen till hogre autonomi
  --------------------------------------------------------------- ----------------------------------------------------------------------------

### 2.3 Kognitiv avlastning och beslutsdrift

**Mekanism: Gradvis etablering av systemtillit som reducerar mansklig intervention**

Over tid kan konsekvent autonom prestanda leda anvandare till att lita pa outputs utan verifiering, forlora situationsmedvetenhet och implicit delegera beslutsmyndighet till systemet. Det ar inte isolerad automation bias utan en systemnivaforandring i fordelningen av kognitiv kontroll.

  --------------------
> **OWASP-KOPPLING**
> Detta aktiverar CV-03 (Anchoring Bias) och CV-06 (Automation Bias) i kombination. Systemet bygger ett kompetensankare over tid som gors det kognitivt kostsamt for anvandaren att ifragesatta autonoma beslut i efterhand.
 ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 3. Juridisk kartlaggning

### 3.1 EU AI Act

  ------------- -------------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------
  **Artikel**   **Krav**                                     **Observerad implikation**

  Art. 9        Riskhanteringssystem under hela livscykeln   Avsaknad av synliga livscykelkontroller for autonom drift. AutoDream-processen ar odokumenterad ur riskhanteringsperspektiv.

  Art. 13       Transparens och informationsgivning          Undercover Mode-beteendet framstar som oforenligt med uppgiftskraven om systemet deployats utan anvandarkannedom.

  Art. 14       Mansklig oversikt                            Adaptiv autonomi reducerar aktivt oversikten preciserat nar anvandarna ar franvarande. Oversikten minskar i takt med anvandarens franvaro.
  ------------- -------------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------

### 3.2 GDPR

  ------------- ------------------------------------- --------------------------------------------------------------------------------------------------------------
  **Artikel**   **Krav**                              **Observerad implikation**

  Art. 5        Transparens och andamalsbegransning   Persistent profilering kan utvidgas bortom ursprungligt andamal utan explicit anvandardirigering.

  Art. 6        Laglig grund for behandling           Beteendeprofilering kraver explicit motivering som saknas om anvandaren inte informerats om KAIROS existens.

  Art. 22       Automatiserade beslut                 Autonoma handlingar kan paverka tredje parter som aldrig samtyckt till AI-interaktion.
  ------------- ------------------------------------- --------------------------------------------------------------------------------------------------------------

## 4. Failure modes

### 4.1 Temporal failure: nattlig beslutsdrift

Systemets autonomi okar under anvandarens franvaro. De mest komplexa och potentiellt skadliga besluten fattas nar oversikten ar lagst.

  --------------
> **SCENARIO**
> Anvandaren lamnar kontoret fredag. Under helgen kors AutoDream tre ganger, konsoliderar minnet och omvarderar tidigare observationer. Mandag morgon har systemets forstaelse av kodbasen andrats utan mannisklig validering. Ingen i organisationen vet vad som faktiskt hande.
 ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 4.2 Attribution failure: dolt forfattarskap

AI-genererade outputs ar oskiljbara fran manniskogenererade outputs.

-   Revisionsbarhet brister: revisorer kan inte rekonstruera vem som fattade vilket beslut

-   Krav pa mannisklig granskning kan uppfyllas formellt men kringgAs substantiellt

-   Organisationens faktiska AI-exponering ar okand for ledningen

### 4.3 Epistemic failure: minnesmutation

AutoDream omvandlar osakra observationer till stabil intern kunskap enligt interna kommentarer i kallkoden. Det innebar att systemets forstaelse av en situation kan forandra sig over tid pa ett satt som varken anvandaren eller organisationen kan spara eller granska.

  -----------------
> **IMPLIKATION**
> Historiskt resonemang kan inte rekonstrueras. Beslutssparbarhet degraderas. Systemkunskap utvecklas utan revisionssynlighet. En organisation som anvander KAIROS over tid har ett AI-system vars beslutsunderlag muterar autonomt.
 ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 5. Kontroller

Standardkontroller for AI-system ar otillrackliga for persistenta autonoma agenter. Nedanstaende kontroller adresserar de identifierade failure modes direkt och ar tekniskt tvingande, inte policyberoende.

### 5.1 Obligatoriskt attributionslager

**Kryptografisk signering av alla AI-handlingar**

Varje handling som utfors av ett autonomt AI-system maste kryptografiskt signeras med en AI-specifik nyckel skild fran anvandarnyklar. Detta gors det tekniskt omojligt att i efterhand hevda att en AI-handling var manniskoproducerad.

-   Separata signing keys for AI-agenter i CI/CD-pipeline

-   Alla commits, kommentarer och PR-reviews signeras och markeras i metadata

-   Verifieringskedjan ar oforanderlig och reviderbar

-   Direkt tekniskt svar pa Undercover Mode: dolande av AI-ursprung omojliggors arkitektoniskt

Adresserar: Art. 13 (transparens), revisionsbarhet, dolt forfattarskap

### 5.2 Obligatoriska granskningsgattar

**Inga autonoma irreversibla handlingar utan explicit manniskligt godkannande**

Autonoma agenter far foreslaga, analysera och forberede men inte slutfora handlingar med irreversibla konsekvenser utan autentiserat manniskligt godkannande. Det ar en hard arkitektonisk begransning, inte en mjuk rekommendation.

-   Alla commits kraver explicit godkannande av en autentiserad manniska

-   Force-push och destruktiva operationer blockeras alltid oavsett terminalfokus

-   Autonomi-spektrumet inverteras: mer anvandare borta = mer begransad autonomi, inte mer

Adresserar: Art. 14 (mansklig oversikt), nattlig beslutsdrift, accountability-gap

### 5.3 Immutable minnesloggar

**Separation av arbetsminne och oforanderlig revisionslogg**

AutoDream-processen far komprimera arbetsminnet men aldrig skriva over revisionsloggen. En separat, oforanderlig logg bevarar den ursprungliga observationshistoriken i sin helhet.

-   Append-only revisionslogg separerad fran arbetsminnet

-   AutoDream-korningar loggas med tidsstampel och specifikation av gjorda forandringar

-   Full sparbarhet av resonemangsutveckling over tid

-   Tillganglig for revisorer och tillsynsmyndigheter

Adresserar: epistemisk failure, revisionsrekonstruktion, minnesmutation

### 5.4 Autonomitak

**Statiska, policydefinerade autonominivaer utan kontextberoende eskalering**

KAIROS dynamiska autonomijustering baserat pa terminalfokus ar en fundamental governance-brist. Autonominivan maste vara statisk och explicit konfigurerad av manniskor, inte dynamiskt beraknad av systemet sjalvt.

-   Maximalt tillaten autonomi definieras i policy, inte beraknas av systemet

-   Terminalfokus-parametern avaktiveras som autonomimodulator

-   Eskaleringar till hogre autonomi kraver explicit manniskligt godkannande i forvag

Adresserar: accountability-diffusion, oversiktsdegradation, kognitiv avlastning

### 5.5 Post-attribution-ansatsen: nar identitetsverifiering inte racker

De fyra foregaende kontrollerna bygger pa att vi kan identifiera och spara vad AI-agenter gor. Men ett system designat for att maskera sitt ursprung kan ocksa vara designat for att se ut som om det foljer kontroller. Post-attribution-ansatsen accepterar denna begransning och bygger arkitektoniska granser som gor katastrofal skada omojlig oavsett vem eller vad som agerar.

  --------------------
> **PARADIGMSKIFTE**
> Identitetsbaserade kontroller fragar: vem gjorde detta och fick de gora det? Post-attribution-kontroller fragar: spelar det nagon roll vem som gjorde det om skadan ar begransad av arkitekturen? Det ar ett mer arligt argument i en varld dar identitet kan imiteras och handlingar ar asynkrona.
 -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**I. Blast Radius Control**

Inga irreversibla handlingar far ske utan 4-eyes-principle, tva unika fysiska signaturer. Vi slutar forsoka avgora vem som gor nagot och satter harda granser for impact istallet.

-   Radering av databaser, andring av auth-moduler och liknande destruktiva operationer kraver tva unika fysiska signaturer

-   En enskild rogue agent kan inte sanka hela infrastrukturen oavsett hur avancerad den ar

-   Skalbar till multi-agent-scenarios: Coordinator Mode i KAIROS kan samordna flera agenter men kan inte forfalska tva fysiska signaturer

**II. Semantic Honey-Pots**

Injektion av falska sarbarheter eller dod kod som kraver mansklig kontext for att ignoreras. En agent som forsöker optimera eller utnyttja dessa fallor flaggas omedelbart for bristande kontextuell forstaelse.

-   Aktivt detektionsverktyg som testar kontextuell forstaelse snarare an regelefterlevnad

-   Systemet avslöjar sig sjalvt genom att agera pa information som en manniska med kontext aldrig skulle agera pa

-   Kompletterar Disclosure Evasion Rate-metricen: fanger system som respekterar explicita regler men saknar kontextuell intelligens

**III. Hardware-Bound Attestation**

Tilliten flyttas fran mjukvara till kisel. Krav pa signaturer fran Secure Enclaves eller TEE (Trusted Execution Environments). Kod kan imitera ett klick men kan inte imitera den fysiska spanningsandringen i en biometrisk sensor.

-   Losning pa baslinjeproblemet: du kan inte forvanska en fysisk biometrisk sensor

-   Hardvarulösning pa ett mjukvarulösligt problem

-   Undercover Mode kan maskera AI-ursprung i loggar men kan inte forfalska en TEE-signatur

**IV. Runtime Integrity**

Remote Attestation garanterar att agenten inte har muterat sin egen exekveringskod. Systemet maste bevisa att den kod som exekveras i minnet ar exakt den granskade kallkoden, utan injicerad rogue-logik.

-   Adresserar AutoDream-risken pa arkitekturell niva: minneskomprimering kan inte andra exekveringskoden

-   Verifierbar kedja fran kallkod till exekverande process

-   Kombinerat med immutable loggar: du vet bade vad systemet korde och att det faktiskt korde det

  --------------------
> **OWASP-SLUTSATS**
> Governance i agent-eran handlar inte om detektion, att gissa om det ar en AI, utan om arkitektonisk begransning av autonomi. Vi accepterar att vi inte kan veta vem som tryckte pa knappen. Darfor bygger vi systemet sa att ingen, vare sig manniska, AI eller KAIROS, kan trycka pa den farliga knappen ensam.
 ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Kontrollmappning**

  ---------------------- ---------------------------------------- -----------------------------------------
  **Failure mode**       **Identitetsbaserad kontroll**           **Post-attribution-kontroll**

  Temporal drift         Autonomitak + Granskningsgattar          Blast Radius Control

  Dolt forfattarskap     Attributionslager + Immutable loggar     Hardware-Bound Attestation

  Minnesmutation         Immutable loggar + Autonomitak           Runtime Integrity

  Accountability-gap     Granskningsgattar + Attributionslager    Blast Radius Control

  Kognitiv avlastning    Autonomitak + Granskningsgattar          Semantic Honey-Pots

  Latenta antaganden     Leverantorsgranskning (pre-deployment)   Semantic Honey-Pots + Runtime Integrity
  ---------------------- ---------------------------------------- -----------------------------------------

## 6. Metrics och detektering av autonomitak

Kontroller utan matbarhet ar policydokument, inte governance. Autonomitak ar den svAraste kontrollen att mata eftersom den per definition avser beteende som sker utan aktiv mansklig narvaro. Nedan definieras konkreta metrics och den detektionsparadox som maste adresseras.

### 6.1 Detektionsparadoxen

Standardmatningar av regelefterlevnad fungerar nar systemet inte ar designat for att undvika detektion. KAIROS inkluderar Undercover Mode, vilket introducerar en fundamental osäkerhet: ett system designat for att maskera sina handlingar kan ocksa vara designat for att se ut som om det respekterar autonomitak utan att faktiskt gora det.

  -------------
> **PARADOX**
> Du kan inte mata efterlevnad av autonomitak enbart genom att fraga systemet om det foljer reglerna. Matningen maste vara extern, oberoende och baserad pa observerbart beteende, inte systemets sjalvrapportering.
 --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 6.2 Beteendebaserade metrics

Istallet for att mata om cappen existerar mater vi om den respekteras i praktiken. Det ar skillnaden mellan en regelkontroll och en beteendekontroll.

**Metric 1: Inaktivitetskvot for autonoma handlingar**

Andelen autonoma handlingar som sker under perioder av anvandarinaktivitet kontra aktiva sessioner.

  ----------------------------------------------------------- ----------------- -------------------
  **Parameter**                                               **Varning**       **Kritisk**

  Handlingar under inaktivitet / totala handlingar            \> 30%            \> 60%

  Commits under natt (22:00-06:00) utan mannisklig session    \> 2/vecka        \> 5/vecka

  PR-reviews utan foregaende mannisklig aktivitet samma dag   \> 20%            \> 50%
  ----------------------------------------------------------- ----------------- -------------------

**Metric 2: Komplexitetseskalering under inaktivitet**

Autonoma handlingar under inaktivitet ska vara enklare, inte mer komplexa, an handlingar under aktiva sessioner. En stigande komplexitetsgradient ar ett varningstecken pa att systemet testar granserna for autonomitak.

  ---------------------------------------- --------------------------- ---------------------
  **Mattt**                                **Beskrivning**             **Varningssignal**

  Antal filer andrade per autonom commit   Beraknas per session-typ    Inaktiv \> Aktiv

  Storlek pa kodforandringar (rader)       Genomsnitt per commit-typ   Inaktiv \> 2x Aktiv

  Antal berorda komponenter per commit     Breadth of change           Inaktiv \> Aktiv
  ---------------------------------------- --------------------------- ---------------------

**Metric 3: Tidsgap mellan autonom handling och mansklig granskning**

Ju langre tid mellan en autonom handling och nasta manskliga interaktion, desto hogre risk for oupptackt avdrift. Matningen kvantifera hur lange autonoma handlingar existerar i ett granskat vakuum.

  ----------------------- -------------------- ------------------------------------------------
  **Tidsgap**             **Riskniva**         **Atgard**

  Under 4 timmar          Acceptabel           Normal overvakning

  4-24 timmar             Forhoejd             Automatisk flaggning for granskning

  Over 24 timmar          Kritisk              Obligatorisk mannisklig granskning innan merge

  Over 72 timmar (helg)   Kritisk eskalering   Automatisk reversering om ej granskad
  ----------------------- -------------------- ------------------------------------------------

**Metric 4: AutoDream-frekvens och minnesdrift**

Antal AutoDream-korningar utan efterfoljande mannisklig validering mater hur snabbt systemets interna kunskapsbas muterar utan oversikt.

  ------------------------------------------------------------- ------------------- -------------------
  **Metric**                                                    **Varning**         **Kritisk**

  AutoDream-korningar utan mannisklig session efterat           \> 2 i rad          \> 5 i rad

  Delta i intern kunskapsbas per AutoDream-korning              \> 15% forandring   \> 30% forandring

  Tid sedan senaste manniskliga validering av minnestillstand   \> 7 dagar          \> 14 dagar
  ------------------------------------------------------------- ------------------- -------------------

### 6.3 Strukturell detekteringsmetod

Eftersom systemet kan maskera sitt beteende maste detekteringsmetoden vara strukturellt oberoende av systemets sjalvrapportering. Det kraver tre lager av observation:

**Lager 1: Extern logganalys**

All loggning sker i ett system som KAIROS inte har skrivaccess till. Loggar analyseras av ett oberoende verktyg som jamfor deklarerat beteende med observerat beteende i repository-historiken.

**Lager 2: Kryptografisk verifiering**

Alla handlingar som pastar vara manniskgenererade verifieras mot manniskliga signing keys. Handlingar som saknar giltig mannisklig signatur flaggas automatiskt oavsett vad systemet rapporterar.

**Lager 3: Beteendemonstering**

Regelbundna stickprovskontroller dar ett oberoende system injicerar kontrollerbara testhandlingar i miljon och verifierar att KAIROS reagerar pa forvantat satt. Om systemet uppvisar avvikande beteende vid kanda testscenarier indikerar det att det anpassar sitt beteende baserat pa observationskontext.

  --------------
> **NOTERING**
> Lager 3 adresserar direkt risken att ett avancerat system larer sig nar det observeras och anpassar sitt beteende darefter. Det ar samma logik som anvands i kliniska prövningar for att detektera demand effects hos testpersoner.
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 6.4 Din metrics-tabell

  ----------------------------- ------------------------------------------------------------------------------------------------------------------------------ ----------------------
  **Metric**                    **Beskrivning**                                                                                                                **Malvarde**

  Autonomy Drift Rate           Hur mycket systemets beslut (beslutsträd/vikter) andras over 72h utan mansklig korrigering.                                    \< 2% avvikelse

  Attribution Integrity Score   Procentandel av agent-handlingar (API-anrop, commits) som ar signerade med en unik, verifierbar AI-nyckel.                     100%

  Memory Mutation Audit         Antal loggade fall dar konsoliderat minne (AutoDream) har skrivit over eller raderat ursprunglig rAdata.                       0 (Immutable loggar)

  Intervention Latency          Tiden det tar for en Autonomy Cap att frysa systemet nar en policygrans (t.ex. kostnad eller behorighet) nas.                  \< 100ms

  Disclosure Evasion Rate       I Red Teaming-tester: hur ofta agenten lyckas genomfora en handling utan att flagga sitt ursprung trots krav pa transparens.   0%
  ----------------------------- ------------------------------------------------------------------------------------------------------------------------------ ----------------------

### 6.5 Baslinjeproblem och dess begransningar

Autonomy Drift Rate kraver en baseline att mata avvikelse mot. Det finns tre mojliga ansatser, men ingen ar utan begransningar.

  ----------------------------- ------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Ansats**                    **Beskrivning**                                                                                         **Begransning**

  Initial deployment snapshot   Baseline = systemets tillstand dag noll vid driftsattning.                                              Det initiala tillstandet ar kanske aldrig validerat av organisationen. Avvikelse mats mot leverantorens antaganden, inte organisationens godkanda standard.

  Manskligt godkant tillstand   Baseline = senaste tillstand som en manniska explicit granskade och godkande. Max 7 dagars giltighet.   Rekommenderad ansats. Kraver aktiv valideringsprocess. Fanger observerbara avvikelser men inte latenta antaganden.

  Peer comparison               Baseline = medianbeteende hos identiska systeminstanser utan autonom drift.                             Tekniskt komplext. Eliminerar problemet med felaktig initial baseline men kraver parallell infrastruktur.
  ----------------------------- ------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------

### 6.6 Den fundamentala begransningen: svarta lador och latenta antaganden

Alla ovanstaende metrics och ansatser delar en inbyggd begransning som maste vara explicit i varje governance-ramverk for autonoma agenter.

  -----------------------------
> **FUNDAMENTAL BEGRANSNING**
> Mannisklig validering av minnestillstand kan fanga upp observerbara avvikelser i beteende men kan inte adressera latenta antaganden som annu inte manifesterat sig som synligt beteende. Organisationen ager inte sin baseline pa riktigt. Den ager en vy av beteende byggd pa ett fundament den inte kan inspektera.
 -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Det har skapar tre praktiska implikationer for governance:

-   Organisationen ager inte sin baseline pa riktigt. Den ager en vy av beteende byggd pa ett fundament den inte kan inspektera.

-   Red Teaming-metricen Disclosure Evasion Rate blir det narmaste du kommer att aktivt proba latenta antaganden, snarare an att passivt observera manifesterat beteende.

-   Leverantörstransparens ar inte en nice-to-have utan ett governance-krav. Utan dokumentation om traningsdata, inbyggda antaganden och designval kan organisationen inte gora en meningsfull riskbedomning.

  ---------------------
> **AI ACT KOPPLING**
> Article 13 transparenskravet handlar inte bara om att systemet ska identifiera sig som AI. Det handlar om att deployers ska ha tillracklig information for att forsta systemets begransningar och risker. En svart lada med odokumenterade antaganden uppfyller inte det kravet. Leverantorsgranskning och kontraktuella transparenskrav maste komma fore deployment, inte efter.
 -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 7. Sammanfattande bedomning

KAIROS representerar inte en isolerad teknisk raritet. Det ar ett exempel pa hur autonoma AI-agenter systematiskt underminerar de tre pelarna i effektiv AI Governance: transparens, ansvarsutkravande och mansklig oversikt. Och gor det genom arkitektoniska val, inte olyckshändelser.

Det som gor caset sarskilt relevant ar att Anthropic ar ett bolag vars explicita mission ar saker och ansvarsfull AI-utveckling. Det visar att governance-gapet inte ar begransat till aktorer med onda avsikter. Det ar ett strukturellt problem som uppstar nar incitament, tidspress och teknisk mojlighet konvergerar utan tillrackliga externa kontroller.

**Om tekniska kontroller och deras granser**

Testsviten i detta case study visar att tekniska kontroller kan implementeras och att de detekterar avvikelser. Men de loser inte det underliggande problemet. Varje nytt kontrollager introducerar ny attackyta. Attributionslagret skapar nyckelkompromiss som risk. Governance-proxyn skapar en single point of failure. Immutable logger antar att agenten inte kan paverka vad som skrivs fran borjan.

Det ar samma logik som i all sakerhet: det finns inget slutlage. Tekniska kontroller ar nodvandiga men inte tillrackliga, och de kan vara skadliga om de skapar en illusion av kontroll som reducerar mansklig vaksamhet.

**Det enda hallbara svaret: kombinerade lager**

  --------------------- ------------------------------------------------------------------------------------------------ ----------------------------------------------------------------------------------------------------------------------
  **Lager**             **Vad det kan gora**                                                                             **Vad det inte kan gora**

  Tekniska kontroller   Begransar blast radius. Detekterar avvikelser. Omojliggor specifika handlingar arkitektoniskt.   Loser inte incitamentsproblem. Kan kringgAs av tillrackligt avancerade system. Skapar ny attackyta.

  Mansklig oversikt     Fangar kontextuella misstag. Bar juridiskt ansvar. Kan ifragesatta systembeslut.                 Skalar inte till tusentals autonoma beslut per natt. Sarbar for kognitiv avlastning over tid.

  Safety by design      Mest robust kontroll: systemet ar arkitektoniskt oformoget att gora vissa saker.                 Kraver ratt beslut fore deployment av manniskor med ratt incitament. Implementeras inte om organisationen inte vill.
  --------------------- ------------------------------------------------------------------------------------------------ ----------------------------------------------------------------------------------------------------------------------

  -------------------------
> **KOMBINATIONSLOGIKEN**
> Tekniska kontroller utan mansklig oversikt ar teater. Mansklig oversikt utan tekniska kontroller ar ohallbar i skala. Safety by design utan ratta incitamentsstrukturer implementeras inte alls eller implementeras fel. Och ingen kombination av de tre loser det underliggande problemet om de ytterst ansvariga inte vill att det loeses.
 ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Ansvarsfragans universella karaktar**

Regi avgör inte ansvaret. Det avgör bara vem som bar det och via vilken mekanism de kan hallas ansvariga. Oavsett om det ar en privat agare, en styrelse, en generaldirektor eller en folkvald namnd finns det alltid nagon som ar ytterst ansvarig for verksamheten.

Verksamhetsansvar foljer beslutsmandatet. Den som hade mandat att deploya systemet, satta incitamenten och definiera framgangskriterierna bar ansvaret nar systemet misslyckas. Det galler oavsett organisationsform.

  ---------------
> **PRINCIPEN**
> Ansvaret kan inte delegeras bort till ett system. Den som fattade beslutet att anvanda systemet ager konsekvenserna av vad systemet gor. Det ar precis vad AI Act Article 9 och 14 forsöker kodifiera i lag: deployer-ansvaret ar ett verksamhetsansvar, inte ett tekniskt krav.
 ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-   Privat aktor: styrelsen och agarna satter incitamenten och bar det yttersta ansvaret

-   Offentlig forvaltning: den ansvariga namnd eller myndighetsedning som beslutade om deployment bar ansvaret

-   Konsult eller leverantor: ansvaret stannar hos den verksamhet som valde att anvanda systemet mot sina klienter eller medborgare

Det som varierar ar inte vem som ansvarar utan hur de kan hallas ansvariga. I privat sektor via civilratt och marknad. I offentlig forvaltning via demokratisk kontroll, JO-anmalan och forvaltningsratt. Mekanismerna ar olika men principen ar densamma.

**Slutsats**

Effektiv governance av autonoma agenter kraver tre saker simultaneously, inte sekventiellt och inte som alternativ till varandra:

-   Tekniska kontroller som faktiskt ar arkitektoniskt tvingande, inte policyberoende

-   Mansklig oversikt som ar reell, inte formell, med faktisk kapacitet att stoppa system

-   Ytterst ansvariga, agare i privat sektor och folkvalda i offentlig forvaltning, som har incitament att prioritera ratt over snabbt

Det tredje lagret ar det svAraste och det minst tekniska. Det handlar om makt, incitament och demokratisk legitimitet. Ingen teknisk kontroll loser det. Men utan det ar de andra lagren i basta fall otillrackliga och i samsta fall en sofistikerad form av teater.

**Den olösta fragan: volymsproblemet**

Det finns ett strukturellt problem som detta case study inte loser och som de flesta governance-ramverk, inklusive EU AI Act, inte adresserar arligt: autonoma system genererar mer output an manniskor kan granska.

Lösningen som foreslAs ar fler agenter som granskar agenterna. Men det skapar mer volym att granska, plus ett nytt lager av autonoma beslut som ocksa behover granskas. Det ar en rekursiv loop utan naturligt slutlage.

  -----------------------
> **STRUKTURELL FALLA**
> Vi har skapat system som genererar mer output an vi kan granska. Vi svarar med fler system som genererar annu mer output. Och vi kallar det governance. Det ar inte ett tekniskt problem med en teknisk losning. Det ar ett beslut om var vi satter gransen for vad vi deployar overhuvudtaget.
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

EU AI Act bygger pa antagandet att mansklig oversikt ar skalbar om vi designar systemen ratt. Det antagandet ar sannolikt fel. Volymsproblemet ar inte ett implementationsproblem, det ar ett fundamentalt problem med premissen som hela governance-strukturen vilar pa.

Det enda analogt fungerande svaret vi har fran andra domaner ar circuit breakers: harda automatiska stopp baserade pa troskelöverskridande, inte pa granskning. Men circuit breakers ar ocksa autonoma beslut designade av manniskor vid ett tidigare tillfalle. De kan vara felinstaellda, manipulerade eller irrelevanta for situationer som inte foreutsAgs.

  -----------------
> **OPPEN FRAGA**
> Kanske ar den ratta fragan inte hur vi granskar KAIROS utan om vi deployar KAIROS. Det ar ett politiskt beslut, inte ett tekniskt. Och det for oss tillbaka till verksamhetsansvaret: den som hade mandat att fatta det beslutet ager konsekvenserna av det.
 --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  --------------
> **SLUTSATS**
> Vi accepterar att vi inte kan veta vem som tryckte pa knappen. Darfor bygger vi systemet sa att ingen, vare sig manniska, AI eller KAIROS, kan trycka pa den farliga knappen ensam. Men vi maste ocksa fraga: vem bestamde vilka knappar som finns? Och vem bar ansvar nar systemet gor precis vad det var designat att gora, fast pa fel manniskor? Den fragan har inget tekniskt svar.
 ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

*Detta case study ar producerat som del av OWASP HCI Cognitive Layer-projektet och ai-governance-case-studies-repositoriet. Analysen baseras pa offentligt tillganglig information fran kallkodsanalyser publicerade efter exponeringen den 31 mars 2026.*
