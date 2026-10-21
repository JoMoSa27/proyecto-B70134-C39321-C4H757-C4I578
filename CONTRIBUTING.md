# Convenciones - Entrega 2

Esto es para que no quede cada pantalla con un estilo distinto. Entrega 3 es solo HTML semantico.

## Quien hace cada pantalla

- Inicio -> index.html (Ale)
- Lobby -> lobby.html (adrian)
- Juego/Tablero -> juego.html (Josue)
- Jugador Eliminado -> eliminado.html (Josue)
- Resultados/Victoria -> resultados.html (Jordan)
- FAQs -> faqs.html (jean)
- About/Creditos -> about.html (Josue)

Las paginas que no son mias ya tienen el head, el header y el footer armados, solo falta meterle
contenido al main de cada una.

## Header y footer

Como es HTML puro no hay como hacer un include, asi que el header y el footer van pegados igual
en cada pagina que los lleve. Copien este bloque tal cual para que no se desalinee el menu entre
pantallas:

```html
<header>
<a href="index.html"><img src="favicon.svg" alt="Logo de WAR BATTLE.IO" width="40" height="40" /></a>
<p>WAR BATTLE.IO</p>
<nav>
<ul>
<li><a href="index.html">Inicio</a></li>
<li><a href="faqs.html">Reglas</a></li>
<li><a href="about.html">About</a></li>
</ul>
</nav>
</header>
```

```html
<footer>
<p>&copy; 2026 Aura Farmers</p>
<p><a href="https://github.com/JoMoSa27/proyecto-B70134-C39321-C4H757-C4I578">GitHub</a> · <a href="about.html">Creditos</a> · UCR</p>
</footer>
```

Juego y Eliminado no llevan header ni footer, porque son pantallas inmersivas.

## Cosas que pide la rubrica

- doctype html5 + `<html lang="es" xmlns="http://www.w3.org/1999/xhtml">` 
- todo en minuscula, atributos con comillas
- cerrar TODAS las etiquetas, hasta los li y los p
- los elementos vacios (meta, link, img) van autocerrados con />
- favicon en todas las paginas (ya esta enlazado en los stubs, no lo borren)
- un solo h1 por pagina, el contenido principal.
- nada de div para todo, usar header/nav/main/section/article/aside/footer.

## Validar

Cada pagina hay que pasarla por validator.w3.org y guardar la captura de que no tira errores,
hay que adjuntarla en la entrega.

## Git

Vamos a trabajar en una rama compartida para esta entrega, cada quien toca solo su archivo. Al final se abre un PR de esa rama hacia main y se deja abierto para que lo revisen.

## Pendientes


- El nombre del juego sigue sin decidirse (ver crazy8s/acuerdo-grupo.md), por eso dice
  "Nombre del Juego" en todas las paginas. Cuando se decida hay que buscar y reemplazar.
- En about.html los github de cada quien estan como @pendiente, que cada uno ponga el suyo.
- Falta decidir si resultados.html lleva header/footer o no. Yo lo deje con header/footer.