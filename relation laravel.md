## NOTE: Langkah-langkah secara umum:
   1. bikin database dan Setting .env
   2. buat file migration trs di migrate
      - Guna file migration : membuat tabel dari laravel
      - PENTING: IDEALNYA 1 TABEL memiliki 1 URL/ROUTE sendiri dengan segala CRUDnya
   3. setting model
      - Guna : 
         - a. bikin relasi antar tabel
         - b. bikin fillable and guarded
   4. setting alur oper tangkap dari route ke controller ke view
      - Proses querynya di controller
   5. reference setting : di file migration :
      1. ONE TO ONE
         - exp: orang > passport ; disini foreign key user ada di passport
      2. ONE TO MANY
         - exp: orang > forum : disini foreign key user ada di forum, soalnya forumnya yang many, makanya referencenya ke orang
      3. MANY TO MANY
         - exp: orang >pelajaran : disini akan dibuat tabel penghubung yang berisi foreign_key dari masing2 tabel yang dihubungkan
   6. guna fungsi relation di Model : buat nyambungin ke Model lain dan di panggil di view exp: {{$user->pelajarans->id}} --> pelajarans ini nama fungsi di Model User buat relasi dari tabel users ke tabel pelajarans      
   7. Model setting: di Model
      1. ONE TO ONE
         - orang hasOne
         - passport belongsTo
      2. ONE TO MANY
         - orang hasMany
         - forum belongsTo
      3. MANY TO MANY
         - orang hasMany
         - pelajaran hasMany
      4. Pivot
      5. Polymorphic    

---
## A INTRO DAN PERSIAPAN
   1. Langkah2 :
      1. Buat databasenya di phpmyadmin
      2. Setting .env untuk databasenya
---
## B INTRO ONE TO ONE
   - dari bikin file dan tabel migrasi sampe lempar tangkap dari route(url) ke controller trs menuju view
   - File2 migration berada di database/migrations
   1. Contoh:
      - Orang > Passport </br>
        Passport > Orang
      - 1 tabel hanya boleh mempunyai 1 item di tabel lain, dan 1 item ini hanya boleh dimiliki oleh 1 coloumn lain
   2. Langkah2 buat tabel user dan passport:
      1. buat table passports: </br>
         ``` php 
         php artisan make:migration create_passports_table --create="passports"
         //create_passports_table --> nama file migrasinya di laravel
         //"passports" --> nama table di databasenya
         ```
      2. file migrasi user sudah langsung ada di laravel
      3. isi coloumn untuk pasports seperti id dan nama + di table passports harus ada foreign key dari id table user, caranya di create_passport_table tambahin:
          ``` php
          $table->integer('user_id')->unsigned();
          $table->foreign('user_id')->references('id') //id tu nama id di tabel user
                ->on('users')->onDelete('cascade');    //users tu nama tabel user
          ```      
      4. php artisan migrate
   3. Langkah2 lempar tangkap buat tabel user:
      1. Buat route:
         ``` php
         - Route::get('user/{id}', 'UserController@showProfile');
              //ngoper {id} dari url ke UserController
         ```     
      2. Buat UserController.php
         - Buat fungsi showProfile($id) :
            - di showProfile :
               ``` php
               return view('user.profile', ['user' => User::findOrFail($id)]);
               //fungsi buat ngarahin ke file profile.blade.php di folder view/user dan ngoper query untuk mencari $id di tabel users yang settingan tabelnya ada di file model User yang nanti disimpan di var user
               ```     
      3. di profile.blade.php:
         ``` html
         <h1>Halo {{$user->name}}</h1>
               //$user dari var user
         ```      
   - Sampe sini Model belum dijelasin
---
## C CARA UBAH LOKASI FILE2 MODEL
   - Biar rapi, kita bisa ngubah tempat semua model berada ke folder Models
   - Langkah2:
      1. Buat folder Models
      2. taro file2 model disana
      3. ubah namespace file model menjadi:
         - `namespace App\Models;`
      4. di controller yang berhubungan dengan modelnya juga diubah usenya:
         - `use App\Models\nama_model;`
---
## D ONE TO ONE
   1. Langkah2 ngehubungin antara tabel use dan tabel passport:
      1. Buat model user dan model passport:
         1. User.php sudah otomatis ada
         2. Passport.php --> `php artisan make:model Passport `
      2. Bikin relasi dari users ke passports
         1. membuat tabel yang ingin dihubungkan menjadi fungsi :
            ``` php
            public function passport()
               {
                 return $this-> hasOne('App\Models\Passport');
                     //ini jika konfensional which means jika foreign keynya == user_id atau local keynya == id

                 return $this-> hasOne('App\Models\Passport', 'foreign_key', 'local_id');
                     //ini jika nggk konfensional which means jika foreign keynya != user_id atau local keynya != id
                }
            ```    
         2. Cara menngunakan di profile.blade.php:
            ``` php
            <h1>Halo {{$user->name}} dengan nomor passportnya {{user->passport->no_pass}}</h1>
            //-->passport itu nama fungsi di Model User yang menghubungkannya dengan tabel passports
            //no_pass itu property yang mw kita akses
            ```   
      3. Bikin relasi dari passport ke user (kebalikannya)
         1. di route:
            ``` php
            Route::get('passport/{id}','UserController@showPassport');
            //tetap di UserController
            ```    
         2. di UserController:
            ``` php
            public function showPassport($id)
            {
              return view('user.passport', ['passport'=>'Passport::findOrFail($id)']);
            }
            ```
         3. di passport.blade.php
            ``` php
            <h1>Pemilik Passport ini adalah{{$passport->user->name}} dengan nomor passportnya {{passport->no_pass}}</h1>
            ```
         4. di Model Passport.php:
            - Buat function untuk tabel user:
               ``` php
               public function user
               {
                  return $this->belongsTo('App\Models\User');
                  //passport ini punya user
               }
               ```
