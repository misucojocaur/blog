+++
title = "Pregunta sobre latencia y privacidad de Meshnet"
tags = [
    "Ask the Developers",
    "Skywire",
    "Meshnet",
    "Privacy",
]
bounty = 4
date = "2017-08-18"
categories = [
    "Skywire",
    "Ask the Developers",
]
+++

* Esta publicación se refiere a Skywire anteriormente en su formación, antes de que se llamara Skywire. Adaptado de una publicación de bitcointalk del 09 de agosto de 2014. *

Cita de: ** CraigM ** el 09 de agosto de 2014, 07:20:24 AM

> El meshnet está destinado a ser una buena herramienta de privacidad con beneficios comparables a tor, pero con menos latencia verdad?

> ¿El objetivo de meshnet es permitir que los nodos de financiación a través de micropagos en skycoin cubran los costes de ancho de banda correctos? ¿Esto no obliga a todos los operadores de nodos a registrar registros detallados y publicados (en sus cadenas de bloques personales) describiendo todas las transacciones que corresponden inherentemente a todos los que envían datos a través de su nodo? Esto parece permitir a cualquier tercero realizar ataques de correlación de tráfico muy parecidos a los de tor, excepto que no necesita acceso a las conexiones. Incluso si no terminan siendo inspeccionables públicamente, al iniciar la sesión todo parece que podría tener algunos problemas reales (puede ser solicitada por la policía y ocupa mucho espacio)

> ¿La versión inicial se enviará con servidor de búsqueda de ruta centralizada verdad? Esto significa que si quieres conectarte con alguien, tienes que decírselo a un tercero, ¿correcto? Parece que este no es un servicio de privacidad como Tor hasta que eso esté arreglado. ¿Hay alguna razón para creer que encontrará una solución a esto pronto (o nunca: es difícil)?

> ¿Cómo se encuentra una ruta a este tercero de confianza que hará la búsqueda de ruta para usted? Supongo que será un caso especial (no use la selección de ruta del lado del remitente), pero tengo curiosidad si tiene otro diseño.

## Respuesta

Sí. En realidad es más rápido que TCP / IP. Los ISP hacen el enrutamiento "patata caliente(hot potato)". La latencia no debería ser peor que TCP / IP y, en teoría, puede ser más rápida.

** Las garantías de privacidad son **

- cada nodo solo conoce el salto anterior y siguiente
- la transmisión entre nodos está encriptada
- la transmisión está encriptada de extremo a extremo
- su transmisión está protegida contra ataques man-in-middle mediante el uso de claves públicas para puntos terminales de nodo (las direcciones de red son claves públicas en la red)
- todo el cifrado es negable y efímero
- el protocolo está diseñado para frustrar los intentos de inspección profunda de paquetes para identificar a los usuarios que ejecutan el protocolo (sin puertos fijos, texto llano conocido en formato de cable, conectividad de nodo fijo para red troncal, etc.)

Por lo tanto, es como un TOR de latencia muy bajo con micropagos para el ancho de banda.

- Es más seguro y tiene mayor privacidad que HTTPS
- es más rápido que TOR y escalas, pero todavía hay ataques en su contra.
- el código es mucho más simple que TOR, por lo que hay menos espacio para puertas traseras o vulnerabilidades ocultas. Solo hay una dependencia externa en toda la implementación.
- Si necesita seguridad absoluta contra ataques de canal de tiempo, debe usar un servicio de mezcla o ejecutar Bitmessage por encima del darknet.
- todas las redes de baja latencia están sujetas a ataques de canal de tiempo

## Servidores de ruta

Sí, los servidores de rutas son un enlace débil. Para una privacidad máxima, debe ejecutar su propio servidor de rutas interno.

Sin embargo, si usa un servidor de rutas público, está conectado a él a través de varios saltos, por lo que no puede identificarlo. Aún sería más seguro ejecutar el tuyo.

## Manejo de micropagos para ancho de banda

La forma en que se manejan los micropagos es a través de un tercero. El nodo se conecta a una "** puerta de enlace **", deposita una moneda con la puerta de enlace para obtener un crédito. El nodo ahora puede generar "direcciones" seudónimas de 64 bits con la puerta de enlace. La puerta de enlace no conoce la identidad del nodo de conexión. Solo conoce el salto anterior por el que se produjo la conexión.

Entonces, si establece doce conexiones a la puerta de enlace, se ven como doce usuarios diferentes a la puerta de enlace. Finalmente, la comunicación a la puerta de enlace se realizará a través de un canal de mensajería asíncrono.

El nodo que reenvía el ancho de banda también se conecta a la puerta de enlace. Los dos nodos ahora se pueden pagar unos a otros a través de la puerta de enlace, sin que la puerta de enlace conozca la identidad de ninguno de los dos nodos. Cuando un nodo tiene suficientes monedas en el crédito (una moneda completa), puede generar una nueva dirección de Skycoin y retirar la moneda a esa dirección. Las pasarelas solo manejan pequeñas cantidades de monedas

Una puerta de enlace en el protocolo de Skycoin es cualquier servidor que tenga monedas o saldos de cuenta en nombre de terceros. Las puertas de enlace son instituciones de depósito y tienen su propio protocolo y API.

**Finalmente,**

- Habrá múltiples pasarelas y transferencias de monedas cruzadas. Estas transacciones ocurren en privado y no aparecen en la cadena de bloques hasta que retire las monedas de la puerta de enlace.
- la mensajería con puerta de enlace se producirá a través de un canal de comunicación asíncrono (cada vapor de mensaje tendrá una nueva identidad seudónima)
- Parte del protocolo de la puerta de enlace es una implementación de OT, que le permite probar si una puerta de enlace en particular está robando monedas. Se firma cada llamada API a la puerta de enlace, luego la puerta de enlace se ejecuta y firma la salida. Entonces, hay una cadena de firmas y transacciones vinculadas y la puerta de enlace no puede hacer que las monedas desaparezcan sin poder falsificar su firma. Si depositas monedas en algún lugar y desaparecen, puedes publicar tu registro de transacciones y luego el propietario del nodo acusado debe generar un registro que demuestre que autorizaste las monedas para ir a algún lado. Si no pueden producir una llamada API firmada, entonces prueba que son mentirosas / deshonestas.
- Eventualmente los intercambios operarán bajo el protocolo de puerta de enlace

Su sugerencia de tener una cadena de bloques pública para los saldos internos en la puerta de enlace es interesante. Creo que poner las transacciones internas en una cadena de bloque personal pública, podría mantener los intercambios honestos mientras se mantiene la privacidad del usuario.
