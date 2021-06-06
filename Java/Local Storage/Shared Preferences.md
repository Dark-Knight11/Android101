# SharedPreferences

## Index
* [Declaration](#declaration)  
* [Initalization](#important-initialization)  
* [Save Data](#saving-data)
* [Read Data](#read-data)  
* [Store Image](#storing-image)  
* [Get Image](#fetching-image)

## Declaration

```Java
SharedPreferences sharedPreferences;
SharedPreferences.Editor editor;
ContextWrapper cw;
```

## Important Initialization

This is an important step. Remember to initialize these variables inside onCreate.   
It would give error if you try to declare them outside onCreate

```Java
// Inside Fragment
public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
    sharedPreferences = getActivity().getSharedPreferences(file-Name, Context.MODE_PRIVATE);
    editor = sharedPreferences.edit();
    cw = new ContextWrapper(getActivity());
    ...
}    

// Inside Activity
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity);
    
    sharedPreferences = getSharedPreferences(file-Name, Context.MODE_PRIVATE);
    editor = sharedPreferences.edit();
    cw = new ContextWrapper(getApplicationContext());
    ...
}    
```

## Saving Data

```Java
// editor.putaDataType(key, value);
editor.putString("name", name);
editor.putString("phone", phone);
editor.commit();
```

## Read Data
```Java
// sharedPreferences.getDataType(key, defaultvalue);
String userName = sharedPreferences.getString("name", "Vedant");
String phoneNo = sharedPreferences.getString("phone", "9999999999");
String emailId = sharedPreferences.getString("email", "default@test.com");
```

## Storing Image
```Java
private void StoreImage() {
    Bitmap bitmap = ((BitmapDrawable) pfp.getDrawable()).getBitmap();   // image in bitmap
    File directory = cw.getDir("Dir-Name", Context.MODE_PRIVATE);
    File mypath = new File(directory,"image-file-name");
    FileOutputStream fos;
    try {
        fos = new FileOutputStream(mypath);
        bitmap.compress(Bitmap.CompressFormat.JPEG, 100, fos);
        fos.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

## Fetching Image
```Java
public void fetchImage() {
    File directory = cw.getDir("Dir-Name", Context.MODE_PRIVATE);
    File filepath  = directory.getAbsoluteFile();
    try {
        File file = new File(filepath, "image-file-name");
        Bitmap bitmap = BitmapFactory.decodeStream(new FileInputStream(file));
        pfp.setImageBitmap(bitmap);     // setting image on ImageView
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    }
}
```