---               
## E ONE TO MANY
   1. Contoh :
      - user > postingan </br>
       postingan > user
      - 1 user bisa bikin banyak postingan dan 1 postingan hanya bisa dibikin oleh 1 user
   2. Langkahnya mirip sama one to one:
      1. bikin file migrasi postingan dan modelnya, isi biasa, dan kasih reference ke user di file migrasi postingan sebagai foreign key:
         ``` php
         $table->integer('user_id')->unsigned();
         $table->foreign('user_id')->references('id') //id tu nama id di tabel user
               ->on('users')->onDelete('cascade');    //users tu nama tabel user
         ```        
      2. `php artisan migrate`
      3. di Modelsnya:
         1. di Model Forum.php kasih fungsi user dengan `belongsTo`:  // karena forum punya si user
            ``` php
            public function user(){
               return $this->belongsTo('App\Models\User');
            }
            ```
         2. di Model User.php kasih fungsi forums dengan hasMany:
            ``` php
            public function forums(){
               return $this->hasMany('App\Models\Forum');
            }
            ```
      4. lempar tangkap ke profile.blade.php, karena ini di profile.blade.php yang kita tangkap adalah id user yang mempunyai postingan ini dan itu:
      ``` php
       <ul>
         @foreach($user->forums as $forum)
          <li>{{$forum->title}}</li>
         @endforeach
       </ul>
      ```
---
## F MANY TO MANY
   1. Contoh:
      - user > pelajaran </br>
        pelajaran > user
      - 1 user bisa milih banyak pelajaran, vice versa
   2. Langkah2:
      - ada sedikit perbedaan disini, kita harus membuat 3 tabel: tabel users, tabel pelajarans, dan tabel pelajaran_user(nama konvensionalnya) sebagai penghubung tabel users dan pelajaran
      1. Bikin file migrasi:
            - `php artisan make:migration create_pelajarans_table --create="pelajarans"`
            -  `php artisan make:migration create_pelajaran_user_table --create="pelajaran_user"`
      2. Isi file migrasinya dengan:
         1. di create_pelajarans_table: normal aja kaya biasa
            ``` php
            $table->increments(id);
            $table->string('title', 100);
            $table->timestamps();
            ```
         2. di create_pelajaran_user_table:
            - minimal harus ada idnya , foreign_key untuk users, dan foreign_key untuk pelajarans
            ``` php
            $table->increments(id);

            $table->integer('user_id')->unsigned();
            $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
            //id tu nama id di tabel users,users tu nama tabel user

            $table->integer('pelajaran_id')->unsigned();
            $table->foreign('pelajaran_id')->references('id')->on('pelajarans')->onDelete('cascade');
            //id tu nama id di tabel pelajarans,pelajarans tu nama tabel user

            $table->timestamps();
            ```
         3. `php artisan migrate`
      3. bikin route dan controller dan view untuk pelajaran:
         1. `Route::get('pelajaran/{id}', pelajaranController@showPelajaran);`
         2. Controller:
            - Codes:
               ``` php
               public function showPelajaran($id){
                  return view('pelajaran',['pelajaran' => Pelajaran::findOrFail($id)]);
               }
               ```
            - import model Pelajaran jga di Controller
            - bikin pelajaran.blade.php
      4. Di Model:
         1. di Model User.php bikin relasi untuk pelajarans:
            ```php
            public function pelajarans(){
               return $this->belongsToMany("App\Models\Pelajaran"); 
               //ini jika ngikutin konfensionalnya which means pake users, pelajarans, pelajaran_user(lihat singular pluralnya)

               return $this->belongsToMany("App\Models\Pelajaran", "pelajaran_user",   "usr_id","pelajaran_id");
            //pelajaran_user --> nama tabel penghubung 
            //usr_id dan pelajaran_id --> foreign_key masing2 tabel
            }                                               
            ```
         2. di Model Pelajaran.php:
            ``` php
            public function users(){
            return $this->belongsToMany("App\Models\User");
            }   
            ```
      5. Di Bladenya:
         1. user.blade.php:
            ``` php
            <ul>
              @foreach($user->pelajarans as $pelajaran)
               <li>{{$pelajaran->title}}</li>
              @endforeach
            </ul>
            ```
         2. pelajaran.blade.php:
            ``` php
            <ul>
              @foreach($pelajaran->users as $user)
               <li>{{$user->name}}</li>
              @endforeach
            </ul>
            ```
