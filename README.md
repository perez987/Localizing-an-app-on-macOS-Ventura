# Localizing an app on macOS Ventura with Xcode 14

[Spanish version](README-es.md)

**How to translate into Spanish a simple application that is by default in English, this is the development language and it is not necessary to change it, just add the files required to localize the user interface. I have chosen [About This Mac](https://github.com/0xCUB3/About-This-Hack) by *0xCUB3*, an application that simulates the appearance of the classic About This Mac before macOS 13 with some improvements. It is an Xcode project that uses swift as programming language and whose structure is not excessively complicated to start in this activity.**

### Prerequisites

You must have Xcode installed, I have used version 14.1 but it should work in previous versions as long as you use swift version 5 or higher. How to know the version of swift installed? From Terminal:

```bash
/Users/yo > swift -version
swift-driver version: 1.62.15 Apple Swift version 5.7.1 (swiftlang-5.7.1.135.3 clang-1400.0.29.51)
Target: x86_64-apple-macosx13.0
```

In this case we see that it is swift 5.7.

Download the project in ZIP format from its GitHub site. Unzip it and you can start working on it by opening the `About This Hack.xcodeproj` file. The first time it opens it can ask you to trust it since it's downloaded from the Internet, click on `Trust and Open`.

### Digital signature of the project

The original project is digitally signed by its developer but it is not possible to use it like this on our PC. Go to `TARGETS >> About This Hack >> Signing and Capabilities`:

* Uncheck `Automatically manage Signing`
* Select `Sign to Run Locally `under `Signing Certificate`.
* This way you avoid error messages during the translation process.

<img width="720" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/2-signing.png">

### Enable localization in the project

What needs to be translated is primarily the user interface that is in the `Main.storyboard` file. You must enable the localization of this file. To do this, select it >> `File Inspector` >> first `Identity and Type` tab >> click on the `Localize...` button >> a dialog box asks to choose the language for which an `lproj` folder will be created >> choose English.

<img width="360" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize1.png">

Going back to the `Main.storyboard File Inspector` >> `Identity and Type` tab >> we see that the file has one locale that is English. This is the basis for adding the Spanish localization.

<img width="360" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize3.png">

Go to `PROJECT >> About This Hack >> Info >> Localizations` >> click on + to add another language >> choose the language you want to add, I have done it with `Spanish (es)` >> a dialog appears to choose the files that are going to be localized to the new language, for now we only see `Main.storyboard`>> `Finish`.

<img width="720" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize4.png">

In `Main.storyboard File Inspector`there are now 2 languages, English and Spanish.

<img width="360" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize5.png">

Next to the languages there is a small dropdown with 2 options: `Interface Builder Storyboard` and `Localizable Strings`:

* `Localizable Strings` creates a file `Main.strings` that contains the strings extracted by Xcode from `Main.storyboard (English)`, it is a text file with pairs of strings, on the left the reference to the original string and on the right the translation (which we will do manually).
* `Interface Builder Storyboard` duplicates `Main.storyboard` to keep the original English version and add the Spanish version, the English one is left as is and the Spanish one is modified to our liking.

### Start with `Main.Strings`

In my case, I have obtained the best result starting with the option of the `Main.strings` file, localizing the strings in a simple way and then switching to `Interface Builder Storyboard` mode to finish retouching `Main.storyboard`. The length of the strings in the 2 languages does not always match and some text can be truncated, so after localizing `Main.strings (Spanish)` I end up with the `Interface Builder Storyboard`.

By default Xcode works with `Interface Builder Storyboard` >> we change it in the drop-down to `Localizable Strings` >> a dialog reports the creation of `Main.strings` and the deletion of `Main.storyboard (Spanish)` that has just been generated >> press `Convert` .

This is what `Main.strings` looks like:

```swift
/* Class = "NSMenuItem"; title = "Preferences…"; ObjectID = "BOF-NM-1cW"; */
"BOF-NM-1cW.title" = "Preferences…";

/* Class = "NSTextFieldCell"; title = "Resolution"; ObjectID = "2z4-hu-X9K"; */
"2z4-hu-X9K.title" = "Resolution";

/* Class = "NSMenuItem"; title = "Support"; ObjectID = "3AK-j7-ksI"; */
"3AK-j7-ksI.title" = "Support";
```

At this point we continue with this file, translating all the English strings into Spanish until completing the task:

```swift
/* Class = "NSMenuItem"; title = "Preferences…"; ObjectID = "BOF-NM-1cW"; */
"BOF-NM-1cW.title" = "Preferencias…";

/* Class = "NSTextFieldCell"; title = "Resolution"; ObjectID = "2z4-hu-X9K"; */
"2z4-hu-X9K.title" = "Resolución";

/* Class = "NSMenuItem"; title = "Support"; ObjectID = "3AK-j7-ksI"; */
"3AK-j7-ksI.title" = "Soporte";
```

### Continue with `Main.storyboard`

With `Main.strings` fully translated, switch the file to `Interface Builder Storyboard` mode in `File Inspector` >> dropdown menu to the right of Spanish >> change `Localizable Strings` to `Interface Builder Storyboard` >> a dialog prompts that `Main.strings` will be converted to `Main.storyboard` and removed from the project.

`Main.storyboard (Spanish)` shows the application windows with all the strings that we have previously translated to Spanish in `Main.strings`.

The next and probably the most laborious step is to go through the windows, text boxes, buttons, labels, etc., to detect which elements have truncated text and fix it. We must modify the size of some elements, check that they do not get out of the layout in the correlative positions, it is even probable that we will change some translation extracted from `Main.strings` to make it match better with the graphical interface.

For example, just created `Main.storyboard (English)` >> `Scene Summary` window >> the string `Disco de Arranque` is truncated because it is longer than the containing text box:

<img width="420" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize6.png">
<img width="420" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize7.png">

There are 2 options: change the `Disco de Arranque` text to make it shorter or increase the width of the text box until the entire string is visible. If the width of this element is changed, the text box to the right of it (the one that will show the name of the boot disk) must be adjusted so that they do not overlap. Do the same for any other strings that are truncated or mismatched in some way:

<img width="420" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize8.png">
<img width="420" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/3-localize9.png">

### Localized app

At the end it is necessary to verify that the application is well translated into Spanish in all its elements. These are the windows we have generated:

<img width="640" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/41-resumen.png">
<img width="640" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/42-pantallas.png">
<img width="640" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/43-discos.png">
<img width="640" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/44-soporte.png">

### Strings outside `Main.storyboard` (optional)

However (this is something that can happen in many Xcode projects) we see that the `Storage` tab, although theoretically localized, has the word `Available` instead of `Disponible`. This means that this string is outside `Main.storyboard (English)` and was therefore not extracted when the `Main.strings (Spanish)` file was generated.

About This Hack's Xcode project uses swift as its primary programming language. Which forces us to search through the swift files until we find the place where the `Available` string is to find a way to localize it. We find it in the `HardwareCollector.swift` file, on line 179:

```swift
        print("%: \(1-percent)")
        return ["""
\(name)
\(size)(\(available)Available)
""",String(1-percent)]
```
    
A way is needed to obtain the language in which the application runs so that `Available` is in the English version and `Disponible` in the Spanish version. Swift has a method `Bundle.main.preferredLocalizations[]` that contains the available localizations in the app, it is an array whose first element `[0]` is the current language in which the program runs. With this method and an `if...else` expression it is possible to achieve the desired result. The previous code is replaced by this one:

```swift
        print("%: \(1-percent)")              
        // Get the main language code (en for English, es for Spanish)
        // This is the device language
        // let locale = NSLocale.current.languageCode
        // print ("Device language: \(locale)") // for testing
        // This is the app language
        let idioma = Bundle.main.preferredLocalizations[0]
        // print("Idioma : \(idioma)")  // for testing
        
        // If it's Spanish   
        if idioma == "es" {
            return ["""
    \(name)
    \(size)(\(available)Disponible)
    """,String(1-percent)]
        }
        else { // // If it isn't Spanish
            return ["""
    \(name)
    \(size)(\(available)Available)
    """,String(1-percent)]
        }
```

Now we see that the `Storage` tab has the word `Disponible` and not `Available`, as corresponds to the Spanish language:

<img width="640" src="https://github.com/perez987/Traducir-app-en-macOS/blob/main/img/43-discos-es.png">

### Get the app localized to Spanish

[About This Hack](https://perez987.es/wp-content/uploads/2023/03/About-This-Hack.zip)
