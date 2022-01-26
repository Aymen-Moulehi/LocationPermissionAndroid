# Runtime Permissions

In Android 6.0 and higher, the Android application permissions model is designed to make permissions more understandable, useful, and secure for users. The model moved Android applications that require dangerous permissions (see [Affected permissions](https://source.android.com/devices/tech/config/runtime_perms#affected-permissions)) from an _install-time_ permission model to a _runtime_ permission model:

-   **Install-time permissions**
    
    (_Android 5.1 and lower_) Users grant [dangerous permissions](https://developer.android.com/guide/topics/permissions/overview#dangerous_permissions) to an app when they install or update the app. Device manufacturers and carriers can preinstall apps with pregranted permissions without notifying the user.
    
-   **Runtime permissions**
    
    (_Android 6.0 – 9_) Users grant dangerous permissions to an app when the app is running. When permissions are requested (such as when the app launches or when the user accesses a specific feature) depends on the application, but the user grants/denies application access to specific permission groups. OEMs/carriers can preinstall apps, but can’t pregrant permissions unless they go through the exception process. (See [Creating exceptions](https://source.android.com/devices/tech/config/runtime_perms#creating-exceptions).)
    
    (_Android 10_) Users see increased transparency and have control over which apps have activity recognition (AR) runtime permissions. Users are prompted by the [runtime permissions dialog](https://source.android.com/devices/tech/config/tristate-perms#tristate-screen) to either always allow, allow while in use, or deny permissions. On an OS upgrade to Android 10, permissions given to apps are retained, but users can go into **Settings** and change them.

---------------------

## AndroidManifest.xml

      <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />  
       <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />



## Interface

    <?xml version="1.0" encoding="utf-8"?>  
    <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"  
      xmlns:app="http://schemas.android.com/apk/res-auto"  
      xmlns:tools="http://schemas.android.com/tools"  
      android:layout_width="match_parent"  
      android:layout_height="match_parent"  
      tools:context=".MainActivity">  
      
      
     <ImageView  android:id="@+id/loc"  
      android:layout_width="100dp"  
      android:layout_height="100dp"  
      android:src="@drawable/ic_baseline_location_on_24"  
      app:layout_constraintLeft_toLeftOf="parent"  
      app:layout_constraintRight_toRightOf="parent"  
      app:layout_constraintTop_toTopOf="parent"  
      app:layout_constraintBottom_toBottomOf="parent"/>  
      
    </androidx.constraintlayout.widget.ConstraintLayout>

## JAVA

    package com.example.requestpermission;  
      
    import androidx.annotation.NonNull;  
    import androidx.appcompat.app.AppCompatActivity;  
      
    import android.Manifest;  
    import android.app.Activity;  
    import android.content.pm.PackageManager;  
    import android.os.Build;  
    import android.os.Bundle;  
    import android.view.View;  
    import android.widget.ImageView;  
    import android.widget.Toast;  
      
    public class MainActivity extends AppCompatActivity {  
        ImageView loc ;  
      @Override  
      protected void onCreate(Bundle savedInstanceState) {  
            super.onCreate(savedInstanceState);  
      setContentView(R.layout.activity_main);  
      loc = findViewById(R.id.loc) ;  
      
      
      loc.setOnClickListener(new View.OnClickListener() {  
                @Override  
      public void onClick(View view) {  
                    reqLocPerm();  
      }  
            });  
      }  
      
        public void reqLocPerm(){  
            if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.M && checkSelfPermission(Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED){  
                requestPermissions(new String[] {Manifest.permission.ACCESS_FINE_LOCATION},1200);  
      }  
        }  
      
        @Override  
      public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {  
            super.onRequestPermissionsResult(requestCode, permissions, grantResults);  
     if (requestCode == 1200 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {  
                Toast.makeText(this, "ok", Toast.LENGTH_SHORT).show();  
      } else {  
                moveTaskToBack(true);  
      android.os.Process.killProcess(android.os.Process.myPid());  
      System.exit(1);  
      }