---
## G PIVOT PADA MANY TO MANY
   - Secara default pada tabel penghubung many to many (tabel pelajaran_user), laravel nggk akan peduli kolom2 lain selain kolom penghubung antar tabel.
   - Oleh karena itu kita punya property bernama pivot untuk  mengakses apa aja kolom yang ada di pelajaran_user.
   - Langkah-langkah:
      1. Di blade : Disini pake pelajaran.blade.php:
         ``` php
         <ul>
           @foreach($pelajaran->users as $user)
            <li> {{$user->name}} </li>
            <li> {{$user->pivot->created_at}} </li> //khusus timestamps
            <li> {{$user->pivot->batasPinjam}} </li> //pivot swlain timestamps
           @endforeach
         </ul>
         ```
      2. Di modelnya : Pelajaran.php:

         ``` php
         public function users(){
          return $this->belongsToMany("App\Models\User")->withTimeStamps() //khusus timestamps
                      ->withPivot('batasPinjam'); //isi dengan kolom2 selain timestamps,jika kolom lebi dari 1 pisahin pake koma withPivot('col1','col2','setrusnya');
         }
         ```
         - Tambahan:
            - Jika pengen filter kolom dengan isi tertentu yang ingin ditampilkan :
               ``` php            
               ->withPivot('batasPinjam')->wherePivot('batasPinjam', 'minggu depan'); 
               //jika cuma satu
               ->withPivot('batasPinjam')->wherePivotIn('batasPinjam', ['minggu depan', 'besok']); 
               //masukin dalam array jika data yang ingin ditampilkan lebih dari 1
                 ```
---
## H POLYMORPHIC
   - 1 tabel bisa nampung banyak model lain
   - Contoh :
      likes | |
      ---|---
      id | 
      likeable_id | isi dengan id forum/pelajaran mana yang di like
      likeable_type | App\Models\Forum atau App\Models\Pelajaran (isi dengan model)
   - Like itu polymorphic karena dalam 1 tabel kita bisa like banyak model lain, disini forum dan pelajaran
   - Langkah-langkah :
      1. buat file migration likes : `php artisan make:migration 'create_likes_table' --create="likes"`
      2. di create_likes_table :
         ``` php
         $table->increments(id);
         $table->integer(likeable_id);
         $table->string('likeable_type', 100);
         ``` 
      3. `php artisan:migrate`
      4. Buat Model Like.php : isi dengan :
         ``` php
         public function likeable(){
            return $this->morphTo();
         }
         ```   
      5. Misal pengen di tempel di Forum : di Model Forum.php :
         ``` php
         public function likes(){
            return $this->morphMany('App\Models\Like', 'likeable')
            //'App\Models\Like' --> tabel polimorfinya
            //'likeable' --> fungsi morph di Model like
         }
         ```
      6. di forum.blade.php : buat nampilin likenya berapa :
         ``` php
         <p>Jumlah like : {{$forum->likes->count()}}<p>
         ``` 
---
## POLYMORPHIC MANY TO MANY
   - 1 tabel bisa nampung banyak model lain, dan tabel tersebut punya banyak jenis
   - Contoh :
      Tag | |
      ---|---
      tags |taggables
      ---|---
      id | tag_id
      nama | taggable_id
      -| taggable_type       
   - keterangan :
      - id --> id tagnya
      - nama --> nama tagnya misal : html.css
      - tag_id --> id tag punya tabel tags (buat ngasih tau misal forum dengan dengan id balbla memiliki tag dengan id ini) 
      - taggable_id --> id forum atau pelajran yang dikasih tag
      - taggable_type --> Modelnya App\Models\Forum , dll   
   - Langkah-langkah :
      1. buat file migration tags : `php artisan make:migration 'create_tag_table' --create="tags"` `
      2. buat file migration taggables : `php artisan make:migration 'create_taggables_table' --create="taggables"` ` 
      3. di create_tag_table :
         ``` php
         $table->increments(id);
         $table->string('name', 100);
         ``` 
      4. di create_taggables_table :
         ``` php
         $table->intege(tag_id);
         $table->integer(taggable_id);
         $table->string('taggable_type', 100);
         ```
      5. `php artisan:migrate`   
      6. Misal pengen di tempel di Forum : Buat Model Tag.php : isi dengan :
         ``` php
         public function forums(){
            return $this->morphByMany('App\Models\Forum', 'taggable');
         }
         ```
      7. di Model Forum :
         ``` php
         public function tags(){
            return $this->morphToMany('App\Models\Tag', 'taggable');
         } 
         ```
      8. di forum.blade.php buat nampilin tag-tagnya :
         ``` php
         <ul>
         @foreach($forum->tags as $tag)
            <li>{{ $tag->name }}</li>
         @endforeach   
         </ul>
         ```      
---
## EAGER LOADING
   - Eager loading tu kita ngeload semuanya dari awal, lawannya lazy loading yang manggil query ketika dipanggil.
   - untuk lengkapnya, baca disini :<br/> 
   https://medium.com/@kouipheng.lee/laravel-eloquent-lazy-vs-eager-loaded-803852c59e1c
   -   

         


