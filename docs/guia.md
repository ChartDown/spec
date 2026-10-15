# Guía rápida de ChartDown

Esta guía te lleva de un archivo vacío a un chart completo para tu banda en unos minutos.
Las reglas completas están en la [especificación](../SPEC.md) (en inglés).

## 1. Empieza con un encabezado

```
---
chartdown: 0.8
title: Open Road
key: G
tempo: 76
meter: 4/4
---
```

`key` es el tono base. En el sistema Nashville, `1` es ese tono: en G, `1` = G,
`4` = C, `5` = D y `6m` = Em. Para un tono menor escribe `key: Em`.

En un tono menor, `1m` es el acorde de tónica. Si tu banda numera las canciones
menores desde la relativa mayor, con la tónica en `6m`, agrega `numbering: 6m`
al encabezado. Los dos charts dicen los mismos acordes.

**¿No usas números?** Escribe los acordes con letra. Con `key: G`, la fila
`G | C D | Em | D/F#` es un chart válido: ChartDown saca los números a partir del
tono, y el playground muestra el chart en números o en letras, en cualquier tono.

## 2. Agrega secciones y compases

Una sección empieza con `##`. Cada línea debajo es una fila de compases, separados con `|`.

```
## Verse 1
1 | 4 1 | 6m 5 | 4 1
```

Los acordes de un mismo compás se reparten el tiempo: `4 1` son dos tiempos de 4 y dos de 1.
Para repartir distinto usa tiempos: `1:3 4:1`.

## 3. Escribe los acordes

| Escribes | Significa |
|---|---|
| `1` `4` `5` | Acordes mayores |
| `6m` o `6-` | Menor |
| `5sus` `4add9` `1maj7` `2m7b5` | Otras calidades |
| `1M7` `7ø` `5+` `5(b9)` | También valen las formas comunes: se leen `1maj7`, `7m7b5`, `5aug`, `5b9` |
| `1/3` | Acorde con bajo (1 con la tercera en el bajo) |
| `b7` `#4` | Acordes fuera del tono |
| `G` `Em` `C/E` | También puedes usar letras |

## 4. Agrega las indicaciones del MD

Esto es lo que hace diferente a ChartDown.

```
## Chorus
> MD: Ábrelo un poco.
> MD drums: Groove ligero, sin platillos.
> MD bass, keys: Solo notas raíz.
```

- `> MD:` es para toda la banda.
- `> MD drums:` es solo para el baterista. Instrumentos: `drums`, `bass`, `keys`, `gtr`, `vox`, `sax`, `tpt` (trompeta), `tbn` (trombón), `vln` (violín).
- Otros instrumentos: decláralos en el encabezado, `instruments: cello, banjo`, y luego escribe `> MD cello: ...`.
- Una señal en un compás: `{drums: in}` o `{bass: walk up}`.

Una app compatible con ChartDown puede mostrarle a cada músico solo las notas de su instrumento.

## 5. Indica cómo se toca

| Escribes | Significa |
|---|---|
| `{p}` `{mf}` `{f}` `{cresc}` | Dinámicas |
| `'4` | Anticipación (push): se toca antes (una corchea) |
| `''4` | Anticipación corta: una semicorchea antes |
| `1!` | Corte: corto y acentuado |
| `<1>` | Sostener el acorde todo el compás |
| `=` | Seguir sosteniendo en el siguiente compás |
| `<1>~` | Calderón: sostener hasta la señal |
| `x` | Sin acorde |
| `1! x:3` | Corte: golpe en el tiempo 1 y tres tiempos de silencio |
| `R[ 1 3 4 #4 5 ]` | Riff al unísono: notas sueltas en lugar de acordes |
| `%` | Repetir el compás anterior |

Un pickup (compás corto que lleva al primer compás completo) empieza con `[p:tiempos]`:
`[p:2] 5 | 1 | 4 | 5`. Es el compás 0 y los compases completos se cuentan desde 1.
Indica quién toca un riff en una nota MD: `> MD gtr, tpt: Riff al unísono en el compás 2.`

## 6. Repeticiones, casillas y reutilizar secciones

```
|: 6m | 4 | 1 | [1] 5 :| 2x
[2] 5 4 5 5 |
```

Para tocar una sección otra vez sin reescribirla:

```
## Chorus 2
> MD: Un poco más grande.
@use Chorus
```

Agrega un número para tocarla más de una vez: `@use Chorus 2x`. Cada vuelta
toca la sección tal como está escrita, así que una sección que se repite 2x,
copiada con `@use ... 2x`, toca sus compases repetidos cuatro veces.

Para cambiar de tono desde ese punto: `@key A`. El compás y el tempo funcionan
igual: `@meter 6/8` y `@tempo 72`. Para un solo compás con otra métrica,
empiézalo con `[m:2/4]`. Cambios graduales de tempo: `{rit}`, `{accel}`,
`{a tempo}`.

### Indicaciones para una sola pasada

Dentro de una repetición, escribe la pasada antes de los instrumentos, igual que
cuentas la repetición:

```
|: 1 {2x drums: in} | 4 | 5 {1x,3x: stop} | 1 :| 3x
> MD 2x bass: Camina el bajo en la segunda pasada.
```

### Navegación

Escribe las marcas que la banda ya conoce, entre llaves sobre un compás:
`{segno}`, `{coda}`, `{to coda}`, `{fine}`, `{D.S. al coda}`, `{D.C. al fine}`,
etc.

```
## Verse
{segno} 1 | 4 | 6m | 5

## Chorus
4 | 1 | 5 {to coda} | 5 {D.S. al coda}

## Outro
{coda} 4 | 5 | <1>~
```

`{to coda}` solo cuenta después del salto. Una app compatible con ChartDown
puede mostrar el orden de ejecución completo.

## 7. Referencias de letra

Una frase corta le dice a la banda dónde va: `1 "Every mile" | 4 1`.
Máximo 8 palabras. ChartDown no es para guardar letras completas.

## 8. Pruébalo

Abre `playground/index.html` en tu navegador, elige un ejemplo y edítalo. El chart se
actualiza mientras escribes, y los avisos te dicen en qué línea hay algo que no se entiende.
