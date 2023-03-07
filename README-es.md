# Traducir una aplicación en macOS Ventura con Xcode 14

[English version](README.md)

**Cómo traducir al español una aplicación sencilla que está por defecto en inglés, este es el idioma de desarrollo (developing language_) y no es necesario cambiarlo, solamente añadir los archivos requeridos para traducir la interfaz de usuario. He escogido [About This Mac](https://github.com/0xCUB3/About-This-Hack) de *0xCUB3*, aplicación que simula el aspecto de la clásica Acerca de este Mac (About This Mac) anterior a macOS 13 con algunas mejoras. Es un proyecto de Xcode que utiliza swift como lenguaje de programación y cuya estructura no es excesivamente complicada para iniciarse en esta actividad.**

### Pasos previos

Hay que tener Xcode instalado, yo he usado la versión 14.1 pero debería funcionar en versiones anteriores a condición de usar swift versión 5 o superior. ¿Cómo saber la versión de swift instalada? Desde Terminal:

```bash
/Users/yo > swift -version
swift-driver version: 1.62.15 Apple Swift version 5.7.1 (swiftlang-5.7.1.135.3 clang-1400.0.29.51)
Target: x86_64-apple-macosx13.0
```

En este caso vemos que se trata de swift 5.7.

Descarga el proyecto en formato ZIP desde su sitio de [GitHub](https://github.com/0xCUB3/About-This-Hack). Descomprímelo y ya puedes empezar a trabajar en él abriendo el archivo About This Hack.xcodeproj. La primera vez que se abre puede pedir que confíes en él al venir de Internet, pulsa en Trust and Open. 

### Firma digital del proyecto

El proyecto en origen está firmado digitalmente por su creador pero no es posible usarlo así en nuestro PC. Ve a `TARGETS >> About This Hack >> Signing and Capabilities`:

* Desmarca `Automatically manage Signing`
* Selecciona `Sign to Run Locally` en Signing Certificate.
* De esta forma evitas avisos de error durante el proceso de traducción.

<img width="720" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/2-signing.png">

### Habilitar las traducciones en el proyecto

Lo que se necesita traducir es primordialmente la interfaz de usuario que está en el archivo `Main.storyboard`. Hay que habilitar la localización de este archivo. Para ello lo seleccionas >> File Inspector >> primera pestaña Identy and Type >> pulsar en el botón `Localize...` >> un cuadro de diálogo solicita elegir el idioma para el que se creará una carpeta `lproj` >> eliges English (Base no).

<img width="360" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize1.png">

Volviendo a File Inspector de `Main.storyboard` >> pestaña Identy and Type >> vemos que el archivo tiene una localización que es English. Esta es la base para añadir la localización en español.

<img width="360" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize3.png">

Ve a `PROJECT >> About This Hack >> Info >> Localizations` >> pulsa en + para añadir otro idioma >> elige el idioma que deseas añadir, yo lo he hecho con Spanish (es) >> aparece un diálogo para elegir los archivos que se van a localizar al nuevo idioma, en este caso solamente vemos `Main.storyboard` >> `Finish`.

<img width="720" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize4.png">

En File Inspector de `Main.storyboard` ahora hay 2 idiomas, English y Spanish.

<img width="360" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize5.png">

Junto a los idiomas hay un pequeño desplegable con 2 opciones: `Interface Builder Storyboard` y `Localizable Strings`:

* `Localizable Strings` crea un archivo Main.strings que contiene las cadenas que Xcode extrae desde `Main.storyboard (English)`, es un archivo de texto con pares de cadenas, a la izquierda la referencia a la cadena original y a la derecha la traducción (que haremos manualmente).
* `Interface Builder Storyboard` duplica `Main.storyboard` para manteniendo la versión inglesa original y añadiendo la versión española, la inglesa se deja tal cual y la española se modifica a nuestro gusto.

### Comenzar con `Main.Strings`

En mi caso el mejor resultado lo he obtenido empezando con la opción `Main.strings`, traduciendo las cadenas de forma sencilla y cambiando después al modo `Interface Builder Storyboard` para terminar retocando `Main.storyboard`. No siempre coincide la longitud de las cadenas en los 2 idiomas y algunos textos pueden verse truncados, por lo que después de preparar `Main.strings (Spanish)` termino con `Interface Builder Storyboard`.

Por defecto Xcode trabaja con `Interface Builder Storyboard` >> lo cambiamos en el desplegable a `Localizable Strings` >> un diálogo informa de la creación de `Main.strings` y de la eliminación de `Main.storyboard (Spanish)` que acaba de ser generado >> pulsas Convert.

Este es el aspecto de `Main.strings`:

```c
/* Class = "NSMenuItem"; title = "Preferences…"; ObjectID = "BOF-NM-1cW"; */
"BOF-NM-1cW.title" = "Preferences…";

/* Class = "NSTextFieldCell"; title = "Resolution"; ObjectID = "2z4-hu-X9K"; */
"2z4-hu-X9K.title" = "Resolution";

/* Class = "NSMenuItem"; title = "Support"; ObjectID = "3AK-j7-ksI"; */
"3AK-j7-ksI.title" = "Support";
```

En este punto continuamos con este archivo, traduciendo al español todas las cadenas inglesas hasta completar la tarea:

```c
/* Class = "NSMenuItem"; title = "Preferences…"; ObjectID = "BOF-NM-1cW"; */
"BOF-NM-1cW.title" = "Preferencias…";

/* Class = "NSTextFieldCell"; title = "Resolution"; ObjectID = "2z4-hu-X9K"; */
"2z4-hu-X9K.title" = "Resolución";

/* Class = "NSMenuItem"; title = "Support"; ObjectID = "3AK-j7-ksI"; */
"3AK-j7-ksI.title" = "Soporte";
```

### Continuar con `Main.storyboard`

Con `Main.strings` completamente traducido, hay que cambiar el archivo al modo `Interface Builder Storyboard` en File Inspector >> menú desplegable a la derecha de Spanish >> cambiar `Localizable Strings` a `Interface Builder Storyboard` >> un diálogo avisa de que `Main.strings` será convertido a `Main.storyboard` y eliminado del proyecto.

`Main.storyboard (Spanish)` muestra las ventanas de la aplicación con todas las cadenas que hayamos traducido anteriormente en `Main.strings` al español.

El paso siguiente y probablemente el más laborioso es repasar las ventanas, cuadros de texto, botones, etiquetas, etc., para detectar qué elementos tienen el texto truncado y arreglarlo. Hay que modificar el tamaño de algunos elementos, repasar que no se desajustan del diseño en la posición correlativa que ocupan, incluso es probable que cambiemos alguna traducción extraída de `Main.strings` para hacerla coincidir mejor con la interfaz gráfica.

Por ejemplo, recién creado `Main.storyboard (Spanish)` >> ventana `Resumen (Resumen Scene)` >> la cadena `Disco de arranque` está truncada por ser más larga que el cuadro de texto que la contiene:

<img width="420" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize6.png">
<img width="420" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize7.png">

Hay 2 opciones: cambiar el texto `Disco de Arranque` para hacerlo más corto o aumentar la anchura del cuadro de texto hasta que se vea toda la cadena. Si se cambia la anchura de este elemento hay que ajustar el cuadro de texto que hay a su derecha (el que mostrará el nombre del disco de arranque) para que no se superpongan. Lo mismo hay que hacer con las demás cadenas que estén truncadas o desajustadas de alguna manera:

<img width="420" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize8.png">
<img width="420" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize9.png">

### Aplicación traducida

Al finalizar hay que comprobar que la aplicación está bien traducida al español en todos sus elementos. Estas son las ventanas que hemos generado:

<img width="640" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/41-resumen.png">
<img width="640" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/42-pantallas.png">
<img width="640" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/43-discos.png">
<img width="640" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/44-soporte.png">

### Cadenas fuera de `Main.storyboard` (opcional)

Sin embargo (esto es algo que puede pasar en muchos proyectos Xcode) vemos que la pestaña Almacenamiento, aunque teóricamente está traducida, tiene la palabra `Available` en lugar de `Disponible`. Esto quiere decir que esta cadena está fuera de `Main.storyboard (English)` y por ello no fue extraída cuando se generó el archivo `Main.strings (Spanish)`.

El proyecto Xcode de About This Hack utiliza swift como lenguaje principal de programación. Lo que nos obliga a buscar en los archivos swift hasta encontrar el sitio en que está la cadena `Available` para buscar alguna manera de traducirla. La encontramos en el archivo `HardwareCollector.swift`, en la línea 179:

```swift
        print("%: \(1-percent)")
        return ["""
\(name)
\(size)(\(available)Available)
""",String(1-percent)]
```
    
Se necesita una forma de obtener el idioma en que arranca la aplicación para que `Available` esté en la versión inglesa y `Disponible` en la española. Swift dispone del método `Bundle.main.preferredLocalizations[]` que contiene las traducciones disponibles en la aplicación, es un array cuyo primer elemento `[0]` es el idioma actual en que se ejecuta el programa. Con este método y una expresión `if...else` es posible conseguir el resultado deseado. El código anterior se reemplaza por este otro:

```swift
        print("%: \(1-percent)")              
        // Obtener el lenguaje principal (en para English, es para Spanish)
        // Este es el lenguaje del dispositivo
        // let locale = NSLocale.current.languageCode
        // Este es el lenguaje de la aplicación
        let idioma = Bundle.main.preferredLocalizations[0]
        
        // Si el código corresponde a español
        if idioma == "es" {
            return ["""
    \(name)
    \(size)(\(available)Disponible)
    """,String(1-percent)]
        }
        else { // si el código corresponde a inglés
            return ["""
    \(name)
    \(size)(\(available)Available)
    """,String(1-percent)]
        }
```

Ahora vemos que la pestaña Almacenamiento tiene la palabra `Disponible` y no `Available`, como corresponde al idioma español:

<img width="640" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/43-discos-es.png">
