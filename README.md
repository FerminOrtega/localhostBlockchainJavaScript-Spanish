# Crear LocalHost Blockchain Con JavaScript

En ese proyecto es un ejempolo de como podemos crear una base de datos de blockchain con JavaScript.

En primer lugar, Blockchain en una base de datos distribuida que mantiene una lista en continuo crecimiento de registro ordenados, el termino esta ligado a conceptos como transacciones, contratos inteligentes o criptomonedas.

Conceptos de las funcionalidades que tenemos que tener en cuenta a la hora de hacer el BlockChain:

* **Estructura de Bloques:** decidir que estructura va a tener los bloques, es decir, los datos que van a contener cada bloque, para el ejemplo van a tener el índice, la marca de tiempo, linea de datos, su hash y el hash anterior. Este hash anterior debe de estar presente en el bloque siguente para que mantenga una integridad toda la cadena de bloques.

* **Hash del Bloque:** El bloque debe tener un hash para poder hacer la cadena de datos ordenada. El SHA-256 rellena el contenido del bloque (índice + anterior hash + marca de tiempo + datos). Esto no tiene nada que ver con la minería, ya que para hacer esto no se necesita hacer ninguna prueba de trabajo para resolverlo.

* **Generar el Bloque:** Debemos saber el hash del bloque anterior y crear el resto de contenido ( indice, hash, datos y merca de tiempo), los datos son proporcionados por el usuario final.

* **Almacenar los Bloques:** Un simple array de javascript sirve para almacenar la cadena de bloques, el primer bloque de esta cadena es llamado “Genesis-Block” y este tiene que ir codificado. 

* **Validar la Integridad de los Bloques:** Debemos poder validar si un bloque o una cadena de bloques son validos en términos de integridad. Sobretodo si los bloque vienen de nuevos nodos entonces debemos decidir si los aceptamos o no.

* **Elegir la cadena más larga:** Siempre debe existir un solo conjunto explicito de bloques en la cadena, en caso de conflicto, los dos nodos generar el numero del bloque 72, esperaremos a que uno de los dos genere el numero 73 y aceptaremos el mas largo y el otro no lo aceptaremos.

* **Comunicarse con otros nodos:** Una parte esencial de un nodo el compartir el blockchain con otros nodos. Esto tiene que cumplir unas reglas para mantener la red sincronizada:
    * Cuando un nodo genera un nuevo bloque, lo transmite a la red.
    * Cuando un nodo se conecta a la red, pregunta por el último bloque.
    * Cuando un nodo encuentra un bloque que tiene un índice mas grande que el bloque conocido actualmente, agrega el bloque a su cadena actual o las consultas de la cadena de bloque completa.

* **Controlar un nodo:** El usuario debe poder controlar su node de alguna manera. En este caso, lo haremos configurando un servidor HTTP. El usuario puede interactuar de la siguente manera:
    * Ver la lista de bloques.
    * Crear un nuevo bloque con el contenido que desee.
    * Ennumerar o agregar mas nodos a la red.

* **Arquitectura:** El nodo realmente se expone a dos servidores web, uno para que el usuario controle el nodo (Servidor HTTP) y otro para la comunicación peer to peer entre nodos (Servidor HTTP Websocket).

**Como no tiene un algoritmo de minería (PoS o PoW) no se puede usar en una red publica.** 

###### Pasos para el manego de BlockChain:

**Inicio**
`npm install`
`HTTP_PORT=3001 P2P_PORT=6001 npm start //Iniciamos el servidor` 
`HTTP_PORT=3002 P2P_PORT=6002 PEERS=ws://localhost:6001 npm start //iniciamos el primer peer` 

**Obtener el BlockChain**
`curl http://localhost:3001/blocks` 

**Crear un bloque**
`curl -H "Content-type:application/json" --data '{"data" : "Some data to the first block"}' http://localhost:3001/mineBlock` 

**Añadir un peer**
`curl -H "Content-type:application/json" --data '{"peer" : "ws://localhost:6001"}' http://localhost:3001/addPeer` 

**Query de peers conectados**
`curl http://localhost:3001/peers` 

