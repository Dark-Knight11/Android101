# Email-Password Authentication

FirebaseAuth mAuth = FirebaseAuth.getInstance();

## Sign Up

Get the user input through input fields in String emailId, and password.  
You can either create a separate function Sign_Up or directly run this code inside Onclick listener. 

Firebase method createUserWithEmailAndPassword(email, password) is used to create new user.   
We check the result of SignUp process through addOnCompleteListener, it returns task.  
if task is successfull then new user is created  
if it is not successfull then we can check for errors, if exception message contains string "The email address is already in use by another account" then that account already exists.
```Java
private void Sign_Up() {
    mAuth.createUserWithEmailAndPassword(emailId, password).addOnCompleteListener(this, task -> {
        if (task.isSuccessful())
            Toast.makeText(SignUp.this, "Account Created Successfully", Toast.LENGTH_SHORT).show();
        else {
            Log.w("SignUp", "createUserWithEmail:failure", task.getException());
            if(task.getException().toString().contains("The email address is already in use by another account"))
                Toast.makeText(SignUp.this, "User already exists please log in", Toast.LENGTH_SHORT).show();
        }
    });
}
```    
## Sign In

Get the user input through input fields in String emailId, and password.  
You can either create a separate function Sign_In or directly run this code inside Onclick listener.  

Firebase method signInWithEmailAndPassword(email, password) is used to sign in user.  
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
            Log.w("SignIn", "signInUserWithEmail:failure", task.getException());
            if (task.getException().toString().contains("The password is invalid"))
                Toast.makeText(SignIn.this, "Wrong Password", Toast.LENGTH_SHORT).show();
            else if (task.getException().toString().contains("There is no user record corresponding to this identifier"))
                Toast.makeText(SignIn.this, "User does not exist. Please create new account", Toast.LENGTH_SHORT).show();
        }
    });
}
```

## Email Verification

You can set up email verification while creating new account.  
You can't use this function solely like this though.  
Firebase method sendEmailVerification() is used to send verification mail.  
But as you can see we don't have option to pass email or password that's why you can't use this function on its own.  
It sends mail to the user whose instance is created at that moment. mAuth.getCurrentUser gets the current logged in user or recently created user.  
While creating new account mAuth.getCurrentUser will give null if called outside SignUp.

```Java
private void sendEmail() {
    mAuth.getCurrentUser().sendEmailVerification().addOnCompleteListener(task -> {
        if(task.isSuccessful()) {
            Toast.makeText(SignUp.this, "Please Check your Email for Verification link", Toast.LENGTH_SHORT).show();
        } else {
            Log.w("sendEmail", "sendEmailVerification:failure", task.getException());
            Toast.makeText(SignUp.this, "Email Not Sent", Toast.LENGTH_SHORT).show();
        }
    });
}
```

Therefore this function should be called inside SignUp function 
```Java
private void Sign_Up() {
    mAuth.createUserWithEmailAndPassword(emailId, password).addOnCompleteListener(this, task -> {
        if (task.isSuccessful())
            sendEmail();
        else {
            Log.w("SignUp", "createUserWithEmail:failure", task.getException());
            if(task.getException().toString().contains("The email address is already in use by another account"))
                Toast.makeText(SignUp.this, "User already exists please log in", Toast.LENGTH_SHORT).show();
        }
    });
}
```

## Additional

You can use this for email field validations

```Java
private boolean fieldsValidation() {
    String regex = "^[a-zA-Z0-9_!#$%&'*+/=?`{|}~^.-]+@[a-zA-Z0-9.-]+$";
    Pattern pattern = Pattern.compile(regex);
    Matcher matcher = pattern.matcher(emailId);
    
    // checks if email field is empty
    if (TextUtils.isEmpty(emailId)) {
        email.setError("Email Required");
        email.requestFocus();
        return false;
    }

    // checks if valid email is entered
    if (!matcher.matches()) {
        email.setError("Please enter valid email");
        email.requestFocus();
        return false;
    }

    return true;
}
```
