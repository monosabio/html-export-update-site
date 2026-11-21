# HTML Export para DBeaver Community

[English](README.md)

HTML Export agrega un botón **HTML Export** al visor de resultados SQL de DBeaver, junto a *Export data*. Con un clic,
las filas en pantalla se convierten en un reporte HTML independiente, que se puede filtrar, ordenar y graficar en Edge
o Chrome, y copiar a Outlook o Teams. La flecha del botón ofrece también **Export complete result**: después de
confirmar, vuelve a ejecutar la consulta una vez y exporta todas las filas, dentro de límites fijos.

Este repositorio es el **sitio de actualizaciones** de la extensión. DBeaver instala y actualiza HTML Export desde esta
dirección:

```
https://monosabio.github.io/html-export-update-site/
```

La misma dirección, abierta en un navegador, muestra la versión disponible, su ZIP instalable (sin necesidad de cuenta
de GitHub), su SHA-256 y la clave de firma. Usá el ZIP solo cuando DBeaver no pueda llegar al sitio
([Instalar sin acceso a la red](#instalar-sin-acceso-a-la-red)).

## Requisitos

HTML Export es **solo la extensión de DBeaver**. No incluye, no descarga ni instala Python ni el generador de
reportes: los dos deben estar instalados en tu computadora antes de que HTML Export pueda producir un reporte.

| Componente | Versión requerida | Notas |
| --- | --- | --- |
| DBeaver Community | **26.2.5** | Otras versiones de DBeaver no están admitidas: DBeaver rechaza la instalación indicando que faltan elementos requeridos |
| Java | 21 o posterior | Alcanza con el Java que trae DBeaver 26.2.5; no hay que instalar nada más |
| Python | **3.10 o posterior** | En Windows, el `python.exe` real (no el alias de Microsoft Store ni un lanzador `.bat`) |
| Generador DBeaver Export | **26.19.1** o **26.20.0**, sin modificaciones | Proyecto aparte, que se instala por separado. HTML Export reconoce la versión instalada y compara 9 de sus archivos con los valores SHA-256 fijos de esa versión; rechaza cualquier otra versión, una copia modificada (incluidos los cambios de fin de línea) y una carpeta que mezcle archivos de ambas versiones |
| Sistema operativo | Windows 11 x64 o Linux x86_64 | Otros sistemas no se probaron |

Este sitio no distribuye el generador DBeaver Export. Sin él, HTML Export se puede instalar, pero no puede generar
reportes.

## Instalar

1. Abrí DBeaver Community 26.2.5.
2. Elegí **Help > Install New Software...**.
3. Pulsá **Add...**.
4. Escribí **Name** `HTML Export` y **Location** `https://monosabio.github.io/html-export-update-site/`, y pulsá
   **Add**.
5. Marcá **HTML Export for DBeaver Community** en la lista y pulsá **Next >** dos veces.
6. En **Review Licenses**, leé la Apache License 2.0, elegí **I accept the terms of the license agreement** y pulsá
   **Finish**.
7. DBeaver hace dos preguntas de seguridad (ver [Preguntas de seguridad](#preguntas-de-seguridad)):
   - **Trust Authorities**: marcá solo la fila `https://monosabio.github.io` y pulsá **Trust Selected**;
   - **Trust Artifacts** ("Do you trust these signers?"): marcá la fila **PGP** solo si su huella es
     `BBA987AAB8D738A0548DB03A88A1DE796ADEF15A` (la [clave de firma](#clave-de-firma)) y pulsá **Trust Selected**.
8. Cuando DBeaver pregunte *Restart DBeaver to apply the software update?*, pulsá **Restart Now**.
9. Ejecutá una consulta en un editor SQL: el botón **HTML Export** aparece en la barra del resultado, junto a
   *Export data*.
10. Indicá las rutas de Python y del generador DBeaver Export ([Configurar](#configurar)).
11. Pulsá **Verify configuration**.

### Instalar sin acceso a la red

1. En una computadora con acceso a Internet, abrí https://monosabio.github.io/html-export-update-site/ y pulsá
   **Descargar ZIP**.
2. Opcional: comprobá el archivo. Su SHA-256 debe ser el que muestran la página y
   [downloads/SHA256SUMS](https://monosabio.github.io/html-export-update-site/downloads/SHA256SUMS). Por ejemplo,
   `Get-FileHash <archivo>.zip` (Windows PowerShell) o `sha256sum <archivo>.zip` (Linux).
3. En DBeaver, elegí **Help > Install New Software...**, pulsá **Add...** y luego **Archive...**, seleccioná el ZIP y
   pulsá **Add**. No lo descomprimas.
4. Seguí desde el paso 5 de [Instalar](#instalar): la misma licencia y las mismas preguntas de seguridad, con la misma
   huella.

DBeaver no busca actualizaciones en un ZIP. Para actualizar más adelante, agregá la URL del sitio como en
[Migrar desde una instalación con ZIP](#migrar-desde-una-instalación-con-zip), o instalá el ZIP siguiente de la misma
forma.

## Configurar

Abrí **Window > Preferences > Editors > Data Editor > HTML Export**:

- **Python executable**: la ruta completa, por ejemplo `C:\Program Files\Python313\python.exe` o `/usr/bin/python3`.
- **DBeaver Export engine folder**: la carpeta del generador DBeaver Export 26.19.1 o 26.20.0 o 26.21.0 instalado, la que contiene
  `formatear_html_dbeaver_v7.py` y `report/`.
- **Output folder for HTML reports**: una carpeta tuya que ya exista.
- **Click on the HTML Export button**: *Export loaded rows* (por defecto, sin SQL) o *Export complete result* (siempre
  pregunta antes de volver a ejecutar la consulta; si la consulta no se puede volver a ejecutar con seguridad, ofrece
  las filas cargadas).
- **NULL cells in the report**: *Blank* (por defecto, como *Export data > HTML* de DBeaver) o *Text NULL*. Solo cambia
  lo que se ve: los filtros y totales del reporte tratan igual NULL, el texto vacío y el texto NULL.
- Opcional: **Open the generated file automatically** y **Show a notification when finished**.

La primera línea de la página muestra la versión instalada de HTML Export y la versión de DBeaver Export que requiere.
El reporte muestra la consulta SQL sobre la tabla, con *Copiar Query*, como el export HTML propio de DBeaver.

Pulsá **Verify configuration** y luego **Apply and Close**. La configuración se conserva al reiniciar DBeaver y al
actualizar HTML Export.

## Qué hacen las dos opciones de exportación

- **Export loaded rows** (un clic en el botón, salvo que lo cambies en las preferencias): exporta las filas que DBeaver ya cargó, con las columnas visibles en
  su orden. Nunca vuelve a ejecutar SQL.
- **Export complete result** (la flecha del botón): pide confirmación y después **vuelve a ejecutar la consulta del
  resultado una vez** y exporta todas sus filas. Solo acepta un único `SELECT` (o `WITH ... SELECT`) de lectura que
  pueda verificar; cualquier otra cosa se rechaza antes de ejecutar nada. Si los datos cambiaron y las filas que ya
  tenías dejaron de ser las primeras del nuevo resultado, no se exporta nada.
- Las dos opciones respetan los mismos límites: como máximo **10.000 filas**, **256 columnas**, **50.000 celdas** y
  2.000.000 de caracteres. Si se supera un límite, no se escribe ningún reporte, ni siquiera parcial.
- Volver a ejecutar una consulta consume tiempo del servidor de base de datos y puede quedar en su auditoría o en el
  historial de consultas de DBeaver. Usá *Export loaded rows* cuando eso importe.

## Actualizar

En DBeaver, **Help > Check for Updates** solo busca una versión nueva del propio DBeaver: no actualiza extensiones, y
DBeaver no las revisa por su cuenta. Para actualizar HTML Export:

1. Elegí **Help > Installation Information**, pestaña **Installed Software**.
2. Seleccioná **HTML Export for DBeaver Community** y pulsá **Update...**.
3. Cuando aparezca la versión nueva, pulsá **Next >**. Si aparece **Review Licenses** (la primera vez que se muestra la
   Apache License 2.0), aceptala. Luego pulsá **Finish**.
4. Respondé las preguntas de seguridad igual que al instalar y pulsá **Restart Now**.

Una actualización no cambia tu configuración ni tus reportes. Las versiones anteriores siguen en este sitio: en
*Install New Software*, desmarcá **Show only the latest versions of available software** y **Hide items that are
already installed** para verlas.

## Migrar desde una instalación con ZIP

Si instalaste una versión anterior de HTML Export desde un archivo ZIP (*Add > Archive*), DBeaver todavía no conoce
este sitio, y *Update...* diría *No updates found*. Hacé esto una vez; no hace falta reinstalar DBeaver ni HTML
Export:

1. Elegí **Help > Install New Software...**, pulsá **Add...**, escribí **Name** `HTML Export` y **Location**
   `https://monosabio.github.io/html-export-update-site/`, y pulsá **Add**.
2. Cuando la lista muestre HTML Export, pulsá **Cancel**. El sitio queda registrado.
3. Elegí **Help > Installation Information**, pestaña **Installed Software**, seleccioná HTML Export y pulsá
   **Update...**.
4. Dejá marcada la versión nueva, pulsá **Next >**, aceptá la Apache License 2.0 en **Review Licenses**, pulsá
   **Finish**, respondé las preguntas de seguridad y pulsá **Restart Now**.
5. Abrí **Window > Preferences > Editors > Data Editor > HTML Export**: tus rutas siguen ahí. Pulsá
   **Verify configuration**.

## Ver la versión instalada

Elegí **Help > Installation Information**, pestaña **Installed Software**. La fila *HTML Export for DBeaver Community*
muestra la versión, por ejemplo `0.2.5.202611200208-5a52e82`. La [página del sitio](https://monosabio.github.io/html-export-update-site/)
lista cada versión publicada con su ZIP y el SHA-256 del ZIP.

## Desinstalar

1. Elegí **Help > Installation Information**, pestaña **Installed Software**.
2. Seleccioná **HTML Export for DBeaver Community**, pulsá **Uninstall...** y luego **Finish**.
3. Reiniciá DBeaver.

DBeaver sigue funcionando como antes. No se borran tus reportes ni tu configuración. Para quitar también el sitio de
actualizaciones, abrí **Window > Preferences > Install/Update > Available Software Sites** y eliminá *HTML Export*.

## Preguntas de seguridad

Instalar por HTTPS garantiza que los archivos llegan sin cambios desde este sitio. No demuestra quién los publicó, por
eso cada versión de este sitio está **firmada con PGP**, y DBeaver pregunta antes de instalarla:

- **Trust Authorities**: si confiás en el contenido de `https://monosabio.github.io`.
- **Trust Artifacts**: *Do you trust these signers?*, con una fila **PGP** que muestra la huella y el nombre de la
  clave. Confiá solo si la huella es exactamente la de abajo. DBeaver recuerda la clave (*Remember selected signers*),
  así que las actualizaciones firmadas con ella no vuelven a preguntar.

Si DBeaver muestra *Do you trust unsigned content of unknown origin?* o una huella distinta para HTML Export, pulsá
**Cancel**: no es una versión publicada aquí.

**Nunca marques "Always trust all content" ni "Always trust all authorities"**: desactivan estas preguntas para
cualquier software de cualquier sitio, ahora y en el futuro. La
[página del sitio](https://monosabio.github.io/html-export-update-site/) también lista el SHA-256 de cada ZIP
publicado.

### Clave de firma

| | |
| --- | --- |
| Huella | `BBA9 87AA B8D7 38A0 548D  B03A 88A1 DE79 6ADE F15A` |
| Sin espacios | `BBA987AAB8D738A0548DB03A88A1DE796ADEF15A` |

DBeaver muestra la huella en minúsculas y sin espacios, por ejemplo `bba987aab8d738a0548db03a88a1de796adef15a`.

## Si no aparece el botón HTML Export

1. Reiniciá DBeaver una vez más.
2. Revisá **Help > Installation Information > Installed Software**. Si HTML Export no aparece, instalalo de nuevo.
3. El botón está en la barra de un **resultado** (resultados del editor SQL o datos de una tabla), no en la barra
   principal.
4. Solo DBeaver Community **26.2.5** está admitido.
5. Si DBeaver se inició con `-clean` después de instalar, HTML Export puede no cargarse. Desinstalalo, reiniciá y
   volvé a instalarlo.
6. Si DBeaver no puede conectarse al sitio, revisá **Window > Preferences > General > Network Connections** (proxy).

## Si Verify configuration o una exportación fallan

HTML Export muestra un código técnico y una explicación breve, nunca tus datos. Los más comunes:

- **Python no encontrado, no ejecutable o incompatible**: indicá en *Python executable* un Python 3.10+ real.
- **Carpeta del motor no encontrada o archivo del motor faltante**: indicá en *DBeaver Export engine folder* la carpeta
  que contiene `formatear_html_dbeaver_v7.py` y `report/`.
- **Archivo del motor modificado**: un archivo del generador difiere de las versiones 26.19.1 y 26.20.0, o la carpeta
  mezcla archivos de ambas. Restaurá una copia sin modificar de una versión; en Windows, comprobá que Git no haya convertido los fines de línea (`core.autocrlf`). No edites archivos
  para saltear la verificación.
- **Límite superado**: reducí las filas cargadas o las columnas visibles.

## Informar un problema

Abrí un issue en este repositorio. Incluí:

- la versión de HTML Export (ver [Ver la versión instalada](#ver-la-versión-instalada));
- tu versión de DBeaver y tu sistema operativo;
- el código de error que mostró HTML Export, si hubo uno.

**No adjuntes datos reales, reportes ni capturas con información confidencial.**

## Licencia

HTML Export se distribuye bajo la [Apache License 2.0](LICENSE) (ver también [NOTICE](NOTICE)). Copyright 2026 Gaston
Wierny. No incluye DBeaver ni el generador DBeaver Export. DBeaver es una marca de su titular; HTML Export es un
proyecto independiente.

## Privacidad

HTML Export funciona localmente. No envía telemetría ni hace llamadas de red; los reportes se escriben solo en la
carpeta que elijas.
