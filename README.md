# Spammers Chilenos
A nadie le gusta recibir basura en su correo. Esta 'empresa' tiene 0 ética respecto al correo basura.
Si usted es parte de esta empresa por favor infórmese y gánese la vida de manera honesta.


¿Cuál es la Ley encargada de sancionar el uso de Spam?

En Chile la Ley Nº 19.496 sobre Protección a los derechos del consumidor es la que sanciona esta práctica comercial, ya que se considera que vulnera el derecho a la libre elección del consumidor, en especial, su derecho a la privacidad. Por lo que los usuarios tienen todo el derecho a exigir que se les remueva de las líneas de destinatarios.


## Este repo enumera los servidores, sus IP y los dominios usados para spammear pero también, a continuación muestra una forma rápida de extraer los campos 'From' desde las carpetas de Spam/Junk en nuestra cuenta de correo.


### Como extraer direcciones desde los correos con exim y agregarlos a una lista negra con Spam Assasin

Agrega en tu archivo de configuración (comunmente en /etc/spamassasin/local.cf)
```
##Blacklisting SPAM
include /etc/spamassassin/blacklist.cf
```
Luego Crea el archivo
```
touch /etc/spamassassin/blacklist.cf
```

El paso siguiente es extraer las direcciones desde los encabezados de los correos, estas direcciones irán en la lista de spam
```
grep -rhw '/ruta/usuario/mail/dominio.tld/cuenta@dominio.cl/.Spam/' -e 'From:' | grep -oP '(?<= <)[^>]+' > blacklist.cf
```
> Es posible que la ubicación de tus correos difiera con el ejemplo, verifica
>> Verifica que los correos en blacklist son efectivamente spam

Bien, el paso final es agregar 'blacklist_from' a nuestro archivo 'blacklist.cf', para hacerlo en un solo disparo podemos agregar:
```
sed -e 's/^/blacklist_from /' -i blacklist.cf
```

Finalmente reiniciamos Spam Assasin y listo.
```
systemctl restart spamassassin
```


