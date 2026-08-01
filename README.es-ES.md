

# Instalador de YNAB 4 de 64 bits + entorno de ejecución moderno de Adobe AIR para macOS

YNAB 4 es una aplicación de 32 bits que no se ejecutará en macOS a partir de macOS Catalina. Catalina es la primera versión de macOS que admite exclusivamente aplicaciones de 64 bits. YNAB anunció que no actualizará YNAB 4 a la arquitectura de 64 bits.

Este script de shell hará lo siguiente:
- descargará la versión final de YNAB 4 para macOS (v4.3.855)
- la convertirá de una aplicación de 32 bits a una de 64 bits
- actualizará el entorno de ejecución de Adobe AIR integrado a una versión más reciente de **HARMAN Adobe AIR** (actualmente fijada en **51.2.2.6**)

Aún necesitarás una clave de licencia válida para usar la aplicación.

Este script de shell no está afiliado a YNAB de ninguna manera y YNAB no lo ha respaldado en absoluto. YNAB ha [terminado oficialmente el soporte](https://web.archive.org/web/20191008235951/https://www.youneedabudget.com/ynab-4-support-will-end-october-2019/) para YNAB 4. No los contactes con solicitudes de soporte de ningún tipo con respecto a este script de shell o al usar YNAB 4 en macOS Catalina. Estás por tu cuenta y lo usas bajo tu propio riesgo.

Este proyecto se basa en [bradleymiller/Y64](https://gitlab.com/bradleymiller/Y64), pero se ha actualizado para producir el resultado “final” con entorno de ejecución moderno.

## Uso

Para ejecutar el script de shell, realiza lo siguiente:

1. Presiona `Command (⌘) + Espacio`
2. Escribe `Terminal` y presiona Enter
3. Copia y pega lo siguiente en la ventana de la terminal y presiona Enter. El script explica lo que hará y luego hace una pausa antes de ejecutarlo.

   ```/bin/sh -c "$(curl -fsSL https://raw.githubusercontent.com/adamgall/ynab4-macos-modernizer/main/install)"```

4. Cuando el script de shell haya finalizado, debes arrastrar la aplicación resultante a tu directorio `/Applications` para instalarla.

Consejo: si ya tienes una versión anterior parcheada de `YNAB 4.app`, cámbiale el nombre primero (por ejemplo, `YNAB 4 (old).app`) para poder revertir fácilmente.

Consejo: para evitar sobrescribir una instalación existente, puedes establecer `OUTPUT_APP_NAME` para compilar un paquete de aplicación con un nombre diferente:

```sh
OUTPUT_APP_NAME="YNAB 4 Remix" /bin/sh -c "$(curl -fsSL https://raw.githubusercontent.com/adamgall/ynab4-macos-modernizer/main/install)"
```

Consejo: inicia la aplicación haciendo doble clic en ella en Finder (o Launchpad). Evita ejecutar `YNAB 4.app/Contents/MacOS/YNAB 4` directamente desde una terminal, ya que algunos entornos de terminal con sandbox pueden interferir con LaunchServices y provocar fallos al inicio.

## Cómo funciona

YNAB 4 no está escrito en un lenguaje compilado. Está escrito en ActionScript y es interpretado por el entorno de ejecución de Adobe AIR cuando ejecutas el programa. Como resultado, es el entorno de ejecución de Adobe AIR el que necesita actualizarse, no el ActionScript. La versión final de YNAB 4 se distribuye con un entorno de ejecución AIR de 32 bits y extensiones nativas de 32 bits; este script parchea la aplicación a `x86_64` y luego incrusta un entorno de ejecución moderno de HARMAN Adobe AIR en el paquete resultante.

En versiones modernas de macOS, el entorno de ejecución AIR integrado más antiguo puede causar un comportamiento degradado (por ejemplo, lentitud y atajos de teclado que no funcionan). Este script actualiza el entorno de ejecución integrado a una compilación más reciente de HARMAN Adobe AIR después de la conversión a 64 bits.

## Limitaciones
- No funcionará con licencias compradas en la App Store a menos que hayas recibido una clave de licencia por separado. Consulta las [Preguntas frecuentes de Mac App Store](https://web.archive.org/web/20170817002804/https://classic.youneedabudget.com/support/article/mac-app-store-faq) de YNAB.
- La función `Check for Updates` no funciona.
- Apple Silicon: esta compilación sigue siendo `x86_64` y se ejecutará bajo Rosetta 2 (porque las extensiones nativas de YNAB 4 no están disponibles como `arm64`).

## Otros métodos

Es posible que puedas ejecutar macOS Mojave o Windows en una máquina virtual para ejecutar la aplicación de 32 bits en Catalina.

## Aspectos legales
**NOTA IMPORTANTE:** Este script de shell no está afiliado a YNAB de ninguna manera y YNAB no lo ha respaldado en absoluto. You Need a Budget y YNAB son marcas comerciales registradas de You Need A Budget LLC y/o una de sus filiales.

EL SOFTWARE SE PROPORCIONA "TAL CUAL", SIN GARANTÍA DE NINGÚN TIPO, EXPRESA O IMPLÍCITA, INCLUYENDO, PERO NO LIMITADO A, LAS GARANTÍAS DE COMERCIABILIDAD, ADECUACIÓN PARA UN PROPÓSITO PARTICULAR Y NO INFRACCIÓN. EN NINGÚN CASO LOS AUTORES NI LOS TITULARES DE LOS DERECHOS DE AUTOR SERÁN RESPONSABLES POR CUALQUIER RECLAMACIÓN, DAÑO U OTRA RESPONSABILIDAD, YA SEA EN UNA ACCIÓN DE CONTRATO, AGRAVIO U OTRA, QUE SURJA DE, FUERA DE O EN CONEXIÓN CON EL SOFTWARE O EL USO U OTROS TRATOS EN EL SOFTWARE.
