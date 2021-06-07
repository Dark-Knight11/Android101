# SharedPreferences

## Declaration

```Java
SharedPreferences sharedPreferences;
SharedPreferences.Editor editor;
```

## Important Initialization

This is an important step. Remember to initialize these variables inside onCreate.   
It would give error if you try to assign them outside onCreate

```Java
// Inside Fragment
public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
    sharedPreferences = getActivity().getSharedPreferences(file-Name, Context.MODE_PRIVATE);
    editor = sharedPreferences.edit();
    ...
}    

// Inside Activity
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity);
    
    sharedPreferences = getSharedPreferences(file-Name, Context.MODE_PRIVATE);
    editor = sharedPreferences.edit();
    ...
}    
```

## Saving Data

```Java
// editor.putDataType(key, value);
editor.putString("name", name);
editor.putString("phone", phone);
editor.putString("email", email);
editor.commit();
```

## Read Data
```Java
// sharedPreferences.getDataType(key, defaultvalue);
String userName = sharedPreferences.getString("name", "Vedant");
String phoneNo = sharedPreferences.getString("phone", "9999999999");
String emailId = sharedPreferences.getString("email", "default@test.com");
```
