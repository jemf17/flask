# Enviar gmail atravez de flask #

A menudo se requiere una aplicación basada en web para tener una función de envío de correo a los usuarios / clientes. La extensión Flask-Mail facilita la configuración de una interfaz simple con cualquier servidor de correo electrónico.

Al principio, la extensión __Flask-Mail__ debe instalarse con la ayuda de la utilidad pip.

~~~ 
pip install Flask-Mail 
~~~

Luego, Flask-Mail debe configurarse estableciendo valores de los siguientes parámetros de la aplicación.

- **MAIL_SERVER:** Nombre / dirección IP del servidor de correo electrónico
- **MAIL_PORT:** Número de puerto del servidor utilizado
- **MAIL_USE_TLS:** Habilitar/deshabilitar el cifrado de capa de seguridad de transporte
- **MAIL_USE_SSL:** Habilitar/deshabilitar el cifrado de capa de sockets seguros
- **MAIL_DEBUG** Soporte de depuración. El valor predeterminado es el estado de depuración de la aplicación Flask
- **MAIL_USERNAME:** Nombre de usuario del remitente
- **MAIL_PASSWORD:** contraseña del remitente
- **MAIL_DEFAULT_SENDER:** establece el remitente predeterminado
- **MAIL_MAX_EMAILS:** Establece el máximo de correos que se enviarán
- **MAIL_SUPPRESS_SEND:** Envío suprimido si app.testing establecido en verdadero
- **MAIL_ASCII_ATTACHMENTS:** Si se establece en verdadero, los nombres de archivo adjuntos se convierten a ASCII

El módulo flask-mail contiene definiciones de las siguientes clases importante

### Clase Correo ###

Gestiona los requisitos de mensajería de correo electrónico. El constructor de la clase tiene la siguiente forma:

~~~ 
flask-mail.Mail(app = None) 
~~~

El constructor toma el objeto de aplicación Flask como parámetro.

### Métodos de clase Correo ###

- **send():** Envía el contenido del objeto de clase de mensaje
- **connect():** Abre la conexión con el host de correo
- **send_message():** Envía objeto de mensaje

### Clase Message ###

Encapsula un mensaje de correo electrónico. El constructor de la clase de mensaje tiene varios parámetros:

~~~
flask-mail.Message(subject, recipients, body, html, sender, cc, bcc, 
reply-to, date, charset, extra_headers, mail_options, rcpt_options)
~~~

### Métodos de clase Mesaage ###

**attach():** agrega un archivo adjunto al mensaje. Este método toma los siguientes parámetros:

- **filename** nombre del archivo para adjuntar
- **content_type** tipo de archivo MIME
- **data** datos de archivo sin procesar
- **disposition** contenido, si la hubiera.

**add_recipient()** agrega otro destinatario al mensaje 

En el siguiente ejemplo, el servidor SMTP del servicio de gmail de Google se utiliza como MAIL_SERVER para la configuración de Flask-Mail.

**Paso 1**  importe la clase de correo y mensaje del módulo flask-mail en el código.

~~~ 
from flask_mail import Mail, Message 
~~~

**Paso 2** luego, Flask-Mail se configura según las siguientes configuraciones.

 ~~~
 app.config['MAIL_SERVER']="smtp.gmail.com"
 app.config['MAIL_PORT'] = 465
 app.config['MAIL_USERNAME'] = "tu@gmail.com"
 app.config['MAIL_PASSWORD'] = "*****"
 app.config['MAIL_USE_TLS'] = False
 app.config['MAIL_USE_SSL'] = True
 ~~~

**Paso 3** crea una instancia de la clase Mail.

~~~
 mail = Mail(app) 
~~~ 

**Paso 4** configure un objeto de mensaje en una función de Python asignada por la regla de URL('/').

~~~
 @app.route("/")
 def index():
    msg = Message('hola', sender = 'tu@gmail.com', recipients = ['victima@gmail.com'])
    msg.body = "hola, como andas?"
    mail.send(msg)
    return "enviar"
~~~

**Paso 5** el código completo se proporciona a continuación. Ejecute el siguiente script en Python Shell y visite **http://localhost:5000/**.

~~~
from flask import Flask
from flask_mail import Mail, Message

app =Flask(__name__)
mail=Mail(app)

app.config['MAIL_SERVER']='smtp.gmail.com'
app.config['MAIL_PORT'] = 465
app.config['MAIL_USERNAME'] = 'yourId@gmail.com'
app.config['MAIL_PASSWORD'] = '*****'
app.config['MAIL_USE_TLS'] = False
app.config['MAIL_USE_SSL'] = True
mail = Mail(app)

@app.route("/")
def index():
   msg = Message('Hello', sender = 'yourId@gmail.com', recipients = ['id1@gmail.com'])
   msg.body = "Hello Flask message sent from Flask-Mail"
   mail.send(msg)
   return "Sent"

if __name__ == '__main__':
   app.run(debug = True)
~~~

Tenga en cuenta que las funciones de inseguridad integradas en el servicio de Gmail pueden bloquear este intento de inicio de sesión. Puede que tenga que reducir el nivel de seguridad. Inicie sesión en su cuenta de Gmail y [cambie la configuracion](https://myaccount.google.com/lesssecureapps)
