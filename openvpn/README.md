1. elegir un nombre para el directorio de openvpn --> OVPN_DATA="ovpn-data"
	OVPN_DATA=/mnt/hdd1/git/openvpn/ovpn-data
2. inicializar los certificados
	docker run -v $OVPN_DATA:/etc/openvpn --rm lunderhage/openvpn-rpi ovpn_genconfig -u udp://arielordf.duckdns.org
	docker run -v $OVPN_DATA:/etc/openvpn --rm -it lunderhage/openvpn-rpi ovpn_initpki

	--ACA TARDA 10 MINUTOS APROX

3. iniciar openvpn
	docker run -v $OVPN_DATA:/etc/openvpn -d -p 1194:1194/udp --cap-add=NET_ADMIN lunderhage/openvpn-rpi 
4. generar el certificado cliente
	docker run -v $OVPN_DATA:/etc/openvpn --rm -it lunderhage/openvpn-rpi easyrsa build-client-full CLIENTNAME nopass
5. obtenemos la configuracion y certificados del cliente
	docker run -v $OVPN_DATA:/etc/openvpn --rm lunderhage/openvpn-rpi ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn
