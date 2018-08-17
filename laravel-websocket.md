# Laravel with Websocket Technology
---
## What is websocket?


<p style="font-size:13pt">Basiclly websocket commonly use to provide a real time feedback data within server and client. For instances, Facebook LIVE features, those comments are submit by viewers and every viewers get all the comments in real time without refresh their entire browser to get the latest comments. Secondly, websockets will be able to provide another user experience on the web such as real time monitoring. For example, users import data from a csv file to the server, and those CSV row data will be batch processing behind the server and the server is able to use the websocket to broadcast the progression back to the user, or notify the user when the progress is done.</p>


## The socket service server

Socket service such as Pusher, socket.io that provide developers an easy way to handle websocket without deploying a new socket server and  doing some configuration on the server.

## Installation 
---
### Step 1: Create a new Pusher account

**As the pusher is mentioned aboved, we won't dive into too much about it. So, create a new pusher account and create your new app in the pusher console. Be sure that copy those app key into the .env file.**

### Step 2: Install the pusher official ```pusher/pusher-php-server``` into your project directory using composer.

```zsh
$ composer require pusher/pusher-php-server
```

### Step 3: Install the pusher frontend JavaScript Library using yarn or NPM

**Using Yarn**
```zsh
$ yarn add pusher-js
```

**Using NPM**
```zsh
$ npm install pusher-js --save
```

### Step 4: Install **laravel-echo** Javascript Libary using yarn or NPM


**Using Yarn**
```zsh
$ yarn add laravel-echo
```

**Using NPM**
```zsh
$ npm install laravel-echo --save
```

## Configuration
---

### Enable ```BroadcastServiceProvider``` in ```config/app.php```

In the services array simply uncomment the BroadcastServiceProvider, out websocket is gonna to use that service, please be sure that BroadcastService Provider is enabled.

### Specify the Pusher configuration in ```config/broadcasting.php```

As the ```.env``` file handle all the environment variables, and it is ignored by the git repository which mean we can place all pusher app keys in ```.env``` file.

```env
PUSHER_APP_ID=<your-pusher-app-id>
PUSHER_APP_KEY=<your-pusher-app-key>
PUSHER_APP_SECRET=<your-pusher-app-secreet>
PUSHER_APP_CLUSTER=<your-pusher-app-cluster>

```

### Extra configuration for cluster [Skip if you are using Laravel version > 5.5]

The cluster commonly known as the region for the pusher's socket server. Be sure that choose the closest region between you to prevent any network delay.

Specify the cluster in the pusher configuratio array

```config/broadcasting.php```
```php
<?php
    return [
        'connections' => [
            'pusher' => [
                    'driver' => 'pusher',
                    'key' => env('PUSHER_APP_KEY'),
                    'secret' => env('PUSHER_APP_SECRET'),
                    'app_id' => env('PUSHER_APP_ID'),
                    // Extra added field 
                    'options' => [
                        'cluster' => env('PUSHER_APP_CLUSTER'),
                        'encrypted' => true
                    ]
            ],
        ]
    ];
```

### Modify the Broadcast Driver in ```.env``` of ```broadcasting.php```

```.env
BROADCAST_DRIVER=pusher
```

## Client side JavaScript setup
---

### **The CSRF-TOKEN field**

By default if the endpoint API url is declare in the ```web.php``` the web middleware will validate all the CSRF token field inside the request headers that sent out front the client side to prevent any cross-site request forgery. To bypass any CSRF validation, you can define your routes inside the ```api.php```, but be sure that the request headers from front end is injected with **X-Requested-With: "XMLHttpRequest"**
- Example in axios
    ```js
    import axios from 'axios'
    axios.defaults.headers.common['X-Requested-With'] = "XMLHttpRequest"
    ```

## Enable frontend pusher library

Inside your ```resources/assets/js/bootstrap.js``` uncomment these lines of code, to import pusher library and laravel-echo in you JavaScript file.
- ```bootstrap.js```
    ```js
    import Echo from 'laravel-echo';
    
    window.Pusher = require('pusher-js')
    window.Echo = new Echo({
        broadcaster: 'pusher',
        key: 'your-pusher-key',

        // asia-pacific-singapore
        cluster:'ap1',
        encrypted： true
    })

    ```

## Real-time comment system development
---

> ### Step 1: Create Comment database model and migration

