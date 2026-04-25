# FinalProjectPy3

import discord
import sqlite3
import random
from discord.ext import commands
from discord import app_commands

TOKEN = "TOKEN"
GUILD_ID = 1492524467576901632

intents = discord.Intents.all()
bot = commands.Bot(command_prefix="!", intents=intents)

# ================= STREAK FORMAT =================
def format_streak(n):
    if n >= 10:
        return f"{n}🔥🔥🔥"
    elif n >= 6:
        return f"{n}🔥🔥"
    elif n >= 1:
        return f"{n}🔥"
    else:
        return str(n)

# ================= DATABASE =================
conn = sqlite3.connect("quiz.db", check_same_thread=False)
cursor = conn.cursor()

cursor.execute("""
CREATE TABLE IF NOT EXISTS questions (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    kategori TEXT,
    question TEXT,
    option_a TEXT,
    option_b TEXT,
    option_c TEXT,
    option_d TEXT,
    answer TEXT
)
""")

cursor.execute("""
CREATE TABLE IF NOT EXISTS leaderboard (
    user_id INTEGER,
    username TEXT,
    kategori TEXT,
    score INTEGER DEFAULT 0,
    PRIMARY KEY (user_id, kategori)
)
""")

conn.commit()

# ================= SEED =================
def seed_data():
    cursor.execute("SELECT COUNT(*) FROM questions")
    if cursor.fetchone()[0] == 0:
        cursor.executemany("""
        INSERT INTO questions VALUES (NULL, ?, ?, ?, ?, ?, ?, ?)
        """, [
        ('matematika', '6 * 5 + 12 = ?', '42'),
        ('matematika', '80 / 8 + 6 * 2 = ?', '22'),
        ('matematika', '7^2 - 3 * 5 = ?', '34'),
        ('matematika', '9 * (6 - 2) + 8 = ?', '44'),
        ('matematika', '60 / 5 + 7 * 3 = ?', '33'),
        ('matematika', '8 * 7 + 10 = ?', '66'),
        ('matematika', '90 - 7 * 6 = ?', '48'),
        ('matematika', '4^2 + 8 * 3 = ?', '40'),
        ('matematika', '72 / 9 + 5 * 4 = ?', '28'),
        ('matematika', '5 * 11 + 9 = ?', '64'),

        ('matematika', '15 + 6 * 4 = ?', '39'),
        ('matematika', '72 / 8 + 5 * 3 = ?', '29'),
        ('matematika', '9^2 - 5 * 6 = ?', '51'),
        ('matematika', '8 * (10 - 6) + 7 = ?', '39'),
        ('matematika', '100 / 5 + 12 * 2 = ?', '44'),
        ('matematika', '7 * 8 - 12 = ?', '44'),
        ('matematika', '9 * 6 + 15 = ?', '69'),
        ('matematika', '50 - 8 * 4 = ?', '18'),
        ('matematika', '6^2 + 4 * 5 = ?', '56'),
        ('matematika', '81 / 9 + 7 * 2 = ?', '23'),

        ('matematika', '25 + 5 * 6 = ?', '55'),
        ('matematika', '90 / 10 + 8 * 3 = ?', '33'),
        ('matematika', '8^2 - 4 * 7 = ?', '36'),
        ('matematika', '6 * (9 - 5) + 10 = ?', '34'),
        ('matematika', '120 / 6 + 3 * 5 = ?', '35'),
        ('matematika', '4 * 9 + 18 = ?', '54'),
        ('matematika', '70 - 6 * 5 = ?', '40'),
        ('matematika', '5^2 + 6 * 4 = ?', '49'),
        ('matematika', '64 / 8 + 9 * 2 = ?', '26'),
        ('matematika', '3 * 12 + 15 = ?', '51'),

        ('matematika', 'Luas persegi sisi 8 = ?', '64'),
        ('matematika', 'Keliling persegi sisi 7 = ?', '28'),
        ('matematika', 'Luas persegi panjang 10 x 5 = ?', '50'),
        ('matematika', 'Keliling persegi panjang 12 x 4 = ?', '32'),
        ('matematika', 'Luas segitiga alas 10 tinggi 6 = ?', '30'),
        ('matematika', 'Keliling segitiga 6+7+8 = ?', '21'),
        ('matematika', 'Luas lingkaran r=7 (22/7) = ?', '154'),
        ('matematika', 'Keliling lingkaran r=7 (22/7) = ?', '44'),
        ('matematika', 'Luas persegi sisi 12 = ?', '144'),
        ('matematika', 'Keliling persegi panjang 15 x 5 = ?', '40'),

        ('matematika', 'Luas persegi sisi 9 = ?', '81'),
        ('matematika', 'Keliling persegi sisi 10 = ?', '40'),
        ('matematika', 'Luas persegi panjang 20 x 3 = ?', '60'),
        ('matematika', 'Keliling persegi panjang 8 x 6 = ?', '28'),
        ('matematika', 'Luas segitiga alas 12 tinggi 4 = ?', '24'),
        ('matematika', 'Luas persegi sisi 6 = ?', '36'),
        ('matematika', 'Keliling persegi sisi 9 = ?', '36'),
        ('matematika', 'Luas persegi panjang 7 x 8 = ?', '56'),
        ('matematika', 'Keliling persegi panjang 9 x 5 = ?', '28'),
        ('matematika', 'Luas segitiga alas 14 tinggi 5 = ?', '35'),

        ('matematika', '2x + 5 = 15, x = ?', '5'),
        ('matematika', '3x = 21, x = ?', '7'),
        ('matematika', 'x + 12 = 20, x = ?', '8'),
        ('matematika', '5x - 10 = 15, x = ?', '5'),
        ('matematika', '2x + 4 = 10, x = ?', '3'),
        ('matematika', '4x = 32, x = ?', '8'),
        ('matematika', 'x - 7 = 5, x = ?', '12'),
        ('matematika', '6x = 48, x = ?', '8'),
        ('matematika', '3x + 6 = 15, x = ?', '3'),
        ('matematika', 'x / 5 = 4, x = ?', '20'),

        ('matematika', '2x + 3 = 11, x = ?', '4'),
        ('matematika', '3x - 6 = 9, x = ?', '5'),
        ('matematika', 'x + 7 = 18, x = ?', '11'),
        ('matematika', '4x - 8 = 8, x = ?', '4'),
        ('matematika', '5x = 45, x = ?', '9'),
        ('matematika', 'x - 9 = 6, x = ?', '15'),
        ('matematika', '2x = 18, x = ?', '9'),
        ('matematika', 'x + 5 = 13, x = ?', '8'),
        ('matematika', '3x + 3 = 12, x = ?', '3'),
        ('matematika', '4x = 20, x = ?', '5'),

        ('matematika', '6 * 5 + 12 = ?', '42', '40', '45', '38', '42'),
        ('matematika', '80 / 8 + 6 * 2 = ?', '22', '20', '24', '18', '22'),
        ('matematika', '7^2 - 3 * 5 = ?', '34', '30', '40', '36', '34'),
        ('matematika', '9 * (6 - 2) + 8 = ?', '44', '40', '48', '42', '44'),
        ('matematika', '60 / 5 + 7 * 3 = ?', '33', '30', '36', '28', '33'),
        ('matematika', '8 * 7 + 10 = ?', '66', '60', '70', '64', '66'),
        ('matematika', '90 - 7 * 6 = ?', '48', '50', '45', '52', '48'),
        ('matematika', '4^2 + 8 * 3 = ?', '40', '36', '44', '38', '40'),
        ('matematika', '72 / 9 + 5 * 4 = ?', '28', '30', '26', '24', '28'),
        ('matematika', '5 * 11 + 9 = ?', '64', '60', '66', '62', '64'),

        ('ips', 'Kegiatan ekonomi yang menghasilkan barang disebut ....', 'produksi', 'distribusi', 'konsumsi', 'investasi', 'produksi'),
        ('ips', 'Kegiatan menyalurkan barang dari produsen ke konsumen disebut ....', 'distribusi', 'produksi', 'konsumsi', 'ekspor', 'distribusi'),
        ('ips', 'Kegiatan menggunakan barang atau jasa disebut ....', 'konsumsi', 'produksi', 'distribusi', 'perdagangan', 'konsumsi'),
        ('ips', 'Petani termasuk pelaku kegiatan ....', 'produksi', 'distribusi', 'konsumsi', 'investasi', 'produksi'),
        ('ips', 'Pedagang termasuk pelaku kegiatan ....', 'distribusi', 'produksi', 'konsumsi', 'industri', 'distribusi'),
        ('ips', 'Orang yang memakai barang disebut ....', 'konsumen', 'produsen', 'distributor', 'investor', 'konsumen'),
        ('ips', 'Orang yang membuat barang disebut ....', 'produsen', 'konsumen', 'distributor', 'pedagang', 'produsen'),
        ('ips', 'Tempat bertemunya penjual dan pembeli disebut ....', 'pasar', 'sekolah', 'rumah', 'kantor', 'pasar'),
        ('ips', 'Kegiatan mengirim barang disebut ....', 'distribusi', 'produksi', 'konsumsi', 'eksplorasi', 'distribusi'),
        ('ips', 'Tujuan kegiatan ekonomi adalah ....', 'memenuhi kebutuhan', 'bersenang-senang', 'bermain', 'berlibur', 'memenuhi kebutuhan'),
        ('ips', 'Batik merupakan warisan budaya berupa ....', 'kain tradisional', 'alat musik', 'tarian modern', 'bangunan', 'kain tradisional'),
        ('ips', 'Tari Saman berasal dari ....', 'Aceh', 'Jawa Barat', 'Bali', 'Papua', 'Aceh'),
        ('ips', 'Wayang kulit berasal dari ....', 'Jawa', 'Sumatra', 'Kalimantan', 'Sulawesi', 'Jawa'),
        ('ips', 'Angklung berasal dari ....', 'Jawa Barat', 'Jawa Timur', 'Bali', 'NTT', 'Jawa Barat'),
        ('ips', 'Reog Ponorogo berasal dari ....', 'Jawa Timur', 'Jawa Tengah', 'Bali', 'Lampung', 'Jawa Timur'),
        ('ips', 'Rumah Gadang berasal dari ....', 'Sumatra Barat', 'Jawa', 'Bali', 'Sulawesi', 'Sumatra Barat'),
        ('ips', 'Tari Kecak berasal dari ....', 'Bali', 'Aceh', 'Jawa', 'Papua', 'Bali'),
        ('ips', 'Lagu daerah “Ampar-Ampar Pisang” berasal dari ....', 'Kalimantan Selatan', 'Bali', 'Jawa Barat', 'Papua', 'Kalimantan Selatan'),
        ('ips', 'Batik diakui UNESCO sebagai ....', 'warisan budaya tak benda', 'bangunan bersejarah', 'alat musik', 'makanan', 'warisan budaya tak benda'),
        ('ips', 'Wayang diakui UNESCO sebagai ....', 'warisan budaya tak benda', 'alam dunia', 'gedung bersejarah', 'produk ekonomi', 'warisan budaya tak benda'),
        ('ips', 'Produk Indonesia yang terkenal di dunia adalah ....', 'kopi', 'gandum', 'keju', 'wine', 'kopi'),
        ('ips', 'Kopi Gayo berasal dari ....', 'Aceh', 'Bali', 'Jawa Barat', 'Sulawesi', 'Aceh'),
        ('ips', 'Rendang berasal dari ....', 'Sumatra Barat', 'Jawa Timur', 'Bali', 'Papua', 'Sumatra Barat'),
        ('ips', 'Batik terkenal sebagai produk ....', 'tekstil', 'logam', 'elektronik', 'plastik', 'tekstil'),
        ('ips', 'Cengkeh adalah hasil ....', 'perkebunan', 'perikanan', 'pertambangan', 'industri', 'perkebunan'),
        ('ips', 'Indonesia dikenal sebagai negara penghasil ....', 'rempah-rempah', 'salju', 'gandum utama', 'minyak zaitun', 'rempah-rempah'),
        ('ips', 'Produk makanan Indonesia yang mendunia adalah ....', 'rendang', 'pizza', 'burger', 'sushi', 'rendang'),
        ('ips', 'UNESCO mengakui warisan dunia untuk melestarikan ....', 'budaya dan alam', 'militer', 'perdagangan', 'politik', 'budaya dan alam'),
        ('ips', 'Candi Borobudur adalah warisan ....', 'dunia', 'lokal', 'pribadi', 'ekonomi', 'dunia'),
        ('ips', 'Taman Nasional Komodo termasuk warisan ....', 'alam dunia', 'budaya', 'industri', 'ekonomi', 'alam dunia'),
        ('ips', 'Komodo hanya hidup di ....', 'Indonesia', 'India', 'Australia', 'Afrika', 'Indonesia'),
        ('ips', 'UNESCO berada di bawah organisasi ....', 'PBB', 'ASEAN', 'NATO', 'OPEC', 'PBB'),
        ('ips', 'Tujuan UNESCO adalah melestarikan ....', 'pendidikan dan budaya', 'militer', 'ekonomi', 'perdagangan', 'pendidikan dan budaya'),
        ('ips', 'Warisan dunia disebut ....', 'World Heritage', 'World Trade', 'World Bank', 'World Market', 'World Heritage'),
        ('ips', 'Candi Prambanan adalah candi ....', 'Hindu', 'Buddha', 'Islam', 'Kristen', 'Hindu'),
        ('ips', 'Borobudur adalah candi ....', 'Buddha', 'Hindu', 'Islam', 'Kristen', 'Buddha'),
        ('ips', 'Batik berasal dari negara ....', 'Indonesia', 'Malaysia', 'Thailand', 'Filipina', 'Indonesia'),
        ('ips', 'Salah satu alat musik tradisional Indonesia adalah ....', 'angklung', 'piano', 'gitar listrik', 'drum modern', 'angklung'),
        ('ips', 'Rumah adat Minangkabau disebut ....', 'Rumah Gadang', 'Honai', 'Joglo', 'Tongkonan', 'Rumah Gadang'),
        ('ips', 'Tari Piring berasal dari ....', 'Sumatra Barat', 'Jawa Barat', 'Bali', 'Papua', 'Sumatra Barat'),
        ('ips', 'Kegiatan menjual barang disebut ....', 'perdagangan', 'produksi', 'konsumsi', 'pertanian', 'perdagangan'),
        ('ips', 'Nelayan bekerja di bidang ....', 'perikanan', 'pertanian', 'industri', 'perdagangan', 'perikanan'),
        ('ips', 'Peternak bekerja di bidang ....', 'peternakan', 'perdagangan', 'industri', 'jasa', 'peternakan'),
        ('ips', 'Petani menghasilkan ....', 'hasil pertanian', 'barang elektronik', 'kendaraan', 'mesin', 'hasil pertanian'),
        ('ips', 'Pekerjaan guru termasuk bidang ....', 'jasa', 'produksi', 'pertanian', 'perdagangan', 'jasa'),
        ('ips', 'Dokter termasuk pekerjaan di bidang ....', 'jasa', 'industri', 'pertanian', 'perdagangan', 'jasa'),
        ('ips', 'Pabrik termasuk kegiatan ....', 'produksi', 'konsumsi', 'distribusi', 'transportasi', 'produksi'),
        ('ips', 'Supir truk membantu kegiatan ....', 'distribusi', 'produksi', 'konsumsi', 'pertanian', 'distribusi'),
        ('ips', 'Toko adalah tempat kegiatan ....', 'perdagangan', 'produksi', 'pertanian', 'industri', 'perdagangan'),
        ('ips', 'Kegiatan ekonomi yang paling awal adalah ....', 'produksi', 'distribusi', 'konsumsi', 'perdagangan', 'produksi'),
        ('ips', 'UNESCO mengakui batik pada tahun ....', '2009', '2010', '2005', '2015', '2009'),
        ('ips', 'Warisan dunia dapat berupa ....', 'budaya dan alam', 'uang', 'kendaraan', 'teknologi', 'budaya dan alam'),
        ('ips', 'Indonesia kaya akan ....', 'budaya', 'salju', 'gurun', 'es abadi', 'budaya'),
        ('ips', 'Produk Indonesia yang diekspor adalah ....', 'kopi', 'es krim impor', 'keju Eropa', 'gandum Amerika', 'kopi'),
        ('ips', 'Ekspor berarti menjual ke ....', 'luar negeri', 'dalam negeri', 'tetangga', 'desa', 'luar negeri'),
        ('ips', 'Impor berarti membeli dari ....', 'luar negeri', 'dalam negeri', 'pasar lokal', 'desa', 'luar negeri'),

        ('bhsIndo', 'Kalimat majemuk bertingkat adalah kalimat yang terdiri dari ....', 'induk dan anak kalimat', 'dua kalimat tunggal', 'satu kata kerja', 'dua subjek', 'induk dan anak kalimat'),
        ('bhsIndo', 'Kalimat utama dalam paragraf biasanya berisi ....', 'ide pokok', 'contoh', 'penjelasan tambahan', 'kata sambung', 'ide pokok'),
        ('bhsIndo', 'Ide pokok disebut juga ....', 'gagasan utama', 'kalimat penjelas', 'contoh kalimat', 'judul', 'gagasan utama'),
        ('bhsIndo', 'Kalimat penjelas berfungsi untuk ....', 'mengembangkan ide pokok', 'menghapus ide pokok', 'mengganti judul', 'menutup paragraf', 'mengembangkan ide pokok'),
        ('bhsIndo', 'Teks eksposisi bertujuan untuk ....', 'memberikan informasi', 'menghibur', 'menipu', 'bercerita', 'memberikan informasi'),
        ('bhsIndo', 'Kalimat majemuk bertingkat menggunakan kata penghubung ....', 'karena', 'dan', 'atau', 'tetapi', 'karena'),
        ('bhsIndo', 'Contoh kalimat majemuk bertingkat adalah ....', 'Saya belajar karena ingin pintar', 'Saya makan dan minum', 'Dia datang lalu pergi', 'Kami bermain dan pulang', 'Saya belajar karena ingin pintar'),
        ('bhsIndo', 'Kalimat utama disebut juga ....', 'kalimat topik', 'kalimat penjelas', 'kalimat akhir', 'kalimat tambahan', 'kalimat topik'),
        ('bhsIndo', 'Ide pokok terdapat pada ....', 'kalimat utama', 'kalimat penjelas', 'judul saja', 'kata acak', 'kalimat utama'),
        ('bhsIndo', 'Teks eksposisi bersifat ....', 'informatif', 'imajinatif', 'dongeng', 'hiburan', 'informatif'),
        ('bhsIndo', 'Imbuhan pe- pada kata "pelari" berarti ....', 'orang yang melakukan kegiatan', 'tempat', 'alat', 'hasil', 'orang yang melakukan kegiatan'),
        ('bhsIndo', 'Imbuhan pe- pada kata "petani" berarti ....', 'orang yang bekerja', 'tempat kerja', 'alat kerja', 'hasil kerja', 'orang yang bekerja'),
        ('bhsIndo', 'Imbuhan pe- pada kata "pelukis" berarti ....', 'orang yang melukis', 'alat lukis', 'tempat lukis', 'hasil lukisan', 'orang yang melukis'),
        ('bhsIndo', 'Kata "pembaca" berasal dari kata dasar ....', 'baca', 'membaca', 'bacaan', 'terbaca', 'baca'),
        ('bhsIndo', 'Kata "penulis" berasal dari kata dasar ....', 'tulis', 'menulis', 'tertulis', 'penulisan', 'tulis'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "pembangunan" berarti ....', 'proses membangun', 'orang membangun', 'alat bangun', 'tempat bangun', 'proses membangun'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "penyelesaian" berarti ....', 'proses menyelesaikan', 'orang selesai', 'alat selesai', 'tempat selesai', 'proses menyelesaikan'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "penelitian" berarti ....', 'proses meneliti', 'orang meneliti', 'alat teliti', 'tempat teliti', 'proses meneliti'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "perubahan" berarti ....', 'proses berubah', 'orang berubah', 'alat berubah', 'tempat berubah', 'proses berubah'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "penciptaan" berarti ....', 'proses mencipta', 'orang mencipta', 'alat cipta', 'hasil ciptaan', 'proses mencipta'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "pengolahan" berasal dari kata dasar ....', 'olah', 'mengolah', 'terolah', 'olah-olah', 'olah'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "pembelajaran" berarti ....', 'proses belajar', 'orang belajar', 'alat belajar', 'tempat belajar', 'proses belajar'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "pemeriksaan" berarti ....', 'proses memeriksa', 'orang memeriksa', 'alat periksa', 'tempat periksa', 'proses memeriksa'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "peningkatan" berarti ....', 'proses menaikkan', 'orang naik', 'alat naik', 'tempat naik', 'proses menaikkan'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "pengawasan" berarti ....', 'proses mengawasi', 'orang awas', 'alat awas', 'tempat awas', 'proses mengawasi'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "penyimpanan" berarti ....', 'proses menyimpan', 'orang menyimpan', 'alat simpan', 'tempat simpan', 'proses menyimpan'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "pengumuman" berarti ....', 'proses mengumumkan', 'orang umum', 'alat umum', 'tempat umum', 'proses mengumumkan'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "penilaian" berarti ....', 'proses menilai', 'orang nilai', 'alat nilai', 'tempat nilai', 'proses menilai'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "penyusunan" berarti ....', 'proses menyusun', 'orang susun', 'alat susun', 'tempat susun', 'proses menyusun'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "pencatatan" berarti ....', 'proses mencatat', 'orang catat', 'alat catat', 'tempat catat', 'proses mencatat'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "pengaturan" berarti ....', 'proses mengatur', 'orang atur', 'alat atur', 'tempat atur', 'proses mengatur'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "pembentukan" berarti ....', 'proses membentuk', 'orang bentuk', 'alat bentuk', 'tempat bentuk', 'proses membentuk'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "penyaringan" berarti ....', 'proses menyaring', 'orang saring', 'alat saring', 'tempat saring', 'proses menyaring'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "pemanfaatan" berarti ....', 'proses memanfaatkan', 'orang manfaat', 'alat manfaat', 'tempat manfaat', 'proses memanfaatkan'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "penyebaran" berarti ....', 'proses menyebar', 'orang sebar', 'alat sebar', 'tempat sebar', 'proses menyebar'),
        ('bhsIndo', 'Ide pokok dapat ditemukan dengan cara ....', 'membaca paragraf', 'melihat gambar', 'membaca judul saja', 'menghapus kalimat', 'membaca paragraf'),
        ('bhsIndo', 'Kalimat utama berisi ....', 'gagasan utama', 'contoh', 'penjelasan', 'kata sambung', 'gagasan utama'),
        ('bhsIndo', 'Kalimat penjelas berfungsi untuk ....', 'menjelaskan ide pokok', 'menghapus ide pokok', 'mengganti paragraf', 'mengakhiri teks', 'menjelaskan ide pokok'),
        ('bhsIndo', 'Teks eksposisi menggunakan pola ....', 'fakta dan penjelasan', 'cerita fiksi', 'puisi', 'dialog', 'fakta dan penjelasan'),
        ('bhsIndo', 'Kalimat majemuk bertingkat terdiri dari ....', 'induk dan anak kalimat', 'dua kata', 'satu kata', 'tanpa subjek', 'induk dan anak kalimat'),
        ('bhsIndo', 'Kalimat majemuk bertingkat memiliki hubungan ....', 'sebab akibat', 'acak', 'tanpa makna', 'tidak jelas', 'sebab akibat'),
        ('bhsIndo', 'Kalimat penjelas biasanya berada setelah ....', 'kalimat utama', 'judul', 'subjek', 'kata kerja', 'kalimat utama'),
        ('bhsIndo', 'Kalimat utama bisa berada di ....', 'awal atau akhir paragraf', 'tengah kata', 'akhir kata', 'judul saja', 'awal atau akhir paragraf'),
        ('bhsIndo', 'Teks eksposisi berisi ....', 'informasi', 'dongeng', 'cerita fiksi', 'pantun', 'informasi'),
        ('bhsIndo', 'Ide pokok adalah ....', 'inti paragraf', 'judul cerita', 'kalimat acak', 'kata sambung', 'inti paragraf'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "pemilihan" berarti ....', 'proses memilih', 'orang memilih', 'alat pilih', 'tempat pilih', 'proses memilih'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "pengiriman" berarti ....', 'proses mengirim', 'orang kirim', 'alat kirim', 'tempat kirim', 'proses mengirim'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "penyusutan" berarti ....', 'proses menyusut', 'orang susut', 'alat susut', 'tempat susut', 'proses menyusut'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "penggantian" berarti ....', 'proses mengganti', 'orang ganti', 'alat ganti', 'tempat ganti', 'proses mengganti'),
        ('bhsIndo', 'Imbuhan pe-an pada kata "pemrosesan" berarti ....', 'proses memproses', 'orang proses', 'alat proses', 'tempat proses', 'proses memproses')
        ])
        conn.commit()

