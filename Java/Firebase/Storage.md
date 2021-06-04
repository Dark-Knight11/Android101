# Firebase Realtime Database

Add dependencies in app-level build.gradle (app/build.gradle)
```Gradle
dependencies {
    // Import the BoM for the Firebase platform
    // Bills of Material (BoM)
    implementation platform('com.google.firebase:firebase-bom:28.1.0')

    // Declare the dependency for the Realtime Database library
    // When using the BoM, you don't specify versions in Firebase library dependencies
    implementation 'com.google.firebase:firebase-database'
}
```
## Uploading data to Realtime DB

Sample Example
```
// Write a message to the database
FirebaseDatabase database = FirebaseDatabase.getInstance();
DatabaseReference myRef = database.getReference("message");

myRef.setValue("Hello, World!");
```
IRL usage   
Create a separate User class for setting User details

```Java
public class User {
    // declare data that you want to upload
    public String Email, Name;

    public User() { }

    // Constructor
    public User(String Email, String Name) {
        this.Email = Email;
        this.Name = Name;
    }
}
```
Create this function from where you want to upload data 
```Java
private uploadData(String emailId, String Name) {
    // create new object and pass the data that you want to upload 
    User user = new User(emailId, Name);
    FirebaseDatabase.getInstance()
        .getReference("Parent-Folder")      // (String value) parent folder will be created if it does not exist
        .child("Child-Folder")              // (String value) (optional) if you want to create a sub folder
        .setValue(user)                     // setValue method actually sets data
        .addOnCompleteListener(task -> {
            if(task.isSuccessful())
                Toast.makeText(SignUp.this, "Data was Uploaded successfully", Toast.LENGTH_SHORT).show();
            else
                Log.w("uploadData", "saveUserDetails:failure", task.getException());
        });
}
```
