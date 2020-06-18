# Image chooser and cropper

[![Developer](https://img.shields.io/badge/Developer%20Website-Ethic%20Hadebe-green.svg)](http://ethichadebe.cf/?i=1)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/42eb7b00b93645c0812c045ab26cb3b7)](https://www.codacy.com/app/dvg4000/circle-menu-android?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=Ramotion/circle-menu-android&amp;utm_campaign=Badge_Grade)
[![Donate](https://img.shields.io/badge/Version-1.1.0-blue.svg)](https://paypal.me/Ramotion)

#### With the help of [Yalantis cropping library](https://yalantis.com/?utm_source=github), This library aims to provide the easiest way to choose, crop and display image in android studio. It also handles permmisions requests.

#### Check this [project on Dribbble](https://dribbble.com/shots/2484752-uCrop-Image-Cropping-Library)

<img src="preview.jpg" width="100" height="400">

# Usage

1. Include the library as local library project.

```groovy
allprojects {
	repositories {
		jcenter()
		maven { url "https://jitpack.io" }
	}
}
```

```groovy 
implementation 'com.github.ethichadebe:Upload-image-android:1.1.0' 
```

2. Create ```res/xml/file_paths.xml```

```xml
<?xml version="1.0" encoding="utf-8"?>
<paths>
<external-path
    name="my_project"	
    path="Pictures/"/>		<!-- path where images from camera will be stored -->
</paths>
```

3. AndroidManifest.xml

	* Add permissions
```xml
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```
	* Inside Application, add provider. In ```android:authorities``` write package name
```xml
    <provider
        android:authorities="my_package"
        android:name="androidx.core.content.FileProvider"
        android:exported="false"
        android:grantUriPermissions="true">
        <meta-data
            android:name="android.support.FILE_PROVIDER_PATHS"
            android:resource="@xml/file_paths"/>
    </provider>
```

3. Inside Activity.

```java
    private static final String TAG = "MainActivity";
    private ImageView ivImage;
    private Dialog myDialog;
    private UploadImage uploadImage;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ivImage = findViewById(R.id.ivImage);
        myDialog = new Dialog(this);
        uploadImage = new UploadImage(this, this, getPackageManager(), myDialog, ivImage,
                "www.ethichadebe.co.za.picturechooserandcropper", TAG);

        ivImage.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                uploadImage.start(); //OnClick envoke popup
            }
        });

    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        uploadImage.onActivityResult(getCacheDir(), requestCode, resultCode, data);
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        uploadImage.onRequestPermissionsResult(requestCode, grantResults);
    }
```
5. You may want to add this to your PROGUARD config:

```
-dontwarn com.yalantis.ucrop**
-keep class com.yalantis.ucrop** { *; }
-keep interface com.yalantis.ucrop** { *; }
```

## License

Copyright 2017, Yalantis

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.