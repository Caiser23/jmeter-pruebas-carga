ruebas de Carga con JMeter - Login Herokuapp
Este repositorio contiene un script de JMeter para realizar pruebas de carga sobre la página de demostración https://the-internet.herokuapp.com/login.
El flujo simulado es:

Acceder a la página de login.

Enviar credenciales válidas (usuario: tomsmith, contraseña: SuperSecretPassword!).

Verificar que el login es exitoso (aparece el mensaje "You logged into a secure area!").

Cerrar sesión y verificar el mensaje de salida.

📁 Estructura del repositorio
text
.
├── PruebaLoginHerokuapp.jmx      # Plan de pruebas de JMeter
├── usuarios.csv                   # Archivo con credenciales (usuario,contraseña)
└── README.md                       # Este archivo
⚙️ Requisitos previos
Java 8+ (JMeter es una aplicación Java).

JMeter 5.4 o superior (descargar desde https://jmeter.apache.org/).

Conexión a internet para acceder a la página de prueba.

🔧 Configuración del archivo CSV
El archivo usuarios.csv debe contener las credenciales en el siguiente formato (sin cabecera si no se desea, pero en el script se esperan las variables username y password):

text
tomsmith,SuperSecretPassword!
tomsmith,SuperSecretPassword!
tomsmith,SuperSecretPassword!
Nota: Para esta página de demostración, solo las credenciales tomsmith / SuperSecretPassword! son válidas. Si quieres probar con múltiples usuarios, puedes repetir la misma línea o usar otras, pero ten en cuenta que el login fallará.

🚀 Cómo ejecutar la prueba
Desde la interfaz gráfica de JMeter
Abre JMeter.

Ve a Archivo → Abrir y selecciona PruebaLoginHerokuapp.jmx.

En el Árbol de resultados (listener) podrás ver el detalle de cada petición.

Ajusta el número de hilos y el ramp-up si lo deseas (en el Grupo de Hilos).

Haz clic en el botón Iniciar (play verde).

Desde línea de comandos (modo no gráfico)
Para ejecutar la prueba y generar un informe HTML:

bash
jmeter -n -t PruebaLoginHerokuapp.jmx -l resultados.jtl -e -o ./informe_html
-n: modo no gráfico.

-t: ruta al script .jmx.

-l: archivo de resultados en bruto (formato JTL).

-e: generar informe HTML al finalizar.

-o: carpeta de salida para el informe (debe estar vacía).

📊 Interpretación de resultados
En el Árbol de resultados (GUI) o en el informe HTML generado, verifica:

Página de Login: código de respuesta 200 (éxito).

Enviar Login: después de la redirección, debe mostrarse la página /secure con código 200 y el texto "You logged into a secure area!".
Nota: En el script se ha separado la verificación en una petición explícita a /secure para mayor claridad (ver más abajo).

Logout: código 200 y mensaje "You logged out of the secure area!".

🔍 Detalle sobre la verificación del login
El script original presentaba un problema: la aserción sobre la petición POST de login fallaba porque el mensaje de éxito no está en la respuesta de redirección (código 303), sino en la página /secure final.
Por eso, el script actual incluye una petición GET explícita a /secure justo después del POST, y es allí donde se comprueba el mensaje de bienvenida. Esto garantiza que la verificación sea correcta.

📦 Subir cambios a GitHub (para colaboradores)
Si has realizado modificaciones y quieres actualizar el repositorio:

bash
# Ver el estado de los archivos modificados
git status

# Añadir los cambios (puedes usar . para añadir todo)
git add .

# Crear un commit con un mensaje descriptivo
git commit -m "Descripción de los cambios realizados"

# Subir los cambios a GitHub
git push
Si es la primera vez que subes una rama, usa:

bash
git push -u origin main
❓ Solución de problemas comunes
Problema	Posible causa	Solución
La aserción de login falla	La aserción está en el POST en lugar de en /secure	Asegúrate de tener la petición GET a /secure con la aserción correspondiente (ya incluida en el script).
Error de conexión	JMeter no puede alcanzar el servidor	Verifica tu conexión a internet. La página the-internet.herokuapp.com debe ser accesible.
El archivo CSV no se encuentra	Ruta incorrecta en la configuración CSV	Usa la ruta absoluta en el elemento Configuración de datos CSV o coloca el CSV en la misma carpeta que el script.
Código de respuesta 404	La ruta de la petición es incorrecta	Revisa que las rutas sean /login, /authenticate, /secure, /logout.
📚 Recursos útiles
Documentación de JMeter

Página de prueba the-internet

Guía de GitHub para principiantes

Autor: Douglas Cárdenas
Licencia: MIT (puedes usar y modificar libremente)
