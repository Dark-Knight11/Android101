# Retrofit

## Dependencies

```Java
// retrofit
implementation 'com.squareup.retrofit2:retrofit:2.9.0'
// json converter (Here I have used GSON but you can use others as well)
implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
```

## Initialisation

Inside manifest

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.example.appName">

    <uses-permission android:name="android.permission.INTERNET" />

    <application

    ...
```

```Java
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("base-url-of-API")
    .addConverterFactory(GsonConverterFactory.create())
    .build();
APIclass apiclass = retrofit.create(APIclass.class);
```

## Usage

```Java
apiClass.getData().enqueue(new Callback<ResponseClass>() {
        @Override
        public void onResponse(Call<ResponseClass> call, Response<ResponseClass> response) {
            res = response.body();
        }

        @Override
        public void onFailure(@NonNull Call<ResponseClass> call, @NonNull Throwable t) {
            Log.i("onFailure: ", t.getMessage());
        }
    });
```

## Example

I'll explain this using two different examples one with [TMDB API](https://developers.themoviedb.org/3) and another with [Harry Potter API](https://hp-api.herokuapp.com/)
