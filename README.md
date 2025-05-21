✅ SOLUSI TEKNIS PENGAMANAN DATA MURID DI LARAVEL + FILAMENT
🔒 1. Aktifkan Validasi dan Sanitasi Input
Tujuan: Menghindari input berbahaya (XSS, SQL Injection)
📌 Implementasi:
•	Gunakan FormRequest:
php artisan make:request StoreStudentRequest
public function rules()
{
    return [
        'nim' => 'required|numeric|unique:students,nim',
        'nama' => 'required|string|max:255',
        'fakultas' => 'required|string|max:100',
    ];
}
•	Gunakan di Controller:
public function store(StoreStudentRequest $request)
{
    Student::create($request->validated());
}
🔐 2. Enkripsi Data Sensitif
Tujuan: Melindungi data yang disimpan di database
📌 Contoh:
use Illuminate\Support\Facades\Crypt;

$encryptedNama = Crypt::encryptString($request->nama);
$decryptedNama = Crypt::decryptString($encryptedNama);
Atau gunakan cast otomatis di model:
protected $casts = [
 'nama' => 'encrypted',
];

👤 3. Autentikasi & Hak Akses (Authorization)
Tujuan: Membedakan akses user admin/guru/siswa
📌 Implementasi:
•	Pakai Laravel Breeze atau Jetstream untuk login
•	Buat middleware role:
php artisan make:middleware RoleMiddleware
if (auth()->user()->role !== 'admin') {
    abort(403);
}
•	Daftarkan di Kernel.php dan pasang di route Filament Panel.

🧾 4. Lindungi File .env dan Git
Tujuan: Jangan upload file sensitif seperti .env ke GitHub
📌 Langkah:
•	Pastikan .gitignore berisi:
.env
/vendor
/node_modules
•	Jika sudah pernah ter-push, hapus dengan:
git rm --cached .env

💾 5. Gunakan HTTPS (SSL)
Tujuan: Enkripsi data saat dikirim antar client-server
📌 Langkah:
•	Saat live deployment, gunakan Let's Encrypt atau SSL dari hosting
•	Set APP_URL=https://domainanda.com di .env

🛡️ 6. Aktifkan CSRF Protection
Tujuan: Mencegah serangan CSRF (Cross Site Request Forgery)
📌 Laravel sudah mengaktifkan CSRF di semua form:
<form method="POST">
  @csrf
</form>

📋 7. Audit Log Akses Data
Tujuan: Lacak siapa mengakses/mengubah data murid
📌 Install package:
composer require spatie/laravel-activitylog
📌 Di model Student:
use Spatie\Activitylog\Traits\LogsActivity;

class Student extends Model
{
    use LogsActivity;

    protected static $logAttributes = ['nim', 'nama', 'fakultas'];
    protected static $logName = 'student';
}

☁️ 8. Backup Database Rutin
Tujuan: Jika data hilang atau corrupt, bisa dikembalikan
📌 Gunakan Laravel Backup (Spatie):
composer require spatie/laravel-backup

🔚 Penutup
Langkah-langkah ini akan memberikan keamanan berlapis terhadap data murid, baik saat input, penyimpanan, maupun akses. Pastikan juga Anda:
•	Update sistem & dependensi secara rutin
•	Gunakan autentikasi 2 faktor (2FA) di GitHub dan server Anda
Jika Anda ingin, saya juga bisa bantu generate implementasi kode langsung sesuai struktur proyek Anda sekarang. Tinggal bilang saja.

