# Mirrorfly-android-ui-kit-sdk

### UI-KIT SDKs for Android

With CONTUS MirrorFly <a href="https://www.mirrorfly.com/docs/UIKit/android/quick-start-version-2" target="_self">Chat SDK for Android</a>, you can efficiently integrate the desired real-time chat features into a client app.

When it comes to the client-side implementation, you can initialize and configure the uikitsdk with minimal efforts. With the server-side, MirrorFly ensures reliable infra-management services for the chat within the app. This page will let you know how to install the UI-KIT SDK in your app.

> **Note :** If you're looking for the fastest way in action with CONTUS MirrorFly <a href="https://www.mirrorfly.com/chat-api-solution.php" target="_self">Chat SDKs</a>, then you need to build your app on top of our sample version. Simply download the sample app and commence your app development. To download sample app <a href="https://github.com/MirrorFly/-MirrorFly-Android-Sample-V2" target="_blank">click here</a>


### Requirements
The requirements for UI-KIT SDK for Android are:
- Android Marshmallow 6.0 (API Level 23) or above
- Java 8 or higher
- Gradle 4.1.0 or higher

### Integrate the Chat SDK

**Step 1:** Create a new project or Open an existing project in Android Studio

**Step 2:** If using Gradle 6.8 or higher, add the following code to your settings.gradle file. If using Gradle 6.7 or lower, add the following code to your root build.gradle file. See <a href="https://docs.gradle.org/6.8/release-notes.html#dm-features" target="_self">this release note</a> to learn more about updates to Gradle.

```groovy
    dependencyResolutionManagement {
        repositories {
           jcenter()
               maven {
                 url "https://repo.mirrorfly.com/release"
       }
    }}
   ```

**Step 3:** Add the following dependencies in the app/build.gradle file.
   ```groovy
   dependencies {
          implementation 'com.mirrorfly.uikitsdk:mf-uikitsdk:1.0.25'
    }
   ```
   
   **Step 4:** Add the below dependencies required by the SDK in the app module/build.gradle file.
   ```groovy
	buildscript {
 
	dependencies {
     classpath 'org.jetbrains.kotlin:kotlin-gradle-plugin:1.6.21'
     def nav_version = "2.3.5"
     classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
 }

}
   ```

**Step 5:** Add the below line in the gradle.properties file, to avoid imported library conflicts.
   ```
   android.enableJetifier=true
   ```

**Step 6:** Open the AndroidManifest.xml and add below permissions.
   ```xml
   <uses-permission android:name="android.permission.INTERNET" />
   ```


## Initialization

To integrate and run Mirrorfly UIKit in your app, you need to initialize it first. 
You can initialize the MirrorFlyUIKit instance by passing the MirrorFlyUIKitAdapter instance as an argument to a parameter in the MirrorFlyUIKit.init() method. 
The MirrorFlyUIKit.init() must be called once in the onCreate() method of your app’s Application instance.

>Note: While registration, the below `registerUser` method will accept the `FCM_TOKEN` as an optional param and pass it across.

**Step 1:** Add the below line in the application class file.

```kotlin
 package com.example.mfuikittest

import android.app.Application
import com.mirrorflyuikitsdk.MirrorFlyUIKit
import com.mirrorflyuikitsdk.adapter.MirrorFlyUIKitAdapter


class BaseApplication : Application() {

    override fun onCreate() {
        super.onCreate()

        MirrorFlyUIKit.initFlySDK(applicationContext,object : MirrorFlyUIKitAdapter {
           
            override fun setAppName(): String? {
                return "YOUR_APP_NAME"
            }
            
            override fun setApplicationID(): String? {
                return "YOUR_APPLICATION_ID"
            }
            
            //Below override methods are optional used for customization
            
            override fun isCallEnabled(): Boolean? {
                return true
            }

            override fun isGroupEnable(): Boolean? {
                return true
            }

            override fun isContactEnable(): Boolean? {
                return true
            }

            override fun isLogoutEnable(): Boolean? {
                return true
            }

            override fun isOtherProfileEnable(): Boolean? {
                return true
            }

            override fun isOwnProfileEnable(): Boolean? {
                return true
            }

            override fun setGoogleTranslationKey(): String? {
                return getString(R.string.google_key)
            }

            override fun onlyOnetoOneChat(): Boolean? {
                return false
            }
        })
        
        MirrorFlyUIKit.defaultThemeMode = MirrorFlyUIKit.ThemeMode.Light
        MirrorFlyUIKit.loginActivity = "LoginActivity"::class.java
    }
}
```
**Step 2:** Add the below line in the Launcher class file.

```kotlin
 class SplashTestActivity : AppCompatActivity() {
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_splash)
        MirrorFlyUIKit.initializeSDK(this@SplashTestActivity,SplashTestActivity::class.java,"YOUR_LICENCE_KEY",object : FlyInitializeSDKCallback{
            
            override fun flyError(
                isSuccess: Boolean,
                throwable: Throwable?,
                data: HashMap<String, Any>
            ) {
                //TODO Error Handling 
            }

            override fun redirectToDashBoard(isSuccess: Boolean) {
                startActivity(Intent(this@SplashTestActivity, MFUIDemoActivity::class.java))
                finish()
            }

            override fun redirectToLogin(isSuccess: Boolean) {
                startActivity(Intent(this@SplashTestActivity, MainActivity::class.java))
                finish()
            }
        })
        
    }

}
```

## Registration

```kotlin
        MirrorFlyUIKit.initUser("USER_IDENTIFIER", "FIREBASE TOKEN", object : InitResultHandler {
        
        override fun onInitResponse(isSuccess: Boolean, e: String) {
            if (isSuccess) {
                Log.d("TAG", "onInitResponse called with: isSuccess = $isSuccess")
            } else {
                Log.e("TAG", "onInitResponse called with: Failure, e = $e")
            }
        }
    })
```


## Display Recent Chat and Call list#
DashBoardActivity is the starting point for launching UIKit in your application. By implementing the code below, you will see a complete list of recent chats that you're made with single and group conversation.

```kotlin
    import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import com.mirrorflyuikitsdk.activities.DashBoardActivity

class MainActivity : DashBoardActivity() { // Add this line.

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState) 
    // If you’re going to inherit `DashBoardActivity`, don’t implement `setContentView()`
    }
}
```