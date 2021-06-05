# Firebase Realtime Database

## Index
* [Dependencies](#dependencies)
* [Write Data](#writing-data-to-realtime-db)
* [Read Data](#reading-data-from-realtime-db)
* [Update Data](#updating-data-on-realtime-db)

## Dependencies
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
## Writing data to Realtime DB

Sample Example
```Java
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
    public String Email, Name, phone;

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
        .getReference("Parent")      // (String value) parent will be created if it does not exist
        .child("Child")              // (String value) (optional) if you want to create a child element
        .setValue(user)              // setValue method actually sets data
        .addOnCompleteListener(task -> {
            if(task.isSuccessful())
                Toast.makeText(SignUp.this, "Data was Uploaded successfully", Toast.LENGTH_SHORT).show();
            else
                Log.w("uploadData", "saveUserDetails:failure", task.getException());
        });
}
```
## Reading Data from Realtime DB
We'll require same User class as defined above for fetching data.
Declare the variables of whose data you wish to fetch in that User class.

```Java
public void readDB() {
    ValueEventListener postListener = new ValueEventListener() {
        @Override
        public void onDataChange(DataSnapshot dataSnapshot) {
            if(dataSnapshot.exists()) {
                User user = dataSnapshot.getValue(User.class);
                nameF.setText(user.Name);
                phoneF.setText(user.phone);
                emailF.setText(user.Email);
            } else
                Toast.makeText(Profile.this, "Data not present", Toast.LENGTH_SHORT).show();
        }
        @Override
        public void onCancelled(DatabaseError databaseError) {
            Log.w("readDB", "loadPost:onCancelled", databaseError.toException());
        }
    };
    mDatabase.child("Parent").child("child").addValueEventListener(postListener);
}
```

## Updating Data on Realtime DB
For updating data we just use a hashmap
`childUpdates.put("key", value);`
```Java
private void updateDB() {
    Map<String, Object> childUpdates = new HashMap<>();
    childUpdates.put("phone", phone);
    childUpdates.put("Name", name);
    childUpdates.put("Email", email);
    mDatabase.child("parent").child("child").updateChildren(childUpdates);
    Toast.makeText(Profile.this, "Data was successfull updated", Toast.LENGTH_SHORT).show();
}
```    
