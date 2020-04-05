## Servidor de Correo con Poste.io

[Poste](https://poste.io/) es un servidor de correo que implementa todas las funcionalidades necesarias para tener un servidor de correos funcional en menos de 5 minutos.

Para ello lo único que necesitamos es descargar el archivo docker-compose.yaml y crear la carpeta data en el lugar donde descargamos el .yml

Una vez que tengamos todo listo podemos levantar el servicio con el siguiente comando

    docker-compose up -d

Esperamos unos instantes y podemos entrar a la consola de administración en https://mail.{TUDOMINIO.COM}/admin.

Al principio veremos una imagen como esta, confirmamos excepcion aceptando el certificado, esto solo pasará una vez.

Allí nos pedirá crear un usuario administrativo, que será nuestro usuario raiz.

Una vez adentro vamos a System Settings -> TLS Certificate -> Generate Certificate

Esperamos unos minutos y nuestro certificado ya será valido!

Una vez hecho todo esto podemos empezar a crear los usuarios y a utilizar el servidor.

Para enviar/ver bandeja de entrada de nuestro correo, está disponible un [rouncube](https://roundcube.net/) en https://mail.{TUDOMINIO.COM}
