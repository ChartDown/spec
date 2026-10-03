# Guía rápida de ChartDown

Esta guía te lleva de un archivo vacío a un chart completo para tu banda en unos minutos.
Las reglas completas están en la [especificación](../SPEC.md) (en inglés).

## 1. Empieza con un encabezado

```
---
chartdown: 0.4
title: Open Road
key: G
tempo: 76
meter: 4/4
---
```

`key` es el tono base. En el sistema Nashville, `1` es ese tono: en G, `1` = G,
`4` = C, `5` = D y `6m` = Em. Para un tono menor escribe `key: Em`.

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
| `5sus` `4add9` `1maj7` | Otras calidades |
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
| `'4` | Anticipación (push): se toca antes |
| `1!` | Corte: corto y acentuado |
| `<1>` | Sostener el acorde todo el compás |
| `=` | Seguir sosteniendo en el siguiente compás |
| `<1>~` | Calderón: sostener hasta la señal |
| `x` | Sin acorde |
| `%` | Repetir el compás anterior |

## 6. Repeticiones, casillas y reutilizar secciones

```
|: 6m | 4 | 1 | [1] 5 :| x2
[2] 5 4 5 5 |
```

Para tocar una sección otra vez sin reescribirla:

```
## Chorus 2
> MD: Un poco más grande.
@use Chorus
```

Para cambiar de tono desde ese punto: `@key A`.

## 7. Referencias de letra

Una frase corta le dice a la banda dónde va: `1 "Every mile" | 4 1`.
Máximo 8 palabras. ChartDown no es para guardar letras completas.

## 8. Pruébalo

Abre `playground/index.html` en tu navegador, elige un ejemplo y edítalo. El chart se
actualiza mientras escribes, y los avisos te dicen en qué línea hay algo que no se entiende.
