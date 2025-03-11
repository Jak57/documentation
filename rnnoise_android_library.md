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
7. Delete native-lib.cpp from ``rnnoise_canceller\app\src\main\cpp`` and add the following files:
    * com_reve_audio_libraries_dlls_audio_filters_rnnoise_RNNoise.cpp
    * com_reve_audio_libraries_dlls_audio_filters_rnnoise_RNNoise.h
    * rnnoise_config_parameters.h
8. Prepare the CMakeLists.txt
   ```
   cmake_minimum_required(VERSION 3.22.1)
   project("rnnoise_canceller")
   
   include_directories(
           rnnoise/src/arch.h
           rnnoise/src/celt_lpc.h
           rnnoise/src/common.h
           rnnoise/src/cpu_support.h
           rnnoise/src/denoise.h
           rnnoise/src/kiss_fft.h
           rnnoise/src/nnet.h
           rnnoise/src/nnet_arch.h
           rnnoise/src/opus_types.h
           rnnoise/src/pitch.h
           rnnoise/src/rnn.h
           rnnoise/src/rnnoise_data.h
           rnnoise/src/rnnoise_data_little.h
           rnnoise/src/vec.h
           rnnoise/src/vec_avx.h
           rnnoise/src/vec_neon.h
           rnnoise/src/rnnoise.h
           com_reve_audio_libraries_dlls_audio_filters_rnnoise_RNNoise.h
           rnnoise_config_parameters.h
   )
   add_library(${CMAKE_PROJECT_NAME} SHARED
           rnnoise/src/celt_lpc.c
           rnnoise/src/denoise.c
           rnnoise/src/kiss_fft.c
           rnnoise/src/nnet.c
           rnnoise/src/nnet_default.c
           rnnoise/src/parse_lpcnet_weights.c
           rnnoise/src/pitch.c
           rnnoise/src/rnn.c
           rnnoise/src/rnnoise_data.c
           rnnoise/src/rnnoise_tables.c
           com_reve_audio_libraries_dlls_audio_filters_rnnoise_RNNoise.cpp
   )
   
   target_compile_definitions(${CMAKE_PROJECT_NAME} PRIVATE USE_WEIGHTS_FILE)
   
   target_link_libraries(${CMAKE_PROJECT_NAME}
           # List libraries link to the target library
           android
           log)
   ```
9. Enter the following commands into the command line for building the shared object (.so) files.
   * ``./gradlew clean``
   * ``./gradlew build``
10. Right-click on the project name ``rnnoise_canceller`` then  select ``New -> Module -> Android Library``.
11. Provide the following information
   ```
   Module name: rnnoise
   Package name: com.example.rnnoise
   Language: Java
   Minimum SDK: API 21 ("Lollipop"; Android 5.0)
   Build configuration language: Kotlin DSL (build.gradle.kts) [Recommended]
   ```
12. Move ``rnnoise_canceller\app\src\main\cpp`` to ``rnnoise_canceller\rnnoise\src\main``.
13. Add the following code block into ``andriod {}`` of the file ``build.gradle.kts (:rnnoise)``
    ```
        externalNativeBuild {
        cmake {
            path = file("src/main/cpp/CMakeLists.txt")
            version = "3.22.1"
        }
    }
    ```
14. In the following folder ``rnnoise_canceller\rnnoise\src\main\java`` place the specified files in the provided locations.
    * ``com\reve\audio\libraries\dlls\audio_filters\rnnoise\RNNoise.java``
    * ``com\reve\util\ShortArrayUtil.java``
    * ``com\reve\test\RNNoiseTest.java``
15. Create a ``assets`` folder and place the model file. ``(Right-click on main -> New -> Folder -> Assets Folder)``
   * ``rnnoise_canceller\rnnoise\src\main\assets\weights_blob_v1.bin``
16. Enter the following commands into the command line for building the shared object (.so) files.
   * ``./gradlew clean``
   * ``./gradlew build``
17. Create another ``assets`` folder and place a sample audio file in the provided location.
   * ``rnnoise_canceller\app\src\main\assets\sample_48k.raw``
18. Place the ``Test`` file for checking performance of the RNNoise canceller in the following location.
   * ``rnnoise_canceller\app\src\main\java\com\example\rnnoise\Test.java``
19. Add the following code into MainActivity.java and run the app.
   ```
   package com.example.rnnoise_canceller;
   
   import androidx.appcompat.app.AppCompatActivity;
   
   import android.content.Context;
   import android.os.Bundle;
   import android.widget.TextView;
   
   import com.example.rnnoise_canceller.databinding.ActivityMainBinding;
   
   public class MainActivity extends AppCompatActivity {
       private ActivityMainBinding binding;
   
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
   
           binding = ActivityMainBinding.inflate(getLayoutInflater());
           setContentView(binding.getRoot());
   
           Context context = this;
           Test test = new Test(context, "sample_48k.raw");
           test.applyTest();
   
           TextView tv = binding.sampleText;
           tv.setText("RNNoise for Android testing done.");
       }
   
       /**
        * A native method that is implemented by the 'rnnoise_canceller' native library,
        * which is packaged with this application.
        */
   }
   ```
  * Note: Modify sample rate based on your audio file in the Test.java class.
20. The processed audio file will be generated in the following location.
    * ``Device Explorer -> sdcard -> Android -> data -> com.example.rnnoise_canceller -> files -> processed_audio.raw``
         

---


**Prepared by**<br>
*Jakir Hasan (Reve Systems'25)*<br>
*Date (creation) - 10/03/25*<br>
*Date (last modification) - 11/03/25*<br>


**Supervised by**<br>
*Md. Maniruzzaman Monir*<br>
*Dhiman Paul*<br>
*Md. Atikul Hassan Auve*<br>
*Nafiul Alam Fuji*<br>
   

