---
layout: post
title:      "Best Movie CLI GEM"
date:       2019-11-19 22:54:20 +0000
permalink:  best_movie_cli_gem
---


So for my CLI project I decided to do something near and dear to my heart. Something that I truly love are movies, from costume and set design, lighting, camera angles and whatever else may go into the creative process. While there were some hiccups in the process I manage create a function scraper that I decided to use on a website called Ranker.com.

So when I set out with to complete the project, I first went to IMDB, but then came to the conclusion that a lesser known website would be better for my needs. After some light googling I came upon https://www.ranker.com/crowdranked-list/the-best-movies-of-all-time . Now this is a crowd ranked list of movies. I used inspect functionality of Google Chrome and made sure that I could parse the HTML on this website with Nokogiri. Using the NOKOGIRI::HTML functionality I was able to select the design blocks within the HTML Code. I knew this sight would work well once I was able to select the elements I wanted.

The idea of my CLI was to parse this website, and pull the information into an easy to use database for anyone. I wanted the CLI to return the corresponding movie to a rank, expressed as an integer, that was input by the user. When the user inputs the ./bin/best_movies under the directory of best_moivies it would prompt you to select a movie rank after giving you a lit of the top 25 movies. What the CLI would return after this was the corresponding rank of the movie, its title, list of lead actors, a short plot description and an additional link to the Wikipedia page of the movie.

As soon as you input the command ./bin/best_movies into the terminal it outputs the current list of movies and makes a request for you to choose a number associated with the movies rank. Once the movie is selected it will pull the information for that movie and provide it back to you in the format of TITLE: Leading Actors: Synopsis: Link: Wikipedia

I am pleased with the functionality of the project as it currently stands, though I do believe there could be more features added to make it a little more complex. The code itself brings in each set of HTML code from the website and saves it to an array of different categories.
```
def self.scrape_ranker 
    doc = Nokogiri::HTML(open('https://www.ranker.com/crowdranked-list/the-best-movies-of-all-time'))
    
    movie_names = doc.search("div.listItem__data a").text
    title2 = movie_names.gsub(/"* ...more/, "!!")
    title = title2.gsub(/"*Casablanca/, "!!Casablanca").split("!!")
 
    ranks = []
      doc.search("strong.listItem__rank.block.center.instapaper_ignore").each do |rank|
       ranks << rank.text
    end 
   
    descriptions = []
       doc.search("div.listItem__data div.listItem__blather.grey.default span").each do |description|
        descriptions << description.text
    end   
    
    url = []
      doc.search("div.listItem__data div.listItem__blather.grey.default span.listItem__wiki.block.default.grey a").each do |anchor|
        url << anchor.attr('href')
    end 
    
    leading_actors = []
      doc.search("div.listItem__data span.listItem__props.block span.listItem__properties.black.default").each do |actor|
        leading_actors << actor.text
      
    end 
Once I had the arrays of ranks, titles, actors, plots and outside URL links, I place them into a nested hash.

 movie_info = {titles: title, rank_spot: ranks, synopsis: descriptions , actors: leading_actors, urls: url}
The hash was then made accessible to the CLI and code be acted upon through a series of requests.

def list_movies
    puts "Top 25 movies"
    @movies = BestMovies::Movie.alltime
    # binding.pry
    @titles, @rank , @synopsis , @actors, @urls = @movies[:titles] ,  @movies[:rank_spot], @movies[:synopsis] , @movies[:actors] , @movies[:urls]
    # @rank = @movies[:rank_spot]
    @titles.each.with_index(1) do |movie, i|
      puts "#{i}. #{movie}"
    end 
  end 
The requests could pull information from the website, if improper info was requested it would prompt you for the correct information or to exit the program.

def menu 
    # puts "enter the rank of the movie you would like the information of:"
   input = nil 
   while input != "exit"
     puts "\n Enter the rank of the movie you would like the information of or type list to see the movies again:"
     input = gets.strip.downcase
     if input.to_i > 0
       puts "#{@titles[input.to_i-1]}\n Starring:#{@actors[input.to_i-1]} \n\n Plot:#{@synopsis[input.to_i-1]} info at #{@urls[input.to_i-1]}"
     elsif input == "list"
        list_movies
     elsif input == "exit"
      nil 
     else 
        
        puts "Invalid input, type list for more options or exit."
      end 
    end 
  end 
```
Once exit was type in the console it would have a goodby message to the user and exit the program.
