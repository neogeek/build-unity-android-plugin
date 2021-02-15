# build-unity-android-plugin

> 🔧 Build Android plugins for Unity without needing to setup an Android project.

## Install

```bash
$ brew tap neogeek/build-unity-android-plugin
$ brew install build-unity-android-plugin
```

## Usage

```bash
$ build-unity-android-plugin -p com.scottdoxey.toast AndroidPlugin.java
```

## Help

```
Usage: build-unity-android-plugin [options] <files>

OPTIONS:
   -v     show the version number and then exit
   -h     show this help message and then exit

   -p     set package name in AndroidManifest.xml
   -m     min SDK version in gradle build
   -t     target SDK version in gradle build
```

## Example

### AndroidPlugin.java

```java
package com.scottdoxey.toast;

import android.content.Context;
import android.view.Gravity;
import android.widget.Toast;

public class AndroidPlugin {

    private Context context;

    public AndroidPlugin(Context context) {

        this.context = context;

    }

    public void ToastMakeText(String message) {

        Toast toast = Toast.makeText(context, message, Toast.LENGTH_LONG);
        toast.setGravity(Gravity.TOP|Gravity.START, 0, 0);
        toast.show();

    }

}
```

**Notice:** If you keep the `.java` file in your Plugins folder, be sure to uncheck all platforms in the import settings. Do not do this for the generated `.aar` file, just the `.java` files.

<img src="https://i.imgur.com/kRqdkqz.jpg" width="400">

### AndroidPlugin.cs

```csharp
using UnityEngine;

public class AndroidPlugin : MonoBehaviour
{

    public void ToastMakeText(string message)
    {

        var javaUnityPlayer = new AndroidJavaClass("com.unity3d.player.UnityPlayer");

        var currentActivity = javaUnityPlayer.GetStatic<AndroidJavaObject>("currentActivity");

        var androidPlugin =
            new AndroidJavaObject("com.scottdoxey.toast.AndroidPlugin", currentActivity);

        androidPlugin.Call("ToastMakeText", message);

    }

}
```
