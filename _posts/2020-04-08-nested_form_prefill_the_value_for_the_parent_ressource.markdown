---
layout: post
title:      "Nested form : Prefill the value of the parent ressource . "
date:       2020-04-08 00:28:58 -0400
permalink:  nested_form_prefill_the_value_for_the_parent_ressource
---




 Ruby on Rails’s collection_select form helper generates an HTTP 'select'  tag and <option> tags that render a dropdown menu with multiple options.
Form helpers allow us to create html forms by using a lot less code. Not only that but some form helpers also handle session authentication, field pre-population, validation and a bunch of other useful tasks.


 In this particular case we will focuss on how to preselect or prefill the value for the parent ressource  in our nested form .
 I m building a new scene form and want a dropdown menu that preselect  the associated `actor`for an instance of Actor, @actor(parent-ressources) and the child `scene` in our nested form .
 
  Let's get it started :
 
```
collection_select(:scene, :actor_id, Actor.all, :id, :name)

 ```
 
 

if `@scene.actor_id` is already 1, this would return an html similar to this :

```
<select name="scene[actor_id]">
  <option value="1" selected="selected">Julo</option>
  <option value="2">Lupita</option>
  <option value="3">Will</option>
	...
</select>
```

We just need to make sure that @my_object.my_method returns a value that matches one of the available option values. If there's a match then that option will be selected.

I created a helper method in my Actor model :

```
def name
    "#{first_name}" 
  end
end
```

same as 

```
def name 
Actor.first_name
end 
```



below is my whole form notice than our method is being call in our form referencing the `:first_name` of our selected actor . 

```
<h2> New Scene </h2>


<%= form_for @scene do |f|%>
<%= f.label :Actor %>
<div><%=collection_select(:scene,  :actor_id,  Actor.all,  :id,  :name)%></div>
<%= f.label :Movie %> 
<div><%= f.collection_select :movie_id, Movie.all,  :id,  :title,  prompt: true %></div>
<%= f.label :Location %>
<div><%= f.text_field :location %></div><br>
<%= f.label :Time %>
<div><%= f.text_field :time %></div><br>
<%= f.label :Acting %>
<div><%=f.text_field :acting %></div><br>
<%= f.submit "Create a Scene" %>
<% end %>
```  

Helpers in rails are meant to clean up view code by allowing us to extract data processing logic from our markup code. By moving the logic out, we get an added benefit, we can test our “view logic” easier. 

Happy Coding!










  























