# Internal Storage

## Declaration

```
ContextWrapper cw;
```

## Initialization

This is an important step.  
Remember to initialize this variables inside onCreate.  
It would give error if you try to assign them outside onCreate

```Java
// Inside Fragment
public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {

    cw = new ContextWrapper(getActivity());
    ...
}    

// Inside Activity
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity);
    
    cw = new ContextWrapper(getApplicationContext());
    ...
} 
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
