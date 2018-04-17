# User Songs

This rails app is a one model app that has a single `Song` model. A song has a title.

Your job is to implement best security practices for this app:

User login and authorization with devise.

We will follow instuctions from the devise website:
[https://github.com/plataformatec/devise](https://github.com/plataformatec/devise)

Add the gem devise to the Gemfile
```
gem 'devise'
```
Install it
```
bundle install
```

```
rails db:create
```

Run the gem's script files so it can generate the default files in the rails app
```
rails generate devise:install
```

Create the devise user model:
```
rails generate devise user
```

Link User to a new foreign key column in songs:
```
rails g migration AddUserToSongs user:references
```

```
rails db:migrate
```

Generate the default devise view files:
```
rails g devise:views
```


Devise might ask you to copy the secret devise key into the initializer file: config/initializers/devise.rb

### Set up authorization:
Add a before action filter to the controller:
```
before_action :authenticate_user!, :except => [ :show, :index ]
```

Restrict the new link in the songs index with the devise helper
```
<% if user_signed_in? %>
<%= link_to 'New Song', new_song_path %>
<% end %>
```

### Other devise helpers:

#### current_user
Make a breakpoint (byebug) in the `index` song controller method.

Inside the console see the value of current_user
```
current_user.id
```
#### user_session
See the current value of `user_session`
```
user_session
```
Set something in the user session

```
user_session[:song_cart] = "Single Ladies"
```

Use `c` to continue out of the breakpoint.

Make another request.

See the value of `user_session` has been retained.

### Adding user associations
Change both model files

app/models/song.rb
```
belongs_to :user
```

app/models/user.rb
```
has_many :song
```

Assign the logged in user as the song creator in app/controllers/songs_controller.rb
```
def create
  @song = Song.new(song_params)

  @song.user = current_user

  if @song.save
    redirect_to @song
  else
    render 'new'
  end
end
```
### Logout Link
```
<%= link_to 'log out', destroy_user_session_url, method: :delete %>
```
