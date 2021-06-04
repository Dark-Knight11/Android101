# Email-Password Authentication

## Sign In

Get the user input through input fields in String emailId, and password.  
You can either create a separate function Sign_In or directly run this code inside set Onclick listener.  

Firebase method signInWithEmailAndPassword(email, password) is used to sign in user.  
where FirebaseAuth mAuth = FirebaseAuth.getInstance();  
We check the result of SignIn process through addOnCompleteListener, it returns task.  
if task is successfull then user is logged in   
if it is not successfull then we can check for errors, if exception message contains string "The password is invalid" then password was wrong. If it contains string "There is no user record corresponding to this identifier" then that email is not registered before.  


```Java
private void Sign_In() {
    mAuth.signInWithEmailAndPassword(emailId, password).addOnCompleteListener(this, task -> {
        if (task.isSuccessful()) {
            Toast.makeText(SignIn.this, "You are logged in", Toast.LENGTH_SHORT).show();
            startActivity(new Intent(SignIn.this, MainActivity.class));
            finish();
        } else {
            Log.w("TAG", "signInUserWithEmail:failure", task.getException());
            if (task.getException().toString().contains("The password is invalid"))
                Toast.makeText(SignIn.this, "Wrong Password", Toast.LENGTH_SHORT).show();
            else if (task.getException().toString().contains("There is no user record corresponding to this identifier"))
                Toast.makeText(SignIn.this, "User does not exist. Please create new account", Toast.LENGTH_SHORT).show();
        }
    });
}
```
