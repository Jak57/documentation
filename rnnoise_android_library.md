# RNNoise library creation for Android

## Platform
Android Studio Meerkat | 2024.3.1

## Instructions for the creation of the library
1. Open Android Studio and go to:<br>
   ``File -> New -> New Project -> Phone and Tablet -> Native C++``<br>
   Then select ``Next``.
2. Provide the following information<br>
   ```
   Name: rnnoise_canceller
   Package name: com.example.rnnoise_canceller
   Save location: C:\Users\Reve_207\AndroidStudioProjects\rnnoise_canceller
   Language: Java
   Minimum SDK: API 21 ("Lollipop"; Android 5.0)
   Build configuration language: Kotlin DSL (build.gradle.kts) [Recommended]
   ```
   Then select ``Next`` -> ``Finish``.
3. Create ``rnnoise`` directory in the following location ``rnnoise_canceller\app\src\main\cpp``.
4. From the trained RNNoise model, copy the ``src`` and ``include`` directory into the ``rnnoise`` folder.
5. Copy the ``rnnoise.h`` file form the ``include`` directory to the ``src`` directory.
6. Edit the ``rnnoise/src/x86/x86cpu.h`` file by changing ``#include "common.h"`` to ``#include "../common.h"``.
7. Enter the following commands into the command line for building the shared object (.so) files.
   * ``./gradlew clean``
   * ``./gradlew build``
8. Right-click on the project name ``rnnoise_canceller`` then  select ``New -> Module -> Android Library``.
9. Provide the following information
   ```
   Module name: rnnoise
   Package name: com.example.rnnoise
   Language: Java
   Minimum SDK: API 21 ("Lollipop"; Android 5.0)
   Build configuration language: Kotlin DSL (build.gradle.kts) [Recommended]
   ```
10. Move ``rnnoise_canceller\app\src\main\cpp`` to ``rnnoise_canceller\rnnoise\src\main``.




10. =========================================================================================================================
11. 
   

