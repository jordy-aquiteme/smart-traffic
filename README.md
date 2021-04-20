# SMART TRAFFIC

## Description 
Le contexte de ce projet se situe dans la découverte et l'utilisation des protocoles de communication à faible coût suivant : 
* XMPP
* MQTT 
qui jouent une grande importance dans le monde l'**IoT**.

## Architecture du projet

![](images/archi_smart_traffic.png?raw=true)

## Fonctionement
[TODO]
![](images/fonctionement_smart_traffic.jpg?raw=true)

## Installation

1. ### **Clone du projet**

```git clone https://github.com/jordy-aquiteme/smart-traffic```

2. ### **Nom de Domaine**

Ajoutez les lignes suivantes dans votre fichier `/etc/hosts`:
* 10.5.0.2 example.com
* 10.5.0.2 pubsub.example.com

:warning: Assurez vous d'être connecté à Internet.

3. ### **Installation de Docker Compose**

https://docs.docker.com/compose/install/

## Usage

[TODO]

1. ### **Démarrage**

    1. Serveurs et passerelles

      Dans une console, tapez la commande ci-dessous :

      ```docker-compose -f smart-traffic/docker-compose.yml up```

      :warning: Le démarrage peut prendre quelques minutes et il faudrait attendre avant de démarrer la simulation !!!

    1. Simulation

      Dans une autre console, tapez la commande ci-dessous :

      ```docker-compose -f smart-traffic/car/docker-compose.yml up```

      Le simulateur d'une voiture est construit comme suit :

      * Un fichier de configuration (sous format ```.yml```), pour simuler les mouvements de la voiture.

        ```yaml 
        car:
          stationId: FR-CAR-1
          stationType: 5
        scenarios:
        - name: normal
          movements:
          - event: /zone/in
            perform: null
            speed: 90.0
            heading: 0
            position:
              latitude: 49.282851
              longitude: 4.107277
        ```
        Ce fichier peut être construit de deux manière :

          * 1ère manière :

            Préparez un fichier ```.csv``` (délimité par des virgules) des coordonnées comme suit :
            latitude | longitude
            ------------ | -------------
            49.282851 | 4.107277
            49.282812 | 4.107097
            49.282844 | 4.106873

            Et exécutez le script ```car/data/code/CarConfigGenerator.py``` et laissez vous guider. 

          * 2ème manière :

            En fournissant un fichier toujours sous format ```.csv``` (délimité par des virgules).

            latitude | longitude | speed | event | perform | causeCode
            -------- | ---------- | ---------- | ---------- | ---------- | ----------
            49.282866319075254 | 4.107265912578656 | 90 | 1 | n | |
            49.28281751032647 | 4.107078855827175 | 90 | 3 | n | |
            49.28010093750807 | 4.099604857129973 | 30 | 3 | o | 4

              * ***event*** : 
                  * Entrée dans une zone : 1
                  * Sortie d'une zone : 2
                  * Message normal : 3

              * ***perform*** : pour indiquer qu'à cette étape nous souhaitons envoyer un message de type DENM.  

              Ensuite il faudra indiquer le code de l'évèvenemt 
              * ***causeCode*** :
                  * Accident : 4
                  * Embouteillage : 5
                  * etc.

              Pour finir, il faudra également exécuter le script ```car/data/code/CarConfigGenerator.py```.

      * Un programme d'exécution ```car/data/code/car.py```.

PS: les différents packets envoyées peuvent être visualisés dans les logs ```docker-compose``` correspondants.