# Simple Splash Screen

Create an empty activity `SplashScreen.java`  
In its layout file design your splash screen.  
In `SplashScreen.java` file write this code.

```Java
public class SplashScreen extends AppCompatActivity {
    private static int SPLASH_TIME_OUT = 3000   // 3 seconds

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.splash_screen);

        new Handler().postDelayed(new Runnable() {
            @Override
            public void run() {
                startActivity(new Intent(SplashScreen.this, MainActivity.class));
                finish();
            }
        }, SPLASH_TIME_OUT);
    }
}
```

Inside Manifest file

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.example.app" >

    <application

        ...

        <activity android:name=".SplashScreen">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity android:name=".MainActivity" />

        ...

    </application>
</manifest>
```
