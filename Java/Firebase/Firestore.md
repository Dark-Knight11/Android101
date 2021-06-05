# Cloud FireStore Database

## Index 
* [Dependencies](#dependencies) 
* [Generating FireStore instance](#initialization)
* [Write Data](#write-data)  


## Dependencies

```Java
dependencies {
    // Import the BoM for the Firebase platform
    implementation platform('com.google.firebase:firebase-bom:28.0.1')

    // Declare the dependency for the Cloud Firestore library
    // When using the BoM, you don't specify versions in Firebase library dependencies
    implementation 'com.google.firebase:firebase-firestore'
}
```

## Initialization

```Java 
FirebaseFirestore db = FirebaseFirestore.getInstance();
```

## Write Data
Add and Set Method
Add method is used when you don't want to define specific Id for document. 
Add method generates some random unique Id whenever you add data
Set method is used when you want to store data under specific unique Id  
In set method you have to give the value of unique Id  

Sample Example
```Java
Map<String, Object> user = new HashMap<>();
user.put("first", "Ada");
user.put("last", "Lovelace");
user.put("born", 1815);

// Add a new document with a generated ID
db.collection("users")
    .add(user)
    .addOnSuccessListener(documentReference -> Log.d("writeData", "DocumentSnapshot added with ID: " + documentReference.getId()))
    .addOnFailureListener(e -> Log.w("writeData", "Error adding document", e));
        
```
Here I'm using users uid generated during Authentication as unique Id for document
```Java
private writeDB() {
    Map<String, Object> userData = new HashMap<>();
    userData.put("phone", phone);
    userData.put("Name", Name);
    userData.put("Email", emailId);

    db.collection("Collection-Name")
        .document((mAuth.getCurrentUser()).getUid())
        .set(userData)
        .addOnSuccessListener(v -> Log.d("writeData", "DocumentSnapshot added with ID: " + (mAuth.getCurrentUser()).getUid()))
        .addOnFailureListener(e -> Log.w("writeData", "Error adding document", e));
}
```                
              
