## Webrtc Up and Down sampling library for Android applications

**Goal**: Develop a native library for Android applications to perform up-sampling and down-sampling using the code base of WebRTC.

**Source of native code**: For developing the library we used code from the following link:
https://git.iptelephony.revesoft.com/shaon/webrtc_audio_filters.

Only necessary source and header files are included in the ``CMakeLists.txt`` file of the Android project.

**Source of Instructions**: To develop the library we follow the same approach as described in the following link:
https://git.iptelephony.revesoft.com/jakir57/rnnoise_android_library.

## Publishing the library
For building the library using Maven and publish it using jFrog the following files have to be modified:

**build.gradle.kts (Project: sampling)**
```
// Top-level build file where you can add configuration options common to all sub-projects/modules.
plugins {
    alias(libs.plugins.android.application) apply false
    alias(libs.plugins.android.library) apply false
}

// New start
buildscript {
    dependencies {
        classpath("org.jfrog.buildinfo:build-info-extractor-gradle:4.31.5")
    }
}
// New end
```

**build.gradle.kts (Module: app)**
```
plugins {
    alias(libs.plugins.android.application)
}

android {
    namespace = "com.example.sampling"
    compileSdk = 35

    defaultConfig {
        applicationId = "com.example.sampling"
        minSdk = 24
        targetSdk = 35
        versionCode = 1
        versionName = "1.0"

        testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            isMinifyEnabled = false
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_11
        targetCompatibility = JavaVersion.VERSION_11
    }

    buildFeatures {
        viewBinding = true
        // New start
        buildConfig = true
        // New end
    }
}

dependencies {
//    implementation(project(":sampling"))
    // New start
    implementation(libs.sampling)
    // New end
    implementation(libs.appcompat)
    implementation(libs.material)
    implementation(libs.constraintlayout)
    testImplementation(libs.junit)
    androidTestImplementation(libs.ext.junit)
    androidTestImplementation(libs.espresso.core)
}
```

**build.gradle.kts (Module: sampling)**
```
// New start
import java.util.Date
// New end

plugins {
    alias(libs.plugins.android.library)
    // New start
    id("com.jfrog.artifactory")
    id("maven-publish")
    // New end
}

android {
    namespace = "com.example.sampling"
    compileSdk = 35

    defaultConfig {
        minSdk = 21

        testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
        consumerProguardFiles("consumer-rules.pro")
    }

    buildTypes {
        release {
            isMinifyEnabled = false
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_11
        targetCompatibility = JavaVersion.VERSION_11
    }

    externalNativeBuild {
        cmake {
            path = file("src/main/cpp/CMakeLists.txt")
            version = "3.22.1"
        }
    }
}

dependencies {

    implementation(libs.appcompat)
    implementation(libs.material)
    testImplementation(libs.junit)
    androidTestImplementation(libs.ext.junit)
    androidTestImplementation(libs.espresso.core)
}

// New start
tasks.named("artifactoryPublish") {
    dependsOn("assembleRelease")
}

publishing {
    publications {
        create<MavenPublication>("aar") {
            groupId = "com.revesoft.dialer"
            artifactId = "sampling"
            version = "1.0.0"

            artifact("$buildDir/outputs/aar/${artifactId}-release.aar")

            pom.withXml {
                val dependenciesNode = asNode().appendNode("dependencies")

                // Iterate over the compile dependencies (excluding test dependencies)
                configurations.getByName("api").allDependencies.forEach {
                    val dependencyNode = dependenciesNode.appendNode("dependency")
                    dependencyNode.appendNode("groupId", it.group)
                    dependencyNode.appendNode("artifactId", it.name)
                    dependencyNode.appendNode("version", it.version)
                }
            }
        }
    }
}
artifactory {
    clientConfig.apply {
        isIncludeEnvVars = true
        info.addEnvironmentProperty("test.adding.dynVar", Date().toString())
    }

    val repo_base_url = "https://maven.iptelephony.revesoft.com/artifactory"
    setContextUrl(repo_base_url)
    val artifactory_username = "dhiman"
    val artifactory_password = "AP8PY28CFAyCdjtAEqqzMKRHoPD"
    publish {
        repository {
            setRepoKey("libs-release-local")
            setUsername(artifactory_username)
            setPassword(artifactory_password)
        }
        defaults {
            publications("aar")

            // Corrected syntax for setting publish artifacts
            setPublishArtifacts(true)

            // Corrected syntax for setting POM publishing
            setPublishPom(true)

            // Corrected syntax for defining properties
            setProperties(
                mapOf(
                    "qa.level" to "basic",
                    "q.os" to "android",
                    "dev.team" to "core"
                )
            )
        }
    }
}
// New end
```

**libs.versions.toml**
```
[versions]
#agp = "8.9.0"
#junit = "4.13.2"
#junitVersion = "1.2.1"
#espressoCore = "3.6.1"
#appcompat = "1.7.0"
#material = "1.12.0"
#constraintlayout = "2.2.1"

agp = "8.9.0"
junit = "4.13.2"
junitVersion = "1.1.5"
espressoCore = "3.5.1"
appcompat = "1.6.1"
material = "1.10.0"
constraintlayout = "2.1.4"

## New start
jfrog-build-info = "4.31.5"
sampling = "1.0.0"
## New end

[libraries]
junit = { group = "junit", name = "junit", version.ref = "junit" }
ext-junit = { group = "androidx.test.ext", name = "junit", version.ref = "junitVersion" }
espresso-core = { group = "androidx.test.espresso", name = "espresso-core", version.ref = "espressoCore" }
appcompat = { group = "androidx.appcompat", name = "appcompat", version.ref = "appcompat" }
material = { group = "com.google.android.material", name = "material", version.ref = "material" }
constraintlayout = { group = "androidx.constraintlayout", name = "constraintlayout", version.ref = "constraintlayout" }
## New start
sampling = { module = "com.revesoft.dialer:sampling", version.ref = "sampling" }
## New end

[plugins]
android-application = { id = "com.android.application", version.ref = "agp" }
android-library = { id = "com.android.library", version.ref = "agp" }
## New start
jfrog-build-info = { id = "org.jfrog.buildinfo", version.ref = "jfrog-build-info" }
## New end
```

**settings.gradle.kts**
```
pluginManagement {
    repositories {
        google {
            content {
                includeGroupByRegex("com\\.android.*")
                includeGroupByRegex("com\\.google.*")
                includeGroupByRegex("androidx.*")
            }
        }
        mavenCentral()
        gradlePluginPortal()
    }
}
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        // New start
        maven("https://jitpack.io")
        maven("https://maven.google.com/")
        maven("https://maven.iptelephony.revesoft.com/artifactory/libs-release-local")
        // New end
    }
}

rootProject.name = "sampling"
include(":app")
include(":sampling")
```

## Building artifacts
1. Click on gradle on the right side of the screen
2. Select sampling -> sampling -> Tasks -> publishing -> artifactoryPublish

The library will be uploaded in the specified path. 


**Prepared by**<br>
*Jakir Hasan (Reve Systems'25)*<br>
*Date (creation) - 08/04/25*<br>
*Date (last modification) - 09/04/25*<br>


**Supervised by**<br>
*Md. Maniruzzaman Monir*<br>
*Sourav Das*<br>
