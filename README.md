#4.0 Fetching Movies from API
async and await
tells to the server that the function inside of async needs some time to be finished and we can put await in front of any function inside of async function. then server will wait until that function finished.

    => has an implicit return
    .map(a => a)
    is the same as
    .map(function(a) {
    return a
    })

#4.1 Rendering the movies
let's think how jsx works(RECAP)
In the index.js, it renders a class component App. And it gives inside of App to the index.html at the class id is "root" and in this case, it is <div id="root"></div> inside of <body />

    import App from './App';
    ReactDOM.render(<App />, document.getElementById('root'));

Then in the App.js(extended component of React.Component because we need 'state'), it fetchs the data from YTS api but using axios, not fetch. Axois is the layer on the top of fetch. And we made it async function to wait axios grab data from api.
So, by using sync and await, we made a function getMovies like below.

    const {data: {data: { movies }}}
    = await axios.get("https://yts-proxy.now.sh/list_movies.json?sort_by=rating");

Above code is same with the code below.

    const initialData = await axios.get("https://yts-proxy.now.sh/list_movies.json?sort_by=rating");
    const movies = initialData.data.data.movies

This getMovies function is automatically runs after page loading because we put it in componentDidMount like below.

    componentDidMount() {
        this.getMovies();
    }

So let's move to 'render' part. In the render function, we declared two vars like below.

    const { isLoading, movies } = this.state;

It means const isLoading and movies from current state. If we do not do this, we have to wrote this.state.isLoading every time we want to use that state.

And at the end, we returned below.

    return (
                <div>
                    {isLoading
                        ? "Loading..."
                        : movies.map(movie => (
                            <Movie
                                key={movie.id}
                                id={movie.id}
                                year={movie.year}
                                title={movie.title}
                                summary={movie.summary}
                                poster={movie.large_cover_image}
                            />
                    ))}
                </div>
           );

It is simpe if statement of javascript.
{isLoading ? {The things to do if isLoading==true} : {The things to do if isLoading==false}}

    movies.map(movie => (<Movie
                                key={movie.id}
                                id={movie.id}
                                year={movie.year}
                                title={movie.title}
                                summary={movie.summary}
                                poster={movie.large_cover_image}/>
                        )
                )

object.map(var => (function)) means apply (function) on every items in object array and returns it as var. So above code works like: apply Movie function on the every movies we get from axios, each item will be declared as movie, and movie.id~poster will be gave to Movie function as props. I AM NOT REALLY SURE THIS MAKE SENSE AND HAVE TO RECAP THIS LATER.

Movie function is imported custom function. We made Movie.js file in the src/ and it has one function component. getting id, year, title, summary, poster from movie and return html code using that. We used PropType to that function to avoid any mistake.

#4.2 Styling the Movies
In the js file, you can use css inside of double bracket like. <div class="className" style={{}}>
But it is not recommended. Instead doing that, make a css file like src/App.css, src/Movie.css.

#4.3 Adding Genres
The html code inside of react component to render, you should use className instead class. It it to avoid any mistake of react and it will looks like normal class in the actual browser inspection.
Like same, if you mean "html" for, you should use htmlFor=.

          {genres.map((genre, index) => (
            <li className="genres__genre">{genre}</li>
          ))}

index is an argument automatically provided by map function. you can use any name to that.

#6.2 Building the Navigation
a href refreshs the page. Use link instead of it. And the Link must be inside of Router.
