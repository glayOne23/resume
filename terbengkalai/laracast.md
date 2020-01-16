## 25 Core Concept : Middleware
- alurnya:
    - di Kernel ada 3 jenis middleware:
        1. $middleware -> These middleware are run during every request to your application ( ini tu global middleware gtu ).
        2. $routeMiddleware ->These middleware may be assigned to groups or used individually.
            - Nah yang dipake buat authetikasi yang ini
        

- how to use middleware to autheticate(only autheticated user can access):
    1. in controller:
        ``` php
        public function __construct(){
            $this->middleware('auth');
        }
        //setiap kali controller dipanggil akan menuju middleware auth dulu
        ```
    2. di route:
        ``` php    
        Route::resource('/films','FilmController')->middleware('auth');
        ```
- Opposite : you can only see this page is not authenticated:
    ``` php
        Route::resource('/films','FilmController')->middleware('guest');
    ```
---
## 26 You May Only View Your Projects
1. u may only view your list of project:
    ``` php 
    public function index(){

        $films = Film::where('user_id', auth()->id())->get();
        return view('film.index', compact('films'));
    }
    ```
    ``` php
    //auth()->id() bisa diganti dengan Auth::id, tapi diatas harus use /Auth blabla
    //auth()->id() return id or false
    //auth()->user() return instance of user
    //auth()->check() return boolean
    //auth()->guest() dia guest
    ```
- menaruh authetikasi di controller pada fungsi2 tertentu:
    ``` php
        public function __construct(){
                $this->middleware('auth')->only(['store', 'update']);
                //atau
                $this->middleware('auth')->except(['store', 'update']);
        }
    ```
2. ngasih tw yang create ini user dengan id itu:
    ``` php
        Film::create([
            'title' => request('title'),
            'description' => request('description'),
            'genre_id' => request('genre_id'),
            'director' => request('director'),
            //kasih tambahan ini buat masukin id
            'owner_id' => auth()->id()
        ]);        
        
        
        return redirect('films');

    ```
---
## 27 Authorization Essentials
- User dengan id lain nggk boleh liat show di film kita:
    ``` php 
    public function show(Film $film)
    {
        //cara 1
        if ($film->user_id !== auth()->id()){
            abort(403);
        }
        //cara 2
        abort_if($film->user_id !== auth()->id(),403);
        //cara 3
        abort_if(! auth()->user()->owns($film),403);     
        //cara 4
        abort_unless(auth()->user()->owns($film),403);     

        return view('film.show', compact('film') );
    }
    ```    
## - Policy
- buat policy: ``` php artisan make:policy FilmPolicy --model=Film```
- di file FilmPolicy:
    ``` php
    use App\Modes\Film;
    use App\Modes\User;

    public function owner(User $user, Film $film)
    {
        return $film->user_id == $user->id;
    }
    ```
- di controller : misal di edit:
    ``` php
    public function edit(Film $film)
    {
        $this->authorize('owner', $film);

        $genres = Genre::all();
        return view('film.edit', compact('film', 'genres') );
    }
    ```


# {{undocumented}
---
## 29 Always Consider Readability
- 
---
## 30 Mail

### Cara 1: Langsung dari Controller:
- di Controller:
    ``` php
    use Illuminate\Support\Facades\Mail;
    use \App\Mail\FilmCreated;

    public function store(Request $request)
    {
        $film = Film::create([
            'title' => request('title'),
            'description' => request('description'),
            'genre_id' => request('genre_id'),
            'director' => request('director'),
            'user_id' => auth()->id()
        ]);

        //kirim email cara langsung di Controller
        Mail::to($film->user->email)->send(
            new FilmCreated($film) 
        );
            
        return redirect('films');
    }
    ```
- ``` php artisan make:mail FilmCreated --markdown="emails.film-created" ```
- di FilmCreated:
    ``` php
    class FilmCreated extends Mailable
    {
        use Queueable, SerializesModels;

        public $film;

        public function __construct($film)
        {
            $this->film = $film; 
        }

        
        public function build()
        {
            return $this->markdown('emails.film-created');
        }
    }
    ```
- di emails.film-created:
    ``` php
    @component('mail::message')

    # Film Baru :{{$film->title}}

    {{$film->description}}

    @component('mail::button', ['url' => '/films/'.$film->id])
    Lihat Film
    @endcomponent

    Thanks,<br>
    {{ config('app.name') }}
    @endcomponent

    ```    
---
## 31 Model Hook and Seesaw
- Hook di model ada macem2 : bisa creating,created, updated, updating, dll
### Cara 2 : Pakai Model Hook :
- cara mirip pertama , cuma yang di controller Film di pindah ke Model Film :
    ``` php
    use Illuminate\Support\Facades\Mail;
    use \App\Mail\FilmCreated;

    protected static function boot(){
        parent::boot();

        static::created(function($film){
            Mail::to($film->user->email)->send(
                new FilmUpdated($film) 
            );
        });
    }
    ```
---
## 32 Custom Event and Listener
### Cara 3 pakai event listener (in case untuk hapus)
- buat mail seperti biasa dengan nama FilmDeleted
- ```php artisan make:event FilmDeletedEvent```
    ``` php
    public $film;

    public function __construct($film)
    {
        $this->film = $film;
    }

    ```
- di controller film:
    ``` php
    use \App\Event\FilmDeletedEvent;

    public function destroy(Film $film)
    {

        $film->delete();

        event(new FilmDeletedEvent($film));

        return redirect('films');
    }
    ```    
- ```php artisan make:listener SendEmailAfterDeletingFilm --event=FilmDeletedEvent```
    ``` php
    use Illuminate\Support\Facades\Mail;
    use \App\Mail\FilmDeleted;

    public function handle(FilmDeletedEvent $event)
    {
        Mail::to($event->film->user->email)->send(
            new FilmDeleted($event->film) 
        );
    }
    ```
- buat emails.film-deleted dan ubah sekeinginan
- daftarkan event dan listener ke EventServiceProvider:
``` php
    protected $listen = [
        Registered::class => [
            SendEmailVerificationNotification::class,
        ],
        
        //yang ini
        FilmDeletedEvent::class =>[
            SendEmailAfterDeletingFilm::class
        ],
    ];
```
---
## 33 User Notification
### Cara 4 pake user notification : cara ini nggk make mail class :
- ``` php artisan make:notification FilmDeleted```
```php
    public $film;

    public function __construct($film)
    {
        $this->film = $film;
    }

    
    public function via($notifiable)
    {
        return ['mail'];
    }

    
    public function toMail($notifiable)
    {
        return (new MailMessage)
                    ->line('Film dengan judul '.$this->film->title. ' telah dihapus')
                    ->action('Kembali ke Film', url('/films/'))
                    ->line('Pakai cara Notifications');
    }
```
- di controller film :
    ``` php
    use \App\Notifications\FilmDeleted;

    public function destroy(Film $film)
    {
        $user = $film->user;
    
        $this->authorize('owner', $film);

        $film->delete();
        
        $user->notify(new FilmDeleted($film));

        return redirect('films');
    }
    ```
- di Model User:
    ``` php
    use Illuminate\Notifications\Notifiable;
    use Illuminate\Contracts\Auth\MustVerifyEmail;
    use Illuminate\Foundation\Auth\User as Authenticatable;

    class User extends Authenticatable
    {
        use Notifiable;
    ``` 

        





