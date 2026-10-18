# Andamio de negocio local para Ehundu

Un sitio para un negocio con local abierto, hecho con [Ehundu](https://ehundu.com), el generador de sitios estáticos en PHP. Es un andamio: la estructura de partida de la web de una tienda, un taller, una consulta o un restaurante. Trae los servicios con su precio, los horarios con sus excepciones, la ubicación con el enlace al mapa, el contacto, unas preguntas frecuentes, una página «Sobre nosotros» y los textos legales de modelo. Cambias los textos, los datos y el CSS, y es tuyo.

Puedes verlo funcionando, sin instalar nada, en [andamio-negocio.ehundu.com](https://andamio-negocio.ehundu.com/).

Necesitas PHP 8.4 o superior y [Composer](https://getcomposer.org).

## Empezar

Si lo has creado con `composer create-project`, ya está todo instalado. Si has usado el botón «Use this template» de GitHub, o has descargado el zip, instala el motor:

```
composer install
```

Las tres órdenes que vas a usar:

```
composer servir      # lo previsualiza en http://127.0.0.1:8000/ y rehace solo lo que cambia
composer compilar    # deja el sitio terminado en la carpeta salida/
composer desplegar   # lo compila y lo sube al servidor que digas en sitio.yml
```

## Lo primero que cambiar

1. En `sitio.yml`, el nombre, la dirección del sitio y la descripción.
2. En `datos/negocio.yml`, los datos de tu negocio: nombre, teléfono, dirección, coordenadas, cómo llegar y redes. Salen en la cabecera, el pie, el contacto y la ubicación sin escribirlos dos veces.
3. En `datos/horarios.yml`, tu horario y tus días especiales.
4. En `datos/titular.yml`, quién es el titular legal del sitio.
5. Tus servicios, en `contenido/servicios/`, y los textos de `index.md`, `donde-estamos.md`, `contacto.md`, `sobre-nosotros.md` y `preguntas-frecuentes.md`.
6. En `publico/css/estilos.css`, los colores: están al principio, en variables, con su pareja para el modo oscuro.

## Los servicios

Cada servicio es un fichero de `contenido/servicios/`. Copia uno, ponle un nombre nuevo (será su dirección: `mi-servicio.md` sale en `/servicios/mi-servicio/`) y cambia su front matter, el bloque entre `---` del principio:

```yaml
titulo: Revisión básica
descripcion: Una frase que sale en la tarjeta.
precio: 25 €
duracion: 30 minutos
destacado: sí
orden: 10
incluye:
  - Revisión de frenos y ajuste.
  - Comprobación de cambios y cadena.
```

`destacado: sí` lo lleva a la portada. `orden` decide el lugar, de menor a mayor. Debajo va el texto, en Markdown.

## Los horarios

`datos/horarios.yml` lleva un día por línea, con sus tramos escritos «de-a» con las horas de 24: `"09:30-13:30"`. Un día sin tramos sale como «Cerrado». Debajo, las excepciones: festivos, vacaciones, un cierre por obras. Lo que ve el visitante es `cuando` y `horario`. Si además pones las fechas (`fechas` para días sueltos, `desde` y `hasta` para un periodo, siempre entre comillas y como «AAAA-MM-DD»), también las leen los buscadores.

El sitio se compila de una vez y no sabe qué día es hoy, así que no puede decir «abierto ahora» ni quitar las excepciones que ya pasaron. Quítalas tú cuando pasen.

## Los datos para los buscadores

La portada y la página de ubicación llevan un bloque de datos estructurados (JSON-LD, del tipo `LocalBusiness`) que Google y otros buscadores usan para enseñar tu negocio con su horario, su dirección y su teléfono. Se genera solo desde `datos/negocio.yml` y `datos/horarios.yml`. Cambia `tipoSchema` por un tipo más concreto si lo hay para lo tuyo (`Dentist`, `Restaurant`, `HairSalon`, `AutoRepair`…; la lista está en [schema.org](https://schema.org/LocalBusiness)). Para comprobarlo, la herramienta de resultados enriquecidos de Google lee el sitio ya publicado.

## La ubicación

La página «Dónde estamos» da la dirección, cómo llegar y un enlace a OpenStreetMap hecho con las coordenadas de `datos/negocio.yml`. No lleva un mapa incrustado a propósito: los mapas de Google o de OpenStreetMap cargan servicios de terceros y ponen cookies, y entonces tendrías que pedir consentimiento. Si aun así quieres uno, ponlo en `plantillas/donde.twig`, y cambia la política de cookies y el aviso legal.

## El contacto

El contacto da el teléfono, el WhatsApp (solo si rellenas el número), el correo y las redes. Un sitio estático no procesa formularios, así que si quieres uno necesitas un servicio que reciba el envío y te lo mande por correo; Ehundu no incluye ninguno. Si lo añades, tendrás que contarlo en la política de privacidad.

## Los textos legales

Aviso legal, política de privacidad y política de cookies, en `contenido/legal/`, con los datos del titular de `datos/titular.yml`. **Son textos de modelo, para un negocio en España que no usa cookies ni analítica, y no sustituyen el consejo de un profesional.** Cada uno lleva arriba un recuadro amarillo que lo dice. Antes de publicar:

1. Pon los datos del titular de verdad en `datos/titular.yml`.
2. Léelos enteros y cámbialos según lo que haga tu sitio. Si añades un formulario, reservas, analítica, un mapa o un vídeo incrustados o publicidad, hay cosas que ya no valen.
3. Cambia la fecha de «Última actualización» y borra el recuadro de cada uno.

## Qué hay en cada carpeta

```
sitio.yml                  el nombre, la dirección, la descripción y adónde se publica
contenido/                 las páginas, en Markdown: cada fichero es una página
  index.md                 la portada
  servicios.md             la lista de servicios
  servicios/               un fichero por servicio; su _datos.yml les da lo que comparten
  donde-estamos.md         la dirección, el mapa y el horario
  contacto.md, sobre-nosotros.md, preguntas-frecuentes.md
  legal/                   el aviso legal, la privacidad y las cookies
datos/                     tu negocio, tus horarios, el titular legal y el menú
plantillas/                el HTML de cada tipo de página, con Twig
parciales/                 trozos de HTML que reutilizan las plantillas (los horarios, los datos estructurados…)
  atajos/aviso.twig        el atajo [aviso], que dibuja el recuadro de los textos legales
publico/                   el CSS y el icono: se copia tal cual a la raíz
```

`salida/` (el sitio compilado) y `.ehundu/` (lo que recuerda el motor) los crea Ehundu y no se versionan.

## Publicarlo

Descomenta la sección `despliegue` de `sitio.yml` y rellénala. La contraseña no va ahí, porque `sitio.yml` se comparte: crea un fichero `.secretos.yml` en la raíz del proyecto, entre comillas simples:

```yaml
despliegue:
  clave: 'la-contraseña'
```

Antes de subir nada, mira qué haría:

```
composer desplegar -- --simular
```

Y cuando todo cuadre, `composer desplegar`. La guía [Desplegar el sitio](https://ehundu.com/documentacion/guias/desplegar/) explica los destinos (carpeta, FTP, SFTP y S3) y cómo decide Ehundu qué sube y qué borra, y [Un alojamiento compartido](https://ehundu.com/documentacion/guias/alojamiento-compartido/) cuenta lo que conviene saber si tu hosting es de los baratos.

## Para seguir

- [Tu primer sitio](https://ehundu.com/documentacion/empezar/tu-primer-sitio/), un recorrido paso a paso.
- [La hoja de referencia rápida](https://ehundu.com/documentacion/empezar/hoja-rapida/), con todo lo que vas a necesitar cada día.
- [La documentación completa](https://ehundu.com/documentacion/).

Licencia MIT.
