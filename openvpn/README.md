# OpenVPN en docker

OpenVPN es uno de los protocolos mas utilizados en cuanto a VPN. Además de tener una platadorma sólida desde hace varios años, ahora se le suma la facilidad de que podemos tener todo el ecosistema funcionando en menos de 10 minutos gracias a Docker.

## Instalacion Sobre Raspberry PI
Elegimos un directorio donde openvpn almacenará los certificados, y lo seteamos como una variable de sesión en el shell. En este caso elegimos el directorio /mnt/hdd1/git/openvpn/ovpn-data que está montado en un disco duro.
```shell
	OVPN_DATA=/mnt/hdd1/git/openvpn/ovpn-data
```	
Inicializamos los certificados necesarios de OpenVPN
```shell
	docker run -v $OVPN_DATA:/etc/openvpn --rm lunderhage/openvpn-rpi ovpn_genconfig -u udp://{TUDOMINIO.COM}
```
```shell
	docker run -v $OVPN_DATA:/etc/openvpn --rm -it lunderhage/openvpn-rpi ovpn_initpki
```
OBSERVACION: ACA TARDA VARIOS MINUTOS PORQUE ESTA GENERANDO UNA LLAVE SEGURA

Iniciamos el servicio OpenVPN en modo daemon
```shell
	docker run -v $OVPN_DATA:/etc/openvpn -d -p 1194:1194/udp --cap-add=NET_ADMIN lunderhage/openvpn-rpi 
```
Generamos el certificado para nuestro cliente. Le pasamos la opcion "nopass", que indica que el cliente se podrá conectar sin ingresar un password. Si queremos hacerlo mas seguro aún, podemos quitar este parámetro y configurar un password secreto para que el cliente pueda conectarse
```shell
	docker run -v $OVPN_DATA:/etc/openvpn --rm -it lunderhage/openvpn-rpi easyrsa build-client-full CLIENTNAME nopass
```
Finalmente obtenemos la configuracion y certificados del cliente con el siguiente comando
```shell
	docker run -v $OVPN_DATA:/etc/openvpn --rm lunderhage/openvpn-rpi ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn
```
Y con esto ya tenemos nuestro servidor OpenVPN corriendo y esperando conexiones.

Para correr OpenVPN sobre docker en un server "normal" ejecutaríamos los siguientes comandos:
```shell
	OVPN_DATA=/mnt/hdd1/git/openvpn/ovpn-data
```	
```shell
	docker run -v $OVPN_DATA:/etc/openvpn --rm kylemanna/openvpn ovpn_genconfig -u udp://{TUDOMINIO.COM}
```
```shell
	docker run -v $OVPN_DATA:/etc/openvpn --rm -it kylemanna/openvpn ovpn_initpki
```
```shell
	docker run -v $OVPN_DATA:/etc/openvpn -d -p 1194:1194/udp --cap-add=NET_ADMIN kylemanna/openvpn 
```
```shell
	docker run -v $OVPN_DATA:/etc/openvpn --rm -it kylemanna/openvpn easyrsa build-client-full CLIENTNAME nopass
```
```shell
	docker run -v $OVPN_DATA:/etc/openvpn --rm kylemanna/openvpn ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn
```

Referencias:

1.[OpenVPN sobre Docker](https://github.com/kylemanna/docker-openvpn)

2.[OpenVPN sobre Raspi](https://github.com/lunderhage/docker-openvpn-rpi)

3.[DuckDNS](http://www.duckdns.org/)

4.[Imagen docker de DuckDNS](https://hub.docker.com/r/linuxserver/duckdns)
