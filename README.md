# nextrace-data

Calendarios que consume la app NextRace (app y widgets). Todo lo que se sube
aquí a `main` lo recogen los iPhone en ≤ 20 min, sin publicar versión nueva.

## Ficheros de temporada

Un fichero por serie y temporada: `<prefijo>_<año>.json`.

| Serie      | Prefijo    |
|------------|------------|
| Formula 1  | `f1`       |
| MotoGP     | `motogp`   |
| NASCAR Cup | `nascar`   |
| WEC        | `wec`      |
| IndyCar    | `indycar`  |
| IMSA       | `imsa`     |
| Formula E  | `formulae` |

Cada fichero es un array de carreras:

```json
{
  "temporada": 2027,
  "nombre_evento": "F1: GP Australia",
  "hora_carrera_utc": "2027-03-14T04:00:00Z",
  "circuito_nombre": "Albert Park",
  "circuito_ubicacion": "Melbourne, Australia",
  "circuito_tipo": "Circuito Urbano",
  "numero_evento": 1
}
```

- `hora_carrera_utc` en ISO 8601 **UTC** (con `Z`).
- `nombre_evento` **debe** empezar por el prefijo corto de la serie
  (`F1:`, `MGP:`, `NCS:`, `WEC:`, `INDY:`, `IMSA:`, `FE:`): la app deduce la
  categoría, el color y el logo de ahí.
- `temporada` manda sobre el nombre del fichero. Calendarios enseña la
  temporada de la próxima carrera, así que una carrera mal etiquetada (p. ej.
  la apertura de Fórmula E en diciembre con el año anterior) desaparecería de
  la vista de su temporada.
- `numero_evento` es opcional.

### Grandes citas

Las carreras más importantes del año llevan además su rótulo, en español y en
inglés. La app las destaca en oro, con un sello, en las tarjetas y en los
widgets:

```json
  "especial": "Triple Corona",
  "especial_en": "Triple Crown"
```

| Rótulo | Inglés | Carreras |
|---|---|---|
| `Triple Corona` | `Triple Crown` | F1: GP Mónaco · INDY: Indianapolis 500 · WEC: 24 Horas de Le Mans |
| `Joya de la corona` | `Crown Jewel` | NCS: Daytona 500 · Coca-Cola 600 · Southern 500 · Brickyard 400 |
| `Clásico de resistencia` | `Endurance Classic` | IMSA: Rolex 24 at Daytona · 12 Horas de Sebring |
| `La Catedral` | `The Cathedral` | MGP: GP Países Bajos (Assen) |

- Solo en la carrera, nunca en el sprint del mismo fin de semana.
- Pocas: si todo es gran cita, nada lo es. Fórmula E no tiene ninguna.
- Los dos campos son opcionales; las versiones de la app anteriores a las
  grandes citas los ignoran.

### Estilo de los textos (en español)

- Eventos de F1 y MotoGP: `GP <país>` / `Sprint <país>` (`F1: GP Japón`).
- Fórmula E: `E-Prix <ciudad> [n]` (`FE: E-Prix Londres 1`).
- WEC: `<n> Horas de <lugar>` (`WEC: 6 Horas de Fuji`); los nombres propios se
  mantienen (`Lone Star Le Mans`).
- NASCAR, IndyCar e IMSA: el nombre oficial del evento, tal cual.
- Ubicación: `Ciudad, País` en español (`Suzuka, Japón`); en EE. UU. y Canadá,
  `Ciudad, Estado/Provincia` (`Austin, Texas`, `Markham, Ontario`).
- Tipo de circuito en minúscula tras la primera palabra (`Circuito urbano`,
  `Óvalo corto`).

## Publicar una temporada nueva

1. Sube `f1_2027.json` (etc.) junto al de 2026. **No borres el anterior**
   hasta que empiece la temporada nueva: la app fusiona ambos, y el de 2026
   sigue sirviendo para enseñar la temporada recién terminada.
2. Nada más. (Solo si algún día publicas un `manifest.json`, recuerda añadir
   el fichero nuevo a su lista: con manifiesto, la app descarga únicamente lo
   que aparece en él.)

Sin manifiesto, la app prueba sola `<prefijo>_<año-1>.json`,
`<prefijo>_<año>.json` y `<prefijo>_<año+1>.json`, así que basta con subir el
fichero.

## manifest.json (opcional — hoy NO está publicado)

Si existe, la app descarga **exactamente** los ficheros que lista para cada
serie. Sirve para retirar un fichero erróneo o usar nombres fuera de la
convención sin publicar versión nueva de la app.

```json
{
  "schema": 1,
  "series": {
    "f1": ["f1_2026.json", "f1_2027.json"]
  }
}
```

Una serie que no aparezca en el manifiesto vuelve a la convención por años.
Un JSON inválido en el manifiesto hace que la app lo ignore entero y use la
convención: valídalo antes de subirlo (`python3 -m json.tool manifest.json`).

## Páginas públicas (GitHub Pages)

`privacy.html` y `support.html` se sirven también en
`https://jrverdes.github.io/nextrace-data/…`. La app y App Store Connect
enlazan hoy a `elrouter.com/nextrace/privacidad/` y `…/soporte/`; estas copias
se mantienen por si algún enlace antiguo apunta aquí. No cambies sus nombres.