seed_data()

# ================= DATABASE FUNCTION =================
def get_question(kategori):
    cursor.execute("""
    SELECT question, option_a, option_b, option_c, option_d, answer
    FROM questions
    WHERE kategori = ?
    ORDER BY RANDOM()
    LIMIT 1
    """, (kategori,))
    
    row = cursor.fetchone()
    if not row:
        return None

    options = [row[1], row[2], row[3], row[4]]
    random.shuffle(options)

    return {
        "question": row[0],
        "options": options,
        "answer": row[5]
    }

# ================= LEADERBOARD =================
def update_score(user_id, username, kategori, score):
    cursor.execute(
        "SELECT score FROM leaderboard WHERE user_id = ? AND kategori = ?",
        (user_id, kategori)
    )
    result = cursor.fetchone()

    if result:
        cursor.execute(
            "UPDATE leaderboard SET score = score + ?, username = ? WHERE user_id = ? AND kategori = ?",
            (score, username, user_id, kategori)
        )
    else:
        cursor.execute(
            "INSERT INTO leaderboard (user_id, username, kategori, score) VALUES (?, ?, ?, ?)",
            (user_id, username, kategori, score)
        )

    conn.commit()

def get_leaderboard(kategori):
    cursor.execute("""
    SELECT username, score 
    FROM leaderboard 
    WHERE kategori = ?
    ORDER BY score DESC 
    LIMIT 10
    """, (kategori,))
    return cursor.fetchall()