```bash
    $ php artisan make:model Comment -mc
```
**The ```-mc``` flags which also create a migration and a controller file.


> ### Step 2: The writing the migration scripts

```php
<?php
...

class CreateCommentsTable extends Migrations {
    public function up(){
        Schema::create('comments', function($table Blueprint){
            $table->increments('id');
            $table->integer('user_id')->unsigned();
            $table->integer('post_id')->unsigned();
            $table->text('comment_body');
            $table->timestamps();

            $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
            $table->foreign('post_id')->references('id')->on('posts')->onDelete('cascade');
        })
    }

    publc function drop(){
        Schema::dropIfExists('comments');
    }
}

>
```

> ### Step 3: Declare mass assignable fields and define model relationships

> ```app\Comment.php```
```php
<?php

class Comment extends Model {
    protected $fillable = ['comment_body' , 'user_id'];

    public function post(){
        return $this->belongsTo(\App\Post::class);

    }
    public function user(){
        return $this->belongsTo(\App\User::class);
        
    }
}

```

> ```app\Post.php```

```php
<?php

class Post extends Model {
    ...
    public function comments(){
        return $this->hasMany(\App\Comment::class);

    }
}
```
> ```app\User.php```

```php
<?php

class User extends Authenticatable {
    ...

    public function comments(){
        return $this->hasMany(\App\Comment::class);

    }
}
```

> ## Declaring API endpoint url in api.php

>```routes/api.php```

```php
Route::get('posts/{post}/comments', "CommentController@index");

Route::middleware('auth:api')->group(function(){
    Route::post('posts/{post}/comment', "CommentController@store");
})
```

> ## Define methods in comment controller

**Create New Controller**
```bash
php artisan make:controlleer CommentController
```

```php
...
use Illuminate\Http\Request;
use App\Post;
use App\Comment;
use Auth;
class CommentController extends Controller {
    public function index(Post $post){
        return response()->json($post->comments()->with('user')->latest()->get());
    }

    public function store(Request $request, Post $post){
        $comment = $post->comments()->create(['comment_body' => $request->comment_body , 'user_id' => auth()->id
        ]);
        
        $comment = Comment::where('id' , $comment->id)->with('user')->first();
        return response()->json($comment);
    }
}

```

> ## Front-end UI development

> ```resources/views/post.blade.php```

```html

@extends('layouts.master')
@section('content')

@endsection

@section('scripts')
<script>
const app = new Vue({
    el : "#app",
    data: {
        comments: {

        },
        commentBox: '',
        post: {{!! $post->toJson() !!}},
        user: {!!Auth::check() ? Auth::user()->toJson() : 'null' !!}
    },
    mounted() {
        this.getComments();
    },
    methods: {
        getComments(){
            axios.get('/api/posts/' + this.post.id + '/comments').then(response => {
              this.comments = response.data;  
            }).catch(error => {
                console.log(error)
            })
        },
        postComments(){
            axios.post('/api/posts/' + this.post.id + '/comment', {
                api_token: this.user.api_token,
                comment_body: this.commentBox
            }).then(response => {
                this.comments.unshift(response.data);
                this.commentBox = "";

            }).catch(error => {
                console.log(error)
            })
        }
    }
})
</script>
@endsection
```
> ## Rendering comments lists

```pug
    .media.mt-2(v-for="comment in comments")
        .media-left
            a(href="#")
                img.media-object(src="http://placeimg.com/80/80")
        .media-body
            h4.media-heading @{{comment.user.name}} said...
            p @{{comment.comment_body}}
            span(style="color:white") @{{comment.date}}
```

> ## Save Comment Button

```pug
    button.btn.btn-primary(v-on:click="postComment") Comment
```

> ## Handling Websockets

### Step 1: Create a new event and named it as NewComment

```bash
php artisan make:event NewComment
```

#### Implements ShouldBroadcast interface into NewComment event class

```php
<?php

namespace App\Events;

use Illuminate\Broadcasting\Channel;
use Illuminate\Queue\SerializesModels;
use Illuminate\Brodcasting\PrivateChannel;
use Illuminate\Broadcasting\PresenceChannel;
use Illuminate\Foundation\Events\Dispatchable;
use Illuminate\Broadcasting\InteractsWithSockets;
use Illuminate\Contracts\Broadcasting\ShoudBroadcast;
use Illuminate\Contracts\Broadcasting\ShoudBroadcastNow;

class NewComment implements ShouldBroadcastNow {

    use Dispatchable, InteractWithSockets, SerializesModels;

    public function __construct(){
        
    }
}


```

