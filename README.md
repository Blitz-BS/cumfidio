Cumfidio permet d'échanger des messages structurés entre des donneurs d'ordre et des prestataires. 

# structure des messages
La structure de ces messages suit la norme [JSON schema](https://json-schema.org/). La structure des messages est disponible dans le répertoire [json_schema](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema).

La [spécification complète](https://json-schema.org/understanding-json-schema/UnderstandingJSONSchema.pdf) des JSON schémas répondra à toutes vos questions !

On décrit ci-dessous les principaux schémas utilisés

## schéma [message](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/message.schema.json)
Un message comprend 4 propriétés obligatoires
1. `from` : [référence](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/reference.schema.json) de l'émetteur
2. `to` : [référence](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/reference.schema.json) du destinataire
3. `timestamp` : horodatage du message
4. `evenement` : [évènement](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/reference.schema.json) porté par le message

## schéma [référence](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/message.schema.json)
Un référence est l'identifiant d'un objet utilisé dans l'application. Une référence comprend deux propriétés
1. `reference` : la référence elle-même
2. `referentiel` : le référentiel qui sert pour cette référence. Ce peut être le référentiel de l'émetteur, du destinataire, du donneur d'ordre ou du prestataire. Donneur d'ordre et prestataire peuvent être alternativement émetteur ou destinataire des messages.

## schéma [évènement](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/evenement.schema.json)
Un évènement comprend à minima deux propriétés
1. `timestamp` : l'horodatage de l'évènement. L'évènement peut être horodaté au même moment que le message si le message est une instruction ou avoir une date antérieure à celle du message s'il s'agit d'une information. 
2. `classe` : la classe d'évènement. Un évènement peut embarquer plusieurs objets différents selon sa classe.

Pour l'instant 4 classes de message sont disponibles
* affectation
* désaffectation
* paiement
* modification de statut
* modification d'une créance
* modification d'une personne physique

### évènement de classe affectation
Un évènement de classe affectation permet d'affecter un [dossier](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/evenement.schema.json) à un prestataire. 

_exemple de [message de classe affectation](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/exemples/affectation.json)_

### évènement de classe désaffectation 
Un évènement de classe désaffectation permet de désaffecter un dossier d'un prestataire. 

_exemple de [message de classe désaffectation](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/exemples/desaffectation.json)_

### évènement de classe paiement
Un évènement de classe paiement permet d'enregistrer un paiement sur un dossier.

Un évènement de clasee paiement comprend 3 propriétés
* le [paiement](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/paiement.schema.json) lui même
* la [référence](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/reference.schema.json) du dossier
* la [référence](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/reference.schema.json) de l'organisation à l'origine du paiement. Ce peut être par exemple le prestataire ou le donneur d'ordre.

_exemple de [message de classe paiement](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/exemples/paiement.json)_

### évènement de classe modification de statut
Cet évènement permet de modifier le statut d'un dossier ou d'une de ses pièces.

Une modification comprend trois propriétés
* la [référence](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/reference.schema.json) du dossier
* la classe d'objet dont le statut est modifié.
* la transition qui modifie le statut.

Pour l'instant la seule transition disponible est `passageEnIrrecouvrable` appliquée à l'objet `creance`. 

> le formalisme suivi est celui des [automates finis](https://fr.wikipedia.org/wiki/Automate_fini). L'objet peut être dans un nombre fini de statuts (états). L'application d'une transition modifie le statut de l'objet. On pourra décrire des systèmes complexes de statuts (états) grâce à des [diagrammes états-transitions](https://fr.wikipedia.org/wiki/Diagramme_%C3%A9tats-transitions)

### évènement de classe modification d'une créance
Cet évènement permet de modifier un créance.

Une modification comprend les propriétés
* la [référence](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/reference.schema.json) de la créance à modifier.
* le nouvel objet [créance](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/creance.schema.json).

Le nouvel objet écrase l'ancien objet. 

### évènement de classe modification d'une personne physique
Cet évènement permet de modifier une personne physique.

Une modification comprend les propriétés
* la [référence](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/reference.schema.json) de la personne physique à modifier
* le nouvel objet [personne physique](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/personnePhysique.schema.json).

Le nouvel objet écrase l'ancien objet. 

## schéma [dossier](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/dossier.schema.json)
Un dossier a possiblement un statut et des pièces. Les pièces possibles sont
* une [requête en injonction de payer](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/requeteInjonctionDePayer.schema.json)
* une [créance](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/creance.schema.json)
* un [titre](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/titre.schema.json)

## schéma [requête en injonction de payer](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/requeteInjonctionDePayer.schema.json)
Une requête en injonction de payer peut être une pièce d'un dossier.

Une créance comprend les propriétés suivantes
* `id` : identifiant de la requête
* `date` : date de la requête
* `demandeur`: personne physique ou morale
* `creance` : créance pour laquelle la requête est effectuée

## schéma [créance](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/creance.schema.json)
Une créance peut être une pièce d'un dossier.

Une créance comprend les propriétés suivantes
* `id` : identifiant de la créance
* `statut` : statut de la créance (due, impayé définitif...)
* `debiteurs`: liste de personnes physiques ou morales
* `creanciers` : liste de personnes physiques ou morales
* `cautions` : liste des cautions
* `montantDu` : montant du par le débiteur au créancier à une date donnée
* `origine` : origine de la créance. La seule origine possible est un [crédit](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/credit.schema.json)

## schéma [titre exécutoire](https://github.com/Blitz-BS/cumfidio/blob/main/json_schema/titreExecutoire.schema.json)
Un titre exécutoire peut être une pièce d'un dossier.

Un titre exécutoire comprend
* `id` : identifiant du titre
* `statut` : statut du titre (délivré, prescrit)
* `idCreance`: identifiant de la créance
* `date` : date du titre
* `dateExecutoire` : date exécutoire du titre
* `libelle`: libellé du titre

# valider votre json

Il existe de [nombreux scripts](https://json-schema.org/implementations.html#validators) permettant de vérifier que votre JSON correspond bien au schéma.

Par exemple sur python, vous pouvez utiliser `jsonschema`
```python
from jsonschema import validate
import json
import requests

url = 'https://raw.githubusercontent.com/Blitz-BS/cumfidio/main/json_schema/address.schema.json'
req = requests.get(url)
if req.status_code == requests.codes.ok:
  req = req.json()  
# %% validation OK
instance = {
    'rue' : '265 rue Denis Papin',
    'codePostal' : '38090',
    'ville' : 'Villefontaine',
    'codePays' : 'FR',
    'gps' : {
        'latitude' : 45.62127,
        'longitude' : 5.16182
    }
}
validate(instance = instance, schema = req)
# %% validation NOK
instance = {
    'rue' : '265 rue Denis Papin',
    'codePostal' : '38090',
    'ville' : 'Villefontaine',
    'codePays' : 'FR',
    'gps' : {
        'latitude' : 45.62127,
        'longitude' : 'zozo'
    }
}
validate(instance = instance, schema = req)
```
