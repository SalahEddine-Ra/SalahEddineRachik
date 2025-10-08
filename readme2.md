sequenceDiagram
    title Scénario Principal - Prise de Commande (Boite Blanche)

    actor Serveur
    participant ec: EcranCommande
    participant gc: GestionnaireCommandes
    participant c: Commande
    participant lc: LigneCommande
    participant m: Menu
    participant t: Table
    participant n: Notificateur

    Serveur->>ec: créerCommande(table5)
    ec->>gc: créerCommande(table5, serveur)
    gc->>c: new Commande(table5, serveur)
    c-->>gc: commandeCréée
    gc->>t: setStatut("OCCUPÉE")
    t-->>gc: confirmation
    gc-->>ec: commandeInitialisée
    
    Serveur->>ec: consulterMenu()
    ec->>m: getPlatsDisponibles()
    m-->>ec: listePlats
    ec-->>Serveur: afficherMenu()

    Serveur->>ec: ajouterPlat("Pizza", 2)
    ec->>gc: ajouterPlat(commandeId, "Pizza", 2)
    gc->>c: ajouterLigneCommande("Pizza", 2)
    c->>lc: new LigneCommande("Pizza", 2)
    lc-->>c: ligneCréée
    c-->>gc: platAjouté
    gc-->>ec: confirmationAjout

    Serveur->>ec: validerCommande()
    ec->>gc: validerCommande(commandeId)
    gc->>c: valider()
    c->>c: setStatut("VALIDÉE")
    c->>n: notifierNouvelleCommande()
    n-->>c: notificationEnvoyée
    c-->>gc: commandeValidée
    gc-->>ec: validationConfirmée
    ec-->>Serveur: commandeEnvoyée
