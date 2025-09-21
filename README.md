<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu KCL?", "id": "Jumlah Arus Pada Simpul Adalah Nol." },
  { "en": "Apa Itu KVL?", "id": "Jumlah Tegangan Pada Loop Adalah Nol." },
  { "en": "Siapa Penemu Hukum Ini?", "id": "Gustav Robert Kirchhoff." },
  { "en": "KCL Adalah Singkatan Dari?", "id": "Kirchhoff's Current Law." },
  { "en": "KVL Adalah Singkatan Dari?", "id": "Kirchhoff's Voltage Law." },
  { "en": "KCL Berbasis Hukum Apa?", "id": "Hukum Kekekalan Muatan Listrik." },
  { "en": "KVL Berbasis Hukum Apa?", "id": "Hukum Kekekalan Energi." },
  { "en": "KCL Berlaku Di Mana?", "id": "Pada Titik Simpul Atau Percabangan." },
  { "en": "KVL Berlaku Di Mana?", "id": "Pada Lintasan Tertutup Atau Loop." },
  { "en": "Apa Itu Simpul (Node)?", "id": "Titik Pertemuan Tiga Atau Lebih Cabang." },
  { "en": "Apa Itu Loop?", "id": "Setiap Lintasan Tertutup Dalam Suatu Rangkaian." },
  { "en": "Rumus Matematis KCL?", "id": "Sigma Arus Masuk Sama Dengan Sigma Arus Keluar." },
  { "en": "Rumus Matematis KVL?", "id": "Sigma Tegangan Dalam Satu Loop Nol." },
  { "en": "Hukum Pertama Kirchhoff Adalah?", "id": "Hukum Arus Kirchhoff Atau KCL." },
  { "en": "Hukum Kedua Kirchhoff Adalah?", "id": "Hukum Tegangan Kirchhoff Atau KVL." },
  { "en": "KCL Berurusan Dengan Besaran Apa?", "id": "Besaran Arus Listrik (Ampere)." },
  { "en": "KVL Berurusan Dengan Besaran Apa?", "id": "Besaran Tegangan Listrik (Volt)." },
  { "en": "Arus Yang Masuk Simpul Dianggap?", "id": "Positif Atau Sesuai Konvensi Awal." },
  { "en": "Arus Yang Keluar Simpul Dianggap?", "id": "Negatif Atau Sesuai Konvensi Awal." },
  { "en": "Apa Itu Cabang (Branch)?", "id": "Elemen Tunggal Seperti Resistor Atau Sumber." },
  { "en": "Apa Itu Analisis Simpul?", "id": "Metode Analisis Rangkaian Menggunakan KCL." },
  { "en": "Apa Itu Analisis Mesh?", "id": "Metode Analisis Rangkaian Menggunakan KVL." },
  { "en": "Apakah Hukum Ini Berlaku Untuk Rangkaian AC?", "id": "Ya Tentu Saja Berlaku." },
  { "en": "Apakah Hukum Ini Berlaku Untuk Rangkaian DC?", "id": "Ya Tentu Saja Berlaku." },
  { "en": "Tujuan Utama Hukum Kirchhoff?", "id": "Menganalisis Rangkaian Listrik Yang Kompleks." },
  { "en": "Apa Itu Penurunan Tegangan (Voltage Drop)?", "id": "Tegangan Berkurang Saat Melewati Resistor." },
  { "en": "Apa Itu Kenaikan Tegangan (Voltage Rise)?", "id": "Tegangan Bertambah Saat Melewati Sumber." },
  { "en": "Arah Loop KVL Ditentukan?", "id": "Secara Bebas Searah Atau Berlawanan Jarum Jam." },
  { "en": "Arah Arus KCL Ditentukan?", "id": "Secara Bebas Masuk Atau Keluar Simpul." },
  { "en": "Konsistensi Konvensi Tanda Penting?", "id": "Ya Sangat Penting Dalam Perhitungan." },
  { "en": "KCL Disebut Juga Hukum?", "id": "Hukum Titik Atau Hukum Simpul." },
  { "en": "KVL Disebut Juga Hukum?", "id": "Hukum Loop Atau Hukum Mesh." },
  { "en": "Muatan Tidak Bisa Diciptakan Di?", "id": "Sebuah Simpul Rangkaian Listrik." },
  { "en": "Energi Tidak Bisa Hilang Di?", "id": "Sebuah Loop Rangkaian Listrik." },
  { "en": "Jika Tiga Arus Masuk Simpul?", "id": "Jumlah Ketiganya Harus Sama Dengan Nol." },
  { "en": "Jika Dua Resistor Dalam Loop?", "id": "Jumlah Tegangannya Sama Dengan Sumber Tegangan." },
  { "en": "Hukum Kirchhoff Adalah Fondasi Dari?", "id": "Analisis Rangkaian Listrik Modern." },
  { "en": "Apa Itu Rangkaian Seri?", "id": "Komponen Terhubung Ujung Ke Ujung." },
  { "en": "Arus Pada Rangkaian Seri?", "id": "Sama Di Setiap Komponen." },
  { "en": "Tegangan Pada Rangkaian Seri?", "id": "Terbagi Di Setiap Komponen." },
  { "en": "Analisis Rangkaian Seri Menggunakan?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
  { "en": "Apa Itu Rangkaian Paralel?", "id": "Komponen Terhubung Di Dua Simpul Yang Sama." },
  { "en": "Tegangan Pada Rangkaian Paralel?", "id": "Sama Di Setiap Komponen." },
  { "en": "Arus Pada Rangkaian Paralel?", "id": "Terbagi Di Setiap Cabang." },
  { "en": "Analisis Rangkaian Paralel Menggunakan?", "id": "Hukum Arus Kirchhoff (KCL)." },
  { "en": "KCL Adalah Konsekuensi Langsung Dari?", "id": "Kekekalan Muatan." },
  { "en": "KVL Adalah Konsekuensi Langsung Dari?", "id": "Kekekalan Energi." },
  { "en": "Apa Itu Arus Simpul?", "id": "Arus Yang Masuk Atau Keluar Simpul." },
  { "en": "Apa Itu Tegangan Loop?", "id": "Penjumlahan Tegangan Dalam Satu Lintasan Tertutup." },
  { "en": "Apa Itu Supernode?", "id": "Gabungan Dua Simpul Yang Dihubungkan Sumber." },
  { "en": "Supernode Digunakan Dalam Analisis?", "id": "Analisis Simpul Lanjutan." },
  { "en": "Apa Itu Supermesh?", "id": "Loop Lebih Besar Untuk Hindari Sumber Arus." },
  { "en": "Supermesh Digunakan Dalam Analisis?", "id": "Analisis Mesh Lanjutan." },
  { "en": "Arah Arus Asumsi Salah?", "id": "Hasil Akhir Akan Bernilai Negatif." },
  { "en": "Arah Loop Asumsi Salah?", "id": "Tidak Ada Pengaruh Pada Hasil Akhir." },
  { "en": "Hukum Ohm Terkait Dengan?", "id": "Hubungan Antara Tegangan Arus Resistansi." },
  { "en": "Rumus Hukum Ohm?", "id": "Tegangan Sama Dengan Arus Kali Resistansi." },
  { "en": "Kombinasi KCL KVL Dan Ohm?", "id": "Dapat Menyelesaikan Hampir Semua Rangkaian." },
  { "en": "Apa Itu Ground?", "id": "Titik Referensi Tegangan Nol." },
  { "en": "Apakah Ground Mempengaruhi KCL?", "id": "Tidak Secara Langsung." },
  { "en": "Apakah Ground Mempengaruhi KVL?", "id": "Tidak Secara Langsung." },
  { "en": "KCL Fokus Pada Konektivitas?", "id": "Ya Bagaimana Komponen Terhubung Di Simpul." },
  { "en": "KVL Fokus Pada Topologi?", "id": "Ya Lintasan Tertutup Dalam Rangkaian." },
  { "en": "Berapa Jumlah Persamaan KCL?", "id": "Jumlah Simpul Dikurangi Satu." },
  { "en": "Berapa Jumlah Persamaan KVL?", "id": "Jumlah Mesh Independen Dalam Rangkaian." },
  { "en": "Apa Itu Mesh?", "id": "Loop Yang Tidak Mengandung Loop Lain." },
  { "en": "Apa Itu Rangkaian Planar?", "id": "Rangkaian Yang Bisa Digambar Tanpa Persilangan." },
  { "en": "Analisis Mesh Berlaku Untuk?", "id": "Hanya Rangkaian Planar Saja." },
  { "en": "Analisis Simpul Berlaku Untuk?", "id": "Semua Jenis Rangkaian Listrik." },
  { "en": "Mana Yang Lebih Umum?", "id": "Analisis Simpul Karena Lebih Umum." },
  { "en": "Arus Yang Masuk Sama Dengan?", "id": "Arus Yang Keluar." },
  { "en": "Jumlah Kenaikan Tegangan Sama Dengan?", "id": "Jumlah Penurunan Tegangan." },
  { "en": "Konsep Ini Adalah Inti Dari?", "id": "Hukum Kirchhoff." },
  { "en": "KCL Menjaga Keseimbangan?", "id": "Keseimbangan Arus Di Setiap Simpul." },
  { "en": "KVL Menjaga Keseimbangan?", "id": "Keseimbangan Tegangan Di Setiap Loop." },
  { "en": "Penerapan KCL Pertama?", "id": "Identifikasi Semua Simpul Dalam Rangkaian." },
  { "en": "Penerapan KVL Pertama?", "id": "Identifikasi Semua Loop Dalam Rangkaian." },
  { "en": "Apa Itu Rangkaian Ekuivalen?", "id": "Rangkaian Lebih Sederhana Dengan Perilaku Sama." },
  { "en": "Hukum Kirchhoff Digunakan Untuk?", "id": "Menyederhanakan Dan Menganalisis Rangkaian." },
  { "en": "Apa Itu Sumber Tegangan Independen?", "id": "Sumber Dengan Tegangan Tetap." },
  { "en": "Apa Itu Sumber Arus Independen?", "id": "Sumber Dengan Arus Tetap." },
  { "en": "Apa Itu Sumber Dependen?", "id": "Sumber Yang Bergantung Besaran Lain." },
  { "en": "Hukum Kirchhoff Berlaku Untuk Semua Sumber?", "id": "Ya Berlaku Untuk Semua Jenis Sumber." },
  { "en": "Kapan Hukum Kirchhoff Tidak Berlaku?", "id": "Pada Frekuensi Sangat Tinggi (Efek Propagasi)." },
  { "en": "Hukum Kirchhoff Mengasumsikan Elemen?", "id": "Elemen Terpusat (Lumped Element)." },
  { "en": "Apa Itu Elemen Terpusat?", "id": "Ukuran Fisik Komponen Diabaikan." },
  { "en": "Hukum Ini Adalah Alat Fundamental?", "id": "Ya Untuk Semua Insinyur Elektro." },
  { "en": "KCL Digunakan Di Pembagi?", "id": "Aturan Pembagi Arus." },
  { "en": "KVL Digunakan Di Pembagi?", "id": "Aturan Pembagi Tegangan." },
  { "en": "Aturan Pembagi Arus Untuk Rangkaian?", "id": "Rangkaian Paralel." },
  { "en": "Aturan Pembagi Tegangan Untuk Rangkaian?", "id": "Rangkaian Seri." },
  { "en": "Apa Itu Arus Cabang?", "id": "Arus Yang Mengalir Melalui Satu Cabang." },
  { "en": "Apa Itu Tegangan Simpul?", "id": "Tegangan Simpul Relatif Terhadap Ground." },
  { "en": "Variabel Tidak Diketahui Analisis Simpul?", "id": "Adalah Tegangan Simpul." },
  { "en": "Variabel Tidak Diketahui Analisis Mesh?", "id": "Adalah Arus Mesh." },
  { "en": "Hukum Ini Penting Untuk Teorema?", "id": "Thevenin Dan Norton." },
  { "en": "Apa Itu Analisis Simpul (Nodal Analysis)?", "id": "Metode Berbasis KCL Untuk Menyelesaikan Rangkaian." },
  { "en": "Variabel Utama Dalam Analisis Simpul?", "id": "Tegangan Pada Setiap Simpul." },
  { "en": "Apa Itu Simpul Referensi?", "id": "Simpul Yang Dianggap Memiliki Tegangan Nol." },
  { "en": "Simpul Referensi Biasa Disebut?", "id": "Tanah Atau Ground." },
  { "en": "Langkah Pertama Analisis Simpul?", "id": "Pilih Satu Simpul Sebagai Referensi." },
  { "en": "Langkah Kedua Analisis Simpul?", "id": "Terapkan KCL Pada Simpul Non-Referensi." },
  { "en": "Langkah Terakhir Analisis Simpul?", "id": "Selesaikan Sistem Persamaan Linear." },
  { "en": "Analisis Simpul Menghasilkan Nilai Apa?", "id": "Tegangan Di Setiap Simpul." },
  { "en": "Apa Itu Analisis Mesh?", "id": "Metode Berbasis KVL Untuk Menyelesaikan Rangkaian." },
  { "en": "Variabel Utama Dalam Analisis Mesh?", "id": "Arus Yang Mengalir Dalam Setiap Mesh." },
  { "en": "Apa Itu Arus Mesh?", "id": "Arus Fiktif Yang Mengalir Dalam Loop." },
  { "en": "Langkah Pertama Analisis Mesh?", "id": "Identifikasi Semua Mesh Dalam Rangkaian." },
  { "en": "Langkah Kedua Analisis Mesh?", "id": "Terapkan KVL Pada Setiap Mesh." },
  { "en": "Langkah Terakhir Analisis Mesh?", "id": "Selesaikan Sistem Persamaan Arus Mesh." },
  { "en": "Analisis Mesh Berlaku Untuk Rangkaian Apa?", "id": "Hanya Untuk Rangkaian Planar." },
  { "en": "Apa Itu Rangkaian Planar?", "id": "Rangkaian Yang Bisa Digambar Datar." },
  { "en": "Analisis Simpul Lebih Umum?", "id": "Ya Karena Berlaku Untuk Semua Rangkaian." },
  { "en": "Arus Cabang Dalam Analisis Mesh?", "id": "Adalah Kombinasi Dari Arus-arus Mesh." },
  { "en": "Hukum Kekekalan Muatan Menyatakan?", "id": "Muatan Tidak Bisa Diciptakan Atau Dimusnahkan." },
  { "en": "KCL Adalah Bentuk Lain Dari?", "id": "Hukum Kekekalan Muatan." },
  { "en": "Hukum Kekekalan Energi Menyatakan?", "id": "Energi Tidak Bisa Diciptakan Atau Dimusnahkan." },
  { "en": "KVL Adalah Bentuk Lain Dari?", "id": "Hukum Kekekalan Energi." },
  { "en": "Apa Itu Aturan Pembagi Tegangan?", "id": "Menghitung Tegangan Pada Rangkaian Seri." },
  { "en": "Dasar Dari Pembagi Tegangan?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
  { "en": "Apa Itu Aturan Pembagi Arus?", "id": "Menghitung Arus Pada Rangkaian Paralel." },
  { "en": "Dasar Dari Pembagi Arus?", "id": "Hukum Arus Kirchhoff (KCL)." },
  { "en": "Di Rangkaian Seri Arusnya?", "id": "Selalu Sama Di Semua Titik." },
  { "en": "Di Rangkaian Paralel Tegangannya?", "id": "Selalu Sama Di Semua Cabang." },
  { "en": "KCL Mengatakan Total Arus Masuk Simpul?", "id": "Sama Dengan Total Arus Keluar." },
  { "en": "KVL Mengatakan Total Kenaikan Tegangan?", "id": "Sama Dengan Total Penurunan Tegangan." },
  { "en": "Apa Itu Konvensi Tanda Pasif?", "id": "Arus Masuk Terminal Positif Penurunan Tegangan." },
  { "en": "Konvensi Ini Penting Untuk?", "id": "Menerapkan KVL Dengan Benar." },
  { "en": "Apakah Arah Arus Asumsi Penting?", "id": "Tidak Hasil Negatif Berarti Arah Terbalik." },
  { "en": "Apakah Arah Loop Penting?", "id": "Tidak Selama Diterapkan Secara Konsisten." },
  { "en": "Apa Itu Sumber Tegangan?", "id": "Memberikan Kenaikan Tegangan (Voltage Rise)." },
  { "en": "Apa Itu Resistor?", "id": "Menyebabkan Penurunan Tegangan (Voltage Drop)." },
  { "en": "Jumlah Persamaan Simpul Independen?", "id": "N - 1 (N Adalah Jumlah Simpul)." },
  { "en": "Jumlah Persamaan Mesh Independen?", "id": "B - N + 1 (B Cabang N Simpul)." },
  { "en": "Apa Itu Superposisi?", "id": "Metode Analisis Untuk Rangkaian Linier." },
  { "en": "Apakah Superposisi Berbasis KCL/KVL?", "id": "Ya Secara Mendasar." },
  { "en": "Apa Itu Teorema Thevenin?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Tegangan." },
  { "en": "Teorema Thevenin Membutuhkan KCL/KVL?", "id": "Ya Untuk Menghitung Tegangan Dan Resistansi." },
  { "en": "Apa Itu Teorema Norton?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Arus." },
  { "en": "Teorema Norton Juga Membutuhkan KCL/KVL?", "id": "Ya Tentu Saja." },
  { "en": "Hukum Kirchhoff Berlaku Untuk Komponen Non-Linier?", "id": "Ya Hukumnya Sendiri Tetap Berlaku." },
  { "en": "Tapi Analisisnya Menjadi?", "id": "Jauh Lebih Sulit." },
  { "en": "Apa Itu Sirkuit Terbuka (Open Circuit)?", "id": "Arus Sama Dengan Nol." },
  { "en": "Apa Itu Sirkuit Tertutup (Short Circuit)?", "id": "Tegangan Sama Dengan Nol." },
  { "en": "Tegangan Sirkuit Terbuka (Voc) Digunakan Di?", "id": "Teorema Thevenin." },
  { "en": "Arus Sirkuit Tertutup (Isc) Digunakan Di?", "id": "Teorema Norton." },
  { "en": "KCL Dan KVL Apakah Saling Independen?", "id": "Ya Keduanya Hukum Fundamental Yang Berbeda." },
  { "en": "KCL Adalah Hukum Tentang?", "id": "Konektivitas Rangkaian." },
  { "en": "KVL Adalah Hukum Tentang?", "id": "Topologi Loop Rangkaian." },
  { "en": "Bagaimana KCL Bekerja?", "id": "Menjumlahkan Semua Arus Di Satu Titik." },
  { "en": "Bagaimana KVL Bekerja?", "id": "Menjumlahkan Semua Tegangan Mengelilingi Loop." },
  { "en": "Apa Itu Rangkaian Jembatan Wheatstone?", "id": "Rangkaian Untuk Mengukur Resistansi." },
  { "en": "Prinsip Keseimbangannya Berbasis?", "id": "KCL Dan KVL." },
  { "en": "Saat Seimbang Arus Galvanometer?", "id": "Sama Dengan Nol." },
  { "en": "Apa Itu Analisis Rangkaian AC?", "id": "Menggunakan Fasor Dan Impedansi." },
  { "en": "Apakah KCL/KVL Berlaku Untuk Fasor?", "id": "Ya Berlaku Untuk Fasor Arus Tegangan." },
  { "en": "Impedansi Menggantikan Apa?", "id": "Resistansi Dalam Perhitungan AC." },
  { "en": "Apa Itu Loop Mendasar (Fundamental Loop)?", "id": "Loop Yang Dibentuk Dengan Menambah Satu Cabang." },
  { "en": "Apa Itu Set Potongan Mendasar?", "id": "Kumpulan Cabang Yang Memisahkan Rangkaian." },
  { "en": "Ini Konsep Lanjutan Dari?", "id": "KCL Dan KVL." },
  { "en": "Di Rangkaian Kapasitor Seri Muatannya?", "id": "Sama Di Setiap Kapasitor." },
  { "en": "Ini Adalah Konsekuensi Dari?", "id": "KCL." },
  { "en": "Di Rangkaian Induktor Paralel Fluksnya?", "id": "Tidak Sama Tergantung Nilai Induktansi." },
  { "en": "Tegangannya Sama Adalah Konsekuensi?", "id": "KVL." },
  { "en": "Hukum Kirchhoff Adalah Hukum Elemen Terpusat?", "id": "Ya Mengasumsikan Sinyal Merambat Seketika." },
  { "en": "Kapan Asumsi Ini Gagal?", "id": "Pada Frekuensi Sangat Tinggi (Microwave)." },
  { "en": "Di Frekuensi Tinggi Kita Menggunakan?", "id": "Analisis Jalur Transmisi (Transmission Line)." },
  { "en": "Hukum Ini Adalah Aproksimasi?", "id": "Aproksimasi Dari Persamaan Maxwell." },
  { "en": "Untuk Sirkuit Biasa Aproksimasinya?", "id": "Sangat Akurat." },
  { "en": "KCL Untuk Titik Apapun?", "id": "Ya Setiap Titik Adalah Simpul." },
  { "en": "KVL Untuk Loop Apapun?", "id": "Ya Setiap Lintasan Tertutup." },
  { "en": "Apa Itu Sumber Tegangan Terkendali?", "id": "Tegangan Bergantung Pada Besaran Lain." },
  { "en": "Apa Itu Sumber Arus Terkendali?", "id": "Arus Bergantung Pada Besaran Lain." },
  { "en": "Hukum Kirchhoff Berlaku?", "id": "Ya Untuk Semua Jenis Sumber Terkendali." },
  { "en": "Bagaimana Cara Memverifikasi Solusi Rangkaian?", "id": "Cek Apakah KCL Dan KVL Terpenuhi." },
  { "en": "KCL Digunakan Untuk Menemukan Arus?", "id": "Arus Yang Tidak Diketahui." },
  { "en": "KVL Digunakan Untuk Menemukan Tegangan?", "id": "Tegangan Yang Tidak Diketahui." },
  { "en": "Apa Itu Matriks Admittansi Simpul?", "id": "Matriks Untuk Sistem Persamaan Analisis Simpul." },
  { "en": "Apa Itu Matriks Impedansi Mesh?", "id": "Matriks Untuk Sistem Persamaan Analisis Mesh." },
  { "en": "KCL Adalah Dasar Dari Prinsip?", "id": "Keseimbangan Arus." },
  { "en": "KVL Adalah Dasar Dari Prinsip?", "id": "Keseimbangan Tegangan." },
  { "en": "Apa Itu Jaringan Listrik?", "id": "Interkoneksi Dari Banyak Komponen Listrik." },
  { "en": "Analisisnya Selalu Dimulai Dengan?", "id": "Penerapan Hukum Kirchhoff." },
  { "en": "KCL Dan KVL Adalah Alfabet?", "id": "Bahasa Dari Rangkaian Listrik." },
  { "en": "KCL Menjelaskan Bagaimana Arus?", "id": "Berinteraksi Di Persimpangan." },
  { "en": "KVL Menjelaskan Bagaimana Energi?", "id": "Dihabiskan Dan Disediakan Dalam Loop." },
  { "en": "Apa Itu Diagram Rangkaian?", "id": "Representasi Grafis Dari Sebuah Sirkuit." },
  { "en": "Simpul Di Diagram Adalah?", "id": "Titik Dimana Garis Bertemu." },
  { "en": "Loop Di Diagram Adalah?", "id": "Setiap Pola Tertutup Yang Bisa Digambar." },
  { "en": "KCL Adalah Kekekalan Muatan Lokal?", "id": "Ya Berlaku Di Setiap Titik Simpul." },
  { "en": "KVL Adalah Kekekalan Energi Lokal?", "id": "Ya Berlaku Di Setiap Loop." },
  { "en": "Apa Itu Potensial Listrik?", "id": "Energi Potensial Per Satuan Muatan." },
  { "en": "Tegangan Adalah Perbedaan?", "id": "Perbedaan Potensial Listrik." },
  { "en": "KVL Menyatakan Potensial Awal Akhir?", "id": "Sama Dalam Satu Lintasan Tertutup." },
  { "en": "Apa Konvensi Tanda Untuk KCL?", "id": "Arus Masuk Positif Arus Keluar Negatif." },
  { "en": "Apakah Konvensi Ini Wajib?", "id": "Tidak Yang Penting Adalah Konsistensi." },
  { "en": "Apa Arti Hasil Arus Negatif?", "id": "Arah Arus Sebenarnya Berlawanan Dengan Asumsi." },
  { "en": "Apa Konvensi Tanda Untuk KVL?", "id": "Kenaikan Tegangan Positif Penurunan Tegangan Negatif." },
  { "en": "Melewati Sumber Dari Minus Ke Plus?", "id": "Adalah Kenaikan Tegangan (Voltage Rise)." },
  { "en": "Melewati Resistor Searah Arus?", "id": "Adalah Penurunan Tegangan (Voltage Drop)." },
  { "en": "Apa Langkah Pertama Menerapkan KCL?", "id": "Identifikasi Semua Simpul Dalam Rangkaian." },
  { "en": "Apa Langkah Kedua Menerapkan KCL?", "id": "Asumsikan Arah Arus Untuk Setiap Cabang." },
  { "en": "Apa Langkah Terakhir Menerapkan KCL?", "id": "Tulis Persamaan KCL Untuk Setiap Simpul." },
  { "en": "Apa Langkah Pertama Menerapkan KVL?", "id": "Identifikasi Semua Loop Dalam Rangkaian." },
  { "en": "Apa Langkah Kedua Menerapkan KVL?", "id": "Asumsikan Arah Loop (Searah Jarum Jam)." },
  { "en": "Apa Langkah Terakhir Menerapkan KVL?", "id": "Tulis Persamaan KVL Untuk Setiap Loop." },
  { "en": "Apa Asumsi Fundamental KCL/KVL?", "id": "Rangkaian Adalah Sistem Elemen Terpusat." },
  { "en": "Apa Arti Elemen Terpusat?", "id": "Efek Listrik Terjadi Seketika Di Seluruh Komponen." },
  { "en": "Kapan Asumsi Ini Tidak Berlaku?", "id": "Saat Ukuran Rangkaian Sebanding Panjang Gelombang." },
  { "en": "Apa Beda Arus Cabang Dan Arus Mesh?", "id": "Arus Cabang Nyata Arus Mesh Fiktif." },
  { "en": "Apa Hubungan Antara Keduanya?", "id": "Arus Cabang Adalah Superposisi Arus Mesh." },
  { "en": "Apa Itu Tegangan Simpul?", "id": "Tegangan Sebuah Simpul Relatif Terhadap Referensi." },
  { "en": "Apa Itu Tegangan Cabang?", "id": "Perbedaan Tegangan Antara Dua Ujung Cabang." },
  { "en": "Hubungan Antara Keduanya?", "id": "Tegangan Cabang Adalah Selisih Tegangan Simpul." },
  { "en": "KCL Adalah Tentang Aliran?", "id": "Ya Aliran Muatan Listrik." },
  { "en": "KVL Adalah Tentang Energi?", "id": "Ya Perubahan Energi Potensial Listrik." },
  { "en": "Jika Tiga Resistor Paralel KCL Digunakan?", "id": "Ya Untuk Menemukan Arus Total." },
  { "en": "Jika Tiga Resistor Seri KVL Digunakan?", "id": "Ya Untuk Menemukan Tegangan Total." },
  { "en": "Apa Itu Simpul Esensial?", "id": "Simpul Tempat Tiga Atau Lebih Elemen Bertemu." },
  { "en": "Apa Itu Simpul Sederhana?", "id": "Simpul Tempat Dua Elemen Bertemu." },
  { "en": "KCL Trivial Di Simpul Sederhana?", "id": "Ya Arus Masuk Sama Dengan Arus Keluar." },
  { "en": "Apa Itu Cabang Esensial?", "id": "Cabang Yang Menghubungkan Dua Simpul Esensial." },
  { "en": "Hukum Kirchhoff Diterapkan Pada Rangkaian Linier?", "id": "Ya Sangat Umum Digunakan." },
  { "en": "Apa Arti Linier Dalam Rangkaian?", "id": "Nilai Komponen Tidak Berubah Dengan Tegangan." },
  { "en": "Contoh Komponen Non-Linier?", "id": "Dioda Dan Transistor." },
  { "en": "Analisis Rangkaian Non-Linier Menggunakan?", "id": "Metode Grafis Atau Aproksimasi Linier." },
  { "en": "Apa Itu Titik Operasi (Q-Point)?", "id": "Titik Kerja DC Dari Komponen Non-Linier." },
  { "en": "Garis Beban DC Diturunkan Dari?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
  { "en": "Apa Itu Supernode?", "id": "Simpul Buatan Yang Mencakup Sumber Tegangan." },
  { "en": "Kenapa Supernode Dibutuhkan?", "id": "Untuk Menyederhanakan Analisis Simpul Dengan Sumber Tegangan." },
  { "en": "Apa Itu Supermesh?", "id": "Loop Buatan Untuk Menghindari Sumber Arus." },
  { "en": "Kenapa Supermesh Dibutuhkan?", "id": "Untuk Menyederhanakan Analisis Mesh Dengan Sumber Arus." },
  { "en": "Apakah KCL Bergantung Pada Jenis Komponen?", "id": "Tidak Hanya Pada Koneksi." },
  { "en": "Apakah KVL Bergantung Pada Jenis Komponen?", "id": "Ya Tegangan Berbeda Untuk Komponen Berbeda." },
  { "en": "Apa Itu Konservasi Muatan?", "id": "Total Muatan Dalam Sistem Tertutup Konstan." },
  { "en": "Bagaimana Ini Menghasilkan KCL?", "id": "Arus Adalah Laju Aliran Muatan." },
  { "en": "Apa Itu Medan Listrik Konservatif?", "id": "Integral Lintasan Tertutupnya Selalu Nol." },
  { "en": "Bagaimana Ini Menghasilkan KVL?", "id": "Tegangan Adalah Integral Lintasan Medan Listrik." },
  { "en": "Kapan Medan Listrik Tidak Konservatif?", "id": "Saat Ada Perubahan Medan Magnet." },
  { "en": "Ini Adalah Dasar Dari Hukum?", "id": "Hukum Induksi Faraday." },
  { "en": "Jadi KVL Punya Batasan?", "id": "Ya Pada Rangkaian Dengan Induktansi Magnetik." },
  { "en": "Untuk Sirkuit Biasa Apakah KVL Aman?", "id": "Ya Sangat Aman Digunakan." },
  { "en": "KCL Adalah Pernyataan Tentang?", "id": "Keseimbangan Aliran." },
  { "en": "KVL Adalah Pernyataan Tentang?", "id": "Keseimbangan Potensial." },
  { "en": "Arus Di Resistor Mengalir Dari?", "id": "Potensial Tinggi Ke Potensial Rendah." },
  { "en": "Ini Menyebabkan Penurunan Tegangan?", "id": "Ya Sesuai Dengan Hukum Ohm." },
  { "en": "Satuan Arus Adalah?", "id": "Ampere (Coulomb Per Detik)." },
  { "en": "Satuan Tegangan Adalah?", "id": "Volt (Joule Per Coulomb)." },
  { "en": "Apa Itu Topologi Rangkaian?", "id": "Struktur Koneksi Antar Komponen." },
  { "en": "Hukum Kirchhoff Adalah Hukum Topologis?", "id": "Ya Benar Sekali." },
  { "en": "Apa Maksudnya?", "id": "Hanya Bergantung Pada Cara Komponen Terhubung." },
  { "en": "Tidak Bergantung Pada Nilai Komponen?", "id": "Tidak Secara Langsung." },
  { "en": "Untuk Mendapatkan Solusi Lengkap Kita Butuh?", "id": "Hukum Kirchhoff Dan Karakteristik Komponen." },
  { "en": "Karakteristik Komponen Contohnya?", "id": "Hukum Ohm Untuk Resistor." },
  { "en": "Apa Itu Rangkaian Resiprokal?", "id": "Sifatnya Sama Jika Sumber Detektor Ditukar." },
  { "en": "Berapa Banyak Simpul Di Rangkaian Seri?", "id": "N-1 Simpul Sederhana." },
  { "en": "Berapa Banyak Simpul Di Rangkaian Paralel?", "id": "Dua Simpul Esensial." },
  { "en": "KCL Digunakan Untuk Menjumlahkan Arus Paralel?", "id": "Ya Benar." },
  { "en": "KVL Digunakan Untuk Menjumlahkan Tegangan Seri?", "id": "Ya Benar." },
  { "en": "Hukum Kirchhoff Mengasumsikan Kawat Ideal?", "id": "Ya Resistansi Kawat Dianggap Nol." },
  { "en": "Apa Itu Kawat Ideal?", "id": "Semua Titik Di Kawat Punya Potensial Sama." },
  { "en": "Apa Itu Simpul Tunggal?", "id": "Seluruh Kawat Penghubung Dianggap Satu Simpul." },
  { "en": "Dalam Praktik Kawat Punya Resistansi?", "id": "Ya Tapi Sangat Kecil." },
  { "en": "Kapan Resistansi Kawat Diperhitungkan?", "id": "Pada Rangkaian Presisi Atau Arus Sangat Besar." },
  { "en": "Apa Itu Titik Referensi?", "id": "Titik Dengan Potensial Didefinisikan Nol." },
  { "en": "Apakah Pilihan Titik Referensi Mempengaruhi Arus?", "id": "Tidak Arus Adalah Besaran Absolut." },
  { "en": "Pilihan Titik Referensi Mempengaruhi Tegangan?", "id": "Ya Tegangan Adalah Besaran Relatif." },
  { "en": "Apa Itu Rangkaian Dual?", "id": "Rangkaian Dimana Peran Arus Tegangan Ditukar." },
  { "en": "KCL Di Satu Rangkaian Mirip?", "id": "KVL Di Rangkaian Dualnya." },
  { "en": "Seri Dual Dengan?", "id": "Paralel." },
  { "en": "Resistor Dual Dengan?", "id": "Konduktor." },
  { "en": "Induktor Dual Dengan?", "id": "Kapasitor." },
  { "en": "Sumber Tegangan Dual Dengan?", "id": "Sumber Arus." },
  { "en": "Analisis Simpul Dual Dengan?", "id": "Analisis Mesh." },
  { "en": "Apa Itu Teori Graf?", "id": "Cabang Matematika Untuk Menganalisis Jaringan." },
  { "en": "Hukum Kirchhoff Bisa Diformulasikan Menggunakan?", "id": "Teori Graf." },
  { "en": "Simpul Menjadi Apa Di Teori Graf?", "id": "Vertex." },
  { "en": "Cabang Menjadi Apa Di Teori Graf?", "id": "Edge." },
  { "en": "KCL Adalah Batasan Pada?", "id": "Matriks Insidensi Graf." },
  { "en": "KVL Adalah Batasan Pada?", "id": "Matriks Loop Graf." },
  { "en": "Apakah KCL/KVL Bisa Gagal?", "id": "Pada Kondisi Ekstrem Ya." },
  { "en": "Contohnya Di Dekat Antena?", "id": "Ya Karena Medan Elektromagnetik Merambat." },
  { "en": "Hukum Kirchhoff Adalah Perkakas?", "id": "Sangat Andal Untuk Sebagian Besar Aplikasi." },
  { "en": "Penting Untuk Dipelajari Pertama?", "id": "Ya Fondasi Utama Teknik Elektro." },
  { "en": "Analisis Simpul Menghasilkan Matriks?", "id": "Matriks Admittansi (Y)." },
  { "en": "Analisis Mesh Menghasilkan Matriks?", "id": "Matriks Impedansi (Z)." },
  { "en": "Keduanya Adalah Cara Sistematis?", "id": "Menerapkan KCL Dan KVL." },
  { "en": "Apa Prinsip Dibalik KCL?", "id": "Muatan Listrik Tidak Bisa Menumpuk Di Simpul." },
  { "en": "Apa Prinsip Dibalik KVL?", "id": "Energi Potensial Kembali Ke Nilai Awal." },
  { "en": "Kenapa Jumlah Arus Di Simpul Nol?", "id": "Karena Arus Masuk Harus Sama Dengan Keluar." },
  { "en": "Kenapa Jumlah Tegangan Di Loop Nol?", "id": "Karena Kembali Ke Titik Awal Yang Sama." },
  { "en": "Apa Analogi KCL Dengan Air?", "id": "Aliran Air Di Percabangan Pipa." },
  { "en": "Debit Air Masuk Sama Dengan?", "id": "Debit Air Keluar." },
  { "en": "Apa Analogi KVL Dengan Ketinggian?", "id": "Naik Turun Gunung Kembali Ke Titik Awal." },
  { "en": "Total Perubahan Ketinggiannya Adalah?", "id": "Nol." },
  { "en": "KCL Adalah Pernyataan Tentang Aliran?", "id": "Ya Aliran Muatan." },
  { "en": "KVL Adalah Pernyataan Tentang Potensial?", "id": "Ya Potensial Energi." },
  { "en": "Apa Itu Rangkaian Listrik?", "id": "Lintasan Tertutup Untuk Aliran Arus." },
  { "en": "Kenapa Rangkaian Harus Tertutup?", "id": "Agar Arus Dapat Mengalir Terus Menerus." },
  { "en": "Hukum Mana Yang Menjamin Ini?", "id": "KCL Menjamin Arus Kembali Ke Sumber." },
  { "en": "Apa Tujuan Utama Analisis Rangkaian?", "id": "Menemukan Arus Dan Tegangan Tidak Dikenal." },
  { "en": "Kenapa Arah Asumsi Arus Diperlukan?", "id": "Untuk Menentukan Tanda Di Persamaan KCL." },
  { "en": "Kenapa Arah Loop Diperlukan?", "id": "Untuk Menentukan Tanda Di Persamaan KVL." },
  { "en": "Apa Itu Tanda Tegangan Pada Resistor?", "id": "Positif Di Sisi Arus Masuk." },
  { "en": "Apa Itu Tanda Tegangan Pada Sumber?", "id": "Ditentukan Oleh Polaritas Baterai." },
  { "en": "KCL Digunakan Pada Rangkaian Paralel Untuk?", "id": "Menjumlahkan Arus Di Setiap Cabang." },
  { "en": "KVL Digunakan Pada Rangkaian Seri Untuk?", "id": "Menjumlahkan Tegangan Di Setiap Komponen." },
  { "en": "Apakah Arus Sama Dengan Muatan?", "id": "Tidak Arus Adalah Laju Aliran Muatan." },
  { "en": "Apakah Tegangan Sama Dengan Energi?", "id": "Tidak Tegangan Adalah Energi Per Muatan." },
  { "en": "KCL Terkait Dengan Persamaan Kontinuitas?", "id": "Ya Bentuk Sederhana Untuk Rangkaian." },
  { "en": "KVL Terkait Dengan Medan Konservatif?", "id": "Ya Medan Elektrostatis Adalah Konservatif." },
  { "en": "Kenapa Simpul Referensi Diperlukan?", "id": "Karena Tegangan Adalah Besaran Relatif." },
  { "en": "Tegangan Diukur Antara?", "id": "Dua Titik." },
  { "en": "Tegangan Simpul Adalah Tegangan Antara?", "id": "Simpul Dan Titik Referensi." },
  { "en": "Kenapa Analisis Mesh Hanya Untuk Planar?", "id": "Karena Definisi Mesh Tidak Jelas." },
  { "en": "Apa Itu Graf Rangkaian?", "id": "Representasi Abstrak Dari Rangkaian." },
  { "en": "KCL Dan KVL Adalah Sifat?", "id": "Sifat Dari Graf Rangkaian." },
  { "en": "Penerapan KCL Selalu Menghasilkan?", "id": "Sistem Persamaan Linier." },
  { "en": "Penerapan KVL Selalu Menghasilkan?", "id": "Sistem Persamaan Linier Juga." },
  { "en": "Solusi Dari Sistem Persamaan?", "id": "Memberikan Nilai Arus Atau Tegangan." },
  { "en": "Apa Asumsi Tentang Sumber Ideal?", "id": "Tidak Memiliki Resistansi Internal." },
  { "en": "Sumber Tegangan Ideal Resistansi Internal?", "id": "Nol." },
  { "en": "Sumber Arus Ideal Resistansi Internal?", "id": "Tak Terhingga." },
  { "en": "Hukum Kirchhoff Berlaku Untuk Sumber Nyata?", "id": "Ya Dengan Memodelkan Resistansi Internalnya." },
  { "en": "Apa Itu Potensial Simpul?", "id": "Sama Dengan Tegangan Simpul." },
  { "en": "Apa Itu Loop Arus?", "id": "Sama Dengan Arus Mesh." },
  { "en": "KCL Memastikan Tidak Ada Muatan?", "id": "Yang Terakumulasi Di Simpul." },
  { "en": "KVL Memastikan Setiap Elektron?", "id": "Kehilangan Semua Energi Yang Didapat." },
  { "en": "Apa Itu Rangkaian Kompleks?", "id": "Rangkaian Yang Bukan Murni Seri Paralel." },
  { "en": "Bagaimana Menganalisis Rangkaian Kompleks?", "id": "Menggunakan Analisis Simpul Atau Mesh." },
  { "en": "KCL Dan KVL Adalah Alat Universal?", "id": "Ya Untuk Rangkaian Elemen Terpusat." },
  { "en": "Apa Itu Konduktansi?", "id": "Kebalikan Dari Resistansi." },
  { "en": "Analisis Simpul Lebih Mudah Menggunakan?", "id": "Konduktansi Daripada Resistansi." },
  { "en": "Apa Itu Sirkuit Ekuivalen?", "id": "Sama Dengan Rangkaian Ekuivalen." },
  { "en": "Apa Itu Sumber Transformasi?", "id": "Mengubah Sumber Tegangan Menjadi Sumber Arus." },
  { "en": "Dasar Dari Sumber Transformasi?", "id": "Teorema Thevenin Dan Norton." },
  { "en": "Apa Itu Jembatan Setimbang?", "id": "Rangkaian Jembatan Wheatstone Dalam Kondisi Nol." },
  { "en": "Apa Itu Prinsip Dualitas?", "id": "Konsep Simetri Antara Arus Dan Tegangan." },
  { "en": "KCL Dual Dengan?", "id": "KVL." },
  { "en": "Simpul Dual Dengan?", "id": "Loop." },
  { "en": "Cabang Seri Dual Dengan?", "id": "Cabang Paralel." },
  { "en": "Sumber Tegangan Dual Dengan?", "id": "Sumber Arus." },
  { "en": "Resistor Dual Dengan?", "id": "Konduktor." },
  { "en": "Induktor Dual Dengan?", "id": "Kapasitor." },
  { "en": "Apa Itu Sirkuit Resonansi?", "id": "Rangkaian RLC Pada Frekuensi Tertentu." },
  { "en": "Analisisnya Membutuhkan?", "id": "KCL Dan KVL Dalam Domain Fasor." },
  { "en": "Apa Itu Teorema Superposisi?", "id": "Analisis Dengan Satu Sumber Aktif Sekaligus." },
  { "en": "Dasar Teorema Superposisi?", "id": "Linearitas Rangkaian." },
  { "en": "KCL Dan KVL Digunakan Di Setiap Langkah?", "id": "Ya Tentu Saja." },
  { "en": "Hasil Akhir Adalah Penjumlahan?", "id": "Hasil Dari Setiap Sumber." },
  { "en": "Superposisi Tidak Berlaku Untuk?", "id": "Perhitungan Daya Listrik." },
  { "en": "Kenapa Tidak Berlaku Untuk Daya?", "id": "Karena Daya Tidak Linier (P=I²R)." },
  { "en": "Tujuan Utama Dari Konvensi Tanda?", "id": "Untuk Menghindari Kesalahan Dalam Perhitungan." },
  { "en": "Jika Arus Masuk Negatif Keluar?", "id": "Positif." },
  { "en": "Jika Kenaikan Tegangan Negatif Penurunan?", "id": "Positif." },
  { "en": "Apa Yang Penting?", "id": "Konsistensi Selama Analisis." },
  { "en": "Apa Itu Lintasan Dalam Rangkaian?", "id": "Urutan Cabang Dari Satu Titik Ke Titik Lain." },
  { "en": "Lintasan Tertutup Disebut?", "id": "Loop." },
  { "en": "KCL Adalah Batasan Fisik Pada?", "id": "Arus." },
  { "en": "KVL Adalah Batasan Fisik Pada?", "id": "Tegangan." },
  { "en": "Apakah Muatan Bisa Bocor Dari Simpul?", "id": "Tidak Dalam Model Rangkaian Ideal." },
  { "en": "Apakah Energi Bisa Diciptakan Di Loop?", "id": "Tidak Dalam Model Rangkaian Ideal." },
  { "en": "Apa Itu Rangkaian Linear?", "id": "Hanya Terdiri Dari Komponen Linier." },
  { "en": "Komponen Linier Adalah?", "id": "Resistor Induktor Kapasitor Ideal." },
  { "en": "Hukum Kirchhoff Adalah Hukum Linier?", "id": "Ya Persamaannya Adalah Persamaan Linier." },
  { "en": "Kenapa KCL/KVL Sangat Fundamental?", "id": "Karena Berasal Dari Hukum Konservasi Fisik." },
  { "en": "Hukum Konservasi Ini Selalu Benar?", "id": "Ya Dalam Ranah Fisika Klasik." },
  { "en": "Apa Itu Rangkaian Aktif?", "id": "Rangkaian Yang Mengandung Sumber Energi." },
  { "en": "Apa Itu Rangkaian Pasif?", "id": "Rangkaian Tanpa Sumber Energi Independen." },
  { "en": "KCL/KVL Berlaku Untuk Keduanya?", "id": "Ya Berlaku Universal." },
  { "en": "Apa Itu Kondisi Awal?", "id": "Tegangan Kapasitor Arus Induktor Awal." },
  { "en": "Hukum Kirchhoff Berlaku Saat Transien?", "id": "Ya Berlaku Setiap Saat." },
  { "en": "Analisis Transien Menggunakan?", "id": "Persamaan Diferensial." },
  { "en": "Persamaan Diferensial Diturunkan Dari?", "id": "KCL KVL Dan Sifat Komponen." },
  { "en": "Apa Itu Kondisi Steady-State?", "id": "Kondisi Rangkaian Setelah Waktu Lama." },
  { "en": "KCL/KVL Juga Berlaku Di Steady-State?", "id": "Ya Tentu Saja." },
  { "en": "Untuk DC Steady-State Kapasitor Menjadi?", "id": "Sirkuit Terbuka." },
  { "en": "Untuk DC Steady-State Induktor Menjadi?", "id": "Sirkuit Tertutup." },
  { "en": "Penyederhanaan Ini Berasal Dari?", "id": "Analisis KCL Dan KVL." },
  { "en": "Hukum Kirchhoff Adalah Perkakas?", "id": "Paling Kuat Dalam Analisis Rangkaian." },
  { "en": "Apa Beda KCL Dan KVL?", "id": "KCL Untuk Arus KVL Untuk Tegangan." },
  { "en": "KCL Untuk Simpul KVL Untuk?", "id": "Loop." },
  { "en": "Apa Beda Analisis Simpul Dan Mesh?", "id": "Simpul Pakai KCL Mesh Pakai KVL." },
  { "en": "Variabel Analisis Simpul?", "id": "Tegangan Simpul." },
  { "en": "Variabel Analisis Mesh?", "id": "Arus Mesh." },
  { "en": "Apa Beda Rangkaian Seri Dan Paralel?", "id": "Seri Satu Jalur Paralel Banyak Jalur." },
  { "en": "Arus Sama Di Rangkaian?", "id": "Seri." },
  { "en": "Tegangan Sama Di Rangkaian?", "id": "Paralel." },
  { "en": "Apa Beda Sumber Tegangan Dan Arus?", "id": "Satu Jaga Tegangan Satu Jaga Arus." },
  { "en": "Apa Beda Hukum Ohm Dan Kirchhoff?", "id": "Ohm Untuk Komponen Kirchhoff Rangkaian." },
  { "en": "Apa Beda Cabang Dan Loop?", "id": "Cabang Satu Elemen Loop Lintasan Tertutup." },
  { "en": "Apa Beda Simpul Dan Mesh?", "id": "Simpul Titik Pertemuan Mesh Loop Kosong." },
  { "en": "KCL Berbasis Konservasi Apa?", "id": "Muatan." },
  { "en": "KVL Berbasis Konservasi Apa?", "id": "Energi." },
  { "en": "Apa Itu Ground?", "id": "Titik Referensi Tegangan Nol Volt." },
  { "en": "Apakah Pilihan Ground Mempengaruhi Arus?", "id": "Tidak." },
  { "en": "Apakah Pilihan Ground Mempengaruhi Tegangan?", "id": "Ya Tegangan Relatif Terhadap Ground." },
  { "en": "Kenaikan Tegangan Disebut Juga?", "id": "Voltage Rise." },
  { "en": "Penurunan Tegangan Disebut Juga?", "id": "Voltage Drop." },
  { "en": "Di Loop Kenaikan Sama Dengan?", "id": "Penurunan." },
  { "en": "Di Simpul Arus Masuk Sama Dengan?", "id": "Arus Keluar." },
  { "en": "Kapan Analisis Mesh Tidak Bisa Dipakai?", "id": "Pada Rangkaian Non-Planar." },
  { "en": "Kapan Analisis Simpul Selalu Bisa Dipakai?", "id": "Selalu Bisa Untuk Rangkaian Apapun." },
  { "en": "Apa Itu Sumber Independen?", "id": "Nilainya Tidak Bergantung Rangkaian." },
  { "en": "Apa Itu Sumber Dependen?", "id": "Nilainya Bergantung Arus Tegangan Lain." },
  { "en": "KCL/KVL Berlaku Untuk Keduanya?", "id": "Ya." },
  { "en": "Apa Itu Sirkuit Terbuka?", "id": "Resistansi Tak Terhingga Arus Nol." },
  { "en": "Apa Itu Hubungan Singkat?", "id": "Resistansi Nol Tegangan Nol." },
  { "en": "Pembagi Tegangan Untuk Rangkaian?", "id": "Seri." },
  { "en": "Pembagi Arus Untuk Rangkaian?", "id": "Paralel." },
  { "en": "Dasar Pembagi Tegangan Adalah?", "id": "KVL." },
  { "en": "Dasar Pembagi Arus Adalah?", "id": "KCL." },
  { "en": "Analisis Rangkaian AC Menggunakan KCL/KVL Pada?", "id": "Fasor." },
  { "en": "Apa Itu Fasor Tegangan?", "id": "Representasi Kompleks Tegangan Sinusoidal." },
  { "en": "Apa Itu Fasor Arus?", "id": "Representasi Kompleks Arus Sinusoidal." },
  { "en": "Jumlah Fasor Arus Di Simpul?", "id": "Nol." },
  { "en": "Jumlah Fasor Tegangan Di Loop?", "id": "Nol." },
  { "en": "Hukum Kirchhoff Bekerja Dengan Bilangan Kompleks?", "id": "Ya Dalam Analisis AC." },
  { "en": "Apa Itu Supernode?", "id": "Gabungan Dua Simpul Yang Dihubungkan Sumber Tegangan." },
  { "en": "KCL Diterapkan Pada?", "id": "Supernode Secara Keseluruhan." },
  { "en": "KVL Diterapkan Dimana?", "id": "Di Dalam Supernode." },
  { "en": "Apa Itu Supermesh?", "id": "Gabungan Dua Mesh Yang Berbagi Sumber Arus." },
  { "en": "KVL Diterapkan Pada?", "id": "Kontur Luar Supermesh." },
  { "en": "KCL Diterapkan Dimana?", "id": "Pada Simpul Dimana Sumber Arus Berada." },
  { "en": "Hasil Negatif Untuk Arus Berarti?", "id": "Arah Sebenarnya Berlawanan." },
  { "en": "Hasil Negatif Untuk Tegangan Berarti?", "id": "Polaritas Sebenarnya Terbalik." },
  { "en": "KCL Menghubungkan Arus-Arus Apa?", "id": "Arus-Arus Cabang Yang Bertemu." },
  { "en": "KVL Menghubungkan Tegangan-Tegangan Apa?", "id": "Tegangan Komponen Dalam Satu Loop." },
  { "en": "Resistansi Ekuivalen Seri Dihitung Dengan?", "id": "Menjumlahkan Semua Resistansi." },
  { "en": "Ini Berasal Dari?", "id": "KVL." },
  { "en": "Resistansi Ekuivalen Paralel Dihitung Dengan?", "id": "Menjumlahkan Kebalikan Resistansi." },
  { "en": "Ini Berasal Dari?", "id": "KCL." },
  { "en": "Apa Itu Rangkaian Resiprokal?", "id": "Rasio Output/Input Sama Jika Ditukar." },
  { "en": "Apa Itu Teorema Millman?", "id": "Metode Cepat Untuk Tegangan Paralel." },
  { "en": "Teorema Millman Berbasis?", "id": "KCL." },
  { "en": "Apa Itu Daya Listrik?", "id": "Laju Transfer Energi." },
  { "en": "Apa Itu Konservasi Daya?", "id": "Daya Dihasilkan Sama Dengan Daya Diserap." },
  { "en": "Ini Konsekuensi Dari?", "id": "KCL Dan KVL." },
  { "en": "Apa Itu Metode Potensial Simpul?", "id": "Sama Dengan Analisis Simpul." },
  { "en": "Apa Itu Metode Arus Loop?", "id": "Sama Dengan Analisis Mesh." },
  { "en": "Hukum Kirchhoff Adalah Hukum Empiris?", "id": "Bukan Berasal Dari Prinsip Fisika." },
  { "en": "Fisikanya Adalah Persamaan?", "id": "Persamaan Maxwell." },
  { "en": "KCL/KVL Adalah Aproksimasi Frekuensi Rendah?", "id": "Ya Tepat Sekali." },
  { "en": "Apa Itu Elemen Terpusat?", "id": "Ukuran Komponen Jauh Lebih Kecil Panjang Gelombang." },
  { "en": "Rangkaian Elektronik Umumnya?", "id": "Sistem Elemen Terpusat." },
  { "en": "Apa Itu Elemen Terdistribusi?", "id": "Ukuran Komponen Sebanding Panjang Gelombang." },
  { "en": "Contoh Elemen Terdistribusi?", "id": "Saluran Transmisi Dan Antena." },
  { "en": "KCL/KVL Perlu Dimodifikasi?", "id": "Ya Untuk Sistem Terdistribusi." },
  { "en": "Bagaimana Hukum Kirchhoff Membantu Debugging?", "id": "Memverifikasi Apakah Tegangan Arus Sesuai." },
  { "en": "Jika KVL Gagal Di Satu Loop?", "id": "Berarti Ada Komponen Rusak Atau Kesalahan." },
  { "en": "Jika KCL Gagal Di Satu Simpul?", "id": "Berarti Ada Jalur Terbuka Atau Hubungan Singkat." },
  { "en": "KCL Untuk Jaringan Pipa?", "id": "Ya Konsepnya Sama." },
  { "en": "KVL Untuk Jaringan Tekanan?", "id": "Ya Konsepnya Sama." },
  { "en": "Analogi Arus Adalah?", "id": "Debit Aliran Fluida." },
  { "en": "Analogi Tegangan Adalah?", "id": "Perbedaan Tekanan Fluida." },
  { "en": "Analogi Resistansi Adalah?", "id": "Hambatan Dalam Pipa." },
  { "en": "Analogi Ini Disebut Analogi?", "id": "Analogi Hidrolik." },
  { "en": "KCL Menyatakan Aliran Tidak Menumpuk?", "id": "Ya Di Persimpangan." },
  { "en": "KVL Menyatakan Tekanan Kembali Normal?", "id": "Ya Setelah Mengelilingi Sirkuit." },
  { "en": "Pentingnya Analogi Ini?", "id": "Membantu Intuisi Dan Pemahaman Konsep." },
  { "en": "Apa Itu Tegangan Common-Mode?", "id": "Tegangan Rata-rata Pada Jalur Diferensial." },
  { "en": "Apa Itu Arus Common-Mode?", "id": "Arus Yang Mengalir Searah." },
  { "en": "Apa Itu Tegangan Diferensial?", "id": "Perbedaan Tegangan Antara Dua Jalur." },
  { "en": "Apa Itu Arus Diferensial?", "id": "Arus Yang Mengalir Berlawanan Arah." },
  { "en": "Sinyal Yang Diinginkan Biasanya?", "id": "Sinyal Diferensial." },
  { "en": "Gangguan (Noise) Biasanya?", "id": "Sinyal Common-Mode." },
  { "en": "Apa Itu Loop Tanah (Ground Loop)?", "id": "Loop Tak Diinginkan Akibat Banyak Titik Ground." },
  { "en": "Menyebabkan Masalah Apa?", "id": "Gangguan Dengung (Hum) Frekuensi Rendah." },
  { "en": "KCL Menjelaskan Arus Terbagi Di?", "id": "Cabang Paralel." },
  { "en": "KVL Menjelaskan Tegangan Terbagi Di?", "id": "Komponen Seri." },
  { "en": "Rangkaian Seri Adalah Pembagi?", "id": "Tegangan." },
  { "en": "Rangkaian Paralel Adalah Pembagi?", "id": "Arus." },
  { "en": "Dasar Dari Semua Teori Rangkaian?", "id": "Adalah Hukum Kirchhoff." },
  { "en": "Apa Itu Kekekalan Muatan?", "id": "Total Muatan Listrik Selalu Tetap." },
  { "en": "Hukum Mana Yang Berasal Dari Ini?", "id": "Hukum Arus Kirchhoff (KCL)." },
  { "en": "Apa Itu Kekekalan Energi?", "id": "Total Energi Selalu Tetap." },
  { "en": "Hukum Mana Yang Berasal Dari Ini?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
  { "en": "Apa Beda Arus Dan Tegangan?", "id": "Arus Aliran Tegangan Adalah Dorongan." },
  { "en": "KCL Menganalisis Apa?", "id": "Aliran Arus Pada Titik Percabangan." },
  { "en": "KVL Menganalisis Apa?", "id": "Energi Tegangan Dalam Lintasan Tertutup." },
  { "en": "Apa Beda Simpul Dan Loop?", "id": "Simpul Adalah Titik Loop Adalah Lintasan." },
  { "en": "Metode Simpul Menggunakan Hukum Apa?", "id": "KCL." },
  { "en": "Metode Mesh Menggunakan Hukum Apa?", "id": "KVL." },
  { "en": "Apakah Hasil Akhir Harus Sama?", "id": "Ya Metode Berbeda Hasil Harus Sama." },
  { "en": "Tujuan Konvensi Tanda?", "id": "Menjaga Konsistensi Dalam Perhitungan." },
  { "en": "Hasil Arus Negatif Berarti Apa?", "id": "Arah Sebenarnya Berlawanan Dari Asumsi." },
  { "en": "Apa Yang Terjadi Di Rangkaian Seri?", "id": "Arus Sama Tegangan Terbagi." },
  { "en": "Apa Yang Terjadi Di Rangkaian Paralel?", "id": "Tegangan Sama Arus Terbagi." },
  { "en": "Aturan Pembagi Tegangan Berbasis?", "id": "KVL." },
  { "en": "Aturan Pembagi Arus Berbasis?", "id": "KCL." },
  { "en": "Apakah Hukum Ini Absolut?", "id": "Tidak Aproksimasi Untuk Frekuensi Rendah." },
  { "en": "Model Yang Digunakan Adalah?", "id": "Model Elemen Terpusat." },
  { "en": "Apa Batasan Hukum Kirchhoff?", "id": "Efek Perambatan Gelombang Diabaikan." },
  { "en": "Kapan Batasan Ini Menjadi Penting?", "id": "Pada Rangkaian Frekuensi Radio (RF)." },
  { "en": "Apa Itu Tegangan Referensi?", "id": "Titik Nol Untuk Semua Pengukuran Tegangan." },
  { "en": "Biasa Disebut Apa?", "id": "Ground Atau Tanah." },
  { "en": "Apa Esensi KCL?", "id": "Apa Pun Yang Masuk Harus Keluar." },
  { "en": "Apa Esensi KVL?", "id": "Setelah Satu Putaran Kembali Ke Awal." },
  { "en": "Hukum Kirchhoff Adalah Perkakas Untuk?", "id": "Menyusun Sistem Persamaan Linier." },
  { "en": "Menyelesaikan Persamaan Ini Memberikan?", "id": "Solusi Untuk Arus Dan Tegangan." },
  { "en": "Apakah KCL Berguna Untuk Pipa Air?", "id": "Ya Konsepnya Sama Persis." },
  { "en": "Debit Air Masuk Simpangan Sama Dengan?", "id": "Debit Air Keluar Simpangan." },
  { "en": "Apakah KVL Berguna Untuk Ketinggian?", "id": "Ya Konsepnya Sama Persis." },
  { "en": "Total Naik Turun Gunung Kembali Ke Awal?", "id": "Adalah Nol." },
  { "en": "Analogi Ini Berguna Untuk?", "id": "Membangun Intuisi." },
  { "en": "Apa Itu Arus Mesh?", "id": "Variabel Fiktif Untuk Analisis KVL." },
  { "en": "Apa Itu Tegangan Simpul?", "id": "Variabel Utama Untuk Analisis KCL." },
  { "en": "Metode Mana Yang Lebih Fundamental?", "id": "Metode Simpul Karena Selalu Berlaku." },
  { "en": "Apa Itu Superposisi?", "id": "Menganalisis Efek Setiap Sumber Secara Terpisah." },
  { "en": "Apakah Superposisi Selalu Berlaku?", "id": "Hanya Untuk Rangkaian Linier." },
  { "en": "Apa Itu Rangkaian Linier?", "id": "Output Proporsional Terhadap Input." },
  { "en": "Dioda Adalah Komponen Linier?", "id": "Tidak Dioda Adalah Non-Linier." },
  { "en": "Apa Itu Teorema Thevenin?", "id": "Menyederhanakan Rangkaian Menjadi Satu Sumber Tegangan." },
  { "en": "Apa Itu Teorema Norton?", "id": "Menyederhanakan Rangkaian Menjadi Satu Sumber Arus." },
  { "en": "Keduanya Membutuhkan Perhitungan Berbasis?", "id": "KCL Dan KVL." },
  { "en": "Apa Itu Sumber Terkendali?", "id": "Sumber Yang Dikontrol Oleh Besaran Lain." },
  { "en": "KCL/KVL Berlaku Untuk Sumber Terkendali?", "id": "Ya Tentu Saja." },
  { "en": "Apa Itu Arus Bocor?", "id": "Arus Yang Tidak Diinginkan." },
  { "en": "KCL Berguna Melacak Arus Bocor?", "id": "Ya Jika Jumlah Arus Tidak Nol." },
  { "en": "Apa Itu Loop Tanah?", "id": "Jalur Arus Tak Diinginkan Melalui Ground." },
  { "en": "KVL Bisa Menganalisis Loop Tanah?", "id": "Ya Tentu Saja." },
  { "en": "Bagaimana Cara Menggunakan KCL?", "id": "Pilih Simpul Tulis Persamaan Arus." },
  { "en": "Bagaimana Cara Menggunakan KVL?", "id": "Pilih Loop Tulis Persamaan Tegangan." },
  { "en": "Konsistensi Adalah Kunci?", "id": "Ya Sangat Penting." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Digunakan Untuk Mengukur Resistansi Tidak Dikenal." },
  { "en": "Kondisi Seimbang Terjadi Ketika?", "id": "Tegangan Antara Titik Tengah Nol." },
  { "en": "Ini Didasarkan Pada Analisis?", "id": "KVL Dan KCL." },
  { "en": "Dalam Rangkaian AC KCL/KVL Diterapkan Pada?", "id": "Fasor Dan Impedansi." },
  { "en": "Fasor Adalah Bilangan Kompleks?", "id": "Ya Mewakili Amplitudo Dan Fasa." },
  { "en": "Hukum Kirchhoff Bekerja Dengan Aljabar Kompleks?", "id": "Ya." },
  { "en": "KCL Adalah Dasar Untuk?", "id": "Teknik Analisis Simpul." },
  { "en": "KVL Adalah Dasar Untuk?", "id": "Teknik Analisis Mesh." },
  { "en": "Apa Itu Dualitas Dalam Rangkaian?", "id": "Konsep Simetri Antara KCL Dan KVL." },
  { "en": "Seri Adalah Dual Dari?", "id": "Paralel." },
  { "en": "Tegangan Adalah Dual Dari?", "id": "Arus." },
  { "en": "Induktor Adalah Dual Dari?", "id": "Kapasitor." },
  { "en": "Loop Adalah Dual Dari?", "id": "Simpul." },
  { "en": "Teori Graf Memberikan Dasar?", "id": "Matematis Untuk KCL Dan KVL." },
  { "en": "KCL Adalah Tentang Aliran Masuk Dan Keluar?", "id": "Benar." },
  { "en": "KVL Adalah Tentang Naik Dan Turun Potensial?", "id": "Benar." },
  { "en": "Potensial Di Titik Yang Sama Selalu?", "id": "Memiliki Nilai Tunggal." },
  { "en": "Ini Adalah Alasan Dibalik?", "id": "KVL." },
  { "en": "Muatan Tidak Bisa Hilang Tiba-tiba?", "id": "Benar." },
  { "en": "Ini Adalah Alasan Dibalik?", "id": "KCL." },
  { "en": "Hukum Kirchhoff Adalah Batu Penjuru?", "id": "Dari Semua Teori Rangkaian." },
  { "en": "Hampir Setiap Analisis Rangkaian Dimulai Dari?", "id": "KCL Atau KVL." },
  { "en": "Apa Itu Resistansi Internal Sumber?", "id": "Resistansi Intrinsik Di Dalam Sumber Nyata." },
  { "en": "KVL Membantu Menganalisis Efeknya?", "id": "Ya Tentu." },
  { "en": "Apa Itu Transfer Daya Maksimum?", "id": "Kondisi Untuk Transfer Daya Optimal." },
  { "en": "Terjadi Saat Resistansi Beban Sama Dengan?", "id": "Resistansi Sumber (Thevenin)." },
  { "en": "Pembuktiannya Menggunakan?", "id": "KVL Dan Kalkulus." },
  { "en": "Hukum Kirchhoff Adalah Hukum Universal?", "id": "Tidak Memiliki Batasan." },
  { "en": "Batasannya Adalah?", "id": "Model Elemen Terpusat." },
  { "en": "Artinya Panjang Gelombang Harus?", "id": "Jauh Lebih Besar Dari Ukuran Rangkaian." },
  { "en": "Untuk Listrik Rumah Tangga Apakah Berlaku?", "id": "Ya Berlaku Sempurna." },
  { "en": "Untuk Chip Komputer Modern?", "id": "Ya Masih Berlaku." },
  { "en": "Untuk Antena?", "id": "Tidak Lagi Berlaku." },
  { "en": "Antena Dianalisis Menggunakan?", "id": "Persamaan Maxwell." },
  { "en": "Apa Itu Pohon (Tree) Dalam Graf?", "id": "Kumpulan Cabang Tanpa Membentuk Loop." },
  { "en": "Konsep Ini Berguna Dalam?", "id": "Analisis Rangkaian Lanjutan." },
  { "en": "KCL Dan KVL Adalah Hukum?", "id": "Linier." },
  { "en": "Hasilnya Adalah Sistem Persamaan?", "id": "Linier." },
  { "en": "Yang Bisa Diselesaikan Dengan?", "id": "Metode Aljabar Linier." },
  { "en": "Apa Itu Sumber Nol?", "id": "Sumber Tegangan Nol Adalah Short Circuit." },
  { "en": "Sumber Arus Nol Adalah?", "id": "Open Circuit." },
  { "en": "Ini Digunakan Dalam Teorema?", "id": "Superposisi." },
  { "en": "Menguasai KCL/KVL Adalah Langkah Pertama?", "id": "Dalam Belajar Teknik Elektro." },
  { "en": "Tanpa KCL/KVL Analisis Rangkaian?", "id": "Hampir Tidak Mungkin Dilakukan." }




        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
