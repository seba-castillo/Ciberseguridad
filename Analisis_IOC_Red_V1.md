# Análisis de compromiso en la red - Network IOCs
## IOC - Indicator of Compromise
Es una señal que nos indica que la red ha sido atacada o se encuentra bajo ataque. 
### Ejemplos:

- Escaneo de puertos
- Uso de puertos no estándar. Por ejemplo observar que SSL se está utilizando
en un puerto para el tráfico web diferente al 443.
- Canal encubierto. Ver que se estan mandando datos a través de un paquete ping.
Estos paquetes no llevan acarrean datos.
- Ver que en nuestra red hay dispositivos que no reconocemos.
## Picos de tráfico

Cualquier aumento brusco de solicitud de conexiones en comparación
con una base dada. No necesariamente es algo malo, va a depender de qué es lo
que está causando ese pico de tráfico.

### DDoS - Distributed Denial of Service
Ataque de denegación de servicio distribuido ocurre cuando un montón de diferentes hots comprometidos tratan
de conectarse a nuestro servidor a la vez ya sea con tráfico de solicitud o de respuesta, para gastar nuestros recursos.
Es necesario crear estrategias ya que el tráfico puede parecer legítimo visto por parte de nuestro servidor
y por lo tanto tratará de responder. Otro problema que tenemos hoy en día es que cualquiera puede
hacerlos, la gente vende botnets.

**Un aumento de tráfico inesperado no necesariamente significa un ataque DDoS, pero sí es un indicador.**

Debemos sumar otros factores que pueden aumentar la conclusión de que es un ataque DDoS:

- Número excesivo de conexiones TIME_WAIT en nuestro balanceador de carga o tabla de estado de nuestra web.
- Alto número de errores HTTP 503, el cual corresponde a logs de Servicio no disponible.

**¿Cómo sabemos que no somos parte del problema y nosotros somos los que estamos infectados y atacando?**

Una gran cantidad de tráfico saliente de nuestra red podría indicar que esto efectivamente es así.

**¿Cómo se mide un ataque DDoS?**

La forma más comun es medir cuánto ancho de banda se está consumiendo. Este consumo puede medirse como el valor
de bytes enviados o recibidos, o como el porcentaje de utilización de nuestro enlace.

### DRDoS - Distributed Reflection Denial of Service
Denegación de servicio por reflexión es un ataque en que el atacante aumenta drásticamente el ancho de banda enviado
a la víctima durante el ataque mediante la aplicación de un factor de amplificación.
Esto puede ocurrir cuando el atacante falsifica la dirección IP de la víctima e intenta abrir conexiones con varios servidores.

- Por ejemplo con TCP, el cliente mandaría un SYN luego recibe un SYN-ACK pero luego no mandaría el ACK el cliente. Esto podría ser como cuando saludas, te saludan y después ignoras y te quedan esperando.
- Por ICMP, uno de los más comunes, se inundaba un servidor con peticiones ICMP, que son peticiones ping. El atacante enviaría una solicitud de difusión de ping de una subred. De esta manera enviaría un ping y recibiría 10,50,100 pings. 
- Por DNS, los atacantes pueden enviar una consulta DNS falsa, la cual la petición es una pequeña cantidad de datos pero la respuesta es una cantidad de datos grande
- Por NTP (Network Time Protocol). Es muy efectivo ya que una sola petición NTP generará una respuesta del servidor para las últimas 600 máquinas con las que ese servidor ha contactado. Así que si hago una petición NTP al servidor, me devolverá 600 veces lo que he pedido.

**Todo esto son indicadores de compromiso pero no necesariamente significan un ataque DDoS pero es un buen indicio de ello.**

### Slashdot Effect
Efecto que causa que un sitio web colapse. Producido cuando un pequeño sitio web se populariza rápidamente debido a la exposición en sitios de sociales para compartir.

## ¿Cómo mitigar un ataque DDoS?

1. Realizar análisis de registros en tiempo real para identificar patrones de tráfico sospechoso y redireccionarlo a un black hole o sink hole. De esta forma evitamos procesar toda esa carga y la redirigimos a un lugar basura.
2. Usar datos de geolocalización y reputación de IP para redirigir o ignorar el tráfico sospechoso.
3. Cerrar agresivamente las conexiones más lentas reduciendo los tiempos de espera en los servidores afectados.
4. Utilizar el almacenamiento en caché y la infraestructura de backend para descargar el procesamiento a otros servidores. Por ejemplo proxys, CDN para redistribuir la cargar a otros servidores y no se lleve todo uno solo.
5. Utilizar servicios de protección DDoS para empresas como CloudFlare o Akamai. Ellos se ponen en frente, antes de llegar a mi pasan por Cloudflare y este comprueba si eres parte de un ataque. Recomendado para pequeños sitios o grandes empresas. La primera probablemente no tiene lo suficiente para hacer una protección propia y la segunda puede costear el servicio sin hacerse problema.

## NUESTRO OBJETIVO COMO EMPRESA Y DEFENSOR DE LA RED

**Sobrevivir al ataque DDoS.**

