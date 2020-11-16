---
title: "Introducción, DNS y DHCP Servicios en Red"
date: "2020-11-16"
---
[tcp]: https://github.com/usarralsmr/usarralsmr.github.io/blob/master/assets/img/2020-11-16-introduccion-dns-dhcp/tcp.png?raw=true
[peer]: https://github.com/usarralsmr/usarralsmr.github.io/blob/master/assets/img/2020-11-16-introduccion-dns-dhcp/peer.png?raw=true
[clienteservidor]: https://github.com/usarralsmr/usarralsmr.github.io/blob/master/assets/img/2020-11-16-introduccion-dns-dhcp/clienteservidor.png?raw=true
[jerarquiadns]: https://github.com/usarralsmr/usarralsmr.github.io/blob/master/assets/img/2020-11-16-introduccion-dns-dhcp/Jerarquia%20DNS.png?raw=true
[zonasdns]: https://github.com/usarralsmr/usarralsmr.github.io/blob/master/assets/img/2020-11-16-introduccion-dns-dhcp/Zonas%20DNS.png?raw=true
[directainversa]: https://github.com/usarralsmr/usarralsmr.github.io/blob/master/assets/img/2020-11-16-introduccion-dns-dhcp/Resoluci%C3%B3n%20directa%20e%20inversa.jpg?raw=true

# Introduccion

*Modelo de red: Acuerdo de uso y definicion de las capas de red. El modelo teorico de referencia es el modelo **OSI*


*Protocolo de red*: Un protocolo define una serie de reglas algoritmos, mensajes,... para que el software y hardware se comuniquen de forma efectiva


## Arquitectura TCP/IP
*Arquitectura:* Conjunto de reglas que definen la funcion de los programas y del hardware de red.
Cuando aparecen las primeras redes creadas por la Agencia Militar de Defensa norteamericana no existia el modelo OSI y por tanto crearon el modelo TCP/IP
![Modelo OSI][tcp]

## Estructuras de red
Dos tipos de estructuras principales:

### Estructura peer-to-peer:
Cada equipo ejerce las mismas funciones. Diseno solo util en redes pequenas
![Estructura Peer to peer][peer]
###Estructura cliente-servidor:
Unos equipos se denominan servidores, qu epermiten interactuar simultaneamente con muchas maquinas clientes de forma eficiente
![Estructura Cliente-servidor][clienteservidor]
## Estructura de las direcciones IP

*Identificadores de red*: Empezando por la izquierda la primera parte se utiliza para identificar la red o la interfaz

*Identificador de dispositivo*: Se utiliza para identificar el equipo o la interfaz en la red

### Clases de direcciones IP
| Clase | ID red | Id equipos | Rango                     | Redes | Equipos |
| ------|:------:|:----------:|:---------------------------:|:-----:|--------:|
|   A   | 8 bits | 24 bits    |  1.0.0.0 - 126.255.255.255     | 128| 16777214|
|   B   | 16 bits |16 bits    |  128.0.0.0 - 191.255.255.255 | 16384| 65534|
|   C   | 24 bits | 8 bits    |  192.0.0.0 - 223.255.255.255 | 2097152| 254|

### Mascaras de subred
Tiene el mismo formato que las IPs pero sirve para diferenciar la parte de red de la parte de host

|Clase A   |   Clase B   | Clase C  |
|----------|:-----------:|---------:|
| 255.0.0.0 |  255.255.0.0 | 255.255.255.0 |

### Puertos
Para saber a que proceso pertenece cada envio es necesario una direccion adicional dentro de la ip,en TCP/IP esta direccion se llama puerto

Rango teorico de los puertos: 0 - 65535

Puertos reservados: 0 - 1023

# Tema 2
**DNS**: Domain Name service, sistema que hace legible las direcciones IP.
## Ejemplo de wikipedia, nodos
| www      | .wikipedia   | .org         | .          |
|----------|--------------|--------------|------------|
| Servicio | Nodo nivel 2 | Nodo nivel 1 | Nodo raíz  |
![Esquema Jerarquia DNS][jerarquiadns]

## Zonas DNS
La parte de la base de datos de un DNS se le llama **zona**, esta puede ser gestionada por mas de un servidor. Estos tienen bases de datos con info completa de las zonas, se les conoce como servidores autoritativos

