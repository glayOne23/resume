1. hubungin laptop ke ssh, masuk ke public_html tempat domain berada
2. pastikan sudah terinstall git dan composer
3. buat codingan di local jadi repo git di github/gitlab
4. Upload di hosting :
	1. balik ke domain dari public_html `cd ..`
		- kenapa disimpan disini, nggk di public_html? karena laravel punya folder publiknya sendiri. kalo semua disimpan di public_html, nanti semua script laravel bsa diakses dan routingannya bisa nggk jalan 
	2. hapus public_html (kita akan tetap bikin public_html, tapi istilahnya shortcut/symbolic link)
		- `rm public_html -rf`
	3. `git clone url_git_clone`
	4. buat symbolic link ke publicnya code laravel
		- `ln -s alamat_public_laravel alamat_symbolic_link`
		- `ln -s /home/ufuisd54535/domains/webprogrammingunpas.tech/wpu-laravel/ public /home/ufuisd54535/domains/webprogrammingunpas.tech/wpu-laravel/public_html`
		   
	5. install vendor:
		- `cd wpu-laravel && composer install`
	6. benerin env nya :
		- `cp .env.example .env`
		- `php artisan key:generate`
		- urus database (cara salahnya itu export import, cara benernya pake migration dan seeder) 	
		  
	