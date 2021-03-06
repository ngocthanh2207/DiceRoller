DiceRoller
==========
This is a Dice Rolling application for Android. It was developed as part of a
class project.

Build Instructions
------------------
### Requirements
+ [Android SDK][android] version 11 or higher
+ [JDK][jdk] version 1.6 or higher
+ [Apache Ant][ant]

It is also possible to compile the app as an Eclipse project using the ADT
plugin for Eclipse. This README will not go over the process for compiling with
Eclipse, but for more information visit the official android documentation
pages on building android apps [on the command line][cmdbuild], [from
Eclipse][eclipsebuild], and [signing apps][sign].

The `$` in the below steps indicate a command shell.

### 1. Set up your environment
Create the ant build files by running:

    $ android update project -p .

This will generate a `build.xml` file which will be used by Ant to build the
source.

### 2. Compilation
Run `ant` to compile. An APK will be generated in the `bin/` directory. Two
common targets for `ant` to build are `debug` and `release`.

    $ ant debug

Running this will generate an APK signed with a special key used for
debugging applications. You can install this to your phone, but you cannot
distribute it over the Play Store

    $ ant release

This will generate an APK which can be made suitable for distribution, but
is initially unsigned. Continue to step 3 to sign your APK.

### 3. APK signing
If you built in `release` mode, you need to sign and align your application. If
you built in `debug` mode, you can skip this step, and go to step 4.  If you
haven't already done so, generate a key using the `keytool` tool which comes
with you JDK.

    $ keytool -genkey -v -keystore <name>.keystore -alias <alias_name> -keyalg RSA -keysize 2048 -validity 10000

The tool will prompt you to enter some identification information. Note that if
you plan on releasing the application, you must use the same private key to
sign each version of the application.  Using the generated keystore file, you
must then sign the application. You will use the `jarsigner` program which
comes with the JDK.

    $ jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore <generated.keystore> <application.apk> <alias_name>

This will modify the provided APK file in-place to create an unaligned APK.
Finally, you must align the APK. Essentially, alignment provides a performance
boost for the application at run time. To align the APK, you will use the
`zipalign` tool which comes with the Android SDK. The syntax is:

    $ zipalign -v 4 <application-unaligned.apk> <application.apk>

The first argument is the signed APK generated earlier, and the second is the
destination of the tool which is the final APK.

### 4. Installation
To install, simply download the generated APK to your android device. Locate it
in the file browser, and open it. The Android operating system should begin the
installation process automatically.

Note that you may need to enable installing applications from unknown sources
before the operating system will allow installation. To do this, go to your
system settings, and check the `Unknown Sources` box under the `Applications`
sub menu.


Licencing
---------
Copyright (c) 2013 Padraic Galen Baker, Ryan Pearce, Wayne Rodenburg, Krysten Thomas, Carlos Valera

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


### Images used
The image of the lion used for the launcher icon was adapted from the following
two images:

1. The [Lion][lionurl] is property of Ltshears, and is licenced under the
   [Creative Commons Attribution-Share Alike 3.0 Unported][lionlicence] license.
2. The [Flame][flameurl] is property of Fir0002/Flagstaffotos, and is licensed under the
   [Creative Commons Attribution-NonCommercial][flamelicense] license.


[android]: http://developer.android.com/sdk/index.html
[jdk]: http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html
[ant]: http://ant.apache.org/
[cmdbuild]: http://developer.android.com/tools/building/building-cmdline.html 
[eclipsebuild]: http://developer.android.com/tools/building/building-eclipse.html
[sign]: http://developer.android.com/tools/publishing/app-signing.html
[lionurl]: http://commons.wikimedia.org/wiki/File:Lion2010_2.jpg
[lionlicence]: http://creativecommons.org/licenses/by-sa/3.0/deed.en
[flameurl]: http://commons.wikimedia.org/wiki/File:Fire02.jpg
[flamelicense]: http://creativecommons.org/licenses/by-nc/2.0/

