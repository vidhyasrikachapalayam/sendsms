
# Ex.No:5 Create Your Own Content Providers to get Contacts details.


## AIM:

To create your own content providers to get contacts details using Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Latest Version)

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as “contentprovider″ and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Get contacts details and Display details give in MainActivity file.

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the contact name and phone number using content providers.
Developed by:Vidhyasri K
Registeration Number : 212222230170
*/
```
## Activity_main.xml:
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="20dp">

    <Button
        android:id="@+id/btnGetContacts"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:backgroundTint="@android:color/holo_purple"
        android:text="GET CONTACTS"
        android:textColor="@android:color/white" />

    <TextView
        android:id="@+id/tvContacts"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:textColor="@android:color/black"
        android:textSize="16sp" />

</LinearLayout>
```
## MainActivity.java:
```
package com.example.contentproviderdemo;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

import android.Manifest;
import android.content.pm.PackageManager;
import android.database.Cursor;
import android.os.Bundle;
import android.provider.ContactsContract;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    private static final int REQUEST_CONTACT_PERMISSION = 1;

    Button btnGetContacts;
    TextView tvContacts;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnGetContacts = findViewById(R.id.btnGetContacts);
        tvContacts = findViewById(R.id.tvContacts);

        btnGetContacts.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (ContextCompat.checkSelfPermission(MainActivity.this,
                        Manifest.permission.READ_CONTACTS) != PackageManager.PERMISSION_GRANTED) {
                    ActivityCompat.requestPermissions(MainActivity.this,
                            new String[]{Manifest.permission.READ_CONTACTS}, REQUEST_CONTACT_PERMISSION);
                } else {
                    getContacts();
                }
            }
        });
    }

    private void getContacts() {
        StringBuilder builder = new StringBuilder();

        Cursor cursor = getContentResolver().query(
                ContactsContract.CommonDataKinds.Phone.CONTENT_URI,
                null, null, null, null);

        if (cursor != null) {
            while (cursor.moveToNext()) {
                String name = cursor.getString(
                        cursor.getColumnIndexOrThrow(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME));
                String phone = cursor.getString(
                        cursor.getColumnIndexOrThrow(ContactsContract.CommonDataKinds.Phone.NUMBER));
                builder.append("Name: ").append(name).append("\nPhone: ").append(phone).append("\n\n");
            }
            cursor.close();
        }

        tvContacts.setText(builder.toString());
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);

        if (requestCode == REQUEST_CONTACT_PERMISSION) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                getContacts();
            } else {
                Toast.makeText(this, "Permission Denied!", Toast.LENGTH_SHORT).show();
            }
        }
    }
}
```
## AndroidManifest.xml:
```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.contentproviderdemo">

    <uses-permission android:name="android.permission.READ_CONTACTS" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.ContentproviderDemo">
        <activity
            android:name=".MainActivity"
            android:exported="true">   <!-- 👈 Add this line -->
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>

</manifest>
```
## OUTPUT
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/29e130f7-72c7-447b-8cb3-48b3ef258f18" />
## RESULT
Thus a Simple Android Application create your own content providers to get contacts details using Android Studio is developed and executed successfully.