## Funcionamiento DNS
### Clasificación de servidores de nombres

*   Servidores autoritativos: Son los que almacenan la información completa de la zona. Debe haber al menos 1 por zona. En general suele haber al menos 2 para evitar fallos y caidas.
    * Servidor primario o maestro: Es el que mantiene los datos, nombres DNS, etc... originales de la zona completa
    * Servidor secundario o esclavo: Este servidor se encarga de replicar los datos para prevención de fallas. Al problema de replicación se le llama transferencia de zonas 
* Servidores no autoritativos: Son aquellos que no almacenan datos de una zona completa. 2 tipos:
    * Renviador: Cuando existen varios servidores, se pueden configurar para que este realize las peticiones al renviador y este los transmita hacia los DNS de Internet. Todo esto para la reducción de trafico en red y para el filtrado a traves de un firewall.
    * Cache: Este almacena por un tiempo los resultados de las consultrasa a otros servidores, para que si se requiere este pueda devolver desde la cache la petición sin tener que hacer una busqueda completa.

### TIpos de consultas

* Consultas recursivas: Si se realiza una peticion recursiva, este debe responder con la información que tiene almacenada en la base de datos local. Si no la tiene, debera encontrarlo buscando en otros servidores. El cliente solo hace la consulta a un server y este se encarga de buscar en los demas
* Consultas iterativas: En este caso es el cliente el que hace las consultas en los diferentes servidores DNS.
## Clientes DNS
### Resolución
#### Resolución directa
Se busca la ip a traves del nombre asociado. Es la mas utilizada
#### Resolución inversa
Se basa en el procedimiento contrario, en resolver el nombre de la ip
![Resolución DNS][directainversa]
### Tipos de registros
| A     | Resuelve el host en una IP                 |
|-------|--------------------------------------------|
| PTR   | Resuelve una IP en un Host                 |
| SOA   | Primer registro de cualquier zona          |
| SRV   | Resuelve el nombre de los servidores       |
| NS    | Identifica el servidor DNS                 |
| MX    | Identifica el servidor de correo           |
| Cname | Resuelve un nombre de host en otro (alias) |

# Tema 3
**DHCP**: Dynamic Host Configuration Protocol, servicio que configura las direcciones IP de los clientes con unas reglas, definidas por el administrador de sistemas.
**Ejemplo**:
Cuando tu conectas el portatil a la red de clase, el servidor DHCP le da automaticamente una dirección IP, evitando que el usuario tenga que entrar en la configuración de red.

## Funcionamiento DNS
### Modos de asignacion:
Un servidor DHCP puede asignar las direcciones de tres maneras diferentes:
*   Asignación automática e ilimitada. El servidor ofrece una dirección IP de manera permanente.
*   Asignación dinámica y limitada. El servidor ofrece una dirección IP con un tiempo limitado.
*   Asignación estatica con reserva. La asignacion de IP realiza el administrador de la red.

### ¿Qué información da el servidor?
El servidor DHCP da la siguiente información:
* Dirección IP del cliente.
* Máscara de subred.
* Puerta de enlace.
* Direcciones de los servidores DNS.
* Nombre o sufijo del dominio DNS.

### Estructura del protocolo.

Todos lo protocolos usan unos paquetes para mandarse la infomación, DHCP usa su propios.
1. El clinte manda un paquete DHCP Discover por difusion, por si hay un servidor DHCP a la escucha.
2. El servidor a recibido este paquete, por lo que responde con un DHCP Offer, ofreciendose a dar una configuración valida.
3. El cliente manda una petición por difusión a la primera oferta aceptada, esto lo hace con DHCP Request.
4. El servidor aceptado manda un DHCP ACK, donde estan todos lo detalles de la concesión. El cliente comprueba si esta configuración esta correcta y si no lo está este manda un paquete DHCP decline y reiniciar el proceso de nuevo cuando reciva un DHCP NACK.


## Vocabulario.
Rango de direcciones: Son las direcciones IP validas que un servidor DHCP puede ofrecer a equipos.
Reserva: Las reservas son aquellas direcciones IP que solo las puede usar un equipo con cierta dirrección MAC.
Concesiones: Una concesión es la asignación de una IP, durante un intervalo de tiempo.
Exclusiones: La exclusion es aquella direccion o rango de direcciones que no se puede conceder.

