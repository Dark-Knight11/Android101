## Harry Potter API example

There is no need of any authentication to use this [API](https://hp-api.herokuapp.com/).

Here we are going to display image so we'll implement glide to do that.

```Java
implementation "com.github.bumptech.glide:glide:4.12.0"
```

## Response Structure

We are going to get Characters so base URL is `http://hp-api.herokuapp.com/api/` and path is `characters`

```Json
[
    {
        "name": "Harry Potter",
        "species": "human",
        "gender": "male",
        "house": "Gryffindor",
        "dateOfBirth": "31-07-1980",
        "yearOfBirth": 1980,
        "ancestry": "half-blood",
        "eyeColour": "green",
        "hairColour": "black",
        "wand": {
            "wood": "holly",
            "core": "phoenix feather",
            "length": 11
        },
        "patronus": "stag",
        "hogwartsStudent": true,
        "hogwartsStaff": false,
        "actor": "Daniel Radcliffe",
        "alive": true,
        "image": "http://hp-api.herokuapp.com/images/harry.jpg"
    }
]
```

## Response Class

Create a response class `Characters.java`

```Java
public void Characters() {
    private String name;
    private String gender;
    private String actor;
    private String house;
    private String image;

    public Characters(String name, String gender, String actor, String house, String image) {
        this.name = name;
        this.gender = gender;
        this.actor = actor;
        this.house = house;
        this.image = image;
    }

    public String getName() {
        return name;
    }

    public String getGender() {
        return gender;
    }

    public String getActor() {
        return actor;
    }

    public String getHouse() {
        return house;
    }

    public String getImage() {
        return image;
    }
}
```

## Interface

Create a Java Interface `PotterApi.java`

```Java
public interface PotterApi {
    @GET("characters")
    Call<List<Characters>> getCharacters();
    // List because response is in List form
}
```

## Usage

```Java
public class MainActivity extends AppCompatActivity {

    List<Characters> res;

    TextView name, gender, actor, house;
    ImageView poster;

    int index = 0

    // retrofit instance
    Retrofit retrofit = new Retrofit.Builder()
            .baseUrl("http://hp-api.herokuapp.com/api/")
            .addConverterFactory(GsonConverterFactory.create())
            .build();

    // connect service to instance
    PotterApi potterApi = retrofit.create(PotterApi.class);

    @Overwrite
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        name = findViewById(R.id.name);
        gender = findViewById(R.id.gender);
        actor = findViewById(R.id.actor);
        house = findViewById(R.id.house);
        poster = findViewById(R.id.poster);

        // call the api
        potterApi.getCharacters().enqueue(new Callback<List<Characters>>() {
            @Override
            public void onResponse(@NonNull Call<List<Characters>> call, @NonNull Response<List<Characters>> response) {
                res = response.body();

                // Random Number generator
                Random rn = new Random(System.currentTimeMillis());
                index = rn.nextInt(res.size() - 1) - 1;

                name.setText(res.get(index).getName());
                gender.setText(res.get(index).getGender());
                actor.setText(res.get(index).getActor());
                house.setText(res.get(index).getHouse());
                Glide.with(MainActivity.this).load(res.get(index).getImage()).into(poster);
            }

            @Override
            public void onFailure(@NonNull Call<List<Characters>> call, @NonNull Throwable t) {
                Log.i("onFailure: ", t.getMessage());
            }
        });
    }
}
```
