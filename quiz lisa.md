##### 1. Guna route, controller, model, view/blade iku apa?
##### 2. gimana alur web itu bisa sampe tampil di laravel? cara lempar tangkapnya pye?
##### 3. di `web.php(route)`, maksud kode ini apa :
    ```php
    Route::get('admin/visi-misi', 'VisiMisiController@index');
    Route::get('admin/visi-misi/create', 'VisiMisiController@create');
    Route::post('admin/visi-misi', 'VisiMisiController@store');
    ```
- kenapa codingan baris ke satu dan dua diatas pake get dan baris ketiga pake post?
- maksud iki apa 'admin/visi-misi' di codingan baris pertama?
- maksud iki apa 'VisiMisiController@index' di codingan pertama?
##### 4. di VisiMisiController (Tentang Controller):
```php
<?php

namespace App\Http\Controllers;

use App\Models\VisiMisi; 
use Illuminate\Http\Request;

class VisiMisiController extends Controller
{
    
    public function index() // 4.1 index() klo diganti namanya boleh nggk? kenapa?
    {
        $visi_misi = VisiMisi::all(); // 4.2 baris ini maksudnya apa?  
        return view('admin.visimisi.index', compact('$visi_misi'));
        // 4.3 guna view() apa?
        // 4.4 'admin.visimisi.index' syntax iki maksudnya apa?
        // 4.5 compact() itu buat aoa?
        // 4.6 compact('$visi_misi')); syntax iki maksudnya apa?
    }
    
    public function create()
    {
        return view('admin.blogs.create');
    }

    
    public function store(Request $request)
    {
        //4.7 kode dibawah buat apa?
        $errors = $request->validate([
            'visi' => 'required',
            'misi' => 'required',
        ]);
    
        // kode dibawah disini ngapain?
        $visi_misi = VisiMisi::create([
            'visi' => request('input1'),
            //4.8 maksud 'visi' merujuk kemana?
            //4.9 request('input1') syntax iki maksudnya apa?
            'misi' => request('input2'),
        ]);

        return redirect('admin/visi-misi');    
    }
}
```
##### 5. di VisiMisi.php (Tentang Model)
```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class VisiMisi extends Model
{
    protected $fillable = ['visi', 'misi']; //5.1 baris ini buar apa?

    //5.2 baris dibawah buar apa dan gimana makenya?
    public function paslon(){
        return $this->BelogsTo('App\Models\Paslon', 'paslon_id', 'id'); 
    }
}
```
##### 6. Tentang View
1. @foreach buat apa?
2. jika ada variable yg dilempar dari Controller, cara nangkepnya gimana?



