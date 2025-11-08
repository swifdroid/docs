# JitPack Distribution

This guide explains how to distribute your Android library via JitPack, making it easy for others to include it as a Gradle dependency.

!!! note ""
    JitPack is the easiest way to distribute your Android library.  
    It builds your project directly from a public GitHub repository and hosts the artifacts for you.

## Publishing

### Project Configuration

Open your library project's `build.gradle.kts`.

Add the `maven-publish` plugin at the top:
```kotlin
plugins {
    id("com.android.library")
    id("maven-publish")
}
```

Configure the `release` build type to disable code minification:
```kotlin
android {
    
    buildTypes {
        release {
            isMinifyEnabled = false
        }
    }
    
}
```

Set up the publishing block to create a Maven publication for the `release` variant:
```kotlin
android {
    
    publishing {
        singleVariant("release") {
            withSourcesJar()
        }
    }
}
```

Add the following publishing configuration at the bottom of the file:
```kotlin
publishing {
    publications {
        afterEvaluate {
        val releaseComponent = components.findByName("release")
        if (releaseComponent != null) {
            create<MavenPublication>("release") {
                from(releaseComponent)

                // Replace with your GitHub username
                groupId = "com.github.username"
                // Replace with your repository name
                artifactId = "repository"
                // Replace with your release tag
                version = "1.0.0"

                pom {
                    // Replace with your library details
                    name.set("My Swift Android Library")
                    description.set("An Android library written in Swift.")
                    url.set("https://github.com/username/repository")

                    licenses {
                        license {
                            // Replace with your chosen license
                            name.set("MIT License")
                            url.set("http://www.opensource.org/licenses/mit-license.php")
                        }
                    }

                    developers {
                        developer {
                            // Replace with your details
                            id.set("imike")
                            name.set("Mikhail Isaev")
                            email.set("imike@swift.stream")
                        }
                    }

                    scm {
                        // Replace with your repository
                        connection.set("scm:git:https://github.com/username/repository.git")
                        developerConnection.set("scm:git:ssh://git@github.com/username/repository.git")
                        url.set("https://github.com/username/repository")
                    }
                }
            }
        }
    }
}
```

### Push to GitHub

JitPack builds your project directly from a public GitHub repository. Make sure to push all your changes to GitHub.

### Tag a Release

JitPack uses Git tags to identify versions of your library. 

Tag your last commit with a version number e.g. `1.0.0`.

```bash
git tag v1.0.0
git push origin v1.0.0
```

### Checking the Release

Go to [JitPack.io](https://jitpack.io/), enter your repository URL, and click "Look up".

Find your tagged release in the list, the log icon should be green.

**If it is red**, click on it to see the build logs and fix any issues. Then, re-tag and push again.

**If it is green**, click on "Get it" to see the dependency instructions.

## Using the Library

### Add JitPack
In your Android project's `settings.gradle.kts`, add JitPack as a repository:
```kotlin
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        maven { url = uri("https://jitpack.io") }
        mavenCentral()
    }
}
```

### Add Dependency
In your app module's `build.gradle.kts` file (`app/build.gradle.kts`), add the dependency for your library:
```kotlin
dependencies {
    implementation("com.github.username:repository:tag")
}
```

Replace `username`, `repository`, and `tag` with your GitHub username, repository name, and the release tag you created.

If your GitHub username is `myuser`, your repository is `myrepo`, and your release tag is `v1.0.0`, your dependency would look like this:
```kotlin
dependencies {
    implementation("com.github.myuser:myrepo:v1.0.0")
}
```

!!! success ""
    Sync Gradle, and you should be able to use your Swift-based Android library in your project!