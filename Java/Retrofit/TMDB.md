## TMDB API Example

For using [TMDB API](https://developers.themoviedb.org/3) you need to create an account and get API key for authentication.  
First you need to implement dependencies inside gradle file as mentioned in [Basics file](/Java/Retrofit/Basics.md)  
We are going to fetch images as well so I'll be using [Glide](https://github.com/bumptech/glide) to display image in Image View.  
Dependency for Glide:

```Java
implementation "com.github.bumptech.glide:glide:4.12.0"
```

## Response from TMBD API

We are going to fetch [Popular Movies](https://developers.themoviedb.org/3/movies/get-popular-movies) data. This is the Response we will get if we make a GET request.

```Json
{
    "page": 1,
    "results": [
        {
            "adult": false,
            "backdrop_path": "/9WlJFhOSCPnaaSmsrv0B4zA8iUb.jpg",
            "genre_ids": [
                28,
                27,
                53,
                80
            ],
            "id": 503736,
            "original_language": "en",
            "original_title": "Army of the Dead",
            "overview": "Following a zombie outbreak in Las Vegas, a group of mercenaries
                take the ultimate gamble: venturing into the quarantine zone to pull off the greatest heist ever attempted.",
            "popularity": 2427.127,
            "poster_path": "/z8CExJekGrEThbpMXAmCFvvgoJR.jpg",
            "release_date": "2021-05-14",
            "title": "Army of the Dead",
            "video": false,
            "vote_average": 6.5,
            "vote_count": 1451
        }
    ],
    "total_pages": 500,
    "total_results": 10000
}
```

## Creating Response Classes

I'm not interested in all fields I only want id, title, poster path, release date and overview.  
For fetching data first we need to create a class which will store response.  
In the response we can see that it has no of pages, a results list and inside that list our data is there. so first we need to get that array.  
Create a java class `MovieList.java` and declare the variables which you want to fetch.  
**Important:** Make sure to name variables exactly same as given in Json response.

```Java
public class MoviesList {
    // page
    int page;
    // results List. We will call the Results class
    private final List<Results> results;

    // getter for page
    public int getPage() {
        return page;
    }

    // getter for results
    public List<Results> getResults() {
        return results;
    }

    // Constructor for page and results
    public MoviesList(int page, List<Results> results) {
        this.page = page;
        this.results = results;
    }
}
```

Now once we get the results list we'll need to seperate data from that list. So create one more class named `Results.java`.  
In this we will declare the data that we need to fetch. Again remember to give exact name to variables as mentioned in response.

```Java
public class Results {
    private final String title;
    private final String overview;
    private final String release_date;
    private final String poster_path;
    private final int id;

    // Constructors
    public Results(String title, String overview, String release_date, String poster_path, int id) {
        this.title = title;
        this.overview = overview;
        this.release_date = release_date;
        this.poster_path = poster_path;
        this.id = id;
    }

    // Getters
    public String getTitle() { return title; }

    public String getOverview() { return overview; }

    public String getRelease_date() { return release_date; }

    public String getPoster_path() { return poster_path; }

    public int getId() { return id; }
}
```

## Creating Interface

Now as we have decalred all response varibles, we'll create the interface which will actually make request to TMDB API.  
Create a Java Interface `MovieDbAPI.java`.  
The URL to which we are makin request is: `https://api.themoviedb.org/3/movie/popular?api_key="api-key"`.  
Base URL for TMDB API is `https://api.themoviedb.org/3/` which we will define when we will create Retrofit Instance.  
For popular movies we need to make Get request to `movie/popular` path.  
Along with the path we also need to supply `api_key` parameter.  
after making request to we will call the MovieList class to store response

```Java
public interface MovieDbAPI {
    //@GET(url-path)
    @GET("movie/popular")
    // Call<responseClass> function()
    Call<MoviesList> getPopular(
        // @Query(parameter-name) dataType variable
        @Query("api_key") String key
    );
}
```

## Using inside activity

We'll need a recycler view to show all content.

Inside Activity or Fragment

```Java
MoviesList res;
String API_KEY = "API-Key";

RecyclerView popMovies;

Retrofit retrofit = new Retrofit.Builder()
        .baseUrl("https://api.themoviedb.org/3/")
        .addConverterFactory(GsonConverterFactory.create())
        .build();
MovieDbAPI movieDbAPI = retrofit.create(MovieDbAPI.class);

...

// res will contain the response and we will pass it to adapter of recycler view
public void getApi() {
    movieDbAPI.getPopular(API_KEY).enqueue(new Callback<MoviesList>() {
        @Override
        public void onResponse(Call<MoviesList> call, Response<MoviesList> response) {
            res = response.body();
            popMovies.setAdapter(new PopularMoviesAdapter(res));
        }

        @Override
        public void onFailure(@NonNull Call<MoviesList> call, @NonNull Throwable t) {
            Log.i("onFailure: ", t.getMessage());
        }
    });
}
```

Inside Recycler View Adapter

```Java
public class PopularMoviesAdapter extends RecyclerView.Adapter<PopularMoviesAdapter.ViewHolder> {

    private Context context;
    MoviesList res;

    public PopularMoviesAdapter(MoviesList res) { this.res = res; }

    @NonNull
    @Override
    public PopularMoviesAdapter.ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        context = parent.getContext();
        View view = LayoutInflater.from(context).inflate(R.layout.popular_movies_list, parent, false);
        return new PopularMoviesAdapter.ViewHolder(view);
    }

    @Override
    public void onBindViewHolder(@NonNull PopularMoviesAdapter.ViewHolder holder, int position) {
        // Glide.with(context).load(url).into(imageView)
        Glide.with(holder.itemView.getContext()).load("https://image.tmdb.org/t/p/w500"+ res.getResults().get(position).getPoster_path()).into(holder.poster);
        holder.movieName.setText(res.getResults().get(position).getTitle());
        holder.date.setText("Release Date: " + res.getResults().get(position).getRelease_date());
        holder.desc.setText(res.getResults().get(position).getOverview());
    }

    @Override
    public int getItemCount() {
        return res.getResults().size();
    }

    public class ViewHolder extends RecyclerView.ViewHolder {

        private final ImageView poster;
        private TextView movieName, title, desc, date;
        public ViewHolder(@NonNull View itemView) {
            super(itemView);

            poster = itemView.findViewById(R.id.poster);
            movieName = itemView.findViewById(R.id.movieName);
            desc = itemView.findViewById(R.id.overview);
            date = itemView.findViewById(R.id.date);
        }
    }
}
```