# ================= QUIZ VIEW =================
class QuizView(discord.ui.View):
    def __init__(self, kategori, user, streak=0):
        super().__init__(timeout=30)
        self.kategori = kategori
        self.user = user
        self.streak = streak

        self.question_data = get_question(kategori)

        if not self.question_data:
            self.stop()
            return

        for option in self.question_data["options"]:
            self.add_item(QuizButton(option, self))

# ================= BUTTON =================
class QuizButton(discord.ui.Button):
    def __init__(self, label, view):
        super().__init__(label=label, style=discord.ButtonStyle.primary)
        self.quiz_view = view

    async def callback(self, interaction: discord.Interaction):
        view = self.quiz_view
        correct = view.question_data["answer"]

        for item in view.children:
            item.disabled = True
            if item.label == correct:
                item.style = discord.ButtonStyle.success
            elif item.label == self.label:
                item.style = discord.ButtonStyle.danger

        # ================= BENAR =================
        if self.label == correct:
            view.streak += 1

            new_view = QuizView(view.kategori, view.user, view.streak)

            embed = discord.Embed(
                title=f"🔥 Streak {format_streak(view.streak)}",
                description=new_view.question_data["question"],
                color=discord.Color.blue()
            )

            await interaction.response.edit_message(
                content=f"✅ BENAR! +1 SCORE 🔥 | Streak: {format_streak(view.streak)}",
                embed=embed,
                view=new_view
            )

        # ================= SALAH =================
        else:
            update_score(
                interaction.user.id,
                interaction.user.name,
                view.kategori,
                view.streak
            )

            await interaction.response.edit_message(
                content=f"❌ Salah! Jawaban: {correct}\n🔥 Streak: {format_streak(view.streak)}",
                view=view
            )

