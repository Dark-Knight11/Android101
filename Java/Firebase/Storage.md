# Firebase Storage

## Index

* [Dependencies](#dependencies)  
* [Generating Storage Instance](#initialization)  
* [Uploading Files](#upload-files)
* [Download Files](#download-files)

## Dependencies
```Java
dependencies {
    // Import the BoM for the Firebase platform
    implementation platform('com.google.firebase:firebase-bom:28.1.0')

    // Declare the dependency for the Cloud Storage library
    // When using the BoM, you don't specify versions in Firebase library dependencies
    implementation 'com.google.firebase:firebase-storage'
}
```

## Initialization

```Java
FirebaseStorage storage = FirebaseStorage.getInstance();
StorageReference storageRef = storage.getReference();
```

## Upload Files
Uploading an Image from local file.  
We'll receive image URI 

```Java
private void upload() {
    if(resultUri!=null) {
        StorageReference pathReference = storageRef.child("images/" + fileName);
        pathReference.putFile(resultUri)
            .addOnSuccessListener(taskSnapshot -> Toast.makeText(getContext(), "Image was uploaded", Toast.LENGTH_SHORT).show())
            .addOnFailureListener(e -> Toast.makeText(getContext(), e.getMessage(), Toast.LENGTH_SHORT).show());
    } else
        Toast.makeText(getContext(), "Please Select an Image", Toast.LENGTH_SHORT).show();
    resultUri = null;
}
```

## Download Files
Downloading an Image of max size 4mb
```Java
public void download() {
    StorageReference pathReference = storageRef.child("images/" + fileName);
    long MAXBYTES = 4096*4096;
    pathReference.getBytes(MAXBYTES)
        .addOnSuccessListener(bytes -> {
            Log.i("TAG", "firebaseImageDwl: ");
            Bitmap bitmap = BitmapFactory.decodeByteArray(bytes, 0, bytes.length);
            pfp.setImageBitmap(bitmap);
        })
        .addOnFailureListener(Throwable::printStackTrace);
}
```  
