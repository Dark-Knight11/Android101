# Retrofit

## Dependencies

```Java
// retrofit
implementation 'com.squareup.retrofit2:retrofit:2.9.0'
// json converter (Here I have used GSON but you can use others as well)
implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
```

## Internet permission

Inside manifest enable internet permission

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.example.appName">

    <uses-permission android:name="android.permission.INTERNET" />

    <application

    ...
```

## Usage

For fetching data from api we need to create a response class and a java interface.  
In reponse class we will define the data that we need to fetch.  
In interface we will define the URL, parameters, headers, etc that we need to make an api call.

Here `APIservice` is the interface  
Inside activity or fragment

```Java
// retrofit instance
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("base-url-of-API")
    .addConverterFactory(GsonConverterFactory.create())
    .build();
// connecting service to instance
APIservice apiService = retrofit.create(APIservice.class);

apiService.getData().enqueue(new Callback<ResponseClass>() {
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
