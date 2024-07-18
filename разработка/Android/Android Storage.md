часть [[Android]] отвечающая за хранение данных приложения на устройстве

#### App-specific storage

This is where you store files that are meant exclusively for your app’s use. You can choose between dedicated directories within the device’s internal storage or dedicated directories within external storage

#### Shared storage
If your app needs to share files with other apps, such as media, documents, or other files, you can store them in shared storage. This allows other apps to access and use those files

#### **Scoped Storage**


- App-specific files (files removes on app unistall)
    - Internal Storage
        - context.filesDir - File(it, "text.txt") data/data/package.name/files/...
        - context.cacheDir
    - external Storage (need permission if API < 20)
        - externalFileDir
        - externalCacheDir
        - externalMediaDirs
- Non app-specific files (need permission)
    - `Enviroment.getDownloadCacheDirectory()`
    - `Enviroment.getRootDirectory()`
    - `Enviroment.getDataDirectory()`
- Media (need permission)
    - MediaStore API
- Documents
    - Storage Access Framework
- ScopeStorage

Extensions
`File.readText()`
`File.readBytes()`

проверка на external

- `if(Environment.getExternalStorageState == Environment.MEDIA_MOUNTED)`

https://youtu.be/wuh7KL6pT4M
