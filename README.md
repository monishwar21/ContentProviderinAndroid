
# Ex.No:4 Create Your Own Content Providers to get Contacts details.


## AIM:

To create your own content providers to get contacts details using Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min. required Artic Fox)

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as “ex.no.3″ and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Get contacts details and Display details give in MainActivity file.

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the text create your own content providers to get contacts details.
Developed by: K MONISHWAR
Registeration Number : 212225230188
*/
```
##activity_main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<ScrollView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fillViewport="true">

    <LinearLayout
        android:id="@+id/main"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="#8DBCEB"
        android:orientation="vertical"
        android:padding="24dp">

        <!-- Title -->
        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:layout_marginBottom="24dp"
            android:gravity="center"
            android:text="@string/title_contact_provider"
            android:textSize="24sp"
            android:textStyle="bold" />

        <!-- Name Input -->
        <EditText
            android:id="@+id/etName"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="16dp"
            android:autofillHints="name"
            android:hint="@string/hint_enter_name"
            android:inputType="textPersonName"
            android:minHeight="48dp"
            android:padding="14dp" />

        <!-- Phone Input -->
        <EditText
            android:id="@+id/etPhone"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="28dp"
            android:autofillHints="phone"
            android:hint="@string/hint_enter_phone_number"
            android:inputType="phone"
            android:minHeight="48dp"
            android:padding="14dp" />

        <!-- Action Buttons Container -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="40dp"
            android:orientation="horizontal">

            <Button
                android:id="@+id/btnAdd"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginEnd="10dp"
                android:layout_weight="1"
                android:backgroundTint="#000000"
                android:paddingVertical="12dp"
                android:text="@string/btn_add_contact" />

            <Button
                android:id="@+id/btnView"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="10dp"
                android:layout_weight="1"
                android:backgroundTint="#161516"
                android:paddingVertical="12dp"
                android:text="@string/btn_view_contacts" />

        </LinearLayout>

        <!-- Saved Contacts Section Header -->
        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="12dp"
            android:text="@string/title_saved_contacts"
            android:textSize="18sp"
            android:textStyle="bold" />

        <!-- Display Contacts -->
        <TextView
            android:id="@+id/tvResult"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:lineSpacingExtra="4dp"
            android:minHeight="120dp"
            android:padding="16dp"
            android:textSize="16sp" />

    </LinearLayout>

</ScrollView>

```
##MainActivity.java
```
package com.example.contact;

import android.app.Activity;
import android.content.ContentValues;
import android.database.Cursor;
import android.os.Bundle;
import android.text.TextUtils;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends Activity {

    private EditText etName, etPhone;
    private Button btnAdd, btnView;
    private TextView tvResult;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        setContentView(R.layout.activity_main);

        etName = findViewById(R.id.etName);
        etPhone = findViewById(R.id.etPhone);
        btnAdd = findViewById(R.id.btnAdd);
        btnView = findViewById(R.id.btnView);
        tvResult = findViewById(R.id.tvResult);

        // Add Contact
        btnAdd.setOnClickListener(v -> {
            String name = etName.getText().toString().trim();
            String phone = etPhone.getText().toString().trim();

            if (TextUtils.isEmpty(name) || TextUtils.isEmpty(phone)) {
                Toast.makeText(MainActivity.this, "Please fill in all fields", Toast.LENGTH_SHORT).show();
                return;
            }

            ContentValues values = new ContentValues();
            values.put(DatabaseHelper.NAME, name);
            values.put(DatabaseHelper.PHONE, phone);

            getContentResolver().insert(
                    ContactProvider.CONTENT_URI,
                    values
            );

            Toast.makeText(
                    MainActivity.this,
                    "Contact Added Successfully",
                    Toast.LENGTH_SHORT
            ).show();

            etName.setText("");
            etPhone.setText("");
        });

        // View Contacts
        btnView.setOnClickListener(v -> {
            Cursor cursor = getContentResolver().query(
                    ContactProvider.CONTENT_URI,
                    null,
                    null,
                    null,
                    null
            );

            StringBuilder result = new StringBuilder();

            if (cursor != null) {
                if (cursor.getCount() == 0) {
                    result.append("No contacts found.");
                } else {
                    while (cursor.moveToNext()) {
                        String name = cursor.getString(
                                cursor.getColumnIndexOrThrow(
                                        DatabaseHelper.NAME
                                )
                        );

                        String phone = cursor.getString(
                                cursor.getColumnIndexOrThrow(
                                        DatabaseHelper.PHONE
                                )
                        );

                        result.append("Name: ").append(name).append("\n");
                        result.append("Phone: ").append(phone).append("\n\n");
                    }
                }
                cursor.close();
            } else {
                result.append("No contacts found.");
            }

            tvResult.setText(result.toString().trim());
        });
    }
}

```
## AndroidManifest.xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Contact">
        <provider
            android:name=".ContactProvider"
            android:authorities="com.example.contact.ContactProvider"
            android:exported="false" />
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:windowSoftInputMode="adjustResize">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>

```

## OUTPUT


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5fc9378f-839e-48d6-83d5-5e679ad41ce9" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7672f06f-d4fb-4d4a-87be-1eeb132010f5" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/39abcf9c-5bad-4ce8-a3fe-29b664b181bf" />



## RESULT
Thus a Simple Android Application create your own content providers to get contacts details using Android Studio is developed and executed successfully.
