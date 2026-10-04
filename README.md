# PhytoDiagnostix

Entrena el ojo: anatomía vegetal y deficiencias foliares con fotos libres y banco propio del profesorado. Aplicación web de un solo fichero.

**Usar la app:** https://fborrasumh.github.io/phytodiagnostix/

## Qué hace

- **Banco de 60 fotos de Wikimedia Commons**: 57 de anatomía histológica (colénquima, parénquima, esclerénquima, xilema, floema, estomas, epidermis y meristemo) y 3 de deficiencia foliar (nitrógeno). Cada foto se pide a Commons al mostrarla; su autoría y licencia se leen de la ficha de Commons y se enseñan debajo de la foto.
- **Corrección por código**: eliges una estructura (o la escribes: se aceptan tildes, mayúsculas y sinónimos como «células oclusivas» o «fibras») y el código la compara con la etiqueta de la foto. Una respuesta ambigua o que no se reconoce no puntúa.
- **Fichas de repaso locales** (sin IA) con los rasgos de cada tejido o carencia y las confusiones típicas entre ellos.
- **Rasgos observados**: el estudiante marca lo que ve y el código le dice si encaja con la estructura correcta (aviso formativo, no puntúa).
- **Progreso local**: acierto por estructura, «Repasar lo que fallo», informe en Word y CSV.
- **Segunda opinión de la IA (opcional)**: la IA elige de una lista cerrada y solo puede citar rasgos de la ficha; lo demás se descarta. Si no coincide con la etiqueta, prevalece la etiqueta y la foto queda marcada para revisión del profesorado.
- **Mis propias fotos**: el estudiante puede subir una microfotografía suya; sin etiqueta no hay veredicto del código, solo una pista orientativa de la IA.
- **Banco del profesorado** con un JSON (ver abajo), búsqueda de fotos libres en Commons para ampliarlo, corrección y ocultación de fotos del banco base, y descarga de los créditos (CSV).

## Banco de fotos del profesorado (JSON)

1. Crea `banco-profesor.json` en la raíz del repositorio, junto a `index.html` (parte de `banco-profesor.ejemplo.json`).
2. Sube tus fotos a una carpeta (p. ej. `fotos/`) y usa su ruta relativa en `url`. También vale `commons` (nombre de un fichero de Commons) o una dirección de `upload.wikimedia.org`.
3. Al abrir la app desde GitHub Pages el fichero se lee solo y tus fotos salen como «Mi profesorado». Si no existe, la app funciona con el banco base.

Cada foto lleva `id`, `modo` (`anatomia` o `deficiencia`), `etiqueta` (de la lista cerrada: anatomía `epidermis`, `estomas`, `parenquima`, `colenquima`, `esclerenquima`, `xilema`, `floema`, `meristemo`; deficiencia `N`, `P`, `K`, `Mg`, `Fe`, `Ca`, `S`, `Zn`, `Mn`, `B`) y, si quieres, `tambien` (otras respuestas válidas), `titulo`, `nota`, `aumento`, `tincion`, `especie`, `autoria` y `licencia`. El campo `corrige` cambia la etiqueta de una foto del banco base (`{"c03": "parenquima"}`) y `ocultar` la retira. La pantalla «Profesorado» valida el fichero y enseña qué fotos se rechazan y por qué. Pon siempre `autoria` y `licencia` de tus fotos.

## Cómo se usa la IA

Con la propia clave de OpenAI, Google Gemini o Anthropic Claude. La clave se guarda solo en el navegador. No hace falta servidor.

## Privacidad

- Se guardan solo en el navegador: respuestas, progreso, ajustes del profesorado y la clave de IA.
- Hacia **Wikimedia Commons** sale la petición de cada foto y de su ficha (autoría y licencia).
- Hacia **tu proveedor de IA** sale, solo si pides la segunda opinión y tras ver y confirmar lo que sale: la foto y tu descripción (con correos, DNI y teléfonos enmascarados). **Las imágenes no se pueden enmascarar**: no subas fotos propias con caras o datos personales. Las fotos propias piden confirmación cada vez.
- La política de seguridad del navegador (CSP) solo permite conectar con Commons, con los tres proveedores de IA y con el propio sitio; las imágenes solo pueden venir del propio sitio, de Commons o de datos incrustados.

## Límites

- **Las etiquetas del banco base salen del título del fichero y de la categoría en Commons, no de una revisión visual de cada foto.** Pueden contener errores: el profesorado debe repasarlas en «Profesorado» (hay un marcador ⚠ para las fotos donde la IA discrepó).
- Una microfotografía suele mostrar varios tejidos; la etiqueta indica el protagonista. Por eso hay respuestas alternativas (`tambien`) en algunas fotos.
- El banco de deficiencia foliar solo trae 3 fotos (nitrógeno): sin fotos del profesorado no hay variedad de carencias.
- Es una herramienta formativa. Las etiquetas viajan en el código de la página: quien inspeccione el HTML puede verlas, así que no sirve para evaluar con nota.
- La IA puede equivocarse; no decide el veredicto y sus rasgos no están verificados.
- Un fichero de Commons puede renombrarse o borrarse: la foto saldría como «no disponible» y el CSV de créditos la marca.
- No se ha probado con claves reales de OpenAI, Gemini ni Claude, ni con la API real de Commons (las pruebas usan respuestas simuladas).

## Autoría

Fernando Borrás Rocher y María Emma García Pastor (Universidad Miguel Hernández de Elche).

**Origen de la idea:** Idea original de María Emma García Pastor (PhytoDiagnostix v1.0: evaluación de microfotografías con IA), ampliada con banco de fotos de licencia libre, corrección por código y banco propio del profesorado.

ORCID: Fernando Borrás Rocher [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573) · María Emma García Pastor [0000-0002-4959-9419](https://orcid.org/0000-0002-4959-9419)

## Cómo citar

Borrás Rocher, F., y García Pastor, M. E. (2026). *PhytoDiagnostix* (v1.1.0) [Software]. Zenodo. https://doi.org/10.5281/zenodo.23142093

## Licencia

MIT. Véase [LICENSE](LICENSE).

## Desarrollo y pruebas

Proyecto Forja: `src/` (`datos.js`, `nucleo.js`, `ui.js`, `body.html`, `app.css`) → `python3 build.py` monta `index.html`. Pruebas: `python3 tests/phyto_test.py` (Playwright, con Commons y proveedores de IA simulados).
