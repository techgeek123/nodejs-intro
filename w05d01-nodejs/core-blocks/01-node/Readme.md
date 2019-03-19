# Node Basics

Read more about fs module [here](https://nodejs.org/api/fs.html), and then try writing and reading [dummy](http://www.lipsum.com/) text from a file.

## Release 0
Do the following
  - Create a `server-1.js` file. Within the file create a server that just serves the sentence "Hello World" in plain text. Set your server to listen to port 3000, and use port 3000 for the rest of your servers throughout the exercise
  - Create a `server-2.js` file that serves two HTML elements: and h1 element, and a paragraph element. The `h1` element should say 'Welcome to My Site' and the `p` element should say 'Content coming soon...'. Both html elements should be included directly in your `server-2.js` file, not in separate files.
  - Create a separate `index.html` file. In the `index.html` file create an `h1` element, a `p` element, and a `button` element, and add some text of your choosing to each element. Then create a `server-3.js` file that serves up the `index.html` page
  - Create another `html` page and call it `page2.html`. Add an `h1` tag into it that says `Welcome to Page 2`. Now create a `server-4.js` file that will serve up the `index.html` page if you go to `localhost:3000/`, and will server the `page2.html` page if you go to `localhost:3000/page-2`
  - Add an `a` tag to your `index.html` page, and give the `a` tag an `href` attribute of `href="/page-2"`. Add a similar `a` tag to your `page2.html` file that links back to `index.html`. Reload your `server-4.js` and check to make sure your links work
  - Try adding 1-2 more HTML pages and adding links to each html page that lets you navigate to any page from any page. Create a `server-5.js` file that serves up all of your html pages

## Release 1
For this exercise, we are going to build a simple command line tool which allows us to make a request to an API and store the data in a text file! We will be using the following modules:
  - `fs` - for reading and writing to a file
  - `process` - for gathering arguments from the command line
  - `request` - for making API requests (this is an external module)

This application should accept a command line argument using `process.argv`. The command line argument should be the name of a movie and your application should make an API request to the [omdb API](https://www.omdbapi.com/) and output the plot of the movie. Your program should also save the name of the movie to a file called `results.txt`.

### Release 2
> 1. Use the [prompt](https://github.com/flatiron/prompt) module to ask a user for some input instead of having to pass in an argument from the command line.
> 2. Your program should accept a command line argument called `leaderboard`. If that command line argument is passed in, your application should return the most popular search based on how many times it appears in `results.txt`

- Go ahead and read about [cheerio](https://cheerio.js.org/) and use it to scrap name, rating and year of release of this film from [here](http://www.imdb.com/title/tt2250912/).

- Find a module to give your current location OR use cheerio to scrap it from [here](http://ipinfo.io/), then use that location to find current weather condition of your location using appropriate weather forecasting module.
