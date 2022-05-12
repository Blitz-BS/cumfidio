# cumfidio
Cumfidio permet d'échanger des messages structurés entre des donneurs d'ordre et des prestataires. 
## structure des messages
La structure de ces messages suit la norme [JSON schema](https://json-schema.org/). La structure des messages est disponible dans le répertoire [json_schema](https://github.com/xbadiche/cumfidio/blob/main/json_schema).

La [spécification complète](https://json-schema.org/understanding-json-schema/UnderstandingJSONSchema.pdf) des JSON schémas répondra à toutes vos questions !

On décrit ci-dessous l'ensemble des schémas utilisés

### schéma [message](https://github.com/xbadiche/cumfidio/blob/main/json_schema/message.schema.json)
Un message comprend 4 propriétés obligatoires
1. `from` : [référence](https://github.com/xbadiche/cumfidio/blob/main/json_schema/reference.schema.json) de l'émetteur
2. `to` : [référence](https://github.com/xbadiche/cumfidio/blob/main/json_schema/reference.schema.json) du destinataire
3. `timestamp` : horodatage du message
4. `evenement` : [évènement](https://github.com/xbadiche/cumfidio/blob/main/json_schema/reference.schema.json) porté par le message

### schéma [référence](https://github.com/xbadiche/cumfidio/blob/main/json_schema/message.schema.json)
Un référence est l'identifiant d'un objet utilisé dans l'application. Une référence comprend deux propriétés
1. `reference` : la référence elle-même
2. `referentiel` : le référentiel qui sert pour cette référence. Ce peut être le référentiel de l'émetteur, du destinataire, du donneur d'ordre ou du prestataire. Donneur d'ordre et prestataire peuvent être alternativement émetteur ou destinataire des messages.

### schéma [évènement](https://github.com/xbadiche/cumfidio/blob/main/json_schema/evenement.schema.json)
Un évènement comprend à minima deux propriétés
1. `timestamp` : l'horodatage de l'évènement. L'évènement peut être horodaté au même moment que le message si le message est une instruction ou avoir une date antérieure à celle du message s'il s'agit d'une information. 
2. `type` : le type d'évènement. Un évènement peut embarquer plusieurs objets différents selon son type.

Pour l'instant 4 types de message sont disponibles
* affectation
* désaffectation
* paiement
* modification de statut

#### évènement de type affectation
Un évènement de type affectation permet d'affecter un dossier à un prestataire. Pour l'instant le seul type de dossier qui peut être affecté est une [créance](https://github.com/xbadiche/cumfidio/blob/main/json_schema/evenement.schema.json).

_exemple de [message de type affectation](https://github.com/xbadiche/cumfidio/blob/main/json_schema/exemples/affectation.json)_

#### évènement de type désaffectation 
Un évènement de type désaffectation permet de désaffecter un dossier d'un prestataire. 

#### évènement de type paiement
Un évènement de type paiement permet d'enregistrer un paiement sur un dossier.

Un évènement de type paiement comprend 3 propriétés
* le [paiement](https://github.com/xbadiche/cumfidio/blob/main/json_schema/paiement.schema.json) lui même
* la [référence](https://github.com/xbadiche/cumfidio/blob/main/json_schema/reference.schema.json) du dossier
* l'identifiant de l'organisation à l'origine du paiement. Ce peut être par exemple le prestataire ou le donneur d'ordre.

#### évènement de type modification de statut
Cet évènement permet de modifier le statut d'un dossier.

Une modification comprend deux propriétés
* la [référence](https://github.com/xbadiche/cumfidio/blob/main/json_schema/reference.schema.json) du dossier
* la transition. 

Pour l'instant la seule transition disponible est `passageEnIrrecouvrable`. 

> le formalisme suivi est celui des [automates finis](https://fr.wikipedia.org/wiki/Automate_fini). L'objet peut être dans un nombre fini de statuts (états). L'application de transition modifie le statut de l'objet.

## Valider votre json

Il existe de [nombreux scripts](https://json-schema.org/implementations.html#validators) permettant de vérifier que votre JSON correspond bien au schéma.

Par exemple sur python, vous pouvez utiliser `jsonschema`
```python
from jsonschema import validate
import json
import requests

url = 'https://raw.githubusercontent.com/xbadiche/cumfidio/main/json_schema/address.schema.json'
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
