# cumfidio
Cumfidio permet d'échanger des messages structurés entre entreprises.
## structure des messages
La structure de ces messages suit la norme [JSON schema](https://json-schema.org/). La structure des messages est disponible dans le répertoire [json_schema](https://github.com/xbadiche/cumfidio/blob/main/json_schema).

La [spécification complète](https://json-schema.org/understanding-json-schema/UnderstandingJSONSchema.pdf) des JSON schémas répondra à toutes vos questions !

## Valider votre json

Il existe de [nombreux scripts](https://json-schema.org/implementations.html#validators) permettant de vérifier que votre JSON correspond bien au schéma.

Par exemple sur python, vous pouvez utiliser `jsonschema`

  from jsonschema import validate
  import json
  import requests
  
  url = 'https://raw.githubusercontent.com/xbadiche/cumfidio/main/json_schema/address.schema.json'
  req = requests.get(url)
  if req.status_code == requests.codes.ok:
    req = req.json()
