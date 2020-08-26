# Idioms/Idiomas

* [English]
* [Español]

# Configuration Tor-SASL + irssi + Freenode
Configuration for irssi with tor + sasl, freenode.

# Motivation

For my friends and some generic pattern matching creature, please!, no more questions about this configuration. RTFM! xD

# Configuration

## Installation (Debian GNU/Linux)

```bash
# apt update
# apt install irssi tor
```

## Freenode user registration

```bash
irssi
```

### Inside Shell of irssi.

* Adding Freenode with tls 6697, never 6667

```bash
/server add -tls -tls_verify -network freenode -port 6697 chat.freenode.net
/save
/connect freenode
```

* Nick registration

```bash
/nick your_nick_here
/msg NickServ REGISTER YourPassword youremail@example.com
/quit
```

Check your email.


## Build your fingerprint

In the OS shell (Bash/Ksh)

```bash
1> openssl req -newkey rsa:2048 -days 730 -x509 -keyout your_nick.key -out your_nick.cert -nodes 
2> cat your_nick.cert your_nick.key > your_nick.pem
3> openssl x509 -sha1 -noout -fingerprint -in your_nick.pem | sed -e 's/^.*=//;s/://g;y/ABCDEF/abcdef/'
4> mv your_nick.pem ~/.irssi/config
```

*1>* **Replace your_nick.key** and **your_nick.cert**, for your nick, example: **innaky.key**, **innaky.cert**.

*2>* Follow the same logic for **your_nick.pem**.

*3>* With This line you get **the fingerprint** for add in your freenode account (the output is similar to: c2ba4db1ccc82cd3f8ec50ebcc3461628f95ab27).

*4>* **copy or move the file .pem** for the irssi config file in your **home directory**.

## Adding your fingerprint certificate in freenode

* Start irssi

```bash
irssi
```

### Inside irssi

```bash
/nick YourNick
/msg NickServ IDENTIFY YourNick YourPassword
/msg NICKSERV CERT ADD c2ba4db1ccc82cd3f8ec50ebcc3461628f95ab27
/quit
```

## Tor configuration

* Call your favorite editor and add in the last line:

```bash
sudo emacs -nw /etc/tor/torrc
mapaddress 10.40.40.40 freenodeok2gncmy.onion
```

## irssi + tor + sasl

* Start irssi

* Inside irssi

```bash
/network add -sasl_username <YourFreenodePassword> -sasl_password ~/.irssi/your_nick.pem -sasl_mechanism EXTERNAL freenodetor
/server add -auto -net freenodetor -ssl -ssl_cert ~/.irssi/your_nick.pem freenodeok2gncmy.onion 6697
/save
```

## Tunelling irssi with tor

```bash
torify irssi -n YourFreenodeUser
```

Example:

```bash
torify irssi -n innaky
```

## Checking tor connection

1. Cloak

Inside of **irssi** console use this command **/whois** and check your user.

The freenode server added in your *cloak* of host, this:

```
~your_user@gateway/tor-sasl/your_user
```

Example:

```
~innaky@gateway/tor-sasl/innaky
```

2. Check certificate fingerprint

If you use the command **/whois** you can see something like this, 

```
has client certificate fingerprint c2ba4db1ccc82cd3f8ec50ebcc3461628f95ab27
```

3. User mode

You can see in your irssi window (bottom left) this (but with your nick):

**innaky(+Zi)**

## Congratulations

If you have the all checks, you are navigating in anonymous form! :)

## Notes

* The freenode user is the same of the *.pem* file.
* *emacs -nw* Emacs is my favorite editor, but you can use *nano*, *vi*, *vim*

* **Customizing another IPv4** 

The 10.40.40.40 is arbitrary you can change it for anyone other.

Example:

You want this IPv4 **10.17.23.41**, ok

You need follow this steps:

1. Edit torrc file

```bash
sudo emacs -nw /etc/tor/torrc
```

Add in the last line:

```bash
mapaddress 10.17.23.41 freenodeok2gncmy.onion
```

2. Restart torrc

```bash
sudo systemctl restart tor
```

3. Irssi file configuration

```bash
emacs -nw ~/.irssi/config
```

Search the **chatnet = "freenodetor";** and change the **address = "10.40.40.40"** by **address = "10.17.23.41"**

The output is similar this:

```bash
  {
    address = "10.17.23.41";
    chatnet = "freenodetor";
    port = "6697";
    use_tls = "yes";
    tls_cert = "~/.irssi/innaky.pem";
    tls_verify = "no";
    autoconnect = "yes";
  },
```
# Configuración Tor-SASL + irssi + Freenode
Configuración con TOR + SASL + irssi para Freenode

# Motivación

Para mis amistades y todos aquellos seres que hagan *pattern matching* con criatura, por favor!, no más preguntas sobre esta configuración *RTFM!* xD

# Configuración

## Instalación (Debian GNU/Linux)

```bash
# apt update
# apt install irssi tor
```

## Registro de nick en freenode

* Ejecuta *irssi*.

```bash
irssi
```

* Dentro de *irssi* escribe

```bash
/server add -tls -tls_verify -network freenode -port 6697 chat.freenode.net
/save
/connect freenode
```

De esta forma le decimos al irssi que cuando pidamos conectarnos a freenode, siempre sea a través del puerto 6697, 
aquí su [RFC](https://www.rfc-editor.org/info/rfc7194)

* Registro

Una vez conectada al Freenode, en la línea de abajo agrega:

```bash
/nick coloca_tu_nick_aquí
/msg NickServ REGISTER TuContraseñaAquí tucorreo@innaky.org
/quit
```

y revisa tu correo electrónico.

# Construye tu *fingerprint* (Huella)

En la consola/shell (Bash, Ksh o Zsh)

```bash
1> openssl req -newkey rsa:2048 -days 730 -x509 -keyout tu_nick.key -out tu_nick.cert -nodes 
2> cat tu_nick.cert tu_nick.key > tu_nick.pem
3> openssl x509 -sha1 -noout -fingerprint -in tu_nick.pem | sed -e 's/^.*=//;s/://g;y/ABCDEF/abcdef/'
4> mv tu_nick.pem ~/.irssi/config
```

Notas:

* 1) Reemplaza *tu_nick* por el tuyo, obvio no? xD
* 2) Igualmente con *tu_nick*
* 3) Esta línea generará tu huella, será algo similar a esto: c2ba4db1ccc82cd3f8ec50ebcc3461628f95ab27 (eso lo agregaremos a tu cuenta freenode)
* 4) el archivo .pem generado, copialo al directorio de irssi.

## Agregando tu huella al freenode

* Llamamos al *irssi*

```bash
irssi
```

* Dentro de irssi, agrega lo siguiente:

```bash
/nick Tu_nick
/msg NickServ IDENTIFY Tu_Nick YourPassword
/msg NICKSERV CERT ADD c2ba4db1ccc82cd3f8ec50ebcc3461628f95ab27
/quit
```
