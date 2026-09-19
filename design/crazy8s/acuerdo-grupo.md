# Diseño individual y acuerdos de grupo — Crazy 8's

## Diseño individual de cada integrante

- **Josué Morera** (`Integrante-C4H757`): bocetó el set completo: Inicio (con modo Práctica y
  Multijugador separados, y unirse por número de sala), Juego, Créditos, Victoria, FAQs, Eliminado
  y una pantalla de Crear/Unirse a Sala.
- **Jean Aguilar Mena** (`Integrante-`): Inicio, Lobby, Eliminado, Victoria, FAQs, y 3 variantes de
  la pantalla de Juego con vista superior (radar circular, iconos de movimiento, armas).
- **Integrante C39321**: variantes abstractas de Inicio (Jugar/Opciones/Unirse) y de la pantalla de
  fin de partida ("perdiste", puesto, power-ups).
- **Integrante C4I578**: 8 variantes del menú principal (posición de usuario/botones) y 8 variantes
  del Lobby (código de sala, lista de jugadores, stats personales, botón de salir).

## Acuerdos de grupo

Con base en dónde coincidieron 3 o más integrantes, o en qué es necesario para que el flujo
funcione, estos son los acuerdos:

| Elemento | Acuerdo | Por qué |
|---|---|---|
| Inicio | Nombre/usuario + botón de entrar, layout centrado (imagen, título, campo, botón) | Coincide en Josué, Jean y C4I578; ya está construido así |
| Cámara del tablero | Vista superior (top-down) con minimapa | 3 de las 4 pantallas de Juego (Jean x3) y el wireframe ya digitalizado la usan; más simple de sincronizar en red que una vista de cerca |
| HUD: armas/objetos | Fila abajo-izquierda, junto a la vida | Así están en Josué y en 2 de 3 variantes de Jean; ya está construido así, no se mueve |
| Lobby | Código de sala + lista de jugadores + estado (listo/esperando) | Coincide en Jean y C4I578; ya está construido así |
| Flujo de Crear/Unirse a sala | **Se agrega** una pantalla/paso antes del Lobby: anfitrión crea sala, invitado se une con código | Necesario para que el Lobby tenga sentido (alguien tiene que generar el código); lo propuso Josué y nadie lo contradijo |
| Ícono de ayuda ("?") en Inicio | **Se agrega**, es de bajo costo | Josué y C4I578 llegaron a la misma idea por separado |
| Modo Práctica (un jugador) | **No se agrega por ahora** | No es parte de los requisitos mínimos (multijugador real); se deja como posible extra si sobra tiempo |
| Stats personales en Lobby | **No se agrega por ahora** | No afecta la aprobación del juego; se deja como mejora para una entrega posterior |
| Botón "Quit"/Salir en Lobby | Se mantiene el botón "Salir" ya existente, sin agregar uno nuevo | Ya cubre lo que pedía C4I578 |

## Pendiente de decidir 

- **Nombre del juego**: ningún sketch propone uno; todos usan "Nombre del Juego"/"Game Name".