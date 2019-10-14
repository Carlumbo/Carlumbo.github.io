---
layout: post
title:      "Apartment Complex Rails Portfolio Project"
date:       2019-10-14 03:50:17 +0000
permalink:  apartment_complex_rails_portfolio_project
---



So I choose to do this rails portfoilio project because in my previous career I had a client that really wante us build him a property managment website. I thought how hard would this be of a task to do it wiht my ruby knowledge. Well, first I thoguht about what I would need in terms of relationships. 

I knew that i would have to have Landlords, Tenents, Apartment Complexs and different floor plans. I knew that these floor plans would have different layouts. What I didnt know was how complicated it could quickly become with all these moving varibles. 

I started coding with laying out my controllers first, and then models. After I had a good idea of what i wanted my models to look like, I then made my databases to refelect that. Once i build the database, thats when I went back and redid the controllers. The most complicated of which was the params for the apartments controller itself 

```
 def apartment_params 
    params.require(:apartment).permit(:name, :address, :landlord_id, :num_of_floor_plans, floor_plans_attributes: [:id, :type, :price, :description, :max_tenants, :apartment_id, layout_ids: [], layouts_attributes: [:name], floor_layouts_attributes:[:quanity, :id]])
  end 
end
```

While that packed a pretty punch I also had a wild time trying to get the create function to seemless blend into the update form. You see, the create form would basically build my apartment complex itself, but then id be parlayed into the update from where i actually build out the floorplans for the apartment. 

so basically id come to the website as a new user, make an account as a landlord, who owns many apartments, and then id build many floor plans throught those apartment relationships. 

the form would display as this 
```
<%= form_for @apartment, url: @url do |f| %>
    Name: <%= f.text_field :name %><br />
    Address: <%= f.text_field :address %><br />
    Number of Floor Plans: <%=f.text_field :num_of_floor_plans %><br />
    <h4>Floor Plans:</h4>
    <%= f.fields_for :floor_plans, @apartment.floor_plans do |fp|%>
        Type: <%= fp.text_field :type %><br />
        Max Tenants:  <%= fp.text_field :max_tenants %><br />
        Rental Price: <%= fp.text_field :price %><br />
        Description: <%= fp.text_field :description %><br />
    
        <%= fp.collection_select :layout_ids, Layout.all, :id, :name %> <br />
        <%= fp.fields_for :layouts, fp.object.layouts.build do |layouts_fields| %>
            <%= label_tag "Create Custom Layout:" %>
            <%= layouts_fields.text_field :name  %>
        <%end%>
        <br />
        <br />
            <% if fp.object.floor_layouts.present? %>
                <%= fp.fields_for :floor_layouts do |fl| %>
                    <%= fl.object.layout.name %>
                    - Quantity? 
                    <%= fl.text_field :quantity %>
                    <br />
                <%end%>
            <%end%>
        <%end%>
    <%=f.submit%>
<%end%>
```

This was quite the thing to code because i didnt want the floor plans to display until I created the apartment complex that they were going to go into. This portfolio project still needs some work done, but i do believe the proper relaitonships for the project are present in the current build. I do look forward to refracting the code over the next couple days. 