# ================= MODE =================
class ModeView(discord.ui.View):
    def __init__(self, user):
        super().__init__(timeout=30)
        self.user = user

    @discord.ui.button(label="Matematika")
    async def matematika(self, interaction: discord.Interaction, button: discord.ui.Button):
        await start_quiz(interaction, "matematika")

    @discord.ui.button(label="Bahasa Indonesia")
    async def ipa(self, interaction: discord.Interaction, button: discord.ui.Button):
        await start_quiz(interaction, "bhsIndo")

    @discord.ui.button(label="IPS")
    async def ips(self, interaction: discord.Interaction, button: discord.ui.Button):
        await start_quiz(interaction, "ips")

# ================= START QUIZ =================
async def start_quiz(interaction, kategori):
    view = QuizView(kategori, interaction.user)

    embed = discord.Embed(
        title="Quiz Dimulai",
        description=view.question_data["question"],
        color=discord.Color.green()
    )

    await interaction.response.edit_message(embed=embed, view=view)

# ================= COMMAND QUIZ =================
@bot.tree.command(name="quiz")
@app_commands.guilds(discord.Object(id=GUILD_ID))
async def quiz(interaction: discord.Interaction):
    embed = discord.Embed(
        title="Pilih Kategori",
        description="Silakan pilih:",
        color=discord.Color.blurple()
    )
    await interaction.response.send_message(embed=embed, view=ModeView(interaction.user))

# ================= LEADERBOARD =================
@bot.tree.command(name="leaderboard")
@app_commands.describe(kategori="matematika / ipa / ips")
@app_commands.guilds(discord.Object(id=GUILD_ID))
async def leaderboard(interaction: discord.Interaction, kategori: str):
    data = get_leaderboard(kategori)

    if not data:
        await interaction.response.send_message("Belum ada data.")
        return

    text = "\n".join([f"{i+1}. {u} — {s}" for i, (u, s) in enumerate(data)])

    embed = discord.Embed(
        title=f"🏆 Leaderboard {kategori.upper()}",
        description=text,
        color=discord.Color.gold()
    )

    await interaction.response.send_message(embed=embed)

# ================= READY =================
@bot.event
async def on_ready():
    await bot.tree.sync(guild=discord.Object(id=GUILD_ID))
    print(f"Login sebagai {bot.user}")

bot.run(TOKEN)  
