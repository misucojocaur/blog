+++
title = "Skywire - Proyecto Skycoin Meshnet"
tags = [
    "Skywire",
    "Meshnet",
]
bounty = 15
date = "2017-08-29"
categories = [
    "Skywire",
    "Overview",
]
+++

! [Skywire: El Nuevo Internet] (https://i.imgur.com/9Jk0gLe.jpg)

<! - MarkdownTOC autolink = "verdadero" corchete = "redondo" ->

- [Introducción] (# introducción)
- [Enrutamiento: descripción general] (# descripción general del enrutamiento)
- [Incentivos: protocolo de pago] (# incentives-payment-protocol)
- [Enrutamiento de origen: encriptación de capa de enlace] (# source-routing-link-layer-encryption)
    - [Protocolo de ejemplo: Nodos 'A' y 'B`] (# ejemplo-protocolo-nodos-a-y-b)
    - [Posibles mejoras:] (# posibles-mejoras)
- [Puerta de enlace IPv4: puenteando los ISP existentes] (# ipv4-gateway-bypassing-existing-isps)
    - [Ejemplo uno] (# ejemplo-uno)
    - [Ejemplo dos] (# ejemplo-dos)
- [Arquitectura Skywire de Servicios Daemon ] (# skywire-daemon-services-architecture)
    - [Ejemplo de servicio: Blockchain Sync] (# example-service-blockchain-sync)
        - [Encontrando Peers] (# finding-peers)
        - [Envío y recepción de mensajes] (# sending-and-receiving-messages)
- [Enrutamiento multi-hogar y agregación de enlaces] (# multi-home-routing-and-link-aggregation)
- [Enrutamiento Meshnet : Almacenar y Reenviar] (# meshnet-routing-store-and-forward)
- [Almacenar y reenviar: utilización de capacidad] (# store-and-forward-capacity-utilization)
- [Almacenar y reenviar: ejemplos] (# store-and-forward-examples)
    - [Ejemplo de operación normal] (# ejemplo-de-operación-normal)
    - [Ejemplo con congestión] (# ejemplo-con-congestión)
    - [Ejemplo con pérdida de paquete] (# example-with-packet-loss)
- [Almacenar y reenviar: Producto de latencia de ancho de banda] (# store-and-forward-bandwidth-latency-product)
- [Almacenar y reenviar: utilización de la capacidad, calidad y servicio] (# store-and-forward-capacity-utilization-quality-and-service)
- [Enrutamiento de origen: conectividad móvil de múltiples rutas] (# source-routing-multi-route-mobile-connectivity)
- [Enrutamiento de Fuente: Nodos Guardia] (# source-routing-guard-nodes)
- [Enrutamiento de origen: Limitaciones de BGP] (# source-routing-limitations-of-bgp)
- [Rutas virtuales: topología de red Skywire a escala] (# rutas virtuales-skywire-network-topology-at-scale)
- [Enrutamiento de origen: Rutas virtuales, topología SONET] (# source-routing-virtual-routes-sonet-topology)
- [Enrutamiento de origen: conectividad asimétrica] (# source-routing-asymmetric-connectivity)
- [Enrutamiento de origen: Descubrimiento de ruta] (# source-routing-route-discovery)

## Introducción

Objetivos de Skywire:

* Aumentar la competencia de banda ancha. Proporcionar una alternativa a los ISP existentes. Span last mile.
* Permitir a las comunidades construir ISP que se ejecutan en la infraestructura operada por el usuario.

Skywire es un nuevo protocolo de darknet.

* Baja latencia (tan rápido como TCP / IP y teóricamente más rápido en la red nativa)
* Alto rendimiento (diseñado para aplicaciones de video, intercambio de archivos y alto rendimiento)
* Preservación de privacidad
* Admite Operación por Wifi (Meshnet)
* Admite la operación de Clearnet (Darknet / Overlay)

Skywire resuelve el problema de incentivo y descarga para el despliegue de la red.

* Los usuarios reciben Skycoins por proporcionar recursos de red
* Los usuarios consumen monedas para consumir recursos de red

Skywire es de acceso abierto.

* Cualquier persona con un cliente puede conectarse a cualquier nodo Skywire
* El objetivo es crear un mallado de acceso abierto global

Skywire preserva la privacidad.

* El tráfico que pasa por su nodo no se remonta a su dirección IP
* El tráfico de reenvío de nodos solo puede ver el salto anterior y siguiente
* Los terceros que observan pasivamente no pueden vincular paquetes individuales a un flujo o usuario
* Los terceros y los nodos de reenvío no pueden leer los contenidos del tráfico

## Enrutamiento: descripción general

La malla de Skywire utiliza un protocolo de almacenamiento y envío enrutado por la fuente.

El núcleo de la red de superposición es una serie de nodos.

* Cada nodo se identifica mediante un hash de clave pública
* Cada nodo recibe mensajes y reenvía mensajes
* Los nodos reciben monedas para el tráfico de reenvío

Para comunicarse entre los nodos `A` y` C`, a través del nodo `B`:

* El nodo `A 'se conecta al nodo` B` y establece una ruta
* El nodo `A` amplía la ruta a` C`
* El tráfico enviado sobre la ruta en `A` llegará a` C`

En una ruta `A -> B -> C -> D`:

* Los nodos solo conocen el salto anterior y siguiente en la ruta.
* `C` sabrá que el mensaje pasó por` B` y se dirige a `D`. `C`, sin embargo, no conocerá la identidad de` A`.
* `B` no puede inferir que` A` es el origen de la ruta.
* `C` no puede inferir que` D` es el destino final del mensaje
* `B` y` C` no pueden leer el contenido del mensaje (cifrado de extremo a extremo)
* Un observador pasivo que no participa en el enrutamiento de un mensaje en particular no puede obtener ninguna información sobre el contenido del mensaje (encriptación de capa de enlace).
* Se pueden agrupar múltiples mensajes de múltiples rutas al mismo destino, lo que reduce la capacidad del observador pasivo para realizar análisis de tráfico.

En la implementación más simple, una ruta es un prefijo de 128 bits.
Cada nodo lee el prefijo y hace una búsqueda en una tabla,para determinar el siguiente nodo para reenviar el paquete a.

La fuente tiene control completo sobre el enrutamiento.

* Cada nodo puede actualizar su protocolo de enrutamiento de forma independiente para satisfacer sus necesidades
* La fuente puede optimizar las rutas de red para rutas de red de baja latencia para VOIP o juegos
* La fuente puede optimizar las rutas de red para el rendimiento de las aplicaciones de video y uso compartido de archivos
* La fuente puede agrupar múltiples rutas al destino para redundancia, latencia reducida y rendimiento

Algunas aplicaciones agruparán múltiples rutas paralelas en la capa de aplicación para:

* privacidad (nodos de gatekeeper, servicios tipo tor de gateway/anon)
* rendimiento
* latencia reducida
* redundancia

Este es el núcleo de la red superpuesta de Skycoin. Es muy simple, pero extremadamente poderoso.
Los detalles técnicos y de implementación serán discutidos más adelante.

Skywire simplemente prefija un paquete con una identificación de ruta.

* El enrutamiento es una búsqueda de tabla muy simple
* Gastos generales por paquete son constantes y no aumentan en rutas largas

Notas:

* El destino no conoce la identidad del origen. La identidad ya no está en la capa de enrutamiento, sino en la capa de aplicación. La identidad debe ser confirmada a través de la infraestructura de clave pública.
* Los ataques de hombre en el medio no son posibles. Una fuente puede verificar el destino a través de su clave pública.
* La privacidad se mejora significativamente desde IPv4, donde cualquier persona que maneje un paquete puede ver el destino, la fuente y el contenido del paquete.
* El rendimiento es superior a IPv4 / BGP porque los ISP utilizan el enrutamiento de patata caliente (hot potato).
* El cifrado de extremo a extremo elimina los ataques de inyección de paquetes y la falsificación. El tráfico spoofing requiere las claves privadas para cada extremo del túnel de conexión.
* La encriptación es rápida. El objetivo es un rendimiento de 10 Gb / s en hardware FPGA, 200 Mb / s en ARM.

## Incentivos: Protocolo de pago

! [Skywire minero] (https://i.imgur.com/2zj4CUV.jpg)

* [Skywire "minero"] (/ statement / skywire-miner-hardware-for-the-next-internet /) *

Los nodos quieren enviar tráfico y recibir monedas. Esto es el equivalente de "minería" en Skycoin y cuántos usuarios obtendrán sus primeras monedas.

Los pagos por tránsito no deben revelar la identidad del nodo fuente. Skycoin usará pagos ciegos en custodia a través de un tercero hasta que se desarrolle un protocolo mejor.

Cada nodo en la ruta registra el tráfico y el nodo de origen registra el tráfico. Establecen pagos de ancho de banda periódicamente.

El origen contiene monedas en custodia con un tercero. Se crea una cuenta de pseudónimo con el tercero. Cada nodo puede verificar la reputación del origen y la capacidad de pago a través del tercero, sin conocer la identidad de la parte. Para el tercero, cada origen aparecerá como múltiples cuentas de pseudónimo no vinculadas. Cada nodo de tránsito aparecerá como múltiples cuentas de pseudónimo no vinculadas.

Las pequeñas transacciones se liquidarán internamente, en transacciones fuera de blockchain. Las transacciones fuera de blockchain pueden retirarse a una dirección recién generada, nunca utilizada antes de que la balanza exceda un umbral (actualmente 1 Skycoin). Esto reduce la hinchazón de la cadena de bloques y fomenta que las microtransacciones se realicen fuera de la cadena de bloques.

## Enrutamiento de Fuente: Encriptación de capa de enlace

Existe un cifrado de capa de enlace predeterminado entre saltos y cifrado extremo a extremo predeterminado. Una aplicación típica usará cifrado de capa de enlace, cifrado de extremo a extremo y luego el cifrado de capa de aplicación apropiado.

La encriptación entre nodos debe ser rápida. Las implementaciones de FPGA deben admitir una operación de 10 Gb / s a ​​la velocidad de la línea. Los procesadores ARM deben ser capaces de soportar 250 Mb / s.

El mejor candidato actual es ChaCha20 con cambio de clave efímera ECC secp256k1.

ChaCha20 solo usa operaciones aritméticas simples, es más rápido que AES para dispositivos integrados y es más resistente a ataques de canales de tiempo que AES.

Una CPU moderna puede realizar operaciones ECDH de 6000 secp256k1 por segundo. La rotación de la tecla de sesión debe ser una vez por segundo o el doble de la latencia de ida y vuelta entre nodos. Debe haber una clave separada para cada dirección de comunicación.

La clave de sesión anterior debe ser acumulada en el secreto recibido a través de ECDH.

La clave de sesión, establecida a través de la criptografía de clave pública (ECC), se utiliza para encriptar comunicaciones utilizando un algoritmo de cifrado asimétrico más rápido (AES, ChaCha20). Esta es la encriptación básica de la capa de enlace entre los nodos.

### Protocolo de ejemplo: Nodos `A` y` B`

- El nodo `A` quiere generar una clave de sesión para enviar datos cifrados al nodo` B`
- El nodo `B` tiene la clave pública` P`, con la clave privada `p`. `P` es un punto en la curva ECC sep256k1. `p` es un entero de 256 bits. `P` es el punto base b, elevado a la potencia de p con la operación de adición de curva.
- El nodo `A` genera una clave pública efímera` Q`, con la clave privada `q`. (El nodo `A` genera aleatoriamente un entero de 20 bytes. Esta es la clave privada` q`. El nodo `A` aumenta el punto base a la potencia de` q`, para generar la clave privada `Q`, que es un punto en la curva).
- El nodo `A` envía,` P` * `q` (el punto en la curva` P`, que es la clave pública de `B`, elevada a la potencia de` q`)
- El nodo `A` envía` P` al nodo `B`
- El nodo `B` recibe` P` y calcula `P * q`, el nodo` A` puede calcular `p * Q`, que son iguales. Este es el secreto compartido, que es hash para generar la clave de sesión.
- `P = b * q`, entonces` P * q` es igual a `(b * p) * q`. `P * q = (b * p) * q = (b * q) * p = Q * p`, ya que` Q = b * q`. `A` conoce` q, Q` y `P`, y` B` conoce `p, P` y` Q`. Entonces 'A' y 'B' pueden calcular `P * q` y` Q * p` y usar esto como su secreto. Sin embargo, un tercero no conoce las claves privadas `q` para` A` ni conoce la clave privada `p` para` B`, por lo que un tercero no puede calcular este "secreto" y, por lo tanto, no puede leer nada cifrado bajo el secreto .
- El nodo `B` confirma la recepción de la actualización de la clave de sesión. El nodo `A` comienza a transmitir bajo la nueva clave de sesión tan pronto como se recibe la confirmación de` B`.
- El nodo `A`, envía mensajes al nodo` B` al encriptarlos usando ChaCha20 usando la clave de sesión, como la clave de encriptación asimétrica.

### Posibles mejoras:

- Actualizaciones de clave de sesión frecuente. Cambio de clave ECDH cada pocos segundos o minutos.
- Mantenga presionada la tecla de sesión anterior con el nuevo secreto de ECDH para generar una nueva clave de sesión.
- Agregue nonces a paquetes y hash secret a nonce para generar la clave para cada mensaje. La misma clave nunca se reutiliza. Reduce el impacto del conocido ataque de texto claro.
- Eliminar el texto plano conocido en los mensajes.
- Calcular mensajes a múltiplos de 16 o 32 bytes.

## Puerta de enlace IPv4: puenteando los ISP existentes

Muchas personas tienen solo una opción para ISP. Esto describe brevemente cómo Skywire puede aumentar la competencia.

Algunas aplicaciones pueden ejecutarse de forma nativa en el espacio de direcciones de Skywire. Algunas aplicaciones como Bittorrent, la sincronización de archivos y las aplicaciones de comunicación se benefician enormemente de la infraestructura de Skywire y se modificarán para que se ejecuten de forma nativa en ella.

Las aplicaciones heredadas, como Netflix, Facebook, Twittter requieren una puerta de enlace de red que conecte la red de superposición Skywire con las redes IPv4 e IPv6.

Un usuario elige una puerta de enlace Skywire que se ejecuta en un servidor en un centro de colocación local. El tráfico IPv4 de los usuarios se canalizará a través de la puerta de enlace (similar a una VPN). La IP de los usuarios aparecerá como la IP del servidor de puerta de enlace. El servidor tiene una conexión gigabit para múltiples redes troncales de Internet rápidas, en proveedores que no califican el límite de Netflix. El usuario tiene múltiples opciones de proveedores para las pasarelas Skyvire IPv4. El proveedor de la puerta de enlace se pagará en Skycoin de forma medida.

El nodo Skywire en el hogar de los usuarios se conecta a la puerta de enlace en todas las rutas posibles. El nodo Skywire canaliza el tráfico IPv4 desde un enrutador a la puerta de enlace en el centro de colocación. La dirección IP del nodo de puerta de enlace es la dirección IP que aparece para el usuario.

### Ejemplo uno

Un usuario tiene un cablemódem de 10 Mb / s. Instalan un enrutador Skywire. El enrutador está conectado a su computadora, a un nodo Skywire Wifi y a su modelo de cable. El enrutador está configurado con Skywire como un túnel IPv4. Ellos enchufan su computadora al enrutador.

El nodo wifi Skywire se conecta a su vecino nodo Skywire a través de wifi, que está conectado a un cablemódem de 10 Mb / s. El vecino también tiene un wifi de 200 Gb / s a ​​5 GHz con una antena direccional punto a punto conectada a un negocio con un nodo wifi Skywire en la misma calle.

El enrutador Skywire de los usuarios hace una primera búsqueda recursiva de los nodos con la conectividad clearnet y establece rutas sobre

* Su cable módem
* Wifi -> su vecino cable módem
* Wifi -> 5 GHz punto a punto -> 100/30 Mb / s de negocios / caída de fibra

El usuario podrá acceder y agregar ancho de banda en todas las rutas, al conectarse al túnel IPv4. En las comunidades donde el ancho de banda agregado y la confiabilidad han alcanzado un cierto nivel, el usuario ya no necesita el cablemódem para la conectividad.

### Ejemplo dos

El negocio de mas abajo de la calle tiene una caída de fibra de 100/30 Mb / s con un SLA. El negocio paga una tarifa fija por internet. Cualquier ancho de banda que no usan se pierde. La empresa conecta un enrutador Skywire. El enrutador tiene 3 puertos. El primer puerto es su conexión WAN, el segundo puerto es su red interna, el tercer puerto va a sus nodos wifi Skywire en el techo. El enrutador almacena en búfer y prioriza el tráfico en la red interna y asigna la capacidad no utilizada al tráfico de Skywire. El operador recibe Skycoins para tránsito, que subsidia el costo de la caída de fibra.

## Arquitectura Daemon de Servicios Skywire

* Cada nodo de Skycoin tiene una publicación pub Secpk256k1.
* Cada nodo de Skycoin tiene una dirección de Skycoin que lo identifica. La dirección es un hash de la clave pública del nodo. Este hash de clave pública es el equivalente de una dirección IP en la red.
* Cada nodo de Skycoin tiene un grupo de conexión de pares al que está conectado. Estos pueden ser pares sobre TCP, conexiones clearnet UDP, conexiones físicas a través de Ethernet directo y peer Wifi (operación de malla). Una conexión también puede ser una "conexión virtual" que se tuneliza a través de una conexión física o clearnet y se describirá más adelante.
* Cada instancia de conexión con un par tiene "canales". Un canal es un entero de 16 bits que es similar a un "puerto" en TCP.
* Todos los mensajes enviados y recibidos tienen un prefijo de una longitud de 32 bits y un canal de 16 bits.
* El canal 0 está reservado para la comunicación entre Skywire Daemons, exponiendo metainformación sobre los servicios que se ejecutan en el Daemon y otros datos necesarios para el funcionamiento de la red.
* Un daemon Skywire puede exponer "servicios" en un canal. Un servicio es un proceso que maneja los mensajes de datos recibidos en un canal y origina mensajes de datos dirigidos a pares y servicios remotos.

### Ejemplo de servicio: Blockchain Sync

* Este ejemplo se refiere a una implementación de Golang, pero la arquitectura de daemon es independiente del idioma. *

Desea sincronizar dos cadenas de bloques personales diferentes con claves públicas A y B. Usted inicia dos instancias del "servicio de sincronización de blockchain", las configura con las claves públicas respectivas y las asocia con Skycoin Daemon. Estos servicios se ejecutan en su daemon local, cada uno en un canal en particular.

#### Encontrar compañeros

El blockchain hace un daemon sync de la clave pública hash y realiza una búsqueda DHT (tabla hash distribuida) para buscar otros pares que sincronicen la cadena de bloques. Una vez que se encuentran los peers, los peers se pueden presentar a otros peers a través de PEX (intercambio entre iguales).

#### Envío y recepción de mensajes

Los servicios en la creación registran una lista de mensajes a los que responden y pueden enviar. Los mensajes son estructuras de Golang. La información de la estructura del mensaje se completa y luego se envía. Los datos llegan y se invoca el método .Handle () en la estructura de mensaje correspondiente.

## Enrutamiento multipropósito y agregación de enlaces

Si tiene un cablemódem de 2 Mb / s y su vecino tiene un módem de cable de 2 Mb / s y cada uno de ustedes está ejecutando nodos Skywire, entonces su nodo Skywire puede conectarse a su nodo y agregar el ancho de banda a través de ambas conexiones. Los paquetes ahora pueden tomar rutas a través de su módem de cable y rutas a través de su cablemódem. Los cablemódems son el punto de estrangulación. Para obtener una conexión de 4 Mb / s, el tráfico debe pasar en rutas paralelas a través de ambos módems.

Aplicaciones como Bittorrent podrán agregar ancho de banda en todas las conexiones disponibles, ya que de forma nativa se abren una gran cantidad de conexiones, que tomarán las rutas del distrito.

## Meshnet Routing: almacenar y reenviar

Para los nodos en el borde de la red que se comunican a través de la malla, existen algunos problemas.

Si tiene una red de ocho saltos a través de Wifi y el 50% de los paquetes caen en cada salto, solo 1 de 256 paquetes lo completará. La caída de paquetes es normal en Wi-Fi, pero el TCP tradicional trata la caída de paquetes como congestión y acelera la velocidad de conexión.

En el borde de la red, Skywire usará almacenar y reenviar a lo largo de las rutas. Esto impone un requisito de memoria en los nodos Skywire, pero mejora significativamente el rendimiento de la red.

Para la ruta `A-> B-> C`

* Cada ruta tiene un buffer.
* Cada nodo sigue enviando los mensajes hasta que sean recibidos y confirmados.
* Si el Buffer de `B-> C` está lleno para la ruta, entonces` A` sabrá esto y dejará de transmitir datos hasta que se haya confirmado que los datos enviados tienen espacio en el búfer.

Por lo tanto, hay dos reconocimientos entre los nodos en la capa de enlace. Un reconocimiento es un reconocimiento afirmativo de que se recibieron segmentos de datos transmitidos. Otro es un reconocimiento de que los datos del buffer han sido enviados y confirmados por el siguiente nodo en la ruta.

## Almacenar y reenviar: utilización de la capacidad

En las redes IP tradicionales, a medida que los enlaces de red se utilizan hacia la capacidad, la eficiencia disminuye. Una red que se ejecuta al 80% de su capacidad enfrenta el riesgo de que un estallido de datos a corto plazo provoque que un enrutador supere la capacidad y los paquetes de red se eliminen.

TCP interpreta los paquetes descartados por cualquier motivo como la congestión y acelera la velocidad de retroceso. Los paquetes descartados también requieren retransmisión bajo TCP e introducen latencia a medida que la aplicación espera el tiempo de espera y la retransmisión antes de que pueda procesar el resto del flujo de paquetes.

Con la operación de almacenamiento y reenvío, los buffers de ruta se llenan y luego no pasa nada. Cuando los buffers se llenan, el nodo entrante deja de enviar datos hasta que se le notifica el espacio en el búfer.

Esta operación de almacenamiento y reenvío es especialmente importante para redes de malla WiFi prácticas. Solo hay tres canales no superpuestos en la banda de 2,4 GHz. La pérdida de paquetes aumenta muy rápidamente y muy temprano en comparación con el punto de saturación de ancho de banda para redes WiFi. La pérdida de paquetes Wifi es inevitable y no indica de manera confiable límites de congestión o capacidad.

Almacenar y reenviar nos permite ejecutar los nodos Wifi a plena capacidad y saturar todo el ancho de banda disponible, sin activar los controles de congestión TCP.

Las redes prácticas requerirán:

* Radio definida por software
* MIMO
* Formación de haz
* Antenas direccionales
* Coordinación cooperativa temporal y geofísica del tiempo de transmisión, potencia de transmisión y uso de frecuencia
* 801.11af frecuencias de espacio en blanco

## Almacenar y reenviar: ejemplos

Cada nodo, para cada ruta, realiza un seguimiento de:

* Tamaño del búfer para recibir el nodo para la ruta
* Tamaño del búfer previsto (se transmiten segmentos de datos incrustados y no procesados)
* Tamaño de búfer asignado
* Desplazamiento, tamaño y secuencia de cada mensaje transmitido que no se ha visto
* Búfer circular de bytes de datagramas salientes que no han recibido un ack

Un segmento de datos en la capa de enlace puede contener mensajes concatenados, desde múltiples rutas dirigidas al mismo nodo. Esto frustra el análisis de tráfico y mejora el rendimiento al permitir datagramas más grandes en redes que admiten MTU más altas.

Para cada mensaje transmitido, hay dos acks. El primer ack es que el datagrama ha sido recibido por el siguiente nodo en la ruta. Esta es una prueba para el datagrama, que puede contener múltiples mensajes, cada uno correspondiente a diferentes rutas. Después de recibir este ack, el nodo ya no necesita retener el datagrama. Si el datagrama no está anotado, entonces necesita ser reenviado.

El segundo ack son actualizaciones sobre los bytes libres restantes en el buffer entrante para una ruta. Si los bytes libres en el buffer para una ruta son lo suficientemente grandes, entonces se pueden transmitir mensajes adicionales para esa ruta.

Otro enfoque posible, mantiene un buffer por remitente en lugar de por ruta, con el receptor enviando mensajes de bloque al remitente para las rutas congestionadas. Esto reduce el número de búsquedas de hash de ruta requeridas por el remitente y es algo con lo que debe experimentarse.

### Ejemplo de funcionamiento normal

Ruta: `A-> B-> C`

* B tiene un buffer de 1024 KB para la ruta
* A envía 512 KB a B
* B reconoce los 512 KB a A
* <A recibe el acuse de recibo (y borra primero 512 KB, ya no necesita ser almacenado)>
* B remite 512 KB a C
* C acusa recibo de los 512 KB
* C reconoce a A que se han reenviado los 512 KB

### Ejemplo con congestión

Ruta: `A-> B-> C`

* B tiene 1024 buffer para la ruta
* A envía 512 KB a B
* A envía 256 KB a B
* A envía 256 KB a B
* <A deja de enviar porque está pendiente, ya es suficiente para llenar el búfer en B>
* B confirma a A la recepción de 512 KB y 512 KB
* B envía 256 KB a C
* C acusa recibo de 256 KB a B
* B confirma a B, reenvío de 256 KB a A
* <A ahora puede enviar, hasta 256 KB de KB adicional>


Se supone que los datos se reciben en el orden enviado para WiFi y conexión de Ethernet directa

### Ejemplo con pérdida de paquetes

Ruta: `A-> B-> C`

* B tiene 1024 buffer para la ruta
* A envía 512 KB a B
* A envía 256 KB a B
* B acks los 256 KB
* A infiere que no se recibieron los 512 KB
* A retransmite los 512 KB
* B acks los 512 KB
* <B ahora puede continuar enviando stream para C>

## Almacenar y reenviar: Producto de latencia de ancho de banda

En almacenamiento y reenvío se impone un requisito de almacenamiento en el nodo transmisor igual al producto de la latencia de ida y vuelta y la velocidad de transmisión multiplicada por la latencia de ida y vuelta. 1 GB de RAM es suficiente para 8000 ms de latencia de ida y vuelta a 1 Gb / s de velocidad de transmisión.

Almacenar y reenviar debe ser predeterminado, pero opcional.


## Almacenar y reenviar: utilización de la capacidad, calidad y servicio

Las descargas de video, audio y archivos están almacenadas en el búfer. El rendimiento promediado absoluto durante un intervalo de tiempo de segundos importa, mientras que la latencia es irrelevante. El resto del tráfico, como solicitudes de sitios web, videojuegos y voip, es en tiempo real y debe entregarse lo más rápido posible.

Con dos niveles de calidad de servicio, "Real Time" y "Bulk", podemos transmitir tráfico de VOIP, sitios web y videojuegos en primer lugar, reduciendo la latencia para este tráfico. El tráfico insensible a la latencia, como el video, la música y el intercambio de archivos, solo fluiría a través del enlace después de que el búfer de tráfico en tiempo real esté vacío.

Podemos utilizar enlaces a casi el 100% de la capacidad al tiempo que disminuimos la latencia para el tráfico en tiempo real. Por lo tanto, proponemos apoyar dos niveles de calidad de servicio para las rutas.

## Source Routing: conectividad móvil de múltiples rutas

Si las conexiones entre nodos son estables, de baja latencia y tienen un ancho de banda alto, una sola ruta es suficiente para la mayoría de las aplicaciones. Algunas aplicaciones, como Bittorrent, abren una gran cantidad de conexiones y de forma nativa pueden usar ancho de banda en todas las rutas disponibles.

Si los enlaces entre nodos son lentos, poco confiables o la conectividad está cambiando, la confiabilidad y el rendimiento requieren que el tráfico se multiplexe en múltiples rutas redundantes.

Si un nodo Skywire que se ejecuta en un teléfono celular está en un automóvil que circula por la calle, las redes a las que se puede acceder cambiarán. Los nodos de red entrarán en rango y otros nodos de red dejarán el rango. El nodo debe tener conectividad continua en la capa de aplicación, incluso cuando las conexiones físicas se crean y se destruyen.

Un enfoque es elegir un conjunto de nodos confiables en la red troncal de la red como puntos de terminación para una ruta y luego transmitir el tráfico a través de estos nodos, a través de un conjunto de múltiples rutas a corto plazo.

## Enrutamiento de Fuente: Confiabilidad de múltiples rutas

Si los enlaces no son confiables o tienen una latencia muy variable, es deseable codificar los datos de la aplicación en múltiples rutas, de modo que los datos puedan recuperarse si se reciben datos de cualquiera de las rutas. Existen códigos fuente y otros métodos de codificación que pueden ser aplicables aquí.

## Enrutamiento de Fuente: Guard Nodes

Para privacidad, si un usuario desea debilitar aún más la capacidad de enlace entre su dirección de nodo Skywire (hash de clave pública) a su dirección IP, puede destinar un conjunto fijo de nodos que se anuncian como puntos de tránsito para el tráfico destinado a su direccion.

## Enrutamiento de Fuente: Limitaciones de BGP

Border Gateway Protocol, el protocolo de enrutamiento dominante actual, maneja el problema de enrutamiento al no mantener ningún estado para los paquetes. En lugar de BGP, permite que cada red cree una serie de reglas ad-hoc para cada uno de sus enrutadores que observan el origen y el destino de un paquete y deciden a qué interfaz de red reenviar el paquete. Los enrutadores se envían mensajes entre sí con información de conectividad y otro algoritmo de enrutamiento se usa para el enrutamiento dentro de un dominio de red.

BGP está diseñado para interactuar con una serie de redes autónomas independientes. BGP tiene una suposición de homogeneidad, se considera que el enrutamiento dentro de un dominio autónomo es centralmente administrado y altamente confiable con enrutamiento homogéneo dentro del dominio. Los ISP de redes de malla y comunitarios serán ad-hoc con conectividad y enrutamiento de dispositivos heterogéneos.

La conectividad en redes de malla, las configuraciones ad-hoc y las redes densamente interconectadas con rutas de enrutamiento de múltiples hogares redundantes violan por completo las suposiciones jerárquicas BGP.

BGP tiene varios problemas que un protocolo de próxima generación debería abordar:

* BGP no es autoconfigurable. Las redes basadas en BGP requieren una amplia experiencia técnica para configurar y operar
* Los sistemas BGP a menudo requieren una configuración manual para evitar el daño y no son resistentes a las configuraciones incorrectas.
* BGP requiere la creación manual de reglas de filtrado de rutas ad-hoc y una mayor complejidad para las redes con conectividad multi-hogar
* Las redes BGP requieren una planificación altamente centralizada
* La NSA ha explotado las fallas en BGP para enrutar el tráfico dirigido a los puntos de intercepción
* Las suposiciones de BGP se vuelven cada vez más tensas, especialmente para redes ad-hoc, malla y móviles
* Las suposiciones jerárquicas de ruta única de BGP hacen que la implementación de multi-homing y otros requisitos de redes de próxima generación sea extremadamente difícil
* BGP sufre graves problemas cuando los enlaces de red no son confiables, como el aleteo de ruta.
* El tamaño de la tabla de enrutamiento BGP crece exponencialmente a medida que proliferan las subredes interconectadas.
* Multihoming provoca una explosión masiva en el tamaño de la tabla de enrutamiento BGP.
* BGP tiene dificultades con el equilibrio de carga y el enrutamiento de múltiples hogares. BGP limita la capacidad en redes prácticas para aprovechar la conectividad paralela entre ubicaciones.
* BGP crea un incentivo para que los ISP vuelquen el tráfico de red a otras redes lo más rápido posible ("Hot Potato Routing"), reduciendo el rendimiento y aumentando la latencia.

No hay alternativa a BGP. BGP es la mejor solución dentro de sus limitaciones de diseño.

El sucesor de BGP debe:

* Sea no jerárquico
* Sea autoconfigurable (cero-conf)
* Funciona bien con densas interconexiones ad hoc y redundantes entre redes

## Rutas virtuales: topología de red Skywire a escala

La implementación del enrutamiento Skywire requiere un nodo para mantener la información de cada ruta que pasa por ella. Los nodos individuales no pueden manejar cientos de miles de rutas individuales y la escalabilidad se logra a través de otro mecanismo.

Skywire está experimentando con un enrutamiento no jerárquico y autoorganizado que admite nativamente multihoming y topografías de red no jerárquicas mientras escala de manera eficiente.

Skywire minimiza el diámetro de la red a medida que la red escala mediante el uso de rutas virtuales. Las rutas virtuales permiten agrupar miles de conexiones a través de una conexión troncal de ancho de banda alto con la sobrecarga de una ruta única.

Una "ruta virtual" crea un túnel sobre una ruta existente:

`A -> B -> C -> D`

La ruta virtual aparece como A-> D. B y C pueden ser conexiones de larga distancia de gran ancho de banda. B y C solo incurren en la sobrecarga de una sola ruta, mientras que A y D incurren en la sobrecarga de mantener las rutas en el túnel A-> D.

La ruta virtual puede contener tráfico de cientos de rutas agrupadas de A a D, mientras que B y C solo experimentan una sobrecarga de una sola ruta. La ruta virtual puede además, agrupar múltiples rutas de red redundantes entre el origen y el destino para el rendimiento, el rendimiento y la redundancia.

Las rutas virtuales permiten que la capacidad de la red se agrupe aproximadamente de forma jerárquica con nodos en cada capa que tienen un ventilador constante y una sobrecarga.

Los nodos en el borde de la red se introducen en los nodos de agregación. Los nodos de agregación perimetral están conectados a un tránsito intradomain de ancho de banda alto y se alimentan a los nodos de la puerta de enlace que interactúan entre redes. Los nodos Gateway alimentan un ancho de banda alto y un transporte de larga distancia.

Las rutas virtuales son una representación de las relaciones de enrutamiento entre dominios existentes, que admiten de forma nativa:

* Enrutamiento no jerárquico (centros de datos)
* Multiple-homing
* Densa interconexión de red entre dominios en diferentes niveles de jerarquía
* Enrutamiento de múltiples rutas dentro y entre los dominios de la red

Las rutas virtuales obedecen una igualdad triangular. Si el costo de una ruta A-> B es C (A-> B), entonces

`C (A-> B-> C)> = C (A-> B) + C (B-> C)`

La preferencia de origen para rutas de baja latencia, bajo costo y bajo salto crea incentivos económicos para crear una topología de red eficiente. La red no es jerárquica y se autoorganiza. Las rutas virtuales que se crean son resúmenes de ruta que naturalmente reflejan el flujo de tráfico.

En BGP, las redes intentan deshacerse del tráfico lo más rápido posible (enrutamiento de patata caliente). En Skywire, las redes compiten para proporcionar tránsito (para recibir incentivos de monedas). Los clientes de Skywire tendrán preferencia, bajo costo, bajo conteo de saltos y rutas de baja latencia. Las redes con capacidad de larga distancia directa entre el origen y el destino tienen una latencia menor y un conteo de saltos inferior y, por lo tanto, reciben preferencia.

Para mayor eficiencia, la capacidad de ancho de banda y abanico (número de rutas que agrupa cada ruta virtual) en cada nivel de la jerarquía de red debe ser constante para lograr un crecimiento constante de la tabla de enrutamiento logarítmico y el diámetro de la red en el número de hosts.

## Enrutamiento de Fuente: rutas virtuales, topología SONET

Una ruta virtual de múltiples entradas y múltiples salidas se puede implementar físicamente como un anillo SONET, con nodos Skywire en cada ciudad a través de la cual pasa la topología SONET. Los nodos Skywire actúan como un enrutador de puerta de enlace entre la red Skywire y la topología SONET.

Los nodos pueden poner en cola grandes datagramas que concatenan múltiples mensajes de la misma fuente al mismo destino para mayor eficiencia.

El mensaje ingresa al nodo Skywire del anillo SONET en un centro de colocación en una ciudad. El destino o la ruta del mensaje se lee y el mensaje se codifica para su transporte a través del segmento SONET. El mensaje llega al nodo Skywire de destino en el segmento SONET y continúa en su ruta.

Por lo tanto, una ruta virtual de múltiples entradas y salidas múltiples es una lista de nodos Skywire, con un costo de tránsito, que describe un anillo SONET o topología totalmente conectada, donde cualquier nodo en la lista tiene tránsito a cualquier otro nodo en la lista.

## Enrutamiento de Fuente: Conectividad asimétrica

Los sistemas wifi de próxima generación tendrán antenas 4x4 y 8x8 en una disposición MIMO de arreglo en fase. Dichos sistemas son capaces de proyectar haces direccionales altamente enfocados. Estos sistemas aumentan significativamente la potencia y la potencia de la señal en el receptor, pero no mejoran simétricamente la ganancia de la antena para las señales de retorno.

De manera similar, se puede recibir una señal wifi amplificada de alta potencia a través de una antena direccional en un sitio a quince millas de distancia, pero la recepción de la señal del sitio no se puede amplificar de manera similar tan fácilmente como se puede potenciar la transmisión.

Proponemos, Rutas asimétricas, para situaciones donde los mensajes pueden ser recibidos por un nodo, pero donde el nodo no puede comunicarse directamente hacia atrás. En una ruta asimétrica, los mensajes de confirmación se retransmiten a través de la red por una ruta, lo que permite la plena utilización de la conectividad asimétrica en canales de comunicación de una vía.

Situaciones donde esto será cada vez más relevante

* Arreglos SONET rurales con Wifi amplificado sobre antenas direccionales
* Conectividad urbana entre antenas altamente direccionales y no direccionales que transmiten a los mismos niveles de potencia
* Penetración de concreto en sistemas 802.11af
* La propagación LiFi sin línea de visión puede transmitir más de 200 Mb / s, pero es muy asimétrica
* Los sistemas Li-Fi de tipo RONJA tienen límites teóricos de capacidad de más de 10 Gb / s de línea de visión y hay una ventaja de costo / configuración para la conectividad asimétrica.

La utilización de conectividad asimétrica y rutas que permiten la transmisión directa de datos de una sola vía entre nodos tiene varios avances, especialmente para desarrollos rurales y la reducción del costo de integración de alta capacidad de las tecnologías de nueva generación.

## Enrutamiento de Fuente: Descubrimiento de Ruta

La puerta de enlace IPv4 y las redes de malla para ISP de la comunidad solo requieren una primera búsqueda de ancho de las rutas para la conectividad de clearnet. Las mejores, más confiables y las rutas de mayor rendimiento tienen una profundidad muy pequeña. Por lo tanto, consideramos el enrutamiento resuelto para este caso. Veremos ruteo general más tarde.
