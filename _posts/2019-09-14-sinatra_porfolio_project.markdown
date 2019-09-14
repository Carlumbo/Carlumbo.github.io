---
layout: post
title:      "Sinatra Porfolio Project "
date:       2019-09-14 15:25:03 +0000
permalink:  sinatra_porfolio_project
---



So for my Sinatra Project I decided to do something that im relatviely familar with. I have an affinity towards plands and i really just like having them around and in my living space.  deceided to make them the main focus of my project and demsontrate the requried goals with this project!


So when i first started out on the project i figured i would make the views first the way i want them to be and then move from there. While not exactly perfect approach I went with the flow that we were instructed to use throughout the learn environment. 

```
<form action="/thanks" method ='post'>

<h1>Welcome to Planted Earth!<h1>
  <div>
  <p>Planted Earth was founded in the year 2019, with the mission to plant one tree for every person on the planet!</p><br />
  <p>WE Believe you can help us meet our goal by 2025!<p>
   <label>Its really simple! all you have to do is click this button!</label>
     <input type="submit" value="submit">

</form>
<form action="/login" method='post'>
 <h2>if youd like the planet to die, go ahead and leave!<h2>
   <label>exit:</label>
   <input type="submit" value="exit">

 </form >

```

Most of the work I had doen had wen t pretty well throughout the project, but i did have an issue with the controllers. After fussing with them for awhile and follwing the examples guidlines, I eventually got to the point where everything was loading throughout the config.ru and in the apporpraite order allowing the contollers to inherit from the  App;lication Controller. Most of my controller code looks similar throughout. 

```
class FlowerGardenController < ApplicationController


  get '/gardens' do
    redirect_if_not_logged_in
    @gardens = FlowerGarden.all
    erb :'flower_gardens/index'
  end

  get '/gardens/new' do
    redirect_if_not_logged_in
    @error_message = params[:error]
    erb :'flower_gardens/new'
  end

  get '/gardens/:id/edit' do
    redirect_if_not_logged_in
    @error_message = params[:error]
    @garden = FlowerGarden.find(params[:id])
    erb :'flower_gardens/edit'
  end

  post '/gardens/:id' do
    redirect_if_not_logged_in
    @garden = FlowerGarden.find(params[:id])
    unless FlowerGarden.valid_params?(params)
      redirect "/gardens/#{@garden.id}/edit?errror=invalie garden election"
    end
    @garden.update(params.select{|c|c=="name"|| c ="size"})
    redirect "/garden/#{@garden.id}"
  end

  get "/garden/:id" do
    redirect_if_not_logged_in
    @garden = FlowerGarden.find(params[:id])
    erb :'flower_gardens/show'
  end

  post "/gardens" do
    redirect_if_not_logged_in

    unless FlowerGarden.valid_params?(params)
      redirect "/bags/new?error=invalid garden selection"
    end
    FlowerGarden.create(params)
    redirect "/gardens"
  end

end

```

The models all import from  ActiveRecord::Base

```
class FlowerGarden < ActiveRecord::Base
  has_many :flowers
  belongs_to :gardener

  def self.valid_params?(params)
    return !params[:name].empty? && !params[:size].empty?
  end


end
```

I am looking forward to refracting my code over the next couple weeks and hearing the instructor feedback!
