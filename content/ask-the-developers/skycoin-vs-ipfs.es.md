+++
title = "Skycoin/CXO vs FileCoin/IPFS"
tags = [
    "Ask the Developers",
    "CXO",
    "Skywire",
    "IPFS",
]
bounty = 4
date = "2017-08-09"
categories = [
    "Ask the Developers",
]
+++

Nos han preguntado cuál es la diferencia entre [CXO] (https://github.com/skycoin/cxo) y IPFS.

File Coin tiene los mismos objetivos que una parte de la infraestructura de Skycoin, pero tiene un enfoque diferente.
Ellos tienen tres protocolos separados:

- IPLD
- IPFS
- Multi-formatos

dónde,

- IPLD es un lenguaje de representación para objetos de datos inmutables y hashes.
- IPFS es un sistema de archivos
- Mult-formats es un estándar de datos autodescriptivo

En Skycoin, CXO hace las tres cosas.

- Multi-formatos es solo un "esquema" en CXO
- IPLD es solo CXO (estructuras de datos inmutables, cadenas hash, replicación, árboles merkle, etc.)
- IPFS es solo una aplicación sobre CXO (CXO es un sistema de objetos inmutable y los archivos son solo objetos, por lo que no necesitan un tratamiento especial porque los archivos son solo un tipo de objeto)

FileCoin hace "prueba de almacenamiento". Mientras que Skycoin toma un enfoque técnico diferente, porque creemos que la prueba de almacenamiento es demasiado complicada y no es realmente lo que el usuario quiere y no es realmente el cuello de botella.

IPFS / IPLD / Filecoin está diseñado para integrarse en la infraestructura web existente y javascript. Mientras skycoin está implementando un nuevo eco-sistema de novo ([skywire] (https://github.com/skycoin/skywire), CXO, CX), porque el ecosistema existente carece de propiedades matemáticas requeridas (javascript no es determinista, implementaciones varían entre navegadores, no se pueden usar para incrustar en blockchain o se usan con todo el potencial debido a problemas heredados).

Y, además, porque la privacidad y la seguridad no se pueden pegar con cinta adhesiva en el nivel superior de una pila, sino que requieren el diseño adecuado de cada componente del hardware. Skycoin adopta un enfoque matemáticamente estricto y elegante que permite una implementación más sencilla al tener un estándar y un ecosistema autónomos.

Entonces file-coin se integrará con el internet existente y solo será una biblioteca de JavaScript que se puede importar. Skycoin tendrá una nueva Internet con protocolos y hardware paralelos.

La aspereza y la desigualdad de impedancia siempre se producen en las interfaces del sistema y skycoin tomó el enfoque de minimizar los componentes al mínimo, siendo autónomo y minimizando las dependencias entre los módulos. Una de las razones por las que no hicimos "prueba de almacenamiento" para los incentivos fue que requería que el almacenamiento de archivos fuera una dependencia del blockchain y consideramos que el almacenamiento de archivos ya funciona de manera eficiente, sin agregar la sobrecarga de un blockchain, por lo que este no era el lugar correcto para poner los incentivos del usuario.

No queríamos construir un "nuevo Internet" que simplemente se apaga como una máquina recreativa porque el usuario no puso suficientes monedas. No queríamos que la experiencia del usuario tuviera que pagar por cada característica, clic de botón, descarga de archivo o acción, cuando cada acción no tiene costo.

Así que Maidsafe, Ethereum, FileCoin / IPFS, todos adoptan un enfoque y una filosofía diferentes, pero van en la misma dirección. Hay diferencias sustanciales en el alcance y la implementación técnica

- Ethereum intenta poner todo en el mundo en una sola cadena de bloques (mientras que Skycoin no pone casi nada en la cadena de bloques, excepto pagos de monedas. Skycoin tiene una cadena de bloques individual para su versión de tokens ERC20, en lugar de poner todas las fichas en una sola cadena de bloques monolítica )
- MaidSafe se preocupa por la identidad y tiene la menor cadena de bloques (mientras que en Skycoin hay identidades globales. Las identidades son solo claves públicas y son pseudo anónimas)
- FileCoin tiene que ver con la prueba de almacenamiento y solo el almacenamiento de archivos (Skycoin también incluye la comunicación y el cálculo como elementos primitivos. El mecanismo de almacenamiento de Skycoin es independiente de la cadena de bloques y solo se monetiza indirectamente)
- Golem solo se preocupa por alquilar servidores / GPU por hora para las monedas (que es lo mismo que EC2 que toma Bitcoin y solo será una característica menor de las redes más avanzadas)


Uno de los problemas del Proyecto Skycoin es que la documentación y [documentos técnicos] (https://www.skycoin.net/whitepapers.html) solo cubren el trabajo sobre el consenso (ya tienen dos años) y no muestran la información actual. trabajo de desarrollo y ecosistema. Por lo tanto, el sitio web debe actualizarse y necesitamos nuevos documentos oficiales sobre cada uno de los subproyectos.

El desarrollo está significativamente por delante de la documentación. Por ejemplo, las aplicaciones CXO ([BBS] (https://github.com/skycoin/bbs)) ya están en fase de prueba y uso ligero, mientras que el documento técnico para CXO todavía no se ha escrito.

Así que aún estamos alcanzando el registro de marketing y comunicación.