> ### Warning : **ShouldBroadcastNow interfaces will not queue the job, it will execute the handler immediately when an event occured, if the is in production please use ShouldBroadcast instead of ShouldBroadcastNow interface.**

### Channel Type

There are different channel class  such as Cannel, PrivateChannel and PresenceChannel each channel has it's different specification that is showed below the table

| Channel Type | Description |
|:----------|:-----------|
| **Channel** | Known as public channel anyone is able to keep listen on the channel event on the frontend |
| **PrivateChannel** | Only authenticated user are able to listen on the channel event |
| **PresenceChannel** | Advance version of the PrivateChannel, such as specify channel for different authenticated user roles |




#### The ```broadcastOn``` methods and constructor

The broadcastOn methods let you specify which channel that you are going to use, and specify the channel name.

The constructor methods let you passed those data payload that can be attached with the broadcast channel. In this scenario, we need to pass in the post data as our payload.

The broadcastWith nethods let your perform some modification for the data that passed into the websocket server (Pusher). Since the channel we use is a public channel, passing all user's secret information such as api_token into the sockets data payload may result a potential security risk in data leak.

```php
...
protected $comment;
public function __construct(Comment $comment) {
    $this->comment = $comment;
}
public function broadcastOn(){
    return new Channel('postComment-'.$this->comment->post->id);
    //Anyone can use this channel
}

public function broadcastWith(){
    return [
        'body' => $this->comment->comment_body,
        'created_at' => $this->comment->created_at->toFormattedDateString(),
        'user'  =>[
            'name' => $this->comment->user->name,
            'avatars' => $this->comment->user->avatars
        ]
    ]
}

```

> ### Trigger the event

As we have setup the event, it's time to trigger the event. To trigger the event we need to trigger it inside the CommentController store methods.

```php
...
use Illuminate\Http\Request;
use App\Post;
use App\Comment;
use Auth;
use App\Events\NewComment;
class CommentController extends Controller {
    public function index(Post $post){
        return response()->json($post->comments()->with('user')->latest()->get());
    }

    public function store(Request $request, Post $post){
        $comment = $post->comments()->create(['comment_body' => $request->comment_body , 'user_id' => auth()->id
        ]);
        
        $comment = Comment::where('id' , $comment->id)->with('user')->first();

        // broadcast to others people.
        broadcast(new NewComment($comment))->toOthers();

        return response()->json($comment);
    }
}

```

> ### Subscribe Channel On Frontend

On frontend, we should tell Laravel Echo subscribe the channel which is postComment-:post_id channel. When a new incoming events come in, we should update the comment list.

Secondly, we should declare a new listen method inside the methods object which keep tracking on incoming channel event.


```js

...

methods: {
    ...,
    listen(){
        Echo.channel('postComment-' + this.post.id).listen('NewComment', (comment) => {

            this.comments.unshift(comment);
        })
    }
},
mounted() {
    // Keep listening on channel when the vue component is mounted.
    this.listen();
}

...

```


## The Presence and Private Channel

> ### Private Channel
Switch public channel to private channel

On Frontend changed to Echo.channel to Echo.private to tell Laravel Echo the current listen channel required an authentication.

> Frontend
```js

...
methods : {
    listen() {
        Echo.private('postComment-' +this.post.id).listen('NewComment', (comment) => {
            console.log(comment);
        })
    }
}
```

> Backend

```app\Events\NewComment.php```
```php
class NewComment extends Event implements ShouldBroadcastNow {
    ...

    public broadcastedOn(){
        return new PrivateChannel('postComment-'.$this->comment->post->id);
    }
}
```


```routes/channels.php```

The ```channels.php``` which allow developers to provide some channel subscribed authorization policy.




```php
Broadcast::channel('App.User.{id}', function($user, $id){
    // The code below shown the current user which passed in the first parameters and the second paremeters channel user id field. With this two parameters we can use comparison to check whether the current log in user has the rights to subscribe on the channel.

    return (int) $user->id === (int) $id;
});

Broadcast::channel('postComment-{id}', function($user, $id){
    // If user logged in, the first parameters with user information will be passed in, and the code will be executed and return an authorized boolean which is true to current logged in user.
    
    return true;
});

```

