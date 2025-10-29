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


  { "en": "Apa Itu Sistem Tenaga (Power System) AC?", "id": "Sistem Gunakan Arus Bolak-Balik." },
  { "en": "Apa Itu Sistem Tenaga (Power System) DC?", "id": "Sistem Gunakan Arus Searah." },
  { "en": "Apa Keuntungan Transmisi (Transmission) AC?", "id": "Mudah Ubah Tegangan (Transformator)." },
  { "en": "Apa Keuntungan Transmisi (Transmission) DC (HVDC)?", "id": "Rugi Rendah Jarak Sangat Jauh." },
  { "en": "Apa Itu Jaringan Interkoneksi (Interconnected Grid)?", "id": "Jaringan Listrik Luas Terhubung Bersama." },
  { "en": "Manfaat Jaringan Interkoneksi (Interconnected Grid)?", "id": "Keandalan Tinggi, Ekonomi Skala." },
  { "en": "Apa Itu Pembangkit Listrik (Power Plant)?", "id": "Fasilitas Konversi Energi Primer Ke Listrik." },
  { "en": "Jenis Pembangkit Listrik (Power Plant) Termal?", "id": "PLTU (Batubara), PLTG (Gas), PLTN (Nuklir)." },
  { "en": "Jenis Pembangkit Listrik (Power Plant) Terbarukan?", "id": "PLTA (Air), PLTS (Surya), PLTB (Angin)." },
  { "en": "Apa Itu Efisiensi (Efficiency) Pembangkit?", "id": "Rasio Energi Listrik Output, Energi Primer Input." },
  { "en": "Apa Itu Turbin (Turbine)?", "id": "Mesin Putar Ubah Energi Fluida Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Sinkron?", "id": "Mesin Ubah Energi Mekanik Ke Listrik AC." },
  { "en": "Apa Itu Sistem Eksitasi (Excitation System)?", "id": "Menyuplai Arus DC Ke Medan Generator." },
  { "en": "Apa Fungsi Sistem Eksitasi (Excitation System)?", "id": "Mengontrol Tegangan Output Generator." },
  { "en": "Apa Itu Governor (Governor) Turbin?", "id": "Mengatur Kecepatan Putaran Turbin." },
  { "en": "Apa Fungsi Governor (Governor) Turbin?", "id": "Mengontrol Frekuensi Sistem Listrik." },
  { "en": "Apa Itu Sinkronisasi (Synchronization) Generator?", "id": "Menghubungkan Generator Ke Jaringan Paralel." },
  { "en": "Syarat Sinkronisasi (Synchronization)?", "id": "Tegangan, Frekuensi, Fasa, Urutan Sama." },
  { "en": "Apa Itu Pembagian Beban (Load Sharing)?", "id": "Distribusi Beban Antar Generator Paralel." },
  { "en": "Bagaimana Atur Pembagian Beban Aktif (P)?", "id": "Melalui Governor (Input Penggerak)." },
  { "en": "Bagaimana Atur Pembagian Beban Reaktif (Q)?", "id": "Melalui Eksitasi (AVR)." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Konduktor Penyalur Listrik Jarak Jauh." },
  { "en": "Jenis Saluran Transmisi (Transmission Line)?", "id": "Saluran Udara Dan Kabel Bawah Tanah." },
  { "en": "Apa Itu Menara Transmisi (Transmission Tower)?", "id": "Struktur Penyangga Konduktor Saluran Udara." },
  { "en": "Apa Itu Isolator (Insulator) Transmisi?", "id": "Komponen Isolasi Konduktor Dari Menara." },
  { "en": "Apa Itu Efek Korona (Corona Effect)?", "id": "Ionisasai Udara Sekitar Konduktor HV." },
  { "en": "Apa Akibat Efek Korona (Corona Effect)?", "id": "Rugi Daya, Noise Radio, Ozon." },
  { "en": "Apa Itu Kabel Bawah Tanah (Underground Cable)?", "id": "Kabel Berisolasi Ditanam Di Tanah." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Transformasi Tegangan, Switching." },
  { "en": "Apa Itu Transformator Daya (Power Transformer)?", "id": "Transformator Kapasitas Besar Gardu Induk." },
  { "en": "Apa Itu Pemutus Tenaga (Circuit Breaker)?", "id": "Saklar Otomatis Pemutus Arus Gangguan." },
  { "en": "Media Pemadam Busur Api (Arc Quenching)?", "id": "Udara, Minyak, Vakum, Gas SF6." },
  { "en": "Apa Itu Pemisah (Disconnector)?", "id": "Saklar Pemisah Tanpa Beban." },
  { "en": "Apa Itu Trafo Instrumen (Instrument Transformer)?", "id": "CT (Current Transformer) Dan PT (Potential Transformer)." },
  { "en": "Apa Itu Rele Proteksi (Protection Relay)?", "id": "Pendeteksi Gangguan, Pemicu Pemutus." },
  { "en": "Apa Itu Sistem Proteksi (Protection System) Utama?", "id": "Proteksi Utama (Main Protection)." },
  { "en": "Apa Itu Sistem Proteksi (Protection System) Cadangan?", "id": "Proteksi Cadangan (Backup Protection)." },
  { "en": "Apa Itu Zona Proteksi (Zone of Protection)?", "id": "Area Dilindungi Sistem Proteksi." },
  { "en": "Apa Itu Proteksi Diferensial (Differential Protection)?", "id": "Proteksi Cepat Sensitif Gangguan Internal." },
  { "en": "Dimana Proteksi Diferensial (Differential Protection) Digunakan?", "id": "Trafo, Generator, Busbar, Saluran." },
  { "en": "Apa Itu Proteksi Jarak (Distance Protection)?", "id": "Proteksi Saluran Transmisi Ukur Impedansi." },
  { "en": "Apa Itu Proteksi Arus Lebih (Overcurrent Protection)?", "id": "Proteksi Berdasarkan Tingkat Arus." },
  { "en": "Apa Itu Proteksi Gangguan Tanah (Ground Fault)?", "id": "Proteksi Mendeteksi Arus Ke Tanah." },
  { "en": "Apa Itu Jaringan Distribusi (Distribution Network)?", "id": "Menyalurkan Listrik Ke Konsumen Akhir." },
  { "en": "Apa Itu Jaringan Primer (Primary Distribution)?", "id": "Jaringan Tegangan Menengah (MV)." },
  { "en": "Apa Itu Jaringan Sekunder (Secondary Distribution)?", "id": "Jaringan Tegangan Rendah (LV)." },
  { "en": "Apa Itu Trafo Distribusi (Distribution Transformer)?", "id": "Menurunkan Tegangan MV Ke LV." },
  { "en": "Apa Itu Jaringan Radial (Radial Network)?", "id": "Satu Arah Sumber Ke Beban." },
  { "en": "Apa Itu Jaringan Loop (Loop Network)?", "id": "Beban Dapat Disuplai Dua Arah." },
  { "en": "Apa Itu Recloser (Recloser)?", "id": "Pemutus Otomatis Tutup Kembali." },
  { "en": "Apa Itu Sectionalizer (Sectionalizer)?", "id": "Pemisah Otomatis Isolasi Gangguan Permanen." },
  { "en": "Apa Itu Sekring Pelebur (Fuse Cutout)?", "id": "Pelindung Sisi Primer Trafo Distribusi." },
  { "en": "Apa Itu Arester Surja (Surge Arrester)?", "id": "Pelindung Tegangan Lebih Surja." },
  { "en": "Apa Itu Pengukuran Energi (Energy Metering)?", "id": "Mengukur Konsumsi Energi Listrik." },
  { "en": "Apa Itu kWh Meter (kWh Meter)?", "id": "Alat Ukur Energi Listrik (Kilowatt-Hour)." },
  { "en": "Apa Itu Meter Cerdas (Smart Meter)?", "id": "Meter Dengan Komunikasi Dua Arah." },
  { "en": "Apa Itu Kualitas Daya (Power Quality)?", "id": "Karakteristik Ideal Tegangan, Arus." },
  { "en": "Masalah Kualitas Daya (Power Quality)?", "id": "Sag, Swell, Harmonisa, Transien." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Frekuensi Kelipatan Fundamental." },
  { "en": "Apa Penyebab Harmonisa (Harmonics)?", "id": "Beban Non-Linear." },
  { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Ukuran Efisiensi Penggunaan Daya." },
  { "en": "Bagaimana Memperbaiki Faktor Daya (Power Factor)?", "id": "Pasang Kapasitor Bank." },
  { "en": "Apa Itu Grounding (Pembumian) Peralatan?", "id": "Menghubungkan Bodi Logam Ke Tanah." },
  { "en": "Apa Itu Keselamatan Listrik (Electrical Safety)?", "id": "Pencegahan Kecelakaan Akibat Listrik." },
  { "en": "Apa Bahaya Utama Listrik?", "id": "Sengatan, Kebakaran, Busur Api." },
  { "en": "Apa Itu APD (Alat Pelindung Diri)?", "id": "Perlengkapan Keselamatan Personal." },
  { "en": "Apa Itu Prosedur LOTO (Lockout/Tagout)?", "id": "Penguncian Sumber Energi Saat Perbaikan." },
  { "en": "Apa Itu Elektronika Analog (Analog Electronics)?", "id": "Pemrosesan Sinyal Kontinu." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Meningkatkan Amplitudo Sinyal." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Memilih Atau Menolak Frekuensi." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Menghasilkan Sinyal Periodik." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "Komponen Penguat Serbaguna." },
  { "en": "Apa Itu Elektronika Digital (Digital Electronics)?", "id": "Pemrosesan Sinyal Diskrit (0, 1)." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Dasar Operasi Logika Digital." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Penyimpan Memori Satu Bit." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Terintegrasi Chip Tunggal." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Mengubah Besaran Fisik Ke Sinyal Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Mengubah Sinyal Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Kontrol Umpan Balik (Feedback Control)?", "id": "Menggunakan Output Mengoreksi Input." },
  { "en": "Apa Itu Kontrol PID (PID Control)?", "id": "Jenis Kontroler Umpan Balik Umum." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Digital Industri." },
  { "en": "Apa Itu Komunikasi (Communication)?", "id": "Proses Pertukaran Informasi." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Informasi Ke Sinyal Pembawa." },
  { "en": "Apa Itu Komunikasi Nirkabel (Wireless Communication)?", "id": "Komunikasi Tanpa Media Kabel." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Pancar/Terima Gelombang Radio." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komputer." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Skala Global." },
  { "en": "Apa Itu IP Address (Alamat IP)?", "id": "Alamat Unik Perangkat Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Penghubung Antar Jaringan." },
  { "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Perlindungan Sistem Komputer Dari Ancaman." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Pengkodean Data Agar Tidak Terbaca." },
  { "en": "Apa Itu Firewall (Firewall)?", "id": "Penyaring Lalu Lintas Jaringan." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Penentuan Nilai Besaran Kuantitatif." },
  { "en": "Apa Itu Akurasi (Accuracy) Pengukuran?", "id": "Kedekatan Hasil Ukur Nilai Benar." },
  { "en": "Apa Itu Presisi (Precision) Pengukuran?", "id": "Konsistensi Hasil Pengukuran Berulang." },
  { "en": "Apa Itu Kalibrasi (Calibration)?", "id": "Penyesuaian Alat Ukur Sesuai Standar." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Sinyal Listrik." },
  { "en": "Apa Itu Bahan (Material) Teknik Elektro?", "id": "Konduktor, Semikonduktor, Isolator, Magnetik." },
  { "en": "Apa Itu Konduktor (Conductor)?", "id": "Bahan Mudah Hantarkan Listrik." },
  { "en": "Apa Itu Isolator (Insulator)?", "id": "Bahan Sulit Hantarkan Listrik." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Bahan Sifat Antara Konduktor, Isolator." },
  { "en": "Apa Itu Bahan Magnetik (Magnetic Material)?", "id": "Bahan Berinteraksi Medan Magnet." },
  { "en": "Apa Itu Superkonduktor (Superconductor)?", "id": "Bahan Resistansi Nol Suhu Rendah." },
  { "en": "Apa Itu Medan Listrik (Electric Field) Statis?", "id": "Medan Listrik Dihasilkan Muatan Diam." },
  { "en": "Apa Itu Medan Magnet (Magnetic Field) Statis?", "id": "Medan Magnet Dihasilkan Arus Konstan (Magnet)." },
  { "en": "Apa Itu Elektrodinamika (Electrodynamics)?", "id": "Studi Medan Elektromagnetik Berubah Waktu." },
  { "en": "Apa Itu Persamaan Maxwell (Maxwell's Equations)?", "id": "Empat Persamaan Dasar Elektrodinamika Klasik." },
  { "en": "Apa Itu Hukum Gauss (Gauss's Law) Listrik?", "id": "Sumber Medan Listrik Adalah Muatan Listrik." },
  { "en": "Apa Itu Hukum Gauss (Gauss's Law) Magnet?", "id": "Tidak Ada Sumber Monopol Magnetik." },
  { "en": "Apa Itu Hukum Faraday (Faraday's Law) Induksi?", "id": "Perubahan Medan Magnet Hasilkan Medan Listrik." },
  { "en": "Apa Itu Hukum Ampere-Maxwell (Ampere-Maxwell Law)?", "id": "Arus Listrik, Perubahan Medan Listrik Hasilkan Medan Magnet." },
  { "en": "Apa Itu Potensial (Potential) Skalar Listrik (V)?", "id": "Potensial Terkait Medan Listrik Statis." },
  { "en": "Apa Itu Potensial (Potential) Vektor Magnetik (A)?", "id": "Potensial Terkait Medan Magnet." },
  { "en": "Apa Itu Gauge Transformation (Transformasi Gauge)?", "id": "Kebebasan Memilih Potensial (V, A)." },
  { "en": "Apa Itu Gauge Lorenz (Lorenz Gauge)?", "id": "Kondisi Gauge Umum Digunakan." },
  { "en": "Apa Itu Gauge Coulomb (Coulomb Gauge)?", "id": "Kondisi Gauge Lain (Divergensi A Nol)." },
  { "en": "Apa Itu Persamaan Gelombang (Wave Equation) Elektromagnetik?", "id": "Persamaan Deskripsi Perambatan Gelombang EM." },
  { "en": "Apa Kecepatan Gelombang Elektromagnetik (Electromagnetic Wave) Vakum?", "id": "Kecepatan Cahaya (c)." },
  { "en": "Apa Itu Vektor Poynting (Poynting Vector)?", "id": "Arah Dan Laju Aliran Energi EM." },
  { "en": "Apa Itu Momentum (Momentum) Elektromagnetik?", "id": "Medan EM Membawa Momentum." },
  { "en": "Apa Itu Tensor Tegangan Maxwell (Maxwell Stress Tensor)?", "id": "Deskripsi Momentum, Gaya Medan EM." },
  { "en": "Apa Itu Radiasi (Radiation)?", "id": "Pancaran Energi Gelombang Elektromagnetik." },
  { "en": "Kapan Muatan Meradiasi (Radiate)?", "id": "Saat Muatan Dipercepat." },
  { "en": "Apa Itu Radiasi Dipol (Dipole Radiation)?", "id": "Radiasi Dari Dipol Listrik Osilasi." },
  { "en": "Apa Itu Rumus Larmor (Larmor Formula)?", "id": "Total Daya Radiasi Muatan Titik Dipercepat." },
  { "en": "Apa Itu Bremsstrahlung (Pengereman Radiasi)?", "id": "Radiasi Akibat Pengereman Muatan Cepat." },
  { "en": "Apa Itu Radiasi Sinkrotron (Synchrotron Radiation)?", "id": "Radiasi Muatan Bergerak Melingkar Kecepatan Tinggi." },
  { "en": "Apa Itu Medan Dekat (Near Field)?", "id": "Daerah Dekat Sumber (Medan Reaktif)." },
  { "en": "Apa Itu Medan Jauh (Far Field)?", "id": "Daerah Jauh Sumber (Medan Radiasi)." },
  { "en": "Apa Itu Potensial Lienard-Wiechert (Lienard-Wiechert Potential)?", "id": "Potensial Dihasilkan Muatan Titik Bergerak." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Struktur Meradiasikan/Menerima Gelombang EM." },
  { "en": "Apa Itu Resistansi Radiasi (Radiation Resistance)?", "id": "Resistansi Ekuivalen Daya Radiasi Antena." },
  { "en": "Apa Itu Penguatan (Gain) Antena?", "id": "Keterarahan Pancaran Energi Antena." },
  { "en": "Apa Itu Pemandu Gelombang (Waveguide)?", "id": "Struktur Pemandu Gelombang Frekuensi Tinggi." },
  { "en": "Apa Itu Mode TE (Transverse Electric)?", "id": "Medan Listrik Tegak Lurus Arah Rambat." },
  { "en": "Apa Itu Mode TM (Transverse Magnetic)?", "id": "Medan Magnet Tegak Lurus Arah Rambat." },
  { "en": "Apa Itu Mode TEM (Transverse Electromagnetic)?", "id": "Medan E, B Tegak Lurus Arah Rambat (Koaksial)." },
  { "en": "Apa Itu Relativitas Khusus (Special Relativity)?", "id": "Teori Ruang Waktu Kerangka Inersial." },
  { "en": "Apa Postulat Dasar Relativitas Khusus?", "id": "Prinsip Relativitas, Kecepatan Cahaya Konstan." },
  { "en": "Apa Itu Transformasi Lorentz (Lorentz Transformation)?", "id": "Transformasi Koordinat Antar Kerangka Inersial." },
  { "en": "Apa Itu Dilatasi Waktu (Time Dilation)?", "id": "Waktu Bergerak Lebih Lambat Pengamat Bergerak." },
  { "en": "Apa Itu Kontraksi Panjang (Length Contraction)?", "id": "Panjang Objek Tampak Memendek Arah Gerak." },
  { "en": "Apa Itu Ruang Waktu Minkowski (Minkowski Spacetime)?", "id": "Model Geometri Ruang Waktu Relativitas Khusus." },
  { "en": "Apa Itu Vektor Empat (Four-Vector)?", "id": "Vektor Ruang Waktu (Posisi, Momentum)." },
  { "en": "Apa Itu Elektromagnetisme Relativistik (Relativistic Electromagnetism)?", "id": "Formulasi Elektromagnetisme Konsisten Relativitas." },
  { "en": "Apa Itu Tensor Medan Elektromagnetik (Field Tensor)?", "id": "Tensor Gabungkan Medan E Dan B." },
  { "en": "Bagaimana Persamaan Maxwell (Maxwell's Equations) Bentuk Kovarian?", "id": "Dua Persamaan Tensor Ringkas." },
  { "en": "Apa Itu Optika (Optics)?", "id": "Studi Perilaku Cahaya." },
  { "en": "Apa Itu Optika Geometri (Geometrical Optics)?", "id": "Pendekatan Cahaya Sinar Lurus." },
  { "en": "Apa Itu Prinsip Fermat (Fermat's Principle)?", "id": "Cahaya Tempuh Lintasan Waktu Minimum." },
  { "en": "Apa Itu Pemantulan (Reflection)?", "id": "Perubahan Arah Cahaya Permukaan Batas." },
  { "en": "Apa Itu Hukum Pemantulan (Law of Reflection)?", "id": "Sudut Datang Sama Dengan Sudut Pantul." },
  { "en": "Apa Itu Pembiasan (Refraction)?", "id": "Pembelokan Cahaya Antar Medium Berbeda." },
  { "en": "Apa Itu Hukum Snellius (Snell's Law)?", "id": "Hubungan Sudut Bias Indeks Bias." },
  { "en": "Apa Itu Indeks Bias (Refractive Index)?", "id": "Rasio Kelajuan Cahaya Vakum, Medium." },
  { "en": "Apa Itu Lensa (Lens)?", "id": "Komponen Optik Membiaskan Cahaya." },
  { "en": "Apa Itu Persamaan Pembuat Lensa (Lensmaker's Equation)?", "id": "Hubungan Fokus, Jari-Jari, Indeks Bias." },
  { "en": "Apa Itu Kekuatan Lensa (Lens Power)?", "id": "Ukuran Kemampuan Fokuskan Cahaya (Dioptri)." },
  { "en": "Apa Itu Aberasi (Aberration) Lensa?", "id": "Ketidaksempurnaan Pembentukan Citra Lensa." },
  { "en": "Jenis Aberasi (Aberration) Utama?", "id": "Sferis, Kromatik, Koma, Astigmatisma." },
  { "en": "Apa Itu Aberasi Sferis (Spherical Aberration)?", "id": "Sinar Paralel Fokus Titik Berbeda." },
  { "en": "Apa Itu Aberasi Kromatik (Chromatic Aberration)?", "id": "Fokus Berbeda Panjang Gelombang Berbeda." },
  { "en": "Apa Itu Optika Fisik (Physical Optics)?", "id": "Pendekatan Cahaya Sebagai Gelombang." },
  { "en": "Apa Itu Interferensi (Interference)?", "id": "Superposisi Gelombang Hasilkan Pola Intensitas." },
  { "en": "Apa Itu Koherensi (Coherence)?", "id": "Hubungan Fasa Stabil Antar Gelombang." },
  { "en": "Apa Itu Difraksi (Diffraction)?", "id": "Penyebaran Gelombang Sekitar Hambatan." },
  { "en": "Apa Itu Polarisasi (Polarization)?", "id": "Orientasi Getaran Medan Listrik Cahaya." },
  { "en": "Apa Itu Mekanika Statistik (Statistical Mechanics)?", "id": "Hubungkan Sifat Mikroskopik Makroskopik Sistem." },
  { "en": "Apa Itu Distribusi Boltzmann (Boltzmann Distribution)?", "id": "Probabilitas Sistem Keadaan Energi Tertentu." },
  { "en": "Apa Itu Fungsi Partisi (Partition Function)?", "id": "Besaran Termodinamika Dari Keadaan Mikro." },
  { "en": "Apa Itu Termodinamika (Thermodynamics)?", "id": "Studi Panas, Kerja, Energi." },
  { "en": "Apa Hukum Pertama Termodinamika?", "id": "Kekekalan Energi." },
  { "en": "Apa Hukum Kedua Termodinamika?", "id": "Entropi (Entropy) Cenderung Meningkat." },
  { "en": "Apa Itu Entropi (Entropy)?", "id": "Ukuran Ketidakteraturan Sistem." },
  { "en": "Apa Itu Temperatur (Temperature)?", "id": "Ukuran Energi Kinetik Rata-Rata." },
  { "en": "Apa Itu Panas (Heat)?", "id": "Transfer Energi Akibat Perbedaan Temperatur." },
  { "en": "Apa Itu Kerja (Work)?", "id": "Transfer Energi Bentuk Teratur." },
  { "en": "Apa Itu Energi Bebas (Free Energy)?", "id": "Energi Tersedia Lakukan Kerja (Gibbs/Helmholtz)." },
  { "en": "Apa Itu Potensial Kimia (Chemical Potential)?", "id": "Perubahan Energi Penambahan Partikel." },
  { "en": "Apa Itu Transisi Fasa (Phase Transition)?", "id": "Perubahan Mendadak Sifat Makroskopik." },
  { "en": "Apa Itu Mekanika Kuantum (Quantum Mechanics)?", "id": "Teori Fisika Dunia Skala Atom." },
  { "en": "Apa Itu Fungsi Gelombang (Wave Function)?", "id": "Deskripsi Keadaan Sistem Kuantum." },
  { "en": "Apa Itu Persamaan Schrödinger (Schrödinger Equation)?", "id": "Persamaan Evolusi Fungsi Gelombang." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Besaran Fisik Nilai Diskrit." },
  { "en": "Apa Itu Prinsip Ketidakpastian (Uncertainty Principle)?", "id": "Batas Akurasi Pengukuran Pasangan Variabel." },
  { "en": "Apa Itu Spin (Spin)?", "id": "Momentum Sudut Intrinsik Partikel." },
  { "en": "Apa Itu Prinsip Eksklusi Pauli (Pauli Exclusion Principle)?", "id": "Dua Fermion Tak Bisa Keadaan Sama." },
  { "en": "Apa Itu Statistik Kuantum (Quantum Statistics)?", "id": "Fermi-Dirac (Fermion), Bose-Einstein (Boson)." },
  { "en": "Apa Itu Foton (Photon)?", "id": "Partikel Kuantum Cahaya." },
  { "en": "Apa Itu Fisika Atom (Atomic Physics)?", "id": "Studi Struktur, Sifat Atom." },
  { "en": "Apa Itu Tingkat Energi (Energy Level) Atom?", "id": "Energi Elektron Terkuantisasi Atom." },
  { "en": "Apa Itu Spektroskopi (Spectroscopy)?", "id": "Analisis Cahaya Dipancarkan/Diserap Atom." },
  { "en": "Apa Itu Fisika Molekuler (Molecular Physics)?", "id": "Studi Struktur, Sifat Molekul." },
  { "en": "Apa Itu Ikatan Kimia (Chemical Bond)?", "id": "Gaya Ikat Atom Membentuk Molekul." },
  { "en": "Apa Itu Fisika Benda Terkondensasi (Condensed Matter)?", "id": "Studi Sifat Materi Padat, Cair." },
  { "en": "Apa Itu Struktur Kristal (Crystal Structure)?", "id": "Susunan Atom Teratur Padatan." },
  { "en": "Apa Itu Struktur Pita (Band Structure)?", "id": "Energi Elektron Diperbolehkan Kristal." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Material Konduktivitas Antara Logam, Isolator." },
  { "en": "Apa Itu Superkonduktor (Superconductor)?", "id": "Material Resistansi Nol Suhu Rendah." },
  { "en": "Apa Itu Fisika Nuklir (Nuclear Physics)?", "id": "Studi Inti Atom." },
  { "en": "Apa Itu Gaya Nuklir Kuat (Strong Nuclear Force)?", "id": "Gaya Ikat Proton, Neutron Inti." },
  { "en": "Apa Itu Radioaktivitas (Radioactivity)?", "id": "Peluruhan Spontan Inti Tak Stabil." },
  { "en": "Apa Itu Fisi (Fission) Nuklir?", "id": "Pembelahan Inti Berat Jadi Lebih Ringan." },
  { "en": "Apa Itu Fusi (Fusion) Nuklir?", "id": "Penggabungan Inti Ringan Jadi Lebih Berat." },
  { "en": "Apa Itu Fisika Partikel (Particle Physics)?", "id": "Studi Partikel Dasar, Interaksinya." },
  { "en": "Apa Itu Model Standar (Standard Model)?", "id": "Teori Partikel Dasar, Gaya Fundamental." },
  { "en": "Apa Itu Quark (Quark)?", "id": "Konstituen Dasar Hadron (Proton, Neutron)." },
  { "en": "Apa Itu Lepton (Lepton)?", "id": "Partikel Dasar (Elektron, Neutrino)." },
  { "en": "Apa Itu Boson Gauge (Gauge Boson)?", "id": "Partikel Perantara Gaya Fundamental." },
  { "en": "Apa Itu Boson Higgs (Higgs Boson)?", "id": "Partikel Terkait Pemberian Massa." },
  { "en": "Apa Itu Kosmologi (Cosmology)?", "id": "Studi Asal Usul, Evolusi Alam Semesta." },
  { "en": "Apa Itu Dentuman Besar (Big Bang)?", "id": "Teori Awal Mula Alam Semesta." },
  { "en": "Apa Itu Radiasi Latar Belakang Kosmik (CMB)?", "id": "Sisa Cahaya Panas Dentuman Besar." },
  { "en": "Apa Itu Materi Gelap (Dark Matter)?", "id": "Materi Tak Terlihat Interaksi Gravitasi." },
  { "en": "Apa Itu Energi Gelap (Dark Energy)?", "id": "Energi Hipotetis Penyebab Ekspansi Dipercepat." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Transistor Efek Medan Gerbang Terisolasi." },
  { "en": "Apa Tiga Terminal Utama MOSFET?", "id": "Gate (Gerbang), Drain (Saluran Keluar), Source (Sumber)." },
  { "en": "Apa Fungsi Terminal Gate (Gerbang)?", "id": "Mengontrol Konduktivitas Antara Drain, Source." },
  { "en": "Apa Lapisan Isolator Di Bawah Gate?", "id": "Silikon Dioksida (SiO₂) Sangat Tipis." },
  { "en": "Apa Keuntungan Utama MOSFET?", "id": "Impedansi Input Gate Sangat Tinggi." },
  { "en": "Apa Dua Jenis Utama MOSFET?", "id": "Enhancement-Mode (Mode Peningkatan) Dan Depletion-Mode (Mode Deplesi)." },
  { "en": "Apa Itu MOSFET Enhancement-Mode (Mode Peningkatan)?", "id": "Normally OFF (Butuh Tegangan Gate Aktif)." },
  { "en": "Apa Itu MOSFET Depletion-Mode (Mode Deplesi)?", "id": "Normally ON (Konduktif Tanpa Tegangan Gate)." },
  { "en": "Apa Dua Tipe Saluran (Channel) MOSFET?", "id": "N-Channel (Saluran N) Dan P-Channel (Saluran P)." },
  { "en": "Pembawa Muatan Mayoritas N-Channel MOSFET?", "id": "Elektron." },
  { "en": "Pembawa Muatan Mayoritas P-Channel MOSFET?", "id": "Lubang (Hole)." },
  { "en": "Tegangan Apa Aktifkan N-Channel Enhancement?", "id": "Tegangan Gate Positif Relatif Source." },
  { "en": "Tegangan Apa Aktifkan P-Channel Enhancement?", "id": "Tegangan Gate Negatif Relatif Source." },
  { "en": "Apa Itu Tegangan Threshold (Threshold Voltage) (Vt)?", "id": "Tegangan Gate Minimum Aktifkan Saluran." },
  { "en": "Apa Daerah Operasi Utama MOSFET?", "id": "Cutoff, Triode (Linear), Saturasi (Aktif)." },
  { "en": "Kapan MOSFET Berada Di Cutoff (Cutoff)?", "id": "Saat Tegangan Gate Kurang Dari Vt (Vgs < Vt)." },
  { "en": "Kapan MOSFET Berada Di Triode (Triode/Linear)?", "id": "Saat Vgs > Vt Dan Vds < (Vgs - Vt)." },
  { "en": "Kapan MOSFET Berada Di Saturasi (Saturation)?", "id": "Saat Vgs > Vt Dan Vds > (Vgs - Vt)." },
  { "en": "Di Daerah Mana MOSFET Bertindak Resistor Variabel?", "id": "Daerah Triode (Triode/Linear)." },
  { "en": "Di Daerah Mana MOSFET Bertindak Sumber Arus Terkontrol?", "id": "Daerah Saturasi (Saturation)." },
  { "en": "Apa Itu Transkonduktansi (Transconductance) (gm)?", "id": "Ukuran Perubahan Arus Drain Per Tegangan Gate." },
  { "en": "Apa Itu Resistansi Output (Output Resistance) (ro)?", "id": "Resistansi Drain Ke Source Di Saturasi." },
  { "en": "Apa Itu Efek Modulasi Panjang Saluran (Channel Length Modulation)?", "id": "Penyebab Resistansi Output (ro) Terbatas." },
  { "en": "Apa Itu Efek Tubuh (Body Effect)?", "id": "Perubahan Vt Akibat Tegangan Substrat/Body." },
  { "en": "Apa Terminal Keempat MOSFET?", "id": "Body (Bulk Atau Substrat)." },
  { "en": "Bagaimana Terminal Body Biasa Dihubungkan?", "id": "Ke Source (Atau Tegangan Paling Rendah/Tinggi)." },
  { "en": "Apa Itu Kapasitansi (Capacitance) Gate-Source (Cgs)?", "id": "Kapasitansi Parasitik Antara Gate, Source." },
  { "en": "Apa Itu Kapasitansi (Capacitance) Gate-Drain (Cgd)?", "id": "Kapasitansi Parasitik Antara Gate, Drain." },
  { "en": "Nama Lain Kapasitansi (Capacitance) Cgd?", "id": "Kapasitansi Miller (Miller Capacitance)." },
  { "en": "Apa Efek Kapasitansi Miller (Miller Effect)?", "id": "Meningkatkan Kapasitansi Input Efektif Penguat." },
  { "en": "Apa Itu CMOS (Complementary MOS)?", "id": "Teknologi Gunakan NMOS Dan PMOS Bersama." },
  { "en": "Apa Keunggulan Utama Logika CMOS?", "id": "Konsumsi Daya Statis Sangat Rendah." },
  { "en": "Apa Struktur Dasar Inverter CMOS?", "id": "Satu PMOS (Pull-Up) Seri Satu NMOS (Pull-Down)." },
  { "en": "Kapan PMOS Aktif Di Inverter CMOS?", "id": "Saat Input Gate Rendah (Low)." },
  { "en": "Kapan NMOS Aktif Di Inverter CMOS?", "id": "Saat Input Gate Tinggi (High)." },
  { "en": "Mengapa Daya Statis CMOS Rendah?", "id": "Salah Satu Transistor Selalu Off." },
  { "en": "Apa Penyebab Utama Konsumsi Daya Dinamis CMOS?", "id": "Pengisian, Pengosongan Kapasitansi Beban." },
  { "en": "Apa Itu Arus Crowbar (Crowbar Current)?", "id": "Arus Singkat Sesaat Kedua Transistor On." },
  { "en": "Apa Itu Gerbang Transmisi (Transmission Gate) CMOS?", "id": "Saklar Analog NMOS Paralel PMOS." },
  { "en": "Keunggulan Gerbang Transmisi (Transmission Gate)?", "id": "Resistansi On Relatif Konstan." },
  { "en": "Apa Itu Latch-Up (Latch-Up) CMOS?", "id": "Struktur SCR Parasitik Sebabkan Hubung Singkat." },
  { "en": "Apa Itu Penguat Common Source (Common Source)?", "id": "Konfigurasi Penguat Dasar MOSFET (Mirip CE)." },
  { "en": "Apa Karakteristik Penguat Common Source?", "id": "Gain Tegangan Tinggi, Impedansi Input Tinggi, Fasa Balik." },
  { "en": "Apa Itu Penguat Common Drain (Common Drain)?", "id": "Pengikut Source (Source Follower) (Mirip CC)." },
  { "en": "Apa Karakteristik Penguat Common Drain?", "id": "Gain Tegangan Mendekati Satu, Impedansi Output Rendah." },
  { "en": "Apa Itu Penguat Common Gate (Common Gate)?", "id": "Konfigurasi Penguat Lain (Mirip CB)." },
  { "en": "Apa Karakteristik Penguat Common Gate?", "id": "Impedansi Input Rendah, Bandwidth Tinggi." },
  { "en": "Apa Itu Beban Aktif (Active Load)?", "id": "Menggunakan Transistor Sebagai Beban Penguat." },
  { "en": "Keuntungan Beban Aktif (Active Load)?", "id": "Gain Tinggi, Area Chip Kecil." },
  { "en": "Apa Itu Cermin Arus (Current Mirror)?", "id": "Menyalin Arus Referensi Ke Cabang Lain." },
  { "en": "Apa Fungsi Cermin Arus (Current Mirror)?", "id": "Sebagai Sumber Arus Bias Atau Beban Aktif." },
  { "en": "Apa Itu Penguat Diferensial (Differential Amplifier) MOSFET?", "id": "Menguatkan Selisih Antara Dua Input." },
  { "en": "Apa Itu Penguat Cascode (Cascode Amplifier)?", "id": "Kombinasi Common Source, Common Gate." },
  { "en": "Keuntungan Penguat Cascode (Cascode Amplifier)?", "id": "Gain Tinggi, Bandwidth Lebar." },
  { "en": "Apa Itu Power MOSFET (MOSFET Daya)?", "id": "MOSFET Dirancang Tangani Arus, Tegangan Tinggi." },
  { "en": "Struktur Umum Power MOSFET (MOSFET Daya)?", "id": "Struktur Vertikal (VDMOS, Trench)." },
  { "en": "Apa Itu RDS(on) (RDS(on)) Power MOSFET?", "id": "Resistansi Drain-Source Saat Kondisi On." },
  { "en": "RDS(on) (RDS(on)) Rendah Diinginkan?", "id": "Ya, Mengurangi Rugi Konduksi." },
  { "en": "Apa Itu Gerbang Muatan (Gate Charge) (Qg)?", "id": "Total Muatan Perlu Charge/Discharge Gate." },
  { "en": "Mengapa Qg (Gerbang Muatan) Penting?", "id": "Mempengaruhi Kecepatan Switching, Rugi Switching." },
  { "en": "Apa Itu Dioda Body (Body Diode) MOSFET?", "id": "Dioda Parasitik Antara Drain, Source." },
  { "en": "Kapan Dioda Body (Body Diode) Konduksi?", "id": "Saat Tegangan Drain Negatif Relatif Source (N-Ch)." },
  { "en": "Apa Itu Avalanche Breakdown (Breakdown Avalanche)?", "id": "Breakdown Tegangan Tinggi Akibat Ionisasi." },
  { "en": "Apa Itu Safe Operating Area (SOA)?", "id": "Batas Tegangan, Arus Aman Operasi MOSFET." },
  { "en": "Apa Kepanjangan SOA (Safe Operating Area)?", "id": "Area Operasi Aman." },
  { "en": "Apa Itu Driver Gerbang (Gate Driver)?", "id": "Rangkaian Penggerak Gerbang MOSFET Cepat." },
  { "en": "Mengapa Perlu Driver Gerbang (Gate Driver)?", "id": "Atasi Kapasitansi Gate Besar, Minimalkan Rugi." },
  { "en": "Apa Itu Penggerak Sisi Rendah (Low-Side Driver)?", "id": "Driver Source MOSFET Terhubung Ground." },
  { "en": "Apa Itu Penggerak Sisi Tinggi (High-Side Driver)?", "id": "Driver Source MOSFET Tidak Terhubung Ground." },
  { "en": "Masalah Penggerak Sisi Tinggi (High-Side Driver)?", "id": "Membutuhkan Tegangan Gate Di Atas Vdd (Bootstrap)." },
  { "en": "Apa Itu Rangkaian Bootstrap (Bootstrap Circuit)?", "id": "Menghasilkan Tegangan Driver Sisi Tinggi." },
  { "en": "Apa Itu Half-Bridge (Setengah Jembatan)?", "id": "Dua Saklar Seri (Sisi Tinggi, Rendah)." },
  { "en": "Apa Itu Full-Bridge (Jembatan Penuh) (H-Bridge)?", "id": "Empat Saklar Kontrol Beban Dua Arah." },
  { "en": "Apa Aplikasi H-Bridge (Jembatan Penuh)?", "id": "Kontrol Motor DC, Inverter." },
  { "en": "Apa Itu Dead Time (Waktu Mati) Bridge?", "id": "Tunda Waktu Antara Matikan Satu, Hidupkan Lain." },
  { "en": "Mengapa Perlu Dead Time (Waktu Mati)?", "id": "Mencegah Shoot-Through (Hubung Singkat)." },
  { "en": "Apa Itu Shoot-Through (Tembus Tembak)?", "id": "Kedua Saklar Seri Konduksi Bersamaan." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Kombinasi MOSFET (Input), BJT (Output)." },
  { "en": "Apa Keunggulan IGBT (Insulated Gate Bipolar Transistor)?", "id": "Kapasitas Arus Tinggi, Drive Mudah (Seperti MOSFET)." },
  { "en": "Apa Kerugian IGBT (Insulated Gate Bipolar Transistor)?", "id": "Lebih Lambat Dari MOSFET, Tail Current." },
  { "en": "Apa Itu Tail Current (Arus Ekor) IGBT?", "id": "Arus Lambat Hilang Saat Turn-Off." },
  { "en": "Dimana IGBT (Insulated Gate Bipolar Transistor) Umum Digunakan?", "id": "Aplikasi Daya Tinggi, Switching Menengah." },
  { "en": "Apa Itu Thyristor (Thyristor) / SCR?", "id": "Saklar Semikonduktor Dapat Dikunci (Latched)." },
  { "en": "Bagaimana Menghidupkan (Turn On) SCR?", "id": "Beri Pulsa Positif Ke Gate." },
  { "en": "Bagaimana Mematikan (Turn Off) SCR?", "id": "Kurangi Arus Anoda Dibawah Arus Tahan." },
  { "en": "Apa Itu Arus Latching (Latching Current)?", "id": "Arus Minimum Agar SCR Tetap On." },
  { "en": "Apa Itu Arus Holding (Holding Current)?", "id": "Arus Minimum Jaga SCR Tetap On." },
  { "en": "Apa Itu TRIAC (Triode for Alternating Current)?", "id": "Thyristor (Thyristor) Dua Arah Kontrol AC." },
  { "en": "Apa Itu DIAC (Diode for Alternating Current)?", "id": "Saklar Pemicu Dua Arah (Picu TRIAC)." },
  { "en": "Apa Itu Gate Turn-Off (GTO) Thyristor?", "id": "Thyristor (Thyristor) Bisa Dimatikan Sinyal Gate." },
  { "en": "Apa Itu Integrated Gate-Commutated Thyristor (IGCT)?", "id": "Perangkat Daya Tinggi Kombinasi GTO, Driver." },
  { "en": "Apa Itu Penyearah Terkendali (Controlled Rectifier)?", "id": "Penyearah Tegangan DC Dapat Diatur (SCR)." },
  { "en": "Apa Itu Sudut Penyulutan (Firing Angle)?", "id": "Sudut Fasa AC Saat SCR Dipicu." },
  { "en": "Apa Itu Cycloconverter (Siklokonverter)?", "id": "Konverter AC Ke AC Frekuensi Berbeda." },
  { "en": "Apa Itu Matrix Converter (Konverter Matriks)?", "id": "Konverter AC-AC Langsung (Tanpa DC Link)." },
  { "en": "Apa Itu DC Link (Tautan DC)?", "id": "Tahap Tegangan DC Perantara Konverter." },
  { "en": "Apa Fungsi Kapasitor DC Link?", "id": "Menyimpan Energi, Menstabilkan Tegangan DC." },
  { "en": "Apa Itu Resonant Converter (Konverter Resonan)?", "id": "Konverter Gunakan Rangkaian Resonansi LC." },
  { "en": "Keuntungan Resonant Converter (Konverter Resonan)?", "id": "Zero Voltage/Current Switching (ZVS/ZCS)." },
  { "en": "Apa Itu Zero Voltage Switching (ZVS)?", "id": "Saklar Hidup/Mati Saat Tegangan Nol." },
  { "en": "Apa Itu Zero Current Switching (ZCS)?", "id": "Saklar Hidup/Mati Saat Arus Nol." },
  { "en": "Manfaat ZVS (Zero Voltage Switching) / ZCS (Zero Current Switching)?", "id": "Mengurangi Rugi Switching, EMI." },
  { "en": "Apa Itu Nilai Puncak (Peak Value) Gelombang AC?", "id": "Amplitudo Maksimum Gelombang Dari Nol." },
  { "en": "Apa Itu Nilai Puncak Ke Puncak (Peak-To-Peak)?", "id": "Selisih Antara Puncak Positif, Negatif." },
  { "en": "Apa Hubungan RMS (Root Mean Square), Puncak Sinus?", "id": "RMS Sama Dengan Puncak Dibagi Akar Dua." },
  { "en": "Apa Itu Faktor Crest (Crest Factor)?", "id": "Rasio Nilai Puncak Terhadap Nilai RMS." },
  { "en": "Berapa Faktor Crest (Crest Factor) Gelombang Sinus?", "id": "Akar Dua (Sekitar 1.414)." },
  { "en": "Apa Itu Faktor Bentuk (Form Factor)?", "id": "Rasio Nilai RMS Terhadap Nilai Rata-Rata Absolut." },
  { "en": "Berapa Faktor Bentuk (Form Factor) Gelombang Sinus?", "id": "Pi Dibagi Dua Akar Dua (≈1.11)." },
  { "en": "Apa Itu Analisis Fourier (Fourier Analysis)?", "id": "Menguraikan Sinyal Periodik Menjadi Harmonisa." },
  { "en": "Apa Itu Harmonisa (Harmonic)?", "id": "Komponen Frekuensi Kelipatan Bulat Fundamental." },
  { "en": "Gelombang Kotak (Square Wave) Mengandung Harmonisa Apa?", "id": "Hanya Mengandung Harmonisa Ganjil." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Ukuran Distorsi Akibat Harmonisa." },
  { "en": "Apa Kepanjangan THD (Total Harmonic Distortion)?", "id": "Total Distorsi Harmonik." },
  { "en": "Apa Itu Sistem Linear Time-Invariant (LTI)?", "id": "Sistem Linear Tidak Berubah Waktu." },
  { "en": "Apa Itu Respon Impuls (Impulse Response) Sistem LTI?", "id": "Output Sistem Terhadap Input Impuls Unit." },
  { "en": "Bagaimana Menentukan Output Sistem LTI?", "id": "Konvolusi Input Dengan Respon Impuls." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematis Integral/Penjumlahan Bergeser." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function) H(s)?", "id": "Transformasi Laplace Dari Respon Impuls." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response) H(jω)?", "id": "Perilaku Sistem Terhadap Input Sinusoidal." },
  { "en": "Bagaimana Dapat Respon Frekuensi Dari H(s)?", "id": "Ganti Variabel s Dengan jω." },
  { "en": "Apa Itu Plot Bode (Bode Plot)?", "id": "Grafik Logaritmik Magnitudo, Fasa Vs Frekuensi." },
  { "en": "Apa Itu Pole (Kutub) Fungsi Transfer?", "id": "Frekuensi Kompleks Penguatan Tak Terhingga." },
  { "en": "Apa Itu Zero (Nol) Fungsi Transfer?", "id": "Frekuensi Kompleks Penguatan Nol." },
  { "en": "Kondisi Kestabilan (Stability) Berdasarkan Pole?", "id": "Semua Pole Di Setengah Kiri Bidang-s." },
  { "en": "Apa Itu Filter Pasif (Passive Filter)?", "id": "Filter Menggunakan Komponen R, L, C." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Menggunakan Komponen Aktif (Op-Amp)." },
  { "en": "Apa Keuntungan Filter Aktif (Active Filter)?", "id": "Penguatan, Tidak Perlu Induktor Besar." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency)?", "id": "Frekuensi Batas Filter (-3dB)." },
  { "en": "Apa Itu Orde (Order) Filter?", "id": "Menentukan Kecuraman Slope (Roll-Off)." },
  { "en": "Apa Itu Respons Butterworth (Butterworth Response)?", "id": "Respons Paling Datar Di Passband." },
  { "en": "Apa Itu Respons Chebyshev (Chebyshev Response)?", "id": "Slope Lebih Curam, Ada Riak Passband." },
  { "en": "Apa Itu Transformasi Bintang-Delta (Y-Δ)?", "id": "Konversi Ekuivalen Antara Konfigurasi Rangkaian." },
  { "en": "Apa Itu Dioda Ideal (Ideal Diode)?", "id": "Saklar Sempurna Satu Arah." },
  { "en": "Apa Itu Tegangan Maju (Forward Voltage) Dioda?", "id": "Jatuh Tegangan Saat Dioda Konduksi." },
  { "en": "Apa Itu Daerah Breakdown (Breakdown Region) Dioda?", "id": "Daerah Tegangan Mundur Arus Besar." },
  { "en": "Apa Itu Model Potongan Linear (Piecewise Linear)?", "id": "Aproksimasi Karakteristik Dioda Garis Lurus." },
  { "en": "Apa Itu Kapasitansi Difusi (Diffusion Capacitance)?", "id": "Kapasitansi Dioda Saat Bias Maju." },
  { "en": "Apa Itu Kapasitansi Transisi (Transition Capacitance)?", "id": "Kapasitansi Dioda Saat Bias Mundur." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Perangkat Tiga Lapis Terkontrol Arus." },
  { "en": "Apa Mode Operasi Utama BJT?", "id": "Cutoff, Aktif, Dan Saturasi." },
  { "en": "Apa Itu Penguatan Beta (β) BJT?", "id": "Rasio Arus Kolektor Terhadap Basis." },
  { "en": "Apa Itu Garis Beban (Load Line) AC?", "id": "Garis Operasi Penguat Sinyal AC." },
  { "en": "Apa Itu Efek Early (Early Effect)?", "id": "Modulasi Lebar Basis Efektif BJT." },
  { "en": "Apa Itu FET (Field Effect Transistor)?", "id": "Perangkat Tiga Terminal Terkontrol Tegangan." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET Dengan Gerbang Terisolasi Oksida." },
  { "en": "Apa Itu Tegangan Threshold (Vt) MOSFET?", "id": "Tegangan Gate Minimum Aktifkan Saluran." },
  { "en": "Apa Itu CMOS (Complementary MOS)?", "id": "Logika Kombinasi NMOS Dan PMOS." },
  { "en": "Apa Keunggulan Utama CMOS?", "id": "Disipasi Daya Statis Sangat Rendah." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Karakteristik Op-Amp Ideal?", "id": "Gain Infinite, Input Z Infinite, Output Z Nol." },
  { "en": "Apa Itu Virtual Short (Hubung Singkat Virtual)?", "id": "Input Inverting, Non-Inverting Sama (Feedback Negatif)." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Output Beda Fasa 180 Derajat." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Buffer Dengan Gain Satu." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Sinyal Mode Sama." },
  { "en": "Apa Itu Slew Rate (Laju Perubahan)?", "id": "Kecepatan Maksimum Perubahan Output Op-Amp." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Penghasil Sinyal Periodik Tanpa Input." },
  { "en": "Apa Syarat Osilasi Barkhausen (Barkhausen Criterion)?", "id": "Gain Loop Satu, Fasa Loop Nol." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Elemen Dasar Komputasi Digital." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Aturan Manipulasi Ekspresi Logika." },
  { "en": "Apa Itu Peta Karnaugh (Karnaugh Map)?", "id": "Metode Grafis Penyederhanaan Logika." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Bistabil." },
  { "en": "Apa Beda Latch Dan Flip-Flop?", "id": "Latch (Level), Flip-Flop (Edge Triggered)." },
  { "en": "Apa Itu Register (Register)?", "id": "Penyimpanan Sementara Data Digital." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Menghitung Jumlah Kejadian Atau Pulsa." },
  { "en": "Apa Itu Memori (Memory)?", "id": "Penyimpanan Informasi Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Volatil Baca Tulis Cepat." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Non-Volatil Hanya Baca." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Konversi Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Konversi Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "Unit Pemroses Pusat (CPU) Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Bus (Bus) Sistem?", "id": "Jalur Komunikasi Antar Komponen." },
  { "en": "Apa Itu Komunikasi Serial (Serial Communication)?", "id": "Transfer Data Satu Bit Sekaligus." },
  { "en": "Apa Itu Komunikasi Paralel (Parallel Communication)?", "id": "Transfer Data Banyak Bit Bersamaan." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Detektor Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Penggerak Berdasarkan Sinyal Kontrol." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Tegangan AC." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Pengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Pengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Pembangkitan Hingga Penggunaan Listrik." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Sistem Terhadap Gangguan." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Kontrol Daya Listrik Semikonduktor." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Modulasi Lebar Pulsa Kontrol Daya." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Pengaturan Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Informasi Output Kembali Ke Input." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Otomatisasi Industri." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komunikasi." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Global." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Kuantifikasi Besaran Fisik." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Pengukuran Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Reproduktifitas Pengukuran." },
  { "en": "Apa Itu Elektromagnetisme (Electromagnetism)?", "id": "Studi Interaksi Listrik Magnet." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Energi Medan Elektromagnetik." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Radiasi/Terima Gelombang EM." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Penumpangan Informasi Ke Pembawa." },
  { "en": "Apa Itu Noise (Derau) Termal?", "id": "Noise Akibat Agitasi Termal Elektron." },
  { "en": "Apa Nama Lain Noise Termal?", "id": "Noise Johnson Atau Noise Nyquist." },
  { "en": "Bagaimana Daya Noise Termal Bergantung Suhu?", "id": "Proporsional Terhadap Suhu Absolut." },
  { "en": "Bagaimana Daya Noise Termal Bergantung Bandwidth?", "id": "Proporsional Terhadap Bandwidth." },
  { "en": "Apa Rumus Daya Noise Termal?", "id": "P_n Sama Dengan K T B." },
  { "en": "Apa Itu Konstanta Boltzmann (k)?", "id": "Konstanta Fisika Terkait Energi Suhu." },
  { "en": "Apa Itu Noise Figure (Angka Derau) (NF)?", "id": "Ukuran Degradasi SNR Oleh Perangkat." },
  { "en": "NF (Noise Figure) Ideal Sama Dengan Berapa?", "id": "Nol Desibel (0 dB) Atau Satu." },
  { "en": "Apa Itu Noise Temperature (Suhu Derau)?", "id": "Ukuran Alternatif Derau Perangkat." },
  { "en": "Apa Hubungan Noise Figure, Noise Temperature?", "id": "NF = 1 + (Te / T₀)." },
  { "en": "Apa Itu T₀ (T Nol)?", "id": "Suhu Referensi Standar (290 K)." },
  { "en": "Apa Itu Friis Formula (Rumus Friis)?", "id": "Menghitung Noise Figure Sistem Kaskade." },
  { "en": "Tahap Mana Paling Pengaruhi Noise Figure Kaskade?", "id": "Tahap Pertama (Penguatan Awal)." },
  { "en": "Apa Itu Link Budget (Anggaran Tautan)?", "id": "Perhitungan Daya, Rugi, Gain Sistem Komunikasi." },
  { "en": "Tujuan Link Budget (Anggaran Tautan)?", "id": "Memastikan Sinyal Diterima Cukup Kuat." },
  { "en": "Parameter Apa Dalam Link Budget (Anggaran Tautan)?", "id": "Daya Pancar, Gain Antena, Rugi Jalur." },
  { "en": "Apa Itu EIRP (Effective Isotropic Radiated Power)?", "id": "Daya Pancar Relatif Antena Isotropik." },
  { "en": "Apa Itu Redaman Ruang Bebas (Free Space Path Loss)?", "id": "Pelemahan Sinyal Akibat Jarak Propagasi." },
  { "en": "Apa Itu Noise Figure (Angka Derau) Sistem?", "id": "Ukuran Total Derau Ditambahkan Sistem." },
  { "en": "Apa Itu G/T (Gain-to-Noise Temperature) Penerima?", "id": "Ukuran Kinerja Sensitivitas Penerima Satelit." },
  { "en": "Apa Itu Carrier-to-Noise Ratio (C/N)?", "id": "Rasio Daya Pembawa Terhadap Daya Derau." },
  { "en": "Apa Kepanjangan C/N (Carrier-to-Noise Ratio)?", "id": "Rasio Pembawa Terhadap Derau." },
  { "en": "Apa Itu Energy Per Bit to Noise Density Ratio (Eb/N₀)?", "id": "Ukuran Kinerja Sistem Komunikasi Digital." },
  { "en": "Hubungan Eb/N₀ (Energy Per Bit to Noise Density Ratio) Dan C/N?", "id": "Eb/N₀ = (C/N) * (Bandwidth / Laju Bit)." },
  { "en": "Apa Itu Implementasi Loss (Rugi Implementasi)?", "id": "Degradasi Kinerja Nyata Dari Teoretis." },
  { "en": "Apa Itu Link Margin (Margin Tautan)?", "id": "Kelebihan Daya Diterima Di Atas Threshold." },
  { "en": "Apa Fungsi Link Margin (Margin Tautan)?", "id": "Kompensasi Fading, Variasi Lainnya." },
  { "en": "Apa Itu Noise (Derau) Aditif Putih Gaussian (AWGN)?", "id": "Model Derau Paling Umum Komunikasi." },
  { "en": "Apa Kepanjangan AWGN (Additive White Gaussian Noise)?", "id": "Derau Gaussian Putih Aditif." },
  { "en": "Mengapa Disebut 'Putih' (White)?", "id": "Spektrum Daya Datar Semua Frekuensi." },
  { "en": "Mengapa Disebut 'Gaussian'?", "id": "Distribusi Amplitudo Mengikuti Gaussian." },
  { "en": "Apa Itu Deteksi Koheren (Coherent Detection)?", "id": "Deteksi Memanfaatkan Informasi Fasa Pembawa." },
  { "en": "Apa Itu Deteksi Non-Koheren (Non-Coherent Detection)?", "id": "Deteksi Tanpa Perlu Informasi Fasa Pembawa." },
  { "en": "Modulasi Apa Membutuhkan Deteksi Koheren?", "id": "PSK (Phase Shift Keying), QAM (Quadrature Amplitude Modulation)." },
  { "en": "Modulasi Apa Bisa Deteksi Non-Koheren?", "id": "ASK (Amplitude Shift Keying), FSK (Frequency Shift Keying)." },
  { "en": "Apa Keuntungan Deteksi Non-Koheren?", "id": "Implementasi Penerima Lebih Sederhana." },
  { "en": "Apa Kerugian Deteksi Non-Koheren?", "id": "Kinerja BER (Bit Error Rate) Lebih Buruk Dari Koheren." },
  { "en": "Apa Itu Carrier Recovery (Pemulihan Pembawa)?", "id": "Estimasi Frekuensi, Fasa Pembawa Penerima." },
  { "en": "Mengapa Carrier Recovery (Pemulihan Pembawa) Penting Koheren?", "id": "Perlu Referensi Fasa Akurat Dekode." },
  { "en": "Metode Carrier Recovery (Pemulihan Pembawa)?", "id": "Costas Loop, Squaring Loop." },
  { "en": "Apa Itu Timing Recovery (Pemulihan Pewaktuan)?", "id": "Estimasi Waktu Sampling Optimal Penerima." },
  { "en": "Mengapa Timing Recovery (Pemulihan Pewaktuan) Penting?", "id": "Menghindari Inter-Symbol Interference (ISI)." },
  { "en": "Metode Timing Recovery (Pemulihan Pewaktuan)?", "id": "Early-Late Gate, Gardner Algorithm." },
  { "en": "Apa Itu Jitter (Jitter) Pewaktuan?", "id": "Variasi Waktu Tepi Sinyal Clock/Data." },
  { "en": "Apa Itu Pulse Shaping (Pembentukan Pulsa)?", "id": "Memfilter Pulsa Data Batasi Bandwidth." },
  { "en": "Tujuan Pulse Shaping (Pembentukan Pulsa)?", "id": "Mengurangi Inter-Symbol Interference (ISI)." },
  { "en": "Contoh Filter Pulse Shaping (Pembentukan Pulsa)?", "id": "Raised Cosine, Root Raised Cosine." },
  { "en": "Apa Itu Kriteria Nyquist (Nyquist Criterion) Untuk ISI Nol?", "id": "Syarat Respon Impuls Agar Tak Ada ISI." },
  { "en": "Apa Itu Filter Root Raised Cosine (RRC)?", "id": "Filter Dibagi Antara Pemancar, Penerima." },
  { "en": "Mengapa Gunakan Filter RRC (Root Raised Cosine)?", "id": "Respon Keseluruhan Raised Cosine (ISI Nol)." },
  { "en": "Apa Itu Scrambling (Pengacakan) Data?", "id": "Mengacak Urutan Bit Data Transmisi." },
  { "en": "Tujuan Scrambling (Pengacakan) Data?", "id": "Memastikan Transisi Bit Cukup (Timing), Hilangkan Pola DC." },
  { "en": "Apa Itu Pengkodean Saluran (Line Coding)?", "id": "Representasi Bit Digital Bentuk Gelombang Baseband." },
  { "en": "Contoh Line Coding (Pengkodean Saluran)?", "id": "NRZ (Non-Return-to-Zero), RZ (Return-to-Zero), Manchester, AMI (Alternate Mark Inversion)." },
  { "en": "Apa Itu Kode Manchester (Manchester Code)?", "id": "Transisi Tengah Bit (Sinkronisasi Sendiri)." },
  { "en": "Apa Itu Komponen DC (Direct Current) Line Code?", "id": "Kandungan Arus Searah Rata-Rata Sinyal." },
  { "en": "Mengapa Komponen DC (Direct Current) Perlu Dihindari?", "id": "Beberapa Sistem Tak Bisa Lewatkan DC." },
  { "en": "Apa Itu Kode Blok (Block Code)?", "id": "Mengkodekan k Bit Jadi n Bit (n>k)." },
  { "en": "Contoh Kode Blok (Block Code) Line Coding?", "id": "Kode 4B/5B, 8B/10B." },
  { "en": "Tujuan Kode 4B/5B Atau 8B/10B?", "id": "Menjamin Transisi Cukup, Keseimbangan DC." },
  { "en": "Apa Itu Teori Deteksi (Detection Theory)?", "id": "Membuat Keputusan Optimal Adanya Sinyal Noise." },
  { "en": "Apa Itu Detektor Matched Filter (Matched Filter)?", "id": "Filter Optimal Maksimalkan SNR Output." },
  { "en": "Apa Itu Detektor Korelasi (Correlation Detector)?", "id": "Mengalikan Sinyal Diterima Referensi Lokal." },
  { "en": "Apa Itu Probabilitas Kesalahan (Error Probability)?", "id": "Probabilitas Membuat Keputusan Salah." },
  { "en": "Apa Itu Fungsi Q (Q-Function)?", "id": "Terkait Ekor Distribusi Gaussian Normal." },
  { "en": "Apa Hubungan BER (Bit Error Rate) Dengan Eb/N₀?", "id": "BER (Bit Error Rate) Turun Eksponensial Kenaikan Eb/N₀." },
  { "en": "Apa Itu Fading (Redaman)?", "id": "Variasi Kekuatan Sinyal Akibat Kanal." },
  { "en": "Apa Itu Fading Skala Besar (Large Scale Fading)?", "id": "Redaman Akibat Jarak, Halangan (Shadowing)." },
  { "en": "Apa Itu Fading Skala Kecil (Small Scale Fading)?", "id": "Fluktuasi Cepat Akibat Multipath." },
  { "en": "Apa Itu Model Fading Rayleigh (Rayleigh Fading)?", "id": "Model Fading Tanpa Komponen Line-of-Sight (LOS)." },
  { "en": "Apa Itu Model Fading Rician (Rician Fading)?", "id": "Model Fading Dengan Komponen Line-of-Sight (LOS)." },
  { "en": "Apa Itu Line-of-Sight (LOS)?", "id": "Jalur Langsung Antara Pemancar, Penerima." },
  { "en": "Apa Itu Waktu Tunda Sebar (Delay Spread)?", "id": "Ukuran Sebaran Waktu Tiba Multipath." },
  { "en": "Apa Itu Doppler Spread (Sebaran Doppler)?", "id": "Ukuran Sebaran Pergeseran Frekuensi Doppler." },
  { "en": "Apa Itu Kanal Time-Varying (Berubah Waktu)?", "id": "Karakteristik Kanal Berubah Seiring Waktu." },
  { "en": "Apa Itu Estimasi Kanal (Channel Estimation)?", "id": "Memperkirakan Respon Impuls Kanal." },
  { "en": "Apa Itu Keanekaragaman (Diversity)?", "id": "Teknik Mitigasi Efek Fading." },
  { "en": "Apa Itu MIMO (Multiple Input Multiple Output)?", "id": "Penggunaan Multi Antena Di Kedua Sisi." },
  { "en": "Apa Itu Beamforming (Pembentukan Berkas)?", "id": "Mengarahkan Energi Antena Array." },
  { "en": "Apa Itu Pengkodean Kanal (Channel Coding)?", "id": "Menambah Redundansi Koreksi Error." },
  { "en": "Apa Itu Interleaving (Interleaving)?", "id": "Mengacak Urutan Bit Atasi Burst Error." },
  { "en": "Apa Itu OFDM (Orthogonal Frequency Division Multiplexing)?", "id": "Teknik Modulasi Tahan Multipath." },
  { "en": "Apa Itu CDMA (Code Division Multiple Access)?", "id": "Pengguna Berbagi Spektrum Kode Unik." },
  { "en": "Apa Itu Penguat Derau Rendah (LNA)?", "id": "Penguat Awal Penerima Noise Figure Rendah." },
  { "en": "Apa Itu Mixer (Mixer)?", "id": "Mengubah Frekuensi Sinyal." },
  { "en": "Apa Itu Osilator Lokal (Local Oscillator) (LO)?", "id": "Sumber Frekuensi Referensi Mixer." },
  { "en": "Apa Itu Filter Frekuensi Menengah (IF Filter)?", "id": "Filter Selektif Setelah Mixer." },
  { "en": "Apa Itu Penguat Frekuensi Menengah (IF Amplifier)?", "id": "Penguat Sinyal Frekuensi IF." },
  { "en": "Apa Itu Detektor (Detector) / Demodulator?", "id": "Ekstraksi Sinyal Baseband Dari IF/RF." },
  { "en": "Apa Itu Automatic Gain Control (AGC)?", "id": "Mengatur Penguatan Penerima Otomatis." },
  { "en": "Apa Kepanjangan AGC (Automatic Gain Control)?", "id": "Kontrol Penguatan Otomatis." },
  { "en": "Apa Fungsi AGC (Automatic Gain Control)?", "id": "Menjaga Level Sinyal Output Konstan." },
  { "en": "Apa Itu Phase Noise (Derau Fasa) Osilator?", "id": "Fluktuasi Fasa Acak Sinyal Osilator." },
  { "en": "Apa Efek Phase Noise (Derau Fasa) Komunikasi?", "id": "Inter-Carrier Interference (ICI), Error Konstelasi." },
  { "en": "Apa Itu Intermodulasi (Intermodulation Distortion) (IMD)?", "id": "Produk Frekuensi Tak Diinginkan Non-Linearitas." },
  { "en": "Apa Kepanjangan IMD (Intermodulation Distortion)?", "id": "Distorsi Intermodulasi." },
  { "en": "Apa Itu Titik Intersep Orde Ketiga (IP3)?", "id": "Ukuran Linearitas Orde Ketiga Perangkat." },
  { "en": "IP3 (Third-Order Intercept Point) Tinggi Berarti Apa?", "id": "Perangkat Lebih Linear." },
  { "en": "Apa Itu Titik Kompresi 1-dB (P1dB)?", "id": "Daya Output Saat Gain Turun 1 dB." },
  { "en": "P1dB (Titik Kompresi 1-dB) Tinggi Berarti Apa?", "id": "Perangkat Bisa Tangani Daya Lebih Besar." },
  { "en": "Apa Itu Rentang Dinamis (Dynamic Range)?", "id": "Rasio Sinyal Terbesar Terhadap Terkecil." },
  { "en": "Apa Itu Rentang Dinamis Bebas Spurious (SFDR)?", "id": "Rasio Sinyal Terhadap Komponen Spurious Terbesar." },
  { "en": "Apa Itu Sensitivitas (Sensitivity) Penerima?", "id": "Level Sinyal Minimum Dapat Dideteksi." },
  { "en": "Apa Itu Selektivitas (Selectivity) Penerima?", "id": "Kemampuan Menolak Sinyal Kanal Berdekatan." },
  { "en": "Apa Itu Blocking (Blocking) Penerima?", "id": "Penurunan Kinerja Akibat Sinyal Kuat Dekat." },
  { "en": "Apa Itu Parameter S (Scattering Parameters)?", "id": "Karakterisasi Jaringan RF Input/Output." },
  { "en": "Apa Itu Smith Chart (Bagan Smith)?", "id": "Alat Grafis Analisis Impedansi RF." },
  { "en": "Apa Itu Garis Transmisi Mikrostrip (Microstrip Line)?", "id": "Saluran Transmisi Dicetak Di PCB." },
  { "en": "Apa Itu Garis Transmisi Coplanar Waveguide (CPW)?", "id": "Saluran Transmisi Konduktor Pusat, Ground Samping." },
  { "en": "Apa Itu Balun (Balun)?", "id": "Konverter Antara Sinyal Seimbang, Tak Seimbang." },
  { "en": "Apa Itu Filter RF (Frekuensi Radio)?", "id": "Komponen Melewatkan/Menolak Frekuensi RF." },
  { "en": "Apa Itu Resonator Dielektrik (Dielectric Resonator)?", "id": "Resonator Frekuensi Tinggi Material Keramik." },
  { "en": "Apa Itu Osilator Terkendali Tegangan (VCO)?", "id": "Frekuensi Output Ditentukan Tegangan Input." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sistem Umpan Balik Kunci Fasa Osilator." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Konverter Antara Gelombang Terpandu, Ruang Bebas." },
  { "en": "Apa Itu Kompatibilitas Elektromagnetik (EMC)?", "id": "Kemampuan Perangkat Berfungsi Tanpa Gangguan EM." },
  { "en": "Apa Itu Interferensi Elektromagnetik (EMI)?", "id": "Gangguan Elektromagnetik Merusak Kinerja." },
  { "en": "Apa Itu Sistem Tenaga Listrik (Power System)?", "id": "Jaringan Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Level Tegangan AC." },
  { "en": "Apa Itu Motor Listrik (Electric Motor)?", "id": "Mengubah Energi Listrik Ke Energi Mekanik." },
  { "en": "Apa Itu Generator Listrik (Electric Generator)?", "id": "Mengubah Energi Mekanik Ke Energi Listrik." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Aplikasi Semikonduktor Kontrol Daya Listrik." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Menggunakan Output Sistem Pengaruhi Input." },
  { "en": "Apa Itu Kontroler PID (PID Controller)?", "id": "Kontroler Proporsional-Integral-Derivatif." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Mendapatkan Nilai Kuantitatif Besaran." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Perangkat Deteksi Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Perangkat Hasilkan Aksi Fisik." },
  { "en": "Apa Itu Pemrosesan Sinyal (Signal Processing)?", "id": "Analisis, Modifikasi Sinyal." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Menguraikan Sinyal Menjadi Komponen Frekuensi." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Sistem Memodifikasi Spektrum Frekuensi Sinyal." },
  { "en": "Apa Itu Komunikasi (Communication)?", "id": "Proses Transfer Informasi." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Penumpangan Informasi Ke Sinyal Pembawa." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komunikasi Data." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Global Antar Jaringan." },
  { "en": "Apa Itu Bahan Konduktor (Conductor Material)?", "id": "Bahan Mudah Alirkan Arus Listrik." },
  { "en": "Apa Itu Bahan Isolator (Insulator Material)?", "id": "Bahan Sulit Alirkan Arus Listrik." },
  { "en": "Apa Itu Bahan Semikonduktor (Semiconductor Material)?", "id": "Bahan Sifat Antara Konduktor, Isolator." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Semikonduktor Searah Arus." },
  { "en": "Apa Itu Transistor (Transistor)?", "id": "Komponen Semikonduktor Penguat/Saklar." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit) (IC)?", "id": "Rangkaian Elektronik Dalam Chip." },
  { "en": "Apa Itu Teknik Digital (Digital Engineering)?", "id": "Desain, Analisis Sistem Digital." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Operasi Logika Biner." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Elemen Dasar Sirkuit Logika." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Bistabil Digital." },
  { "en": "Apa Itu Register (Register)?", "id": "Penyimpanan Sementara Sekelompok Bit." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Sirkuit Sekuensial Menghitung Pulsa." },
  { "en": "Apa Itu Memori (Memory)?", "id": "Penyimpanan Data Digital." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "Unit Pemroses Pusat (CPU) Dalam IC." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Sistem Komputer Kecil Dalam IC." },
  { "en": "Apa Itu Bahasa Assembly (Assembly Language)?", "id": "Bahasa Pemrograman Tingkat Rendah." },
  { "en": "Apa Itu Bahasa C (C Language)?", "id": "Bahasa Pemrograman Populer Sistem Tertanam." },
  { "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Sistem Komputer Fungsi Spesifik." },
  { "en": "Apa Itu Real-Time Operating System (RTOS)?", "id": "Sistem Operasi Respon Waktu Deterministik." },
  { "en": "Apa Itu Field Programmable Gate Array (FPGA)?", "id": "IC Logika Dapat Diprogram Ulang." },
  { "en": "Apa Itu Hardware Description Language (HDL)?", "id": "Bahasa Deskripsi Desain Perangkat Keras." },
  { "en": "Contoh Bahasa HDL (Hardware Description Language)?", "id": "VHDL, Verilog." },
  { "en": "Apa Itu Application Specific Integrated Circuit (ASIC)?", "id": "IC Dirancang Khusus Aplikasi Tertentu." },
  { "en": "Apa Beda FPGA Dan ASIC?", "id": "FPGA (Fleksibel), ASIC (Kinerja Optimal)." },
  { "en": "Apa Itu Pemrosesan Sinyal Digital (DSP)?", "id": "Digital Signal Processing (DSP)." },
  { "en": "Apa Aplikasi DSP (Digital Signal Processing)?", "id": "Audio, Video, Komunikasi, Radar." },
  { "en": "Apa Itu Transformasi Fourier Cepat (FFT)?", "id": "Algoritma Efisien Analisis Spektral." },
  { "en": "Apa Itu Filter Digital (Digital Filter)?", "id": "Algoritma Modifikasi Spektrum Sinyal Digital." },
  { "en": "Apa Itu Vektor (Vector)?", "id": "Besaran Punya Nilai Dan Arah." },
  { "en": "Apa Itu Skalar (Scalar)?", "id": "Besaran Hanya Punya Nilai." },
  { "en": "Apa Itu Kalkulus Diferensial (Differential Calculus)?", "id": "Studi Laju Perubahan (Turunan)." },
  { "en": "Apa Itu Kalkulus Integral (Integral Calculus)?", "id": "Studi Akumulasi (Integral)." },
  { "en": "Apa Itu Persamaan Diferensial (Differential Equation)?", "id": "Persamaan Libatkan Fungsi, Turunannya." },
  { "en": "Apa Itu Transformasi Laplace (Laplace Transform)?", "id": "Alat Matematis Analisis Sistem LTI." },
  { "en": "Apa Itu Transformasi Z (Z-Transform)?", "id": "Alat Matematis Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Itu Aljabar Linear (Linear Algebra)?", "id": "Studi Vektor, Matriks, Ruang Linear." },
  { "en": "Apa Itu Matriks (Matrix)?", "id": "Susunan Angka Bentuk Baris, Kolom." },
  { "en": "Apa Itu Determinan (Determinant) Matriks?", "id": "Nilai Skalar Terkait Matriks Persegi." },
  { "en": "Apa Itu Nilai Eigen (Eigenvalue)?", "id": "Skalar Karakteristik Transformasi Linear." },
  { "en": "Apa Itu Vektor Eigen (Eigenvector)?", "id": "Vektor Arahnya Tak Berubah Transformasi." },
  { "en": "Apa Itu Probabilitas (Probability)?", "id": "Ukuran Kemungkinan Terjadinya Peristiwa." },
  { "en": "Apa Itu Variabel Acak (Random Variable)?", "id": "Variabel Nilainya Hasil Proses Acak." },
  { "en": "Apa Itu Distribusi Probabilitas (Probability Distribution)?", "id": "Fungsi Deskripsi Probabilitas Nilai Variabel Acak." },
  { "en": "Apa Itu Distribusi Normal (Normal Distribution)?", "id": "Distribusi Gaussian (Bentuk Lonceng)." },
  { "en": "Apa Itu Nilai Harapan (Expected Value)?", "id": "Nilai Rata-Rata Tertimbang Variabel Acak." },
  { "en": "Apa Itu Varians (Variance)?", "id": "Ukuran Sebaran Data Sekitar Rata-Rata." },
  { "en": "Apa Itu Standar Deviasi (Standard Deviation)?", "id": "Akar Kuadrat Varians." },
  { "en": "Apa Itu Proses Stokastik (Stochastic Process)?", "id": "Kumpulan Variabel Acak Diindeks Waktu." },
  { "en": "Apa Itu Proses Stasioner (Stationary Process)?", "id": "Proses Statistiknya Tak Berubah Waktu." },
  { "en": "Apa Itu Korelasi (Correlation)?", "id": "Ukuran Hubungan Statistik Dua Variabel." },
  { "en": "Apa Itu Autokorelasi (Autocorrelation)?", "id": "Korelasi Sinyal Dengan Versi Tertunda." },
  { "en": "Apa Itu Densitas Spektral Daya (Power Spectral Density)?", "id": "Distribusi Daya Sinyal Berdasarkan Frekuensi." },
  { "en": "Apa Teorema Wiener-Khinchin (Wiener-Khinchin Theorem)?", "id": "Hubungan Autokorelasi, Densitas Spektral Daya." },
  { "en": "Apa Itu Optimasi (Optimization)?", "id": "Mencari Solusi Terbaik Masalah Matematis." },
  { "en": "Apa Itu Fungsi Objektif (Objective Function)?", "id": "Fungsi Yang Diminimalkan/Dimaksimalkan Optimasi." },
  { "en": "Apa Itu Kendala (Constraint)?", "id": "Batasan Harus Dipenuhi Solusi Optimasi." },
  { "en": "Apa Itu Pemrograman Linear (Linear Programming)?", "id": "Optimasi Fungsi Linear Kendala Linear." },
  { "en": "Apa Itu Pemrograman Non-Linear (Non-Linear Programming)?", "id": "Optimasi Fungsi/Kendala Non-Linear." },
  { "en": "Apa Itu Metode Numerik (Numerical Methods)?", "id": "Algoritma Aproksimasi Solusi Matematis." },
  { "en": "Contoh Metode Numerik (Numerical Methods)?", "id": "Metode Newton-Raphson, Runge-Kutta." },
  { "en": "Apa Itu Analisis Kompleks (Complex Analysis)?", "id": "Kalkulus Fungsi Variabel Kompleks." },
  { "en": "Apa Itu Bilangan Kompleks (Complex Number)?", "id": "Bilangan Bentuk a + jb." },
  { "en": "Apa Itu Formula Euler (Euler's Formula)?", "id": "e^(jx) = Cos(x) + J Sin(x)." },
  { "en": "Apa Itu Medan Vektor (Vector Field)?", "id": "Memberi Vektor Setiap Titik Ruang." },
  { "en": "Apa Itu Gradien (Gradient) Skalar?", "id": "Vektor Laju Perubahan Maksimum Skalar." },
  { "en": "Apa Itu Divergensi (Divergence) Vektor?", "id": "Ukuran Sumber/Sink Medan Vektor." },
  { "en": "Apa Itu Curl (Curl) Vektor?", "id": "Ukuran Rotasi Medan Vektor." },
  { "en": "Apa Itu Teorema Divergensi (Divergence Theorem)?", "id": "Hubungan Integral Volume Divergensi, Fluks Permukaan." },
  { "en": "Apa Itu Teorema Stokes (Stokes' Theorem)?", "id": "Hubungan Integral Permukaan Curl, Sirkulasi Lintasan." },
  { "en": "Apa Itu Medan Konservatif (Conservative Field)?", "id": "Curl Nol (Integral Lintasan Tertutup Nol)." },
  { "en": "Apa Itu Potensial Skalar (Scalar Potential)?", "id": "Fungsi Skalar Gradiennya Medan Konservatif." },
  { "en": "Apa Itu Medan Solenoidal (Solenoidal Field)?", "id": "Divergensi Nol (Tak Ada Sumber/Sink)." },
  { "en": "Apa Itu Potensial Vektor (Vector Potential)?", "id": "Fungsi Vektor Curl-nya Medan Solenoidal." },
  { "en": "Apa Itu Operator Laplacian (Laplacian Operator)?", "id": "Divergensi Dari Gradien (∇²)." },
  { "en": "Apa Itu Persamaan Laplace (Laplace's Equation)?", "id": "∇²V = 0 (Tanpa Sumber Muatan)." },
  { "en": "Apa Itu Persamaan Poisson (Poisson's Equation)?", "id": "∇²V = -ρ/ε (Dengan Sumber Muatan)." },
  { "en": "Apa Itu Metode Gambar (Method of Images)?", "id": "Teknik Solusi Masalah Elektrostatis Konduktor." },
  { "en": "Apa Ide Dasar Metode Gambar?", "id": "Mengganti Konduktor Dengan Muatan Gambar." },
  { "en": "Apa Itu Muatan Gambar (Image Charge)?", "id": "Muatan Hipotetis Penuhi Kondisi Batas." },
  { "en": "Apa Itu Metode Variabel Terpisah?", "id": "Teknik Solusi Persamaan Diferensial Parsial." },
  { "en": "Kapan Metode Variabel Terpisah Digunakan?", "id": "Saat Solusi Bisa Dinyatakan Perkalian Fungsi." },
  { "en": "Apa Itu Deret Fourier (Fourier Series)?", "id": "Representasi Fungsi Periodik Jumlah Sinus Cosinus." },
  { "en": "Apa Itu Koefisien Fourier (Fourier Coefficient)?", "id": "Amplitudo Komponen Sinus Cosinus Deret." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Generalisasi Deret Fourier Fungsi Non-Periodik." },
  { "en": "Apa Itu Domain Waktu (Time Domain)?", "id": "Representasi Sinyal Sebagai Fungsi Waktu." },
  { "en": "Apa Itu Domain Frekuensi (Frequency Domain)?", "id": "Representasi Sinyal Sebagai Fungsi Frekuensi." },
  { "en": "Apa Hubungan Transformasi Laplace, Fourier?", "id": "Laplace Generalisasi Fourier Variabel Kompleks." },
  { "en": "Apa Itu Teorema Konvolusi (Convolution Theorem)?", "id": "Konvolusi Waktu = Perkalian Frekuensi." },
  { "en": "Apa Itu Fungsi Delta Dirac (Dirac Delta Function)?", "id": "Fungsi Ideal Impuls Satuan Tak Terhingga." },
  { "en": "Apa Sifat Utama Fungsi Delta Dirac?", "id": "Integral Sama Dengan Satu, Nol Dimana Saja Lain." },
  { "en": "Apa Itu Fungsi Step Heaviside (Heaviside Step Function)?", "id": "Fungsi Bernilai Nol Negatif, Satu Positif." },
  { "en": "Apa Hubungan Fungsi Delta, Step?", "id": "Delta Adalah Turunan Dari Step." },
  { "en": "Apa Itu Fungsi Green (Green's Function)?", "id": "Solusi Persamaan Diferensial Sumber Titik." },
  { "en": "Apa Kegunaan Fungsi Green (Green's Function)?", "id": "Solusi Masalah Non-Homogen Superposisi." },
  { "en": "Apa Itu Elektromagnetisme Komputasi (CEM)?", "id": "Computational Electromagnetics (CEM)." },
  { "en": "Apa Kepanjangan CEM (Computational Electromagnetics)?", "id": "Elektromagnetisme Komputasi." },
  { "en": "Apa Tujuan CEM (Computational Electromagnetics)?", "id": "Solusi Numerik Masalah Elektromagnetik Kompleks." },
  { "en": "Metode Numerik CEM (Computational Electromagnetics) Utama?", "id": "MoM, FEM, FDTD." },
  { "en": "Apa Itu Method of Moments (MoM)?", "id": "Metode Solusi Persamaan Integral (Permukaan)." },
  { "en": "Apa Kepanjangan MoM (Method of Moments)?", "id": "Metode Momen." },
  { "en": "Kapan MoM (Method of Moments) Cocok Digunakan?", "id": "Masalah Radiasi, Hamburan (Antena, RCS)." },
  { "en": "Apa Itu Finite Element Method (FEM)?", "id": "Metode Solusi Persamaan Diferensial (Volume)." },
  { "en": "Apa Kepanjangan FEM (Finite Element Method)?", "id": "Metode Elemen Hingga." },
  { "en": "Kapan FEM (Finite Element Method) Cocok Digunakan?", "id": "Masalah Struktur Kompleks, Material Beragam." },
  { "en": "Apa Itu Finite Difference Time Domain (FDTD)?", "id": "Metode Solusi Persamaan Maxwell Domain Waktu." },
  { "en": "Apa Kepanjangan FDTD (Finite Difference Time Domain)?", "id": "Domain Waktu Beda Hingga." },
  { "en": "Kapan FDTD (Finite Difference Time Domain) Cocok Digunakan?", "id": "Analisis Transien, Masalah Pita Lebar." },
  { "en": "Apa Itu Diskretisasi (Discretization)?", "id": "Membagi Domain Kontinu Jadi Elemen Kecil." },
  { "en": "Apa Itu Grid (Grid) Atau Mesh (Mesh)?", "id": "Kumpulan Elemen Hasil Diskretisasi." },
  { "en": "Apa Itu Kondisi Batas Serap (Absorbing Boundary Condition)?", "id": "Simulasi Batas Ruang Terbuka FDTD/FEM." },
  { "en": "Contoh Kondisi Batas Serap (Absorbing Boundary Condition)?", "id": "Perfectly Matched Layer (PML)." },
  { "en": "Apa Kepanjangan PML (Perfectly Matched Layer)?", "id": "Lapisan Tercocok Sempurna." },
  { "en": "Apa Itu Perangkat Lunak Simulasi EM?", "id": "Software Implementasi Metode CEM (HFSS, CST)." },
  { "en": "Apa Itu HFSS (High Frequency Structure Simulator)?", "id": "Software Simulasi EM Berbasis FEM." },
  { "en": "Apa Itu CST (Computer Simulation Technology) Microwave Studio?", "id": "Software Simulasi EM Berbasis FDTD/FEM." },
  { "en": "Apa Itu Optika Fourier (Fourier Optics)?", "id": "Analisis Sistem Optik Menggunakan Transformasi Fourier." },
  { "en": "Apa Itu Fungsi Transfer Optik (OTF)?", "id": "Optical Transfer Function (OTF)." },
  { "en": "Apa Kepanjangan OTF (Optical Transfer Function)?", "id": "Fungsi Transfer Optik." },
  { "en": "Apa Fungsi OTF (Optical Transfer Function)?", "id": "Karakterisasi Resolusi, Kontras Sistem Optik." },
  { "en": "Apa Itu Modulation Transfer Function (MTF)?", "id": "Magnitudo Dari OTF (Kontras Vs Frekuensi)." },
  { "en": "Apa Kepanjangan MTF (Modulation Transfer Function)?", "id": "Fungsi Transfer Modulasi." },
  { "en": "Apa Itu Phase Transfer Function (PTF)?", "id": "Fasa Dari OTF (Pergeseran Fasa)." },
  { "en": "Apa Kepanjangan PTF (Phase Transfer Function)?", "id": "Fungsi Transfer Fasa." },
  { "en": "Apa Itu Batas Difraksi (Diffraction Limit)?", "id": "Batas Resolusi Fundamental Sistem Optik." },
  { "en": "Apa Itu Angka F (f-number) Lensa?", "id": "Rasio Jarak Fokus Terhadap Diameter Apertur." },
  { "en": "Angka F (f-number) Kecil Berarti Apertur Bagaimana?", "id": "Apertur Lebih Besar (Lebih Banyak Cahaya)." },
  { "en": "Apa Itu Kedalaman Fokus (Depth of Focus)?", "id": "Rentang Jarak Citra Tetap Fokus." },
  { "en": "Apa Itu Kedalaman Medan (Depth of Field)?", "id": "Rentang Jarak Objek Tampak Fokus Citra." },
  { "en": "Apa Itu Holografi (Holography)?", "id": "Teknik Rekaman Informasi Amplitudo Fasa Cahaya." },
  { "en": "Apa Perbedaan Hologram Dan Foto?", "id": "Hologram Rekam Fasa (Hasilkan Citra 3D)." },
  { "en": "Apa Itu Mikroskopi (Microscopy)?", "id": "Teknik Visualisasi Objek Sangat Kecil." },
  { "en": "Apa Itu Perbesaran (Magnification)?", "id": "Rasio Ukuran Citra Terhadap Objek Asli." },
  { "en": "Apa Itu Resolusi (Resolution) Mikroskop?", "id": "Jarak Minimum Dua Titik Terpisah." },
  { "en": "Apa Itu Apertur Numerik (Numerical Aperture) (NA)?", "id": "Ukuran Kemampuan Lensa Kumpulkan Cahaya." },
  { "en": "NA (Numerical Aperture) Tinggi Berarti Resolusi Bagaimana?", "id": "Resolusi Lebih Baik (Lebih Kecil)." },
  { "en": "Apa Itu Mikroskop Medan Terang (Bright Field)?", "id": "Mikroskop Optik Paling Umum." },
  { "en": "Apa Itu Mikroskop Medan Gelap (Dark Field)?", "id": "Hanya Cahaya Hambur Objek Masuk Lensa." },
  { "en": "Apa Itu Mikroskop Kontras Fasa (Phase Contrast)?", "id": "Visualisasi Sampel Transparan Perbedaan Fasa." },
  { "en": "Apa Itu Mikroskop Kontras Interferensi Diferensial (DIC)?", "id": "Differential Interference Contrast (DIC)." },
  { "en": "Apa Kepanjangan DIC (Differential Interference Contrast)?", "id": "Kontras Interferensi Diferensial." },
  { "en": "Apa Fungsi Mikroskop DIC (Differential Interference Contrast)?", "id": "Citra 3D Semu Sampel Transparan." },
  { "en": "Apa Itu Mikroskop Fluoresensi (Fluorescence)?", "id": "Visualisasi Molekul Fluoresen Sampel." },
  { "en": "Apa Itu Fluorofor (Fluorophore)?", "id": "Molekul Memancarkan Cahaya Fluoresensi." },
  { "en": "Apa Itu Mikroskop Konfokal (Confocal Microscopy)?", "id": "Hasilkan Citra Optik Irisan Tipis (Resolusi Tinggi)." },
  { "en": "Bagaimana Mikroskop Konfokal (Confocal Microscopy) Bekerja?", "id": "Gunakan Lubang Jarum (Pinhole) Blokir Cahaya Luar Fokus." },
  { "en": "Apa Itu Mikroskop Super-Resolusi (Super-Resolution)?", "id": "Teknik Mikroskopi Lampaui Batas Difraksi." },
  { "en": "Contoh Teknik Super-Resolusi (Super-Resolution)?", "id": "STED (Stimulated Emission Depletion), PALM (Photoactivated Localization Microscopy), STORM (Stochastic Optical Reconstruction Microscopy)." },
  { "en": "Apa Itu Mikroskop Elektron (Electron Microscope)?", "id": "Gunakan Berkas Elektron Pencitraan Resolusi Tinggi." },
  { "en": "Mengapa Mikroskop Elektron Resolusi Lebih Tinggi?", "id": "Panjang Gelombang Elektron Jauh Lebih Pendek." },
  { "en": "Apa Itu Mikroskop Elektron Transmisi (TEM)?", "id": "Transmission Electron Microscope (TEM)." },
  { "en": "Apa Fungsi TEM (Transmission Electron Microscope)?", "id": "Pencitraan Resolusi Tinggi Struktur Internal." },
  { "en": "Apa Itu Mikroskop Elektron Pemindai (SEM)?", "id": "Scanning Electron Microscope (SEM)." },
  { "en": "Apa Fungsi SEM (Scanning Electron Microscope)?", "id": "Pencitraan Resolusi Tinggi Topografi Permukaan." },
  { "en": "Apa Itu Mikroskop Gaya Atom (AFM)?", "id": "Atomic Force Microscope (AFM)." },
  { "en": "Apa Kepanjangan AFM (Atomic Force Microscope)?", "id": "Mikroskop Gaya Atom." },
  { "en": "Apa Prinsip Kerja AFM (Atomic Force Microscope)?", "id": "Pindai Permukaan Jarum Sangat Tajam." },
  { "en": "Apa Itu Mikroskop Pemindai Terowongan (STM)?", "id": "Scanning Tunneling Microscope (STM)." },
  { "en": "Apa Prinsip Kerja STM (Scanning Tunneling Microscope)?", "id": "Arus Terowongan Kuantum Jarum, Permukaan." },
  { "en": "Permukaan Apa Bisa Dicitrakan STM?", "id": "Hanya Permukaan Konduktif Listrik." },
  { "en": "Apa Itu Spektroskopi (Spectroscopy)?", "id": "Studi Interaksi Radiasi Elektromagnetik Materi." },
  { "en": "Apa Itu Spektrum (Spectrum)?", "id": "Distribusi Intensitas Radiasi Vs Frekuensi/Lambda." },
  { "en": "Apa Itu Spektroskopi Serapan (Absorption Spectroscopy)?", "id": "Mengukur Penyerapan Radiasi Oleh Materi." },
  { "en": "Apa Itu Spektroskopi Emisi (Emission Spectroscopy)?", "id": "Mengukur Radiasi Dipancarkan Materi Tereksitasi." },
  { "en": "Apa Itu Spektroskopi Fluoresensi (Fluorescence Spectroscopy)?", "id": "Mengukur Emisi Fluoresensi Materi." },
  { "en": "Apa Itu Spektroskopi Raman (Raman Spectroscopy)?", "id": "Mengukur Hamburan Cahaya Inelastis Materi." },
  { "en": "Apa Itu Spektroskopi Inframerah (Infrared Spectroscopy) (IR)?", "id": "Mengukur Serapan Radiasi Inframerah (Vibrasi)." },
  { "en": "Apa Itu Spektroskopi UV-Vis (UV-Vis Spectroscopy)?", "id": "Mengukur Serapan Radiasi UV/Tampak (Elektronik)." },
  { "en": "Apa Itu Spektroskopi Resonansi Magnetik Nuklir (NMR)?", "id": "Nuclear Magnetic Resonance (NMR)." },
  { "en": "Apa Kepanjangan NMR (Nuclear Magnetic Resonance)?", "id": "Resonansi Magnetik Nuklir." },
  { "en": "Apa Fungsi NMR (Nuclear Magnetic Resonance)?", "id": "Penentuan Struktur Molekul Detail." },
  { "en": "Apa Itu Spektroskopi Massa (Mass Spectrometry)?", "id": "Mengukur Rasio Massa Terhadap Muatan Ion." },
  { "en": "Apa Fungsi Spektroskopi Massa (Mass Spectrometry)?", "id": "Identifikasi Komposisi Kimia, Massa Molekul." },
  { "en": "Apa Itu Spektroskopi Serapan Atom (AAS)?", "id": "Mengukur Konsentrasi Unsur Logam." },
  { "en": "Apa Itu Spektroskopi Emisi Atom Plasma (ICP-AES/OES)?", "id": "Analisis Unsur Multi Elemen Presisi Tinggi." },
  { "en": "Apa Itu Spektroskopi Fotoelektron Sinar-X (XPS)?", "id": "X-ray Photoelectron Spectroscopy (XPS)." },
  { "en": "Apa Kepanjangan XPS (X-ray Photoelectron Spectroscopy)?", "id": "Spektroskopi Fotoelektron Sinar-X." },
  { "en": "Apa Fungsi XPS (X-ray Photoelectron Spectroscopy)?", "id": "Analisis Komposisi Kimia Permukaan." },
  { "en": "Apa Itu Difraksi Sinar-X (X-Ray Diffraction) (XRD)?", "id": "Penentuan Struktur Kristal Material." },
  { "en": "Apa Itu Elektrolit (Electrolyte)?", "id": "Zat Konduktif Mengandung Ion Bebas." },
  { "en": "Apa Itu Ion (Ion)?", "id": "Atom Atau Molekul Bermuatan Listrik." },
  { "en": "Apa Itu Anoda (Anode) Elektrokimia?", "id": "Elektroda Tempat Terjadi Oksidasi." },
  { "en": "Apa Itu Katoda (Cathode) Elektrokimia?", "id": "Elektroda Tempat Terjadi Reduksi." },
  { "en": "Apa Itu Elektrolisis (Electrolysis)?", "id": "Penguraian Senyawa Listrik Arus DC." },
  { "en": "Apa Hukum Faraday (Faraday's Law) Pertama Elektrolisis?", "id": "Massa Proporsional Muatan Listrik Total." },
  { "en": "Apa Hukum Faraday (Faraday's Law) Kedua Elektrolisis?", "id": "Massa Proporsional Berat Ekuivalen Kimia." },
  { "en": "Apa Itu Konstanta Faraday (Faraday Constant) (F)?", "id": "Muatan Per Mol Elektron." },
  { "en": "Apa Itu Sel Galvanik (Galvanic Cell)?", "id": "Hasilkan Listrik Reaksi Redoks Spontan." },
  { "en": "Nama Lain Sel Galvanik (Galvanic Cell)?", "id": "Sel Volta (Voltaic Cell)." },
  { "en": "Apa Itu Potensial Elektroda (Electrode Potential)?", "id": "Beda Potensial Elektroda, Elektrolit." },
  { "en": "Apa Itu Potensial Elektroda Standar (Standard Electrode Potential)?", "id": "Potensial Elektroda Kondisi Standar (1M, 1atm)." },
  { "en": "Apa Elektroda Hidrogen Standar (SHE)?", "id": "Elektroda Referensi Potensial Nol Volt." },
  { "en": "Apa Kepanjangan SHE (Standard Hydrogen Electrode)?", "id": "Elektroda Hidrogen Standar." },
  { "en": "Apa Itu Deret Volta (Electrochemical Series)?", "id": "Urutan Unsur Potensial Reduksi Standar." },
  { "en": "Apa Itu Potensial Sel (Cell Potential) (Esel)?", "id": "Beda Potensial Antara Katoda, Anoda." },
  { "en": "Rumus Potensial Sel (Cell Potential) Standar (E°sel)?", "id": "E°katoda Dikurangi E°anoda." },
  { "en": "Reaksi Spontan Jika E°sel Bagaimana?", "id": "E°sel Bernilai Positif." },
  { "en": "Apa Itu Persamaan Nernst (Nernst Equation)?", "id": "Hubungan Potensial Sel Non-Standar Konsentrasi." },
  { "en": "Apa Itu Baterai (Battery)?", "id": "Satu Atau Lebih Sel Galvanik." },
  { "en": "Apa Itu Baterai Primer (Primary Battery)?", "id": "Baterai Sekali Pakai (Non-Rechargeable)." },
  { "en": "Apa Itu Baterai Sekunder (Secondary Battery)?", "id": "Baterai Dapat Diisi Ulang (Rechargeable)." },
  { "en": "Contoh Baterai Primer (Primary Battery)?", "id": "Alkaline, Zinc-Carbon, Lithium Primer." },
  { "en": "Contoh Baterai Sekunder (Secondary Battery)?", "id": "Lead-Acid (Aki), NiCd, NiMH, Li-Ion." },
  { "en": "Apa Itu Baterai Aki (Lead-Acid Battery)?", "id": "Baterai Sekunder Umum Mobil." },
  { "en": "Elektrolit (Electrolyte) Baterai Aki (Lead-Acid)?", "id": "Larutan Asam Sulfat (H₂SO₄)." },
  { "en": "Apa Itu Baterai Nikel-Kadmium (NiCd)?", "id": "Baterai Sekunder (Efek Memori)." },
  { "en": "Apa Itu Baterai Nikel-Metal Hidrida (NiMH)?", "id": "Baterai Sekunder Kapasitas Lebih Tinggi NiCd." },
  { "en": "Apa Itu Baterai Lithium-Ion (Li-Ion)?", "id": "Baterai Sekunder Kepadatan Energi Tinggi." },
  { "en": "Dimana Baterai Li-Ion (Lithium-Ion) Digunakan?", "id": "Ponsel, Laptop, Kendaraan Listrik." },
  { "en": "Apa Keunggulan Baterai Li-Ion (Lithium-Ion)?", "id": "Ringan, Kepadatan Energi Tinggi, Tanpa Efek Memori." },
  { "en": "Apa Kerugian Baterai Li-Ion (Lithium-Ion)?", "id": "Lebih Mahal, Perlu Proteksi Khusus." },
  { "en": "Apa Itu Thermal Runaway (Lolos Termal) Baterai?", "id": "Reaksi Berantai Pemanasan Baterai (Bahaya)." },
  { "en": "Apa Itu Battery Management System (BMS)?", "id": "Sirkuit Elektronik Pengaman Baterai Li-Ion." },
  { "en": "Apa Kepanjangan BMS (Battery Management System)?", "id": "Sistem Manajemen Baterai." },
  { "en": "Fungsi BMS (Battery Management System)?", "id": "Proteksi Overcharge, Overdischarge, Suhu." },
  { "en": "Apa Itu Kapasitas (Capacity) Baterai (Ah)?", "id": "Jumlah Muatan Dapat Disimpan (Ampere-Hour)." },
  { "en": "Apa Itu Energi Spesifik (Specific Energy) Baterai?", "id": "Energi Per Satuan Massa (Wh/kg)." },
  { "en": "Apa Itu Kepadatan Energi (Energy Density) Baterai?", "id": "Energi Per Satuan Volume (Wh/L)." },
  { "en": "Apa Itu Daya Spesifik (Specific Power) Baterai?", "id": "Daya Maksimum Per Satuan Massa (W/kg)." },
  { "en": "Apa Itu C-Rate (C-Rate) Baterai?", "id": "Laju Pengisian/Pengosongan Relatif Kapasitas." },
  { "en": "Apa Arti 1C (1C) Pengisian Baterai 2Ah?", "id": "Arus Pengisian Dua Ampere." },
  { "en": "Apa Itu Depth of Discharge (DoD)?", "id": "Persentase Kapasitas Baterai Yang Digunakan." },
  { "en": "Apa Kepanjangan DoD (Depth of Discharge)?", "id": "Kedalaman Pengosongan." },
  { "en": "Apa Itu State of Charge (SoC)?", "id": "Persentase Sisa Kapasitas Baterai." },
  { "en": "Apa Kepanjangan SoC (State of Charge)?", "id": "Keadaan Muatan." },
  { "en": "Apa Itu Siklus Hidup (Cycle Life) Baterai?", "id": "Jumlah Siklus Isi-Kosong Hingga Degradasi." },
  { "en": "Apa Itu Efek Memori (Memory Effect)?", "id": "Penurunan Kapasitas Tampak Baterai NiCd/NiMH." },
  { "en": "Apa Itu Sel Bahan Bakar (Fuel Cell)?", "id": "Konversi Energi Kimia Bahan Bakar Listrik." },
  { "en": "Bahan Bakar (Fuel) Umum Sel Bahan Bakar?", "id": "Hidrogen (H₂)." },
  { "en": "Apa Jenis Utama Sel Bahan Bakar?", "id": "PEMFC, SOFC, PAFC." },
  { "en": "Apa Kepanjangan PEMFC (Proton Exchange Membrane Fuel Cell)?", "id": "Sel Bahan Bakar Membran Pertukaran Proton." },
  { "en": "Apa Kepanjangan SOFC (Solid Oxide Fuel Cell)?", "id": "Sel Bahan Bakar Oksida Padat." },
  { "en": "Keunggulan Sel Bahan Bakar (Fuel Cell)?", "id": "Efisiensi Tinggi, Emisi Rendah (Air)." },
  { "en": "Apa Itu Superkapasitor (Supercapacitor)?", "id": "Kapasitor Kepadatan Energi Sangat Tinggi." },
  { "en": "Nama Lain Superkapasitor (Supercapacitor)?", "id": "Ultrakapasitor, EDLC." },
  { "en": "Apa Kepanjangan EDLC (Electric Double Layer Capacitor)?", "id": "Kapasitor Lapisan Ganda Elektrik." },
  { "en": "Prinsip Kerja EDLC (Electric Double Layer Capacitor)?", "id": "Penyimpanan Muatan Lapisan Ganda Elektroda." },
  { "en": "Apa Itu Pseudokapasitor (Pseudocapacitor)?", "id": "Superkapasitor Penyimpanan Muatan Reaksi Redoks." },
  { "en": "Keunggulan Superkapasitor (Supercapacitor) Dibanding Baterai?", "id": "Daya Sangat Tinggi, Siklus Hidup Panjang." },
  { "en": "Kerugian Superkapasitor (Supercapacitor) Dibanding Baterai?", "id": "Kepadatan Energi Lebih Rendah." },
  { "en": "Aplikasi Superkapasitor (Supercapacitor)?", "id": "Pengereman Regeneratif, Cadangan Daya Singkat." },
  { "en": "Apa Itu Konversi Energi (Energy Conversion)?", "id": "Mengubah Energi Satu Bentuk Ke Lain." },
  { "en": "Contoh Konverter Energi (Energy Converter)?", "id": "Motor, Generator, Sel Surya, Termokopel." },
  { "en": "Apa Itu Efisiensi (Efficiency) Konversi?", "id": "Rasio Energi Output Berguna, Input Total." },
  { "en": "Apa Sumber Energi Primer (Primary Energy)?", "id": "Sumber Energi Bentuk Alam (Batubara, Surya)." },
  { "en": "Apa Itu Sumber Energi Sekunder (Secondary Energy)?", "id": "Bentuk Energi Hasil Konversi (Listrik)." },
  { "en": "Apa Itu Energi Terbarukan (Renewable Energy)?", "id": "Energi Sumber Alami Tak Terbatas." },
  { "en": "Apa Itu Energi Tak Terbarukan (Non-Renewable Energy)?", "id": "Energi Sumber Terbatas (Bahan Bakar Fosil)." },
  { "en": "Contoh Energi Tak Terbarukan (Non-Renewable Energy)?", "id": "Batubara, Minyak Bumi, Gas Alam, Nuklir (Fisi)." },
  { "en": "Apa Itu Bahan Bakar Fosil (Fossil Fuel)?", "id": "Sumber Energi Sisa Organisme Purba." },
  { "en": "Dampak Pembakaran Bahan Bakar Fosil?", "id": "Polusi Udara, Emisi Gas Rumah Kaca." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Surya (PLTS)?", "id": "Konversi Cahaya Matahari Jadi Listrik." },
  { "en": "Komponen Utama PLTS (Pembangkit Listrik Tenaga Surya)?", "id": "Panel Surya (PV), Inverter, Baterai (Opsional)." },
  { "en": "Apa Itu Sistem PLTS On-Grid (Tersambung Jaringan)?", "id": "Terhubung Jaringan Listrik PLN." },
  { "en": "Apa Itu Sistem PLTS Off-Grid (Mandiri)?", "id": "Tidak Terhubung Jaringan (Perlu Baterai)." },
  { "en": "Apa Itu Sistem PLTS Hibrida (Hybrid)?", "id": "Kombinasi Sumber (Surya, Jaringan, Baterai)." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Angin (PLTB)?", "id": "Konversi Energi Kinetik Angin Jadi Listrik." },
  { "en": "Komponen Utama PLTB (Pembangkit Listrik Tenaga Bayu)?", "id": "Turbin Angin (Bilah, Rotor, Generator)." },
  { "en": "Apa Itu Ladang Angin (Wind Farm)?", "id": "Kumpulan Turbin Angin Satu Lokasi." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Air (PLTA)?", "id": "Konversi Energi Potensial/Kinetik Air Listrik." },
  { "en": "Apa Itu Turbin Air (Hydro Turbine)?", "id": "Ubah Energi Air Jadi Putaran Mekanis." },
  { "en": "Jenis Turbin Air (Hydro Turbine)?", "id": "Pelton, Francis, Kaplan." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Mikrohidro (PLTMH)?", "id": "PLTA Skala Kecil (Sungai Kecil)." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Panas Bumi (PLTP)?", "id": "Memanfaatkan Panas Dari Dalam Bumi." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Biomassa (PLTBm)?", "id": "Pembakaran Bahan Organik Hasilkan Listrik." },
  { "en": "Contoh Bahan Bakar Biomassa (Biomass)?", "id": "Kayu, Sampah Pertanian, Kotoran Ternak." },
  { "en": "Apa Itu Biogas (Biogas)?", "id": "Gas Metana Hasil Penguraian Anaerobik Biomassa." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Laut?", "id": "Memanfaatkan Energi Ombak, Pasang Surut, OTEC." },
  { "en": "Apa Tantangan Energi Terbarukan (Renewable Energy)?", "id": "Intermitensi (Tergantung Cuaca), Biaya Awal." },
  { "en": "Apa Itu Penyimpanan Energi (Energy Storage)?", "id": "Menyimpan Energi Untuk Digunakan Nanti." },
  { "en": "Mengapa Penyimpanan Energi (Energy Storage) Penting Terbarukan?", "id": "Mengatasi Intermitensi, Menstabilkan Jaringan." },
  { "en": "Apa Itu Jaringan Cerdas (Smart Grid)?", "id": "Jaringan Listrik Modern Komunikasi Dua Arah." },
  { "en": "Fitur Smart Grid (Jaringan Cerdas)?", "id": "Meter Cerdas, Otomatisasi, Integrasi Terbarukan." },
  { "en": "Apa Itu Efisiensi Energi (Energy Efficiency)?", "id": "Menggunakan Lebih Sedikit Energi Hasil Sama." },
  { "en": "Apa Itu Konservasi Energi (Energy Conservation)?", "id": "Mengurangi Pemakaian Energi." },
  { "en": "Apa Itu Audit Energi (Energy Audit)?", "id": "Analisis Penggunaan Energi Cari Penghematan." },
  { "en": "Apa Itu Manajemen Energi (Energy Management)?", "id": "Pemantauan, Pengendalian Konsumsi Energi." },
  { "en": "Apa Itu Label Efisiensi Energi (Energy Label)?", "id": "Informasi Tingkat Efisiensi Produk." },
  { "en": "Apa Itu Standar Kinerja Energi Minimum (MEPS)?", "id": "Minimum Energy Performance Standards (MEPS)." },
  { "en": "Apa Kepanjangan MEPS (Minimum Energy Performance Standards)?", "id": "Standar Kinerja Energi Minimum." },
  { "en": "Apa Itu Elektronika Biomedis (Biomedical Electronics)?", "id": "Aplikasi Elektronika Di Bidang Medis." },
  { "en": "Apa Itu Sinyal Biomedis (Biomedical Signal)?", "id": "Sinyal Listrik Dihasilkan Tubuh (ECG, EEG)." },
  { "en": "Apa Itu Elektrokardiogram (ECG/EKG)?", "id": "Perekaman Aktivitas Listrik Jantung." },
  { "en": "Gelombang Apa Saja Dalam Sinyal ECG?", "id": "Gelombang P, Kompleks QRS, Gelombang T." },
  { "en": "Apa Representasi Gelombang P ECG?", "id": "Depolarisasi Atrium Jantung." },
  { "en": "Apa Representasi Kompleks QRS ECG?", "id": "Depolarisasi Ventrikel Jantung." },
  { "en": "Apa Representasi Gelombang T ECG?", "id": "Repolarisasi Ventrikel Jantung." },
  { "en": "Apa Itu Elektroensefalogram (EEG)?", "id": "Perekaman Aktivitas Listrik Otak." },
  { "en": "Apa Gelombang Otak (Brainwave) Utama EEG?", "id": "Delta, Theta, Alfa, Beta, Gama." },
  { "en": "Gelombang Otak (Brainwave) Apa Dominan Saat Tidur Lelap?", "id": "Gelombang Delta (Delta Waves)." },
  { "en": "Gelombang Otak (Brainwave) Apa Dominan Saat Rileks Tenang?", "id": "Gelombang Alfa (Alpha Waves)." },
  { "en": "Gelombang Otak (Brainwave) Apa Dominan Saat Aktif Berpikir?", "id": "Gelombang Beta (Beta Waves)." },
  { "en": "Apa Itu Elektromiogram (EMG)?", "id": "Perekaman Aktivitas Listrik Otot." },
  { "en": "Aplikasi EMG (Electromyogram)?", "id": "Diagnosis Gangguan Saraf Otot, Kontrol Prostetik." },
  { "en": "Apa Itu Elektrookulogram (EOG)?", "id": "Perekaman Gerakan Mata." },
  { "en": "Apa Kepanjangan EOG (Electrooculogram)?", "id": "Elektrookulogram." },
  { "en": "Apa Itu Elektrogastrogram (EGG)?", "id": "Perekaman Aktivitas Listrik Lambung." },
  { "en": "Apa Itu Elektroda (Electrode) Biomedis?", "id": "Konduktor Kontak Tubuh Rekam Sinyal." },
  { "en": "Jenis Elektroda (Electrode) Biomedis?", "id": "Permukaan (Surface), Jarum (Needle), Mikro." },
  { "en": "Material Umum Elektroda (Electrode) Permukaan?", "id": "Perak/Perak Klorida (Ag/AgCl)." },
  { "en": "Mengapa Perlu Gel (Gel) Elektroda?", "id": "Mengurangi Impedansi Kontak Kulit Elektroda." },
  { "en": "Apa Itu Impedansi (Impedance) Elektroda-Kulit?", "id": "Hambatan Kompleks Antara Elektroda, Kulit." },
  { "en": "Apa Itu Penguat Biopotensial (Biopotential Amplifier)?", "id": "Penguat Khusus Sinyal Biomedis." },
  { "en": "Karakteristik Penguat Biopotensial Ideal?", "id": "CMRR Tinggi, Input Z Tinggi, Noise Rendah." },
  { "en": "Mengapa CMRR (Common Mode Rejection Ratio) Tinggi Penting?", "id": "Menolak Interferensi Mode Bersama (60Hz)." },
  { "en": "Apa Itu Penguat Instrumentasi (Instrumentation Amplifier)?", "id": "Penguat Diferensial Cocok Sinyal Biomedis." },
  { "en": "Apa Itu Driven Right Leg (DRL) Circuit?", "id": "Rangkaian Mengurangi Noise Mode Bersama ECG." },
  { "en": "Apa Kepanjangan DRL (Driven Right Leg)?", "id": "Kaki Kanan Digerakkan." },
  { "en": "Apa Itu Isolasi (Isolation) Medis?", "id": "Pemisahan Elektrikal Pasien Dari Peralatan." },
  { "en": "Mengapa Isolasi (Isolation) Medis Penting?", "id": "Mencegah Risiko Sengatan Listrik Pasien." },
  { "en": "Apa Itu Arus Bocor (Leakage Current) Medis?", "id": "Arus Kecil Mengalir Lewat Pasien." },
  { "en": "Standar Apa Mengatur Keamanan Peralatan Medis?", "id": "Standar IEC 60601." },
  { "en": "Apa Itu Defibrilator (Defibrillator)?", "id": "Alat Kejut Listrik Atasi Fibrilasi Jantung." },
  { "en": "Apa Itu Fibrilasi Ventrikel (Ventricular Fibrillation)?", "id": "Kontraksi Jantung Kacau Tak Efektif." },
  { "en": "Apa Itu Alat Pacu Jantung (Pacemaker)?", "id": "Perangkat Implant Stimulasi Ritme Jantung." },
  { "en": "Apa Itu Electrosurgical Unit (ESU)?", "id": "Alat Bedah Listrik (Potong, Koagulasi)." },
  { "en": "Apa Kepanjangan ESU (Electrosurgical Unit)?", "id": "Unit Bedah Elektro." },
  { "en": "Frekuensi Apa Digunakan ESU (Electrosurgical Unit)?", "id": "Frekuensi Radio Tinggi (Cegah Stimulasi Saraf)." },
  { "en": "Apa Itu Pencitraan Medis (Medical Imaging)?", "id": "Teknik Visualisasi Struktur Internal Tubuh." },
  { "en": "Apa Itu Sinar-X (X-Ray) Medis?", "id": "Pencitraan Berbasis Atenuasi Sinar-X." },
  { "en": "Jaringan Apa Menyerap Sinar-X Paling Kuat?", "id": "Tulang (Tampak Putih Di Film)." },
  { "en": "Apa Itu Tomografi Terkomputasi (CT Scan)?", "id": "Pencitraan Sinar-X Lintas Penampang 3D." },
  { "en": "Apa Kepanjangan CT (Computed Tomography)?", "id": "Tomografi Terkomputasi." },
  { "en": "Apa Itu Pencitraan Resonansi Magnetik (MRI)?", "id": "Pencitraan Medan Magnet, Gelombang Radio." },
  { "en": "Apa Kepanjangan MRI (Magnetic Resonance Imaging)?", "id": "Pencitraan Resonansi Magnetik." },
  { "en": "Apa Keunggulan MRI (Magnetic Resonance Imaging)?", "id": "Kontras Jaringan Lunak Sangat Baik (Tanpa Radiasi)." },
  { "en": "Apa Itu Ultrasonografi (Ultrasound)?", "id": "Pencitraan Gelombang Suara Frekuensi Tinggi." },
  { "en": "Prinsip Dasar Ultrasonografi (Ultrasound)?", "id": "Pemantulan Gelombang Suara Batas Jaringan." },
  { "en": "Apa Itu Efek Doppler (Doppler Effect) Ultrasonografi?", "id": "Mengukur Kecepatan Aliran Darah." },
  { "en": "Apa Itu Kedokteran Nuklir (Nuclear Medicine)?", "id": "Penggunaan Radioisotop Diagnostik, Terapi." },
  { "en": "Apa Itu Pencitraan PET (Positron Emission Tomography)?", "id": "Pencitraan Fungsional Deteksi Positron." },
  { "en": "Apa Kepanjangan PET (Positron Emission Tomography)?", "id": "Tomografi Emisi Positron." },
  { "en": "Apa Itu Pencitraan SPECT (Single Photon Emission CT)?", "id": "Pencitraan Fungsional Deteksi Foton Tunggal." },
  { "en": "Apa Kepanjangan SPECT (Single Photon Emission Computed Tomography)?", "id": "Tomografi Terkomputasi Emisi Foton Tunggal." },
  { "en": "Apa Itu Endoskopi (Endoscopy)?", "id": "Visualisasi Dalam Tubuh Kamera Fleksibel." },
  { "en": "Apa Itu Terapi Radiasi (Radiotherapy)?", "id": "Pengobatan Kanker Menggunakan Radiasi Pengion." },
  { "en": "Apa Itu Akselerator Linear (Linac) Medis?", "id": "Penghasil Sinar-X/Elektron Energi Tinggi Terapi." },
  { "en": "Apa Itu Prostetik (Prosthesis)?", "id": "Penggantian Bagian Tubuh Artifisial." },
  { "en": "Apa Itu Implan Koklea (Cochlear Implant)?", "id": "Alat Bantu Dengar Elektronik Tuli Sensorineural." },
  { "en": "Apa Itu Antarmuka Otak-Komputer (BCI)?", "id": "Komunikasi Langsung Otak Dengan Perangkat Luar." },
  { "en": "Apa Itu Robotika Medis (Medical Robotics)?", "id": "Penggunaan Robot Dalam Prosedur Medis." },
  { "en": "Contoh Robot Bedah (Surgical Robot)?", "id": "Sistem Bedah Da Vinci." },
  { "en": "Apa Itu Telemedisin (Telemedicine)?", "id": "Pelayanan Kesehatan Jarak Jauh Teknologi." },
  { "en": "Apa Itu Wearable Health Device (Perangkat Kesehatan Pakai)?", "id": "Perangkat Monitor Kesehatan Dipakai Tubuh." },
  { "en": "Contoh Wearable Health Device (Perangkat Kesehatan Pakai)?", "id": "Smartwatch Monitor Detak Jantung, Sensor Glukosa." },
  { "en": "Apa Itu Bioinformatika (Bioinformatics)?", "id": "Aplikasi Komputasi Analisis Data Biologis." },
  { "en": "Apa Itu Sekuensing Genom (Genome Sequencing)?", "id": "Menentukan Urutan Lengkap DNA Organisme." },
  { "en": "Apa Itu CRISPR-Cas9 (CRISPR-Cas9)?", "id": "Teknologi Penyuntingan Gen Presisi." },
  { "en": "Apa Itu Rekayasa Jaringan (Tissue Engineering)?", "id": "Membuat Jaringan Biologis Fungsional Laboratorium." },
  { "en": "Apa Itu Biomaterial (Biomaterial)?", "id": "Material Berinteraksi Sistem Biologis." },
  { "en": "Contoh Biomaterial (Biomaterial)?", "id": "Implan Logam, Polimer, Keramik." },
  { "en": "Apa Itu Biomekanika (Biomechanics)?", "id": "Studi Mekanika Sistem Biologis." },
  { "en": "Apa Itu Sistem Pengiriman Obat (Drug Delivery)?", "id": "Metode Menghantarkan Obat Ke Target Tubuh." },
  { "en": "Apa Itu Nanomedicine (Nanomedicine)?", "id": "Aplikasi Nanoteknologi Di Bidang Medis." },
  { "en": "Apa Itu Lab-on-a-Chip (Lab Dalam Chip)?", "id": "Integrasi Fungsi Laboratorium Chip Kecil." },
  { "en": "Apa Itu Point-of-Care Testing (POCT)?", "id": "Pengujian Diagnostik Dekat Pasien." },
  { "en": "Apa Kepanjangan POCT (Point-of-Care Testing)?", "id": "Pengujian Titik Perawatan." },
  { "en": "Apa Itu Sistem Informasi Rumah Sakit (HIS)?", "id": "Hospital Information System (HIS)." },
  { "en": "Apa Kepanjangan HIS (Hospital Information System)?", "id": "Sistem Informasi Rumah Sakit." },
  { "en": "Apa Itu Rekam Medis Elektronik (EMR/EHR)?", "id": "Electronic Medical/Health Record." },
  { "en": "Apa Kepanjangan EMR (Electronic Medical Record)?", "id": "Rekam Medis Elektronik." },
  { "en": "Apa Kepanjangan EHR (Electronic Health Record)?", "id": "Rekam Kesehatan Elektronik." },
  { "en": "Apa Itu PACS (Picture Archiving Communication System)?", "id": "Sistem Penyimpanan, Komunikasi Gambar Medis." },
  { "en": "Apa Kepanjangan PACS (Picture Archiving Communication System)?", "id": "Sistem Komunikasi Pengarsipan Gambar." },
  { "en": "Apa Itu Standar DICOM (Digital Imaging Communications Medicine)?", "id": "Standar Format, Komunikasi Gambar Medis." },
  { "en": "Apa Kepanjangan DICOM (Digital Imaging Communications Medicine)?", "id": "Komunikasi Pencitraan Digital Kedokteran." },
  { "en": "Apa Itu Standar HL7 (Health Level Seven)?", "id": "Standar Pertukaran Informasi Kesehatan Elektronik." },
  { "en": "Apa Kepanjangan HL7 (Health Level Seven)?", "id": "Tingkat Kesehatan Tujuh." },
  { "en": "Apa Itu HIPAA (Health Insurance Portability Accountability Act)?", "id": "Undang-Undang Privasi Data Kesehatan AS." },
  { "en": "Apa Kepanjangan HIPAA (Health Insurance Portability Accountability Act)?", "id": "Undang-Undang Portabilitas Akuntabilitas Asuransi Kesehatan." },
  { "en": "Apa Itu Kecerdasan Buatan (AI) Medis?", "id": "Aplikasi AI Diagnostik, Perawatan, Riset." },
  { "en": "Contoh AI (Artificial Intelligence) Medis?", "id": "Analisis Citra Medis, Penemuan Obat." },
  { "en": "Apa Itu Validasi (Validation) Peralatan Medis?", "id": "Memastikan Alat Penuhi Spesifikasi, Aman." },
  { "en": "Apa Itu Regulasi (Regulation) Peralatan Medis?", "id": "Aturan Pemerintah Keamanan, Efektivitas Alat." },
  { "en": "Badan Regulasi (Regulation) Alat Medis AS?", "id": "FDA (Food and Drug Administration)." },
  { "en": "Apa Itu Tanda CE (CE Marking)?", "id": "Tanda Kepatuhan Standar Eropa." },
  { "en": "Apa Itu Uji Klinis (Clinical Trial)?", "id": "Pengujian Keamanan, Efektivitas Obat/Alat Baru." },
  { "en": "Apa Itu Etika Biomedis (Bioethics)?", "id": "Studi Isu Moral Bidang Biomedis." },
  { "en": "Apa Itu Osilasi (Oscillation) Relaksasi?", "id": "Osilasi Dihasilkan Pengisian/Pengosongan Periodik." },
  { "en": "Contoh Rangkaian Osilasi Relaksasi?", "id": "Multivibrator Astabil Menggunakan IC 555." },
  { "en": "Apa Itu Siklus Kerja (Duty Cycle)?", "id": "Persentase Waktu Sinyal Dalam Keadaan Tinggi." },
  { "en": "Bagaimana Rumus Siklus Kerja (Duty Cycle)?", "id": "Duty Cycle = (Waktu Tinggi / Periode) * 100%." },
  { "en": "Apa Itu Gelombang Persegi (Square Wave)?", "id": "Gelombang Kotak Siklus Kerja 50%." },
  { "en": "Apa Itu Analisis Fourier (Fourier Analysis)?", "id": "Penguraian Sinyal Periodik Komponen Sinusoidal." },
  { "en": "Apa Itu Deret Fourier (Fourier Series)?", "id": "Representasi Matematis Sinyal Periodik." },
  { "en": "Apa Itu Frekuensi Fundamental (Fundamental Frequency)?", "id": "Frekuensi Terendah Komponen Deret Fourier." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Frekuensi Kelipatan Bulat Frekuensi Fundamental." },
  { "en": "Gelombang Kotak (Square Wave) Mengandung Harmonisa Apa?", "id": "Hanya Harmonisa Ganjil." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Representasi Frekuensi Sinyal Non-Periodik." },
  { "en": "Apa Itu Spektrum Frekuensi (Frequency Spectrum)?", "id": "Plot Amplitudo/Fasa Komponen Frekuensi." },
  { "en": "Apa Itu Domain Waktu (Time Domain)?", "id": "Representasi Sinyal Sebagai Fungsi Waktu." },
  { "en": "Apa Itu Domain Frekuensi (Frequency Domain)?", "id": "Representasi Sinyal Sebagai Fungsi Frekuensi." },
  { "en": "Apa Itu Filter (Filter) Elektronik?", "id": "Rangkaian Memodifikasi Spektrum Frekuensi Sinyal." },
  { "en": "Apa Itu Filter Lolos Rendah (Low Pass)?", "id": "Melewatkan Frekuensi Rendah, Meredam Tinggi." },
  { "en": "Apa Itu Filter Lolos Tinggi (High Pass)?", "id": "Melewatkan Frekuensi Tinggi, Meredam Rendah." },
  { "en": "Apa Itu Filter Lolos Pita (Band Pass)?", "id": "Melewatkan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Tolak Pita (Band Stop)?", "id": "Meredam Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency)?", "id": "Frekuensi Batas Operasi Filter (-3dB)." },
  { "en": "Apa Itu Orde (Order) Filter?", "id": "Menentukan Kecuraman Slope (Roll-Off)." },
  { "en": "Apa Itu Respons Butterworth (Butterworth Response)?", "id": "Respons Passband Paling Datar." },
  { "en": "Apa Itu Respons Chebyshev (Chebyshev Response)?", "id": "Slope Lebih Curam, Ada Riak (Ripple)." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Menggunakan Komponen Aktif (Op-Amp)." },
  { "en": "Apa Itu Filter Pasif (Passive Filter)?", "id": "Filter Hanya Komponen Pasif (R, L, C)." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Karakteristik Op-Amp (Operational Amplifier) Ideal?", "id": "Gain Infinite, Input Z Infinite, Output Z Nol." },
  { "en": "Apa Itu Penguatan Loop Terbuka (Open Loop Gain)?", "id": "Penguatan Op-Amp Tanpa Umpan Balik." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Mengurangi Gain, Meningkatkan Stabilitas." },
  { "en": "Apa Itu Penguatan Loop Tertutup (Closed Loop Gain)?", "id": "Penguatan Op-Amp Dengan Umpan Balik Negatif." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Output Beda Fasa 180° Dengan Input." },
  { "en": "Apa Rumus Gain Penguat Inverting?", "id": "Av = -Rf / Rin." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Rumus Gain Penguat Non-Inverting?", "id": "Av = 1 + (Rf / Rin)." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Penguat Non-Inverting Dengan Gain 1." },
  { "en": "Apa Fungsi Pengikut Tegangan (Voltage Follower)?", "id": "Sebagai Buffer (Penyangga) Impedansi." },
  { "en": "Apa Itu Impedansi Input (Input Impedance) Tinggi?", "id": "Tidak Membebani Sumber Sinyal." },
  { "en": "Apa Itu Impedansi Output (Output Impedance) Rendah?", "id": "Mampu Memberi Arus Ke Beban." },
  { "en": "Apa Itu Penguat Penjumlah (Summing Amplifier)?", "id": "Output Adalah Jumlahan Terbobot Input." },
  { "en": "Apa Itu Penguat Diferensial (Differential Amplifier)?", "id": "Menguatkan Selisih Antara Dua Input." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Sinyal Yang Sama (Noise)." },
  { "en": "Apa Itu Integrator (Integrator) Op-Amp?", "id": "Output Adalah Integral Input Terhadap Waktu." },
  { "en": "Apa Itu Diferensiator (Differentiator) Op-Amp?", "id": "Output Adalah Turunan Input Terhadap Waktu." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Membandingkan Dua Tegangan, Output Digital." },
  { "en": "Apa Itu Schmitt Trigger (Schmitt Trigger)?", "id": "Komparator Dengan Histeresis." },
  { "en": "Apa Itu Histeresis (Hysteresis)?", "id": "Memiliki Dua Level Tegangan Threshold." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Semikonduktor Searah Arus." },
  { "en": "Apa Itu Bias Maju (Forward Bias)?", "id": "Memungkinkan Arus Lewat Dioda." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias)?", "id": "Menghambat Arus Lewat Dioda." },
  { "en": "Apa Itu Tegangan Maju (Forward Voltage) (Vf)?", "id": "Jatuh Tegangan Saat Dioda Konduksi." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah Arus AC Menjadi DC." },
  { "en": "Apa Itu Penyearah Setengah Gelombang (Half-Wave)?", "id": "Menggunakan Satu Dioda." },
  { "en": "Apa Itu Penyearah Gelombang Penuh (Full-Wave)?", "id": "Menggunakan Dioda Bridge Atau Trafo CT." },
  { "en": "Apa Itu Dioda Bridge (Bridge Rectifier)?", "id": "Empat Dioda Konfigurasi Jembatan." },
  { "en": "Apa Itu Kapasitor Filter (Filter Capacitor)?", "id": "Menghaluskan Tegangan DC Setelah Penyearah." },
  { "en": "Apa Itu Riak (Ripple) Tegangan?", "id": "Variasi AC Kecil Pada Tegangan DC." },
  { "en": "Apa Itu Dioda Zener (Zener Diode)?", "id": "Regulator Tegangan Paralel Bias Mundur." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Memancarkan Cahaya." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Sensor Intensitas Cahaya." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Perangkat Penguat/Saklar Terkontrol Arus." },
  { "en": "Apa Tiga Terminal BJT (Bipolar Junction Transistor)?", "id": "Basis (Base), Kolektor (Collector), Emitor (Emitter)." },
  { "en": "Apa Itu Penguatan Beta (β) BJT?", "id": "Rasio Arus Kolektor Terhadap Basis." },
  { "en": "Apa Itu Daerah Aktif (Active Region) BJT?", "id": "Daerah Operasi Penguatan." },
  { "en": "Apa Itu Daerah Saturasi (Saturation) BJT?", "id": "Daerah Operasi Saklar Tertutup (On)." },
  { "en": "Apa Itu Daerah Cutoff (Cutoff) BJT?", "id": "Daerah Operasi Saklar Terbuka (Off)." },
  { "en": "Apa Itu Transistor FET (Field Effect Transistor)?", "id": "Perangkat Penguat/Saklar Terkontrol Tegangan." },
  { "en": "Apa Tiga Terminal FET (Field Effect Transistor)?", "id": "Gate (Gerbang), Drain (Saluran), Source (Sumber)." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Jenis FET Paling Umum (Gerbang Terisolasi)." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit) (IC)?", "id": "Rangkaian Elektronik Dalam Chip Tunggal." },
  { "en": "Apa Itu Skala Integrasi (Scale of Integration)?", "id": "Jumlah Komponen Dalam Satu IC." },
  { "en": "Apa Itu SSI (Small Scale Integration)?", "id": "Integrasi Skala Kecil." },
  { "en": "Apa Itu MSI (Medium Scale Integration)?", "id": "Integrasi Skala Menengah." },
  { "en": "Apa Itu LSI (Large Scale Integration)?", "id": "Integrasi Skala Besar." },
  { "en": "Apa Itu VLSI (Very Large Scale Integration)?", "id": "Integrasi Skala Sangat Besar." },
  { "en": "Apa Itu Logika Digital (Digital Logic)?", "id": "Operasi Berbasis Nilai Biner (0, 1)." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Elemen Dasar Operasi Logika Digital." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Penyimpan Memori Satu Bit." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Simpan Data Biner." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Pencacah Urutan Digital." },
  { "en": "Apa Itu Multiplexer (Multiplexer) (MUX)?", "id": "Pemilih Jalur Data Digital." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) (DEMUX)?", "id": "Distributor Jalur Data Digital." },
  { "en": "Apa Itu Memori (Memory)?", "id": "Penyimpanan Informasi Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Volatil Baca Tulis Cepat." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Non-Volatil Hanya Baca." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "Unit Pemroses Pusat (CPU) Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Chip Tunggal (CPU, Memori, I/O)." },
  { "en": "Apa Itu Bus (Bus) Data?", "id": "Jalur Paralel Transfer Data Digital." },
  { "en": "Apa Itu Bus (Bus) Alamat?", "id": "Jalur Penentu Lokasi Memori/Periferal." },
  { "en": "Apa Itu Bus (Bus) Kontrol?", "id": "Jalur Sinyal Pengatur Operasi Bus." },
  { "en": "Apa Itu Komunikasi Serial (Serial Communication)?", "id": "Transfer Data Satu Bit Per Waktu." },
  { "en": "Apa Itu UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Antarmuka Komunikasi Serial Asinkron." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Antarmuka Komunikasi Serial Sinkron." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Bus Komunikasi Serial Dua Kawat." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Konverter Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Konverter Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Pengubah Besaran Fisik Ke Sinyal Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Pengubah Sinyal Listrik Ke Aksi Mekanis." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Tegangan Arus Bolak-Balik." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Pengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Pengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Sistem Terhadap Gangguan." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Tanah Untuk Keselamatan." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Penggunaan Semikonduktor Kontrol Daya." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Pengubah Daya DC Menjadi AC." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Pengaturan Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback) Kontrol?", "id": "Penggunaan Output Untuk Mengatur Input." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Digital Industri Otomatisasi." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Penentuan Nilai Besaran Kuantitatif." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu Kekuatan Medan Listrik (E)?", "id": "Gaya Listrik Per Satuan Muatan." },
  { "en": "Apa Satuan Kekuatan Medan Listrik?", "id": "Volt Per Meter (V/m)." },
  { "en": "Apa Itu Fluks Listrik (Electric Flux)?", "id": "Ukuran Aliran Medan Listrik." },
  { "en": "Apa Hukum Gauss (Gauss's Law) Listrik?", "id": "Fluks Listrik Total Terkait Muatan." },
  { "en": "Apa Itu Potensial (Potential) Listrik?", "id": "Energi Potensial Per Satuan Muatan." },
  { "en": "Apa Satuan Potensial (Potential) Listrik?", "id": "Volt (V)." },
  { "en": "Apa Itu Permukaan Ekuipotensial (Equipotential Surface)?", "id": "Permukaan Dengan Potensial Sama." },
  { "en": "Apa Itu Gradien (Gradient) Potensial?", "id": "Hubungan Medan Listrik Dan Potensial." },
  { "en": "Apa Itu Dielektrik (Dielectric)?", "id": "Bahan Isolator Dalam Medan Listrik." },
  { "en": "Apa Itu Konstanta Dielektrik (Dielectric Constant)?", "id": "Ukuran Kemampuan Simpan Energi Listrik." },
  { "en": "Apa Itu Polarisasi (Polarization) Dielektrik?", "id": "Pergeseran Muatan Internal Dielektrik." },
  { "en": "Apa Itu Arus Konduksi (Conduction Current)?", "id": "Aliran Muatan Bebas (Elektron)." },
  { "en": "Apa Itu Arus Konveksi (Convection Current)?", "id": "Aliran Muatan Akibat Gerakan Medium." },
  { "en": "Apa Itu Rapat Arus (Current Density) (J)?", "id": "Arus Listrik Per Satuan Luas." },
  { "en": "Apa Satuan Rapat Arus (Current Density)?", "id": "Ampere Per Meter Persegi (A/m²)." },
  { "en": "Apa Itu Persamaan Kontinuitas (Continuity Equation)?", "id": "Menyatakan Kekekalan Muatan Listrik." },
  { "en": "Apa Itu Medan Magnet (Magnetic Field) (B)?", "id": "Dihasilkan Arus Listrik Atau Magnet." },
  { "en": "Apa Satuan Medan Magnet (Magnetic Field) (B)?", "id": "Tesla (T)." },
  { "en": "Apa Itu Kuat Medan Magnet (H)?", "id": "Intensitas Medan Magnet." },
  { "en": "Apa Satuan Kuat Medan Magnet (H)?", "id": "Ampere Per Meter (A/m)." },
  { "en": "Apa Hubungan Antara B Dan H?", "id": "B Sama Dengan μ Kali H." },
  { "en": "Apa Itu Permeabilitas (Permeability) Magnetik (μ)?", "id": "Kemampuan Bahan Mendukung Medan Magnet." },
  { "en": "Apa Itu Hukum Biot-Savart (Biot-Savart Law)?", "id": "Hitung Medan Magnet Elemen Arus." },
  { "en": "Apa Itu Hukum Ampere (Ampere's Law)?", "id": "Hubungan Integral Medan Magnet, Arus." },
  { "en": "Apa Itu Gaya Lorentz (Lorentz Force)?", "id": "Gaya Pada Muatan Bergerak Medan EM." },
  { "en": "Apa Itu Gaya Magnetik (Magnetic Force) Kawat?", "id": "Gaya Pada Kawat Berarus Medan Magnet." },
  { "en": "Apa Itu Fluks (Flux) Magnetik (Φ)?", "id": "Jumlah Total Garis Medan Magnet." },
  { "en": "Apa Satuan Fluks (Flux) Magnetik?", "id": "Weber (Wb)." },
  { "en": "Apa Itu Hukum Gauss (Gauss's Law) Magnet?", "id": "Fluks Magnet Total Permukaan Tertutup Nol." },
  { "en": "Implikasi Hukum Gauss (Gauss's Law) Magnet?", "id": "Tidak Ada Monopol Magnetik." },
  { "en": "Apa Itu Rangkaian Magnetik (Magnetic Circuit)?", "id": "Analogi Rangkaian Listrik Untuk Magnet." },
  { "en": "Apa Itu Gaya Gerak Magnet (MMF)?", "id": "Sumber Fluks Magnet (Analog Tegangan)." },
  { "en": "Apa Kepanjangan MMF (Magnetomotive Force)?", "id": "Gaya Gerak Magnet." },
  { "en": "Apa Rumus MMF (Magnetomotive Force)?", "id": "MMF Sama Dengan N Kali I." },
  { "en": "Apa Satuan MMF (Magnetomotive Force)?", "id": "Ampere-Lilit (Ampere-Turns)." },
  { "en": "Apa Itu Reluktansi (Reluctance) (ℜ)?", "id": "Hambatan Terhadap Aliran Fluks Magnet." },
  { "en": "Apa Hukum Ohm (Ohm's Law) Rangkaian Magnetik?", "id": "MMF Sama Dengan Fluks Kali Reluktansi." },
  { "en": "Apa Itu Permeansi (Permeance)?", "id": "Kebalikan Dari Reluktansi (Reluctance)." },
  { "en": "Apa Itu Bahan Feromagnetik (Ferromagnetic Material)?", "id": "Bahan Permeabilitas Sangat Tinggi (Besi)." },
  { "en": "Apa Itu Kurva B-H (B-H Curve)?", "id": "Grafik Karakteristik Bahan Magnetik." },
  { "en": "Apa Itu Saturasi (Saturation) Magnetik?", "id": "Kondisi Fluks Maksimum Bahan." },
  { "en": "Apa Itu Histeresis (Hysteresis) Magnetik?", "id": "Ketergantungan Fluks Riwayat Medan Magnet." },
  { "en": "Apa Itu Loop Histeresis (Hysteresis Loop)?", "id": "Grafik B-H Siklus Penuh." },
  { "en": "Apa Itu Remanensi (Remanence)?", "id": "Sisa Fluks Saat H Nol." },
  { "en": "Apa Itu Koersivitas (Coercivity)?", "id": "Medan H Balik Hilangkan Remanensi." },
  { "en": "Apa Itu Rugi Histeresis (Hysteresis Loss)?", "id": "Energi Hilang Akibat Siklus Histeresis." },
  { "en": "Apa Itu Arus Eddy (Eddy Current)?", "id": "Arus Induksi Berputar Inti Konduktif." },
  { "en": "Apa Itu Rugi Arus Eddy (Eddy Current Loss)?", "id": "Energi Hilang Jadi Panas Arus Eddy." },
  { "en": "Bagaimana Mengurangi Rugi Arus Eddy?", "id": "Gunakan Inti Berlaminasi Atau Ferit." },
  { "en": "Apa Itu Bahan Magnet Lunak (Soft Magnet)?", "id": "Loop Histeresis Sempit, Rugi Rendah." },
  { "en": "Apa Itu Bahan Magnet Keras (Hard Magnet)?", "id": "Loop Histeresis Lebar (Magnet Permanen)." },
  { "en": "Apa Itu Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Pembangkitan GGL Medan Magnet Berubah." },
  { "en": "Apa Hukum Faraday (Faraday's Law) Induksi?", "id": "GGL Induksi Proporsional Laju Perubahan Fluks." },
  { "en": "Apa Hukum Lenz (Lenz's Law)?", "id": "Arah Arus Induksi Melawan Perubahan." },
  { "en": "Apa Itu GGL (Gaya Gerak Listrik) Gerak?", "id": "GGL Induksi Konduktor Bergerak Medan B." },
  { "en": "Apa Itu GGL (Gaya Gerak Listrik) Transformator?", "id": "GGL Induksi Fluks Berubah Waktu." },
  { "en": "Apa Itu Induktansi (Inductance) (L)?", "id": "Rasio Fluks Terkopel Terhadap Arus." },
  { "en": "Apa Satuan Induktansi (Inductance)?", "id": "Henry (H)." },
  { "en": "Apa Itu Induktansi Diri (Self-Inductance)?", "id": "Fluks Dihasilkan Arus Sendiri." },
  { "en": "Apa Itu Induktansi Mutual (Mutual Inductance)?", "id": "Fluks Dihasilkan Arus Kumparan Lain." },
  { "en": "Apa Itu Koefisien Kopling (k)?", "id": "Ukuran Fraksi Fluks Terkopel (0-1)." },
  { "en": "Apa Rumus Induktansi Mutual (M)?", "id": "M = k * √(L1 * L2)." },
  { "en": "Apa Energi (Energy) Tersimpan Induktor?", "id": "U = ½ * L * I²." },
  { "en": "Apa Itu Persamaan Maxwell (Maxwell's Equations)?", "id": "Empat Persamaan Dasar Elektromagnetisme." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Medan E, B Tanpa Medium." },
  { "en": "Apa Kecepatan Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Kecepatan Cahaya (c)." },
  { "en": "Apa Itu Sinyal (Signal)?", "id": "Besaran Fisik Membawa Informasi." },
  { "en": "Apa Itu Sinyal Analog (Analog Signal)?", "id": "Sinyal Nilai Kontinu." },
  { "en": "Apa Itu Sinyal Digital (Digital Signal)?", "id": "Sinyal Nilai Diskrit." },
  { "en": "Apa Itu Sinyal Periodik (Periodic Signal)?", "id": "Sinyal Berulang Pola Tetap." },
  { "en": "Apa Itu Sinyal Aperiodik (Aperiodic Signal)?", "id": "Sinyal Tidak Memiliki Pola Berulang." },
  { "en": "Apa Itu Sinyal Deterministik (Deterministic Signal)?", "id": "Sinyal Dapat Diprediksi Matematis." },
  { "en": "Apa Itu Sinyal Acak (Random Signal)?", "id": "Sinyal Tidak Dapat Diprediksi (Noise)." },
  { "en": "Apa Itu Sinyal Waktu Kontinu (Continuous Time)?", "id": "Sinyal Terdefinisi Setiap Saat t." },
  { "en": "Apa Itu Sinyal Waktu Diskrit (Discrete Time)?", "id": "Sinyal Terdefinisi Hanya Waktu Diskrit n." },
  { "en": "Apa Itu Sistem (System)?", "id": "Memproses Sinyal Input Hasilkan Output." },
  { "en": "Apa Itu Sistem Linear (Linear System)?", "id": "Memenuhi Prinsip Superposisi." },
  { "en": "Apa Itu Sistem Invarian Waktu (Time-Invariant)?", "id": "Respon Tidak Berubah Geseran Waktu." },
  { "en": "Apa Itu Sistem LTI (Linear Time-Invariant)?", "id": "Sistem Linear Dan Invarian Waktu." },
  { "en": "Apa Itu Respon Impuls (Impulse Response) h(t)?", "id": "Output Sistem LTI Terhadap Input Impuls." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematis Output Sistem LTI." },
  { "en": "Apa Itu Sistem Kausal (Causal System)?", "id": "Output Hanya Tergantung Input Sekarang, Lalu." },
  { "en": "Apa Itu Sistem Stabil (Stable System) (BIBO)?", "id": "Input Terbatas Hasilkan Output Terbatas." },
  { "en": "Apa Kepanjangan BIBO (Bounded-Input Bounded-Output)?", "id": "Input Terbatas Output Terbatas." },
  { "en": "Apa Itu Deret Fourier (Fourier Series)?", "id": "Representasi Sinyal Periodik Jumlahan Sinusoid." },
  { "en": "Apa Itu Koefisien Fourier (Fourier Coefficient)?", "id": "Amplitudo Komponen Harmonisa Sinyal." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Representasi Frekuensi Sinyal Aperiodik." },
  { "en": "Apa Itu Spektrum Frekuensi (Frequency Spectrum)?", "id": "Plot Amplitudo, Fasa Vs Frekuensi." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Sinyal?", "id": "Rentang Frekuensi Signifikan Sinyal." },
  { "en": "Apa Itu Transformasi Laplace (Laplace Transform)?", "id": "Generalisasi Transformasi Fourier (Domain-s)." },
  { "en": "Apa Variabel Kompleks (Complex Variable) (s) Laplace?", "id": "s = σ + jω." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function) H(s)?", "id": "Rasio Output/Input Domain-s." },
  { "en": "Apa Itu Pole (Kutub) Sistem?", "id": "Akar Penyebut Fungsi Transfer." },
  { "en": "Apa Itu Zero (Nol) Sistem?", "id": "Akar Pembilang Fungsi Transfer." },
  { "en": "Apa Itu Kestabilan (Stability) Domain-s?", "id": "Semua Pole Di Setengah Kiri Bidang-s." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response)?", "id": "H(jω) (Fungsi Transfer s = jω)." },
  { "en": "Apa Itu Diagram Bode (Bode Plot)?", "id": "Plot Magnitudo (dB), Fasa (Derajat) Vs Log(ω)." },
  { "en": "Apa Itu Pencuplikan (Sampling)?", "id": "Konversi Sinyal Kontinu Ke Diskrit." },
  { "en": "Apa Itu Teorema Sampling Nyquist (Nyquist Sampling Theorem)?", "id": "Laju Sampling Minimal 2x Frekuensi Maks." },
  { "en": "Apa Laju Nyquist (Nyquist Rate)?", "id": "Dua Kali Frekuensi Maksimum Sinyal." },
  { "en": "Apa Itu Aliasing (Aliasing)?", "id": "Distorsi Frekuensi Akibat Sampling Lambat." },
  { "en": "Apa Itu Filter Anti-Aliasing (Anti-Aliasing Filter)?", "id": "Filter Low-Pass Sebelum Sampling." },
  { "en": "Apa Itu Rekonstruksi (Reconstruction) Sinyal?", "id": "Konversi Sinyal Diskrit Ke Kontinu." },
  { "en": "Apa Itu Filter Rekonstruksi (Reconstruction Filter)?", "id": "Filter Low-Pass Setelah DAC." },
  { "en": "Apa Itu Transformasi Z (Z-Transform)?", "id": "Alat Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function) H(z)?", "id": "Rasio Output/Input Domain-Z." },
  { "en": "Apa Itu Lingkaran Satuan (Unit Circle) Bidang-Z?", "id": "Lingkaran Jari-Jari Satu Pusat Nol." },
  { "en": "Kondisi Kestabilan (Stability) Domain-Z?", "id": "Semua Pole Di Dalam Lingkaran Satuan." },
  { "en": "Apa Itu Nilai Puncak (Peak Value) Gelombang AC?", "id": "Amplitudo Maksimum Gelombang Dari Nol." },
  { "en": "Apa Itu Nilai Puncak Ke Puncak (Peak-To-Peak)?", "id": "Selisih Antara Puncak Positif, Negatif." },
  { "en": "Apa Hubungan RMS (Root Mean Square), Puncak Sinus?", "id": "RMS Sama Dengan Puncak Dibagi Akar Dua." },
  { "en": "Apa Itu Faktor Crest (Crest Factor)?", "id": "Rasio Nilai Puncak Terhadap Nilai RMS." },
  { "en": "Berapa Faktor Crest (Crest Factor) Gelombang Sinus?", "id": "Akar Dua (Sekitar 1.414)." },
  { "en": "Apa Itu Faktor Bentuk (Form Factor)?", "id": "Rasio Nilai RMS Terhadap Nilai Rata-Rata Absolut." },
  { "en": "Berapa Faktor Bentuk (Form Factor) Gelombang Sinus?", "id": "Pi Dibagi Dua Akar Dua (≈1.11)." },
  { "en": "Apa Itu Analisis Fourier (Fourier Analysis)?", "id": "Menguraikan Sinyal Periodik Menjadi Harmonisa." },
  { "en": "Apa Itu Harmonisa (Harmonic)?", "id": "Komponen Frekuensi Kelipatan Bulat Fundamental." },
  { "en": "Gelombang Kotak (Square Wave) Mengandung Harmonisa Apa?", "id": "Hanya Mengandung Harmonisa Ganjil." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Ukuran Distorsi Akibat Harmonisa." },
  { "en": "Apa Kepanjangan THD (Total Harmonic Distortion)?", "id": "Total Distorsi Harmonik." },
  { "en": "Apa Itu Sistem Linear Time-Invariant (LTI)?", "id": "Sistem Linear Tidak Berubah Waktu." },
  { "en": "Apa Itu Respon Impuls (Impulse Response) Sistem LTI?", "id": "Output Sistem Terhadap Input Impuls Unit." },
  { "en": "Bagaimana Menentukan Output Sistem LTI?", "id": "Konvolusi Input Dengan Respon Impuls." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematis Integral/Penjumlahan Bergeser." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function) H(s)?", "id": "Transformasi Laplace Dari Respon Impuls." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response) H(jω)?", "id": "Perilaku Sistem Terhadap Input Sinusoidal." },
  { "en": "Bagaimana Dapat Respon Frekuensi Dari H(s)?", "id": "Ganti Variabel s Dengan jω." },
  { "en": "Apa Itu Plot Bode (Bode Plot)?", "id": "Grafik Logaritmik Magnitudo, Fasa Vs Frekuensi." },
  { "en": "Apa Itu Pole (Kutub) Fungsi Transfer?", "id": "Frekuensi Kompleks Penguatan Tak Terhingga." },
  { "en": "Apa Itu Zero (Nol) Fungsi Transfer?", "id": "Frekuensi Kompleks Penguatan Nol." },
  { "en": "Kondisi Kestabilan (Stability) Berdasarkan Pole?", "id": "Semua Pole Di Setengah Kiri Bidang-s." },
  { "en": "Apa Itu Filter Pasif (Passive Filter)?", "id": "Filter Menggunakan Komponen R, L, C." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Menggunakan Komponen Aktif (Op-Amp)." },
  { "en": "Apa Keuntungan Filter Aktif (Active Filter)?", "id": "Penguatan, Tidak Perlu Induktor Besar." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency)?", "id": "Frekuensi Batas Filter (-3dB)." },
  { "en": "Apa Itu Orde (Order) Filter?", "id": "Menentukan Kecuraman Slope (Roll-Off)." },
  { "en": "Apa Itu Respons Butterworth (Butterworth Response)?", "id": "Respons Paling Datar Di Passband." },
  { "en": "Apa Itu Respons Chebyshev (Chebyshev Response)?", "id": "Slope Lebih Curam, Ada Riak Passband." },
  { "en": "Apa Itu Transformasi Bintang-Delta (Y-Δ)?", "id": "Konversi Ekuivalen Antara Konfigurasi Rangkaian." },
  { "en": "Apa Itu Dioda Ideal (Ideal Diode)?", "id": "Saklar Sempurna Satu Arah." },
  { "en": "Apa Itu Tegangan Maju (Forward Voltage) Dioda?", "id": "Jatuh Tegangan Saat Dioda Konduksi." },
  { "en": "Apa Itu Daerah Breakdown (Breakdown Region) Dioda?", "id": "Daerah Tegangan Mundur Arus Besar." },
  { "en": "Apa Itu Model Potongan Linear (Piecewise Linear)?", "id": "Aproksimasi Karakteristik Dioda Garis Lurus." },
  { "en": "Apa Itu Kapasitansi Difusi (Diffusion Capacitance)?", "id": "Kapasitansi Dioda Saat Bias Maju." },
  { "en": "Apa Itu Kapasitansi Transisi (Transition Capacitance)?", "id": "Kapasitansi Dioda Saat Bias Mundur." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Perangkat Tiga Lapis Terkontrol Arus." },
  { "en": "Apa Mode Operasi Utama BJT?", "id": "Cutoff, Aktif, Dan Saturasi." },
  { "en": "Apa Itu Penguatan Beta (β) BJT?", "id": "Rasio Arus Kolektor Terhadap Basis." },
  { "en": "Apa Itu Garis Beban (Load Line) AC?", "id": "Garis Operasi Penguat Sinyal AC." },
  { "en": "Apa Itu Efek Early (Early Effect)?", "id": "Modulasi Lebar Basis Efektif BJT." },
  { "en": "Apa Itu FET (Field Effect Transistor)?", "id": "Perangkat Tiga Terminal Terkontrol Tegangan." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET Dengan Gerbang Terisolasi Oksida." },
  { "en": "Apa Itu Tegangan Threshold (Vt) MOSFET?", "id": "Tegangan Gate Minimum Aktifkan Saluran." },
  { "en": "Apa Itu CMOS (Complementary MOS)?", "id": "Logika Kombinasi NMOS Dan PMOS." },
  { "en": "Apa Keunggulan Utama CMOS?", "id": "Disipasi Daya Statis Sangat Rendah." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Karakteristik Op-Amp Ideal?", "id": "Gain Infinite, Input Z Infinite, Output Z Nol." },
  { "en": "Apa Itu Virtual Short (Hubung Singkat Virtual)?", "id": "Input Inverting, Non-Inverting Sama (Feedback Negatif)." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Output Beda Fasa 180 Derajat." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Buffer Dengan Gain Satu." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Sinyal Mode Sama." },
  { "en": "Apa Itu Slew Rate (Laju Perubahan)?", "id": "Kecepatan Maksimum Perubahan Output Op-Amp." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Penghasil Sinyal Periodik Tanpa Input." },
  { "en": "Apa Syarat Osilasi Barkhausen (Barkhausen Criterion)?", "id": "Gain Loop Satu, Fasa Loop Nol." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Elemen Dasar Komputasi Digital." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Aturan Manipulasi Ekspresi Logika." },
  { "en": "Apa Itu Peta Karnaugh (Karnaugh Map)?", "id": "Metode Grafis Penyederhanaan Logika." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Bistabil." },
  { "en": "Apa Beda Latch Dan Flip-Flop?", "id": "Latch (Level), Flip-Flop (Edge Triggered)." },
  { "en": "Apa Itu Register (Register)?", "id": "Penyimpanan Sementara Data Digital." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Menghitung Jumlah Kejadian Atau Pulsa." },
  { "en": "Apa Itu Memori (Memory)?", "id": "Penyimpanan Informasi Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Volatil Baca Tulis Cepat." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Non-Volatil Hanya Baca." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Konversi Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Konversi Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "Unit Pemroses Pusat (CPU) Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Bus (Bus) Sistem?", "id": "Jalur Komunikasi Antar Komponen." },
  { "en": "Apa Itu Komunikasi Serial (Serial Communication)?", "id": "Transfer Data Satu Bit Sekaligus." },
  { "en": "Apa Itu Komunikasi Paralel (Parallel Communication)?", "id": "Transfer Data Banyak Bit Bersamaan." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Detektor Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Penggerak Berdasarkan Sinyal Kontrol." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Tegangan AC." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Pengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Pengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Pembangkitan Hingga Penggunaan Listrik." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Sistem Terhadap Gangguan." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Kontrol Daya Listrik Semikonduktor." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Modulasi Lebar Pulsa Kontrol Daya." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Pengaturan Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Informasi Output Kembali Ke Input." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Otomatisasi Industri." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komunikasi." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Global." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Kuantifikasi Besaran Fisik." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Pengukuran Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Reproduktifitas Pengukuran." },
  { "en": "Apa Itu Elektromagnetisme (Electromagnetism)?", "id": "Studi Interaksi Listrik Magnet." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Energi Medan Elektromagnetik." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Radiasi/Terima Gelombang EM." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Penumpangan Informasi Ke Pembawa." },
  { "en": "Apa Itu Noise (Derau) Termal?", "id": "Noise Akibat Agitasi Termal Elektron." },
  { "en": "Apa Nama Lain Noise Termal?", "id": "Noise Johnson Atau Noise Nyquist." },
  { "en": "Bagaimana Daya Noise Termal Bergantung Suhu?", "id": "Proporsional Terhadap Suhu Absolut." },
  { "en": "Bagaimana Daya Noise Termal Bergantung Bandwidth?", "id": "Proporsional Terhadap Bandwidth." },
  { "en": "Apa Rumus Daya Noise Termal?", "id": "P_n Sama Dengan K T B." },
  { "en": "Apa Itu Konstanta Boltzmann (k)?", "id": "Konstanta Fisika Terkait Energi Suhu." },
  { "en": "Apa Itu Noise Figure (Angka Derau) (NF)?", "id": "Ukuran Degradasi SNR Oleh Perangkat." },
  { "en": "NF (Noise Figure) Ideal Sama Dengan Berapa?", "id": "Nol Desibel (0 dB) Atau Satu." },
  { "en": "Apa Itu Noise Temperature (Suhu Derau)?", "id": "Ukuran Alternatif Derau Perangkat." },
  { "en": "Apa Hubungan Noise Figure, Noise Temperature?", "id": "NF = 1 + (Te / T₀)." },
  { "en": "Apa Itu T₀ (T Nol)?", "id": "Suhu Referensi Standar (290 K)." },
  { "en": "Apa Itu Friis Formula (Rumus Friis)?", "id": "Menghitung Noise Figure Sistem Kaskade." },
  { "en": "Tahap Mana Paling Pengaruhi Noise Figure Kaskade?", "id": "Tahap Pertama (Penguatan Awal)." },
  { "en": "Apa Itu Link Budget (Anggaran Tautan)?", "id": "Perhitungan Daya, Rugi, Gain Sistem Komunikasi." },
  { "en": "Tujuan Link Budget (Anggaran Tautan)?", "id": "Memastikan Sinyal Diterima Cukup Kuat." },
  { "en": "Parameter Apa Dalam Link Budget (Anggaran Tautan)?", "id": "Daya Pancar, Gain Antena, Rugi Jalur." },
  { "en": "Apa Itu Deteksi Koheren (Coherent Detection)?", "id": "Deteksi Sinyal Menggunakan Referensi Fasa." },
  { "en": "Apa Itu Deteksi Non-Koheren (Non-Coherent Detection)?", "id": "Deteksi Sinyal Tanpa Referensi Fasa." },
  { "en": "Apa Itu Laju Kesalahan Bit (BER)?", "id": "Rasio Bit Error Terhadap Total Bit." },
  { "en": "BER (Bit Error Rate) Rendah Menunjukkan Kinerja Apa?", "id": "Kinerja Komunikasi Yang Baik." },
  { "en": "Apa Itu Energi Per Bit (Eb)?", "id": "Energi Sinyal Untuk Satu Bit Data." },
  { "en": "Apa Itu Kepadatan Derau (N₀)?", "id": "Daya Derau Per Satuan Bandwidth." },
  { "en": "Apa Itu Eb/N₀ (Rasio Energi Bit Ke Derau)?", "id": "Ukuran Kinerja Kunci Komunikasi Digital." },
  { "en": "Eb/N₀ (Rasio Energi Bit Ke Derau) Tinggi Berarti Apa?", "id": "Kualitas Sinyal Baik (BER Rendah)." },
  { "en": "Apa Itu Pengkodean Kanal (Channel Coding)?", "id": "Menambah Redundansi Untuk Deteksi Error." },
  { "en": "Apa Tujuan Pengkodean Kanal (Channel Coding)?", "id": "Memperbaiki Kinerja BER (Bit Error Rate)." },
  { "en": "Apa Itu Laju Kode (Code Rate)?", "id": "Rasio Bit Informasi Terhadap Total Bit." },
  { "en": "Apa Itu Interleaving (Interleaving)?", "id": "Mengubah Urutan Bit Data Transmisi." },
  { "en": "Apa Tujuan Interleaving (Interleaving)?", "id": "Mengatasi Error Beruntun (Burst Error)." },
  { "en": "Apa Itu Ekualiser (Equalizer) Kanal?", "id": "Filter Mengkompensasi Distorsi Kanal." },
  { "en": "Apa Itu Interferensi Antar Simbol (ISI)?", "id": "Simbol Berdekatan Saling Mengganggu." },
  { "en": "Apa Penyebab ISI (Inter-Symbol Interference)?", "id": "Multipath Fading, Bandwidth Kanal Terbatas." },
  { "en": "Apa Itu Diagram Mata (Eye Diagram)?", "id": "Visualisasi Kualitas Sinyal Digital." },
  { "en": "Apa Itu Pembukaan Mata (Eye Opening)?", "id": "Menunjukkan Margin Terhadap Noise, Jitter." },
  { "en": "Apa Itu Multiplexing (Multipleksasi)?", "id": "Berbagi Satu Kanal Banyak Sinyal." },
  { "en": "Apa Itu FDM (Frequency Division Multiplexing)?", "id": "Multiplexing Pembagian Frekuensi." },
  { "en": "Apa Itu TDM (Time Division Multiplexing)?", "id": "Multiplexing Pembagian Waktu." },
  { "en": "Apa Itu WDM (Wavelength Division Multiplexing)?", "id": "Multiplexing Pembagian Panjang Gelombang (Optik)." },
  { "en": "Apa Itu Akses Ganda (Multiple Access)?", "id": "Banyak Pengguna Berbagi Satu Kanal." },
  { "en": "Apa Itu FDMA (Frequency Division Multiple Access)?", "id": "Akses Ganda Pembagian Frekuensi." },
  { "en": "Apa Itu TDMA (Time Division Multiple Access)?", "id": "Akses Ganda Pembagian Waktu." },
  { "en": "Apa Itu CDMA (Code Division Multiple Access)?", "id": "Akses Ganda Pembagian Kode." },
  { "en": "Apa Itu Jaringan Seluler (Cellular Network)?", "id": "Area Layanan Dibagi Sel-Sel Kecil." },
  { "en": "Apa Itu Handoff (Handoff) / Handover?", "id": "Perpindahan Otomatis Antar Sel." },
  { "en": "Apa Itu Penggunaan Ulang Frekuensi (Frequency Reuse)?", "id": "Gunakan Frekuensi Sama Sel Berjauhan." },
  { "en": "Apa Itu Sistem Tenaga (Power System) Listrik?", "id": "Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Itu Pembangkit Listrik (Power Plant)?", "id": "Fasilitas Pembangkit Energi Listrik." },
  { "en": "Jenis Pembangkit Listrik (Power Plant) Termal?", "id": "PLTU (Batubara), PLTG (Gas), PLTN (Nuklir)." },
  { "en": "Jenis Pembangkit Listrik (Power Plant) Terbarukan?", "id": "PLTA (Air), PLTS (Surya), PLTB (Angin)." },
  { "en": "Apa Itu Transmisi (Transmission) Listrik?", "id": "Penyaluran Daya Jarak Jauh (HV/EHV)." },
  { "en": "Apa Itu Distribusi (Distribution) Listrik?", "id": "Penyaluran Daya Ke Konsumen (MV/LV)." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Transformasi Tegangan, Switching." },
  { "en": "Apa Fungsi Transformator Daya (Power Transformer)?", "id": "Menaikkan/Menurunkan Tegangan Sistem." },
  { "en": "Apa Fungsi Pemutus Tenaga (Circuit Breaker)?", "id": "Memutus Arus Gangguan Secara Otomatis." },
  { "en": "Apa Fungsi Pemisah (Disconnector)?", "id": "Memisahkan Peralatan Tanpa Beban Arus." },
  { "en": "Apa Itu Rele Proteksi (Protection Relay)?", "id": "Mendeteksi Gangguan, Memberi Sinyal Trip." },
  { "en": "Apa Itu Trafo Arus (Current Transformer) (CT)?", "id": "Mengukur Arus Tinggi Sistem Proteksi." },
  { "en": "Apa Itu Trafo Tegangan (Voltage Transformer) (PT/VT)?", "id": "Mengukur Tegangan Tinggi Sistem Proteksi." },
  { "en": "Apa Itu Gangguan (Fault) Sistem Tenaga?", "id": "Kondisi Abnormal (Hubung Singkat)." },
  { "en": "Apa Itu Hubung Singkat (Short Circuit)?", "id": "Kontak Impedansi Rendah Antar Konduktor." },
  { "en": "Apa Itu Sistem Grounding (Grounding System)?", "id": "Koneksi Ke Tanah Untuk Keamanan/Operasi." },
  { "en": "Apa Itu Kualitas Daya (Power Quality)?", "id": "Karakteristik Gelombang Tegangan, Arus Ideal." },
  { "en": "Masalah Kualitas Daya (Power Quality) Umum?", "id": "Sag, Swell, Harmonisa, Transien." },
  { "en": "Apa Itu Sag (Sag) Tegangan?", "id": "Penurunan Sesaat Tegangan RMS." },
  { "en": "Apa Itu Swell (Swell) Tegangan?", "id": "Kenaikan Sesaat Tegangan RMS." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Frekuensi Kelipatan Fundamental." },
  { "en": "Apa Penyebab Harmonisa (Harmonics)?", "id": "Beban Non-Linear (Elektronika Daya)." },
  { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Ukuran Efisiensi Penggunaan Daya Listrik." },
  { "en": "Apa Itu Koreksi Faktor Daya (Power Factor Correction)?", "id": "Memperbaiki Faktor Daya (Pasang Kapasitor)." },
  { "en": "Apa Itu Jaringan Cerdas (Smart Grid)?", "id": "Jaringan Listrik Modern Komunikasi Digital." },
  { "en": "Apa Itu Energi Terbarukan (Renewable Energy)?", "id": "Energi Sumber Tak Habis (Surya, Angin)." },
  { "en": "Apa Itu Sel Fotovoltaik (Photovoltaic Cell)?", "id": "Mengubah Cahaya Matahari Langsung Listrik." },
  { "en": "Apa Itu Turbin Angin (Wind Turbine)?", "id": "Mengubah Energi Angin Jadi Listrik." },
  { "en": "Apa Itu Penyimpanan Energi (Energy Storage)?", "id": "Menyimpan Energi Listrik (Baterai)." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Loop Terbuka (Open Loop) Kontrol?", "id": "Kontrol Tanpa Umpan Balik Output." },
  { "en": "Apa Itu Loop Tertutup (Closed Loop) Kontrol?", "id": "Kontrol Menggunakan Umpan Balik Output." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Mengukur Output Bandingkan Dengan Setpoint." },
  { "en": "Apa Itu Setpoint (Setpoint)?", "id": "Nilai Target Diinginkan Sistem Kontrol." },
  { "en": "Apa Itu Kesalahan (Error) Kontrol?", "id": "Selisih Antara Setpoint, Nilai Aktual." },
  { "en": "Apa Itu Kontroler (Controller)?", "id": "Memproses Error Hasilkan Sinyal Kontrol." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Perangkat Eksekusi Sinyal Kontrol (Motor)." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Perangkat Pengukur Variabel Proses." },
  { "en": "Apa Itu Kontrol On-Off (On-Off Control)?", "id": "Jenis Kontrol Paling Sederhana (Dua Posisi)." },
  { "en": "Apa Itu Kontrol Proporsional (P)?", "id": "Output Kontrol Proporsional Error." },
  { "en": "Apa Itu Kontrol Integral (I)?", "id": "Output Kontrol Proporsional Integral Error." },
  { "en": "Apa Fungsi Kontrol Integral (I)?", "id": "Menghilangkan Error Keadaan Tunak." },
  { "en": "Apa Itu Kontrol Derivatif (D)?", "id": "Output Kontrol Proporsional Laju Error." },
  { "en": "Apa Fungsi Kontrol Derivatif (D)?", "id": "Memberi Respon Antisipasi Perubahan." },
  { "en": "Apa Itu Kontroler PID (PID Controller)?", "id": "Gabungan Kontrol Proporsional, Integral, Derivatif." },
  { "en": "Apa Itu Kestabilan (Stability) Sistem Kontrol?", "id": "Kemampuan Sistem Kembali Seimbang." },
  { "en": "Apa Itu Respon Transien (Transient Response)?", "id": "Perilaku Sistem Sebelum Stabil." },
  { "en": "Apa Itu Respon Keadaan Tunak (Steady State)?", "id": "Perilaku Sistem Setelah Stabil." },
  { "en": "Apa Itu Overshoot (Overshoot) Respon?", "id": "Output Melebihi Setpoint Sementara." },
  { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Waktu Respon Capai Level Tertentu." },
  { "en": "Apa Itu Waktu Turun (Settling Time)?", "id": "Waktu Respon Masuk Batas Toleransi." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function)?", "id": "Representasi Laplace Input-Output Sistem." },
  { "en": "Apa Itu Pole (Kutub) Sistem?", "id": "Akar Penyebut Fungsi Transfer." },
  { "en": "Apa Itu Zero (Nol) Sistem?", "id": "Akar Pembilang Fungsi Transfer." },
  { "en": "Kestabilan (Stability) Tergantung Pole Atau Zero?", "id": "Tergantung Posisi Pole (Kutub)." },
  { "en": "Dimana Pole (Kutub) Sistem Stabil?", "id": "Di Setengah Kiri Bidang Kompleks-s." },
  { "en": "Apa Itu Diagram Blok (Block Diagram)?", "id": "Representasi Grafis Sistem Kontrol." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response)?", "id": "Respon Sistem Input Sinusoidal." },
  { "en": "Apa Itu Diagram Bode (Bode Plot)?", "id": "Plot Magnitudo, Fasa Respon Frekuensi." },
  { "en": "Apa Itu Gain Margin (GM) (Batas Penguatan)?", "id": "Ukuran Kestabilan Relatif Gain." },
  { "en": "Apa Itu Phase Margin (PM) (Batas Fasa)?", "id": "Ukuran Kestabilan Relatif Fasa." },
  { "en": "Apa Itu Kontrol Digital (Digital Control)?", "id": "Implementasi Kontroler Komputer Digital." },
  { "en": "Apa Itu Transformasi Z (Z-Transform)?", "id": "Alat Analisis Sistem Waktu Diskrit." },
  { "en": "Kestabilan (Stability) Sistem Waktu Diskrit?", "id": "Semua Pole Di Dalam Lingkaran Satuan." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Kontrol Industri Otomatisasi." },
  { "en": "Apa Itu Bahasa Ladder Logic (Logika Tangga)?", "id": "Bahasa Pemrograman Grafis PLC." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Teknologi Desain, Konstruksi, Operasi Robot." },
  { "en": "Apa Itu Derajat Kebebasan (DOF) Robot?", "id": "Jumlah Gerakan Independen Robot." },
  { "en": "Apa Itu Kinematika (Kinematics) Robot?", "id": "Studi Gerakan Robot Tanpa Gaya." },
  { "en": "Apa Itu Dinamika (Dynamics) Robot?", "id": "Studi Gerakan Robot Memperhitungkan Gaya." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Kemampuan Komputer Menginterpretasi Gambar." },
  { "en": "Apa Itu Pemrosesan Sinyal (Signal Processing)?", "id": "Analisis, Modifikasi Sinyal Elektronik." },
  { "en": "Apa Nama Lain Jembatan Wheatstone?", "id": "Jembatan Resistansi Empat Lengan." },
  { "en": "Apa Fungsi Jembatan Wheatstone?", "id": "Mengukur Resistansi Tidak Dikenal Presisi." },
  { "en": "Kapan Jembatan Wheatstone Dikatakan Seimbang?", "id": "Arus Galvanometer Bernilai Nol." },
  { "en": "Apa Itu Galvanometer (Galvanometer)?", "id": "Alat Ukur Arus Sangat Sensitif." },
  { "en": "Apa Itu Jembatan Kelvin (Kelvin Bridge)?", "id": "Modifikasi Jembatan Wheatstone Resistansi Rendah." },
  { "en": "Mengapa Jembatan Kelvin (Kelvin Bridge) Lebih Baik?", "id": "Mengeliminasi Efek Resistansi Kontak." },
  { "en": "Apa Itu Jembatan AC (AC Bridge)?", "id": "Jembatan Ukur Impedansi (L, C)." },
  { "en": "Sebutkan Contoh Jembatan AC (AC Bridge)?", "id": "Maxwell, Hay, Schering, Wien." },
  { "en": "Apa Fungsi Jembatan Maxwell (Maxwell Bridge)?", "id": "Mengukur Induktansi (Kualitas Rendah Menengah)." },
  { "en": "Apa Fungsi Jembatan Hay (Hay Bridge)?", "id": "Mengukur Induktansi (Kualitas Tinggi)." },
  { "en": "Apa Fungsi Jembatan Schering (Schering Bridge)?", "id": "Mengukur Kapasitansi, Faktor Disipasi." },
  { "en": "Apa Fungsi Jembatan Wien (Wien Bridge)?", "id": "Mengukur Frekuensi Sinyal AC." },
  { "en": "Apa Itu Potensiometer (Potentiometer)?", "id": "Resistor Variabel Tiga Terminal." },
  { "en": "Sebagai Apa Potensiometer (Potentiometer) Digunakan?", "id": "Pembagi Tegangan Yang Dapat Diatur." },
  { "en": "Apa Tiga Kaki Potensiometer (Potentiometer)?", "id": "Dua Ujung Tetap, Satu Wiper." },
  { "en": "Apa Itu Wiper (Wiper) Potensiometer?", "id": "Kontak Bergerak Mengatur Pembagian." },
  { "en": "Apa Itu Taper (Taper) Potensiometer?", "id": "Hubungan Antara Posisi, Resistansi." },
  { "en": "Apa Itu Taper (Taper) Linear?", "id": "Resistansi Berubah Linear Posisi Wiper." },
  { "en": "Apa Itu Taper (Taper) Logaritmik (Audio)?", "id": "Resistansi Berubah Logaritmik (Kontrol Volume)." },
  { "en": "Apa Itu Rheostat (Rheostat)?", "id": "Resistor Variabel Kontrol Arus." },
  { "en": "Apa Perbedaan Utama Potensiometer, Rheostat?", "id": "Potensiometer (3 Kaki, Tegangan), Rheostat (2 Kaki, Arus)." },
  { "en": "Apa Itu Termistor (Thermistor)?", "id": "Resistor Semikonduktor Peka Suhu." },
  { "en": "Apa Kepanjangan NTC (Negative Temperature Coefficient)?", "id": "Koefisien Suhu Negatif." },
  { "en": "Bagaimana Sifat Termistor NTC (Negative Temperature Coefficient)?", "id": "Resistansi Turun Saat Suhu Naik." },
  { "en": "Apa Kepanjangan PTC (Positive Temperature Coefficient)?", "id": "Koefisien Suhu Positif." },
  { "en": "Bagaimana Sifat Termistor PTC (Positive Temperature Coefficient)?", "id": "Resistansi Naik Saat Suhu Naik." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu Berbasis Resistansi Logam." },
  { "en": "Logam Apa Digunakan RTD (Resistance Temperature Detector)?", "id": "Platina (Platinum) (Umumnya Pt100)." },
  { "en": "Mana Lebih Linear, Termistor Atau RTD?", "id": "RTD (Resistance Temperature Detector) Jauh Lebih Linear." },
  { "en": "Mana Lebih Sensitif, Termistor Atau RTD?", "id": "Termistor (Thermistor) Jauh Lebih Sensitif." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Peka Intensitas Cahaya." },
  { "en": "Nama Lain LDR (Light Dependent Resistor)?", "id": "Fotokonduktor (Photoconductor) Atau Sel Fotosel." },
  { "en": "Bagaimana Resistansi LDR (Light Dependent Resistor) Saat Gelap?", "id": "Resistansi Sangat Tinggi." },
  { "en": "Bagaimana Resistansi LDR (Light Dependent Resistor) Saat Terang?", "id": "Resistansi Menjadi Rendah." },
  { "en": "Apa Itu Varistor (Varistor)?", "id": "Resistor Tergantung Tegangan (VDR)." },
  { "en": "Apa Kepanjangan VDR (Voltage Dependent Resistor)?", "id": "Resistor Tergantung Tegangan." },
  { "en": "Apa Nama Populer Varistor (Varistor)?", "id": "MOV (Metal Oxide Varistor)." },
  { "en": "Apa Fungsi Utama Varistor (Varistor) (MOV)?", "id": "Melindungi Rangkaian Dari Lonjakan Tegangan." },
  { "en": "Bagaimana Resistansi Varistor (Varistor) Normal?", "id": "Resistansi Sangat Tinggi." },
  { "en": "Bagaimana Resistansi Varistor (Varistor) Saat Lonjakan?", "id": "Resistansi Sangat Rendah (Menjepit Tegangan)." },
  { "en": "Apa Itu Strain Gauge (Strain Gauge)?", "id": "Sensor Mengukur Regangan Mekanis." },
  { "en": "Prinsip Kerja Strain Gauge (Strain Gauge)?", "id": "Resistansi Kawat Berubah Saat Teregang." },
  { "en": "Aplikasi Strain Gauge (Strain Gauge) Umum?", "id": "Sel Beban (Load Cell) Timbangan." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Periodik." },
  { "en": "Apa Syarat Terjadi Osilasi (Oscillation)?", "id": "Kriteria Barkhausen (Gain Loop 1, Fasa 0)." },
  { "en": "Apa Jenis Umpan Balik (Feedback) Osilator?", "id": "Umpan Balik Positif (Positive Feedback)." },
  { "en": "Apa Itu Osilator (Oscillator) RC?", "id": "Osilator Gunakan Jaringan Resistor-Kapasitor." },
  { "en": "Contoh Osilator (Oscillator) RC?", "id": "Osilator Geser Fasa, Jembatan Wien." },
  { "en": "Apa Itu Osilator (Oscillator) LC?", "id": "Osilator Gunakan Jaringan Induktor-Kapasitor." },
  { "en": "Contoh Osilator (Oscillator) LC?", "id": "Colpitts, Hartley, Clapp." },
  { "en": "Apa Itu Rangkaian Tank (Tank Circuit)?", "id": "Kombinasi Resonansi Paralel L Dan C." },
  { "en": "Apa Itu Osilator (Oscillator) Kristal?", "id": "Osilator Frekuensi Sangat Stabil." },
  { "en": "Bahan Apa Kristal (Crystal) Osilator?", "id": "Kuarsa (Quartz) Piezoelektrik." },
  { "en": "Apa Itu Stabilitas (Stability) Frekuensi?", "id": "Kemampuan Osilator Pertahankan Frekuensi." },
  { "en": "Osilator (Oscillator) Mana Paling Stabil?", "id": "Osilator Kristal (Crystal Oscillator)." },
  { "en": "Apa Itu Multivibrator (Multivibrator)?", "id": "Osilator Penghasil Gelombang Non-Sinusoidal." },
  { "en": "Apa Itu Multivibrator Astabil (Astable)?", "id": "Tidak Punya Keadaan Stabil (Osilator)." },
  { "en": "Apa Itu Multivibrator Monostabil (Monostable)?", "id": "Punya Satu Keadaan Stabil (Timer)." },
  { "en": "Apa Itu Multivibrator Bistabil (Bistable)?", "id": "Punya Dua Keadaan Stabil (Flip-Flop)." },
  { "en": "IC (Integrated Circuit) Apa Populer Multivibrator?", "id": "IC (Integrated Circuit) Timer 555." },
  { "en": "Apa Itu Osilator Terkendali Tegangan (VCO)?", "id": "Frekuensi Output Dikontrol Tegangan Input." },
  { "en": "Apa Kepanjangan VCO (Voltage Controlled Oscillator)?", "id": "Osilator Terkendali Tegangan." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sistem Umpan Balik Pengunci Fasa." },
  { "en": "Komponen Utama PLL (Phase-Locked Loop)?", "id": "Detektor Fasa, Filter, VCO." },
  { "en": "Aplikasi PLL (Phase-Locked Loop)?", "id": "Sintesis Frekuensi, Demodulasi FM." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Rangkaian Meningkatkan Amplitudo Sinyal." },
  { "en": "Apa Itu Penguatan (Gain)?", "id": "Rasio Sinyal Output Terhadap Input." },
  { "en": "Apa Satuan Penguatan (Gain)?", "id": "Tanpa Satuan Atau Desibel (dB)." },
  { "en": "Apa Itu Penguat Tegangan (Voltage Amplifier)?", "id": "Menguatkan Amplitudo Tegangan Sinyal." },
  { "en": "Apa Itu Penguat Arus (Current Amplifier)?", "id": "Menguatkan Amplitudo Arus Sinyal." },
  { "en": "Apa Itu Penguat Daya (Power Amplifier)?", "id": "Menguatkan Daya Total Sinyal (V x I)." },
  { "en": "Apa Itu Impedansi Input (Input Impedance) Penguat?", "id": "Impedansi Dilihat Sumber Sinyal." },
  { "en": "Impedansi Input (Input Impedance) Penguat Tegangan Ideal?", "id": "Tak Terhingga." },
  { "en": "Impedansi Input (Input Impedance) Penguat Arus Ideal?", "id": "Nol." },
  { "en": "Apa Itu Impedansi Output (Output Impedance) Penguat?", "id": "Impedansi Dilihat Beban." },
  { "en": "Impedansi Output (Output Impedance) Penguat Tegangan Ideal?", "id": "Nol." },
  { "en": "Impedansi Output (Output Impedance) Penguat Arus Ideal?", "id": "Tak Terhingga." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Penguat?", "id": "Rentang Frekuensi Penguatan Efektif." },
  { "en": "Apa Itu Distorsi (Distortion) Penguat?", "id": "Perubahan Bentuk Sinyal Tak Diinginkan." },
  { "en": "Apa Itu Distorsi Amplitudo (Amplitude Distortion)?", "id": "Penguatan Tidak Linear." },
  { "en": "Apa Itu Distorsi Frekuensi (Frequency Distortion)?", "id": "Penguatan Tidak Sama Semua Frekuensi." },
  { "en": "Apa Itu Distorsi Fasa (Phase Distortion)?", "id": "Pergeseran Fasa Tidak Sama Frekuensi." },
  { "en": "Apa Itu Noise (Derau) Penguat?", "id": "Sinyal Acak Ditambahkan Oleh Penguat." },
  { "en": "Apa Itu Angka Derau (Noise Figure)?", "id": "Ukuran Degradasi SNR (Signal-to-Noise Ratio) Penguat." },
  { "en": "Apa Itu Penguat Kelas A (Class A)?", "id": "Transistor Aktif Selama 360 Derajat Siklus." },
  { "en": "Apa Keuntungan Penguat Kelas A?", "id": "Linearitas Paling Baik (Distorsi Rendah)." },
  { "en": "Apa Kerugian Penguat Kelas A?", "id": "Efisiensi Sangat Rendah (Banyak Panas)." },
  { "en": "Apa Itu Penguat Kelas B (Class B)?", "id": "Transistor Aktif Selama 180 Derajat Siklus." },
  { "en": "Konfigurasi Apa Penguat Kelas B?", "id": "Konfigurasi Push-Pull (Dua Transistor)." },
  { "en": "Apa Keuntungan Penguat Kelas B?", "id": "Efisiensi Jauh Lebih Baik Dari Kelas A." },
  { "en": "Apa Kerugian Penguat Kelas B?", "id": "Distorsi Crossover (Crossover Distortion)." },
  { "en": "Apa Itu Penguat Kelas AB (Class AB)?", "id": "Kompromi Antara Kelas A Dan B." },
  { "en": "Tujuan Penguat Kelas AB (Class AB)?", "id": "Menghilangkan Distorsi Crossover Kelas B." },
  { "en": "Apa Itu Penguat Kelas C (Class C)?", "id": "Transistor Aktif Kurang 180 Derajat." },
  { "en": "Apa Keuntungan Penguat Kelas C?", "id": "Efisiensi Sangat Tinggi." },
  { "en": "Apa Kerugian Penguat Kelas C?", "id": "Sangat Non-Linear (Distorsi Tinggi)." },
  { "en": "Dimana Penguat Kelas C Digunakan?", "id": "Penguat RF (Frekuensi Radio) Tertala." },
  { "en": "Apa Itu Penguat Kelas D (Class D)?", "id": "Penguat Switching (PWM)." },
  { "en": "Apa Keuntungan Penguat Kelas D?", "id": "Efisiensi Paling Tinggi (Sangat Efisien)." },
  { "en": "Apa Itu Penguat Diferensial (Differential Amplifier)?", "id": "Menguatkan Selisih Dua Sinyal Input." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Op-Amp Menolak Sinyal Sama." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Mengembalikan Sebagian Output Ke Input." },
  { "en": "Apa Efek Umpan Balik Negatif?", "id": "Menstabilkan Gain, Mengurangi Distorsi." },
  { "en": "Apa Efek Umpan Balik Positif?", "id": "Menyebabkan Osilasi (Digunakan Osilator)." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Op-Amp Membalik Fasa Output 180 Derajat." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Op-Amp Fasa Output Sama Input." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Penguat Op-Amp Gain Satu." },
  { "en": "Apa Nama Lain Pengikut Tegangan?", "id": "Buffer (Penyangga) Op-Amp." },
  { "en": "Impedansi Input (Input Impedance) Pengikut Tegangan?", "id": "Sangat Tinggi." },
  { "en": "Impedansi Output (Output Impedance) Pengikut Tegangan?", "id": "Sangat Rendah." },
  { "en": "Apa Fungsi Pengikut Tegangan (Voltage Follower)?", "id": "Isolasi Impedansi (Cegah Beban)." },
  { "en": "Apa Itu Penguat Penjumlah (Summing Amplifier)?", "id": "Output Jumlahan Terbobot Input." },
  { "en": "Apa Itu Penguat Diferensial (Differential Amplifier)?", "id": "Menguatkan Selisih Dua Tegangan Input." },
  { "en": "Apa Itu Penguat Instrumentasi (Instrumentation Amplifier)?", "id": "Penguat Diferensial Presisi Tinggi." },
  { "en": "Ciri Utama Penguat Instrumentasi (Instrumentation Amplifier)?", "id": "CMRR (Common Mode Rejection Ratio) Tinggi, Gain Mudah Diatur." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Sinyal Mode Bersama." },
  { "en": "Apa Itu Integrator (Integrator) Op-Amp?", "id": "Output Adalah Integral Input." },
  { "en": "Apa Itu Diferensiator (Differentiator) Op-Amp?", "id": "Output Adalah Turunan Input." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Membandingkan Dua Tegangan Input." },
  { "en": "Apa Output Komparator (Comparator)?", "id": "Tegangan Saturasi Positif Atau Negatif." },
  { "en": "Apa Itu Schmitt Trigger (Schmitt Trigger)?", "id": "Komparator (Comparator) Dengan Histeresis." },
  { "en": "Apa Fungsi Histeresis (Hysteresis) Schmitt Trigger?", "id": "Mencegah Osilasi Output (Noise Switching)." },
  { "en": "Apa Itu Filter (Filter) Aktif?", "id": "Filter Menggunakan Komponen Aktif (Op-Amp)." },
  { "en": "Keuntungan Filter (Filter) Aktif?", "id": "Bisa Penguatan, Tanpa Induktor." },
  { "en": "Apa Itu Filter (Filter) Sallen-Key?", "id": "Topologi Filter Aktif Orde Dua Populer." },
  { "en": "Apa Itu Osilator (Oscillator) Jembatan Wien?", "id": "Penghasil Gelombang Sinus Relatif Murni." },
  { "en": "Apa Itu Osilator (Oscillator) Geser Fasa?", "id": "Osilator (Oscillator) Menggunakan Jaringan RC." },
  { "en": "Apa Itu Osilator (Oscillator) Colpitts?", "id": "Osilator (Oscillator) LC (Pembagi Kapasitif)." },
  { "en": "Apa Itu Osilator (Oscillator) Hartley?", "id": "Osilator (Oscillator) LC (Pembagi Induktif)." },
  { "en": "Apa Itu Osilator (Oscillator) Kristal?", "id": "Osilator (Oscillator) Frekuensi Sangat Stabil." },
  { "en": "Apa Itu Timer (Timer) IC 555?", "id": "IC Serbaguna Timer, Osilator." },
  { "en": "Mode Astabil (Astable Mode) IC 555?", "id": "Sebagai Osilator Gelombang Kotak." },
  { "en": "Mode Monostabil (Monostable Mode) IC 555?", "id": "Sebagai Timer Satu Kali Tembak (One-Shot)." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit) (IC)?", "id": "Banyak Komponen Dalam Satu Chip Silikon." },
  { "en": "Apa Itu Skala Integrasi (Scale Integration) IC?", "id": "Ukuran Jumlah Komponen Per Chip." },
  { "en": "Apa Itu VLSI (Very Large Scale Integration)?", "id": "Integrasi Skala Sangat Besar." },
  { "en": "Apa Itu Fabrikasi (Fabrication) IC?", "id": "Proses Pembuatan Chip IC." },
  { "en": "Apa Bahan Dasar (Substrate) IC?", "id": "Silikon (Silicon) Murni." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Proses Pencetakan Pola Sirkuit." },
  { "en": "Apa Itu Doping (Doping) Semikonduktor?", "id": "Penambahan Impuritas Kontrol Konduktivitas." },
  { "en": "Apa Itu Logika Digital (Digital Logic)?", "id": "Elektronika Berbasis Level Biner." },
  { "en": "Apa Itu Bit (Bit)?", "id": "Satuan Dasar Informasi Digital (0/1)." },
  { "en": "Apa Itu Byte (Byte)?", "id": "Delapan Bit." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Blok Bangunan Dasar Logika Digital." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Untuk Logika Digital." },
  { "en": "Apa Itu Tabel Kebenaran (Truth Table)?", "id": "Tabel Output Untuk Semua Input Logika." },
  { "en": "Apa Itu Gerbang (Gate) AND?", "id": "Output Tinggi Hanya Jika Semua Input Tinggi." },
  { "en": "Apa Itu Gerbang (Gate) OR?", "id": "Output Tinggi Jika Salah Satu Input Tinggi." },
  { "en": "Apa Itu Gerbang (Gate) NOT (Inverter)?", "id": "Output Adalah Kebalikan Input." },
  { "en": "Apa Itu Gerbang (Gate) NAND?", "id": "Inversi Dari Gerbang AND." },
  { "en": "Apa Itu Gerbang (Gate) NOR?", "id": "Inversi Dari Gerbang OR." },
  { "en": "Apa Itu Gerbang (Gate) XOR (Exclusive OR)?", "id": "Output Tinggi Jika Input Berbeda." },
  { "en": "Apa Itu Gerbang (Gate) XNOR (Exclusive NOR)?", "id": "Output Tinggi Jika Input Sama." },
  { "en": "Apa Itu Gerbang Universal (Universal Gate)?", "id": "Gerbang NAND Dan NOR." },
  { "en": "Apa Itu Sirkuit Kombinasional (Combinational Circuit)?", "id": "Output Tergantung Input Saat Ini Saja." },
  { "en": "Contoh Sirkuit Kombinasional (Combinational Circuit)?", "id": "Adder, Decoder, Multiplexer." },
  { "en": "Apa Itu Sirkuit Sekuensial (Sequential Circuit)?", "id": "Output Tergantung Input, Keadaan Terdahulu." },
  { "en": "Contoh Sirkuit Sekuensial (Sequential Circuit)?", "id": "Flip-Flop, Counter, Register." },
  { "en": "Apa Itu Sinyal Clock (Clock Signal)?", "id": "Sinyal Sinkronisasi Sirkuit Sekuensial." },
  { "en": "Apa Itu Latch (Latch)?", "id": "Elemen Memori Sensitif Level Sinyal." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Sensitif Tepi Sinyal." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) SR?", "id": "Set-Reset Flip-Flop." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) D?", "id": "Data Flip-Flop (Menyimpan Data)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) T?", "id": "Toggle Flip-Flop (Membalik Keadaan)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) JK?", "id": "Flip-Flop Universal (Set, Reset, Toggle)." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Penyimpan Data." },
  { "en": "Apa Itu Register Geser (Shift Register)?", "id": "Menggeser Data Bit Per Bit." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Pencacah Urutan Biner." },
  { "en": "Apa Itu Counter (Pencacah) Sinkron?", "id": "Semua Flip-Flop Di-Clock Bersamaan." },
  { "en": "Apa Itu Counter (Pencacah) Asinkron (Ripple)?", "id": "Output Memicu Clock Flip-Flop Berikutnya." },
  { "en": "Apa Itu Modulus (Modulus) Counter?", "id": "Jumlah Keadaan (State) Counter." },
  { "en": "Apa Itu Counter (Pencacah) Dekade?", "id": "Counter Modulus Sepuluh (0-9)." },
  { "en": "Apa Itu Memori (Memory)?", "id": "Perangkat Penyimpanan Data Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Volatil (Data Hilang Tanpa Daya)." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Non-Volatil (Data Tetap)." },
  { "en": "Apa Itu Memori Flash (Flash Memory)?", "id": "Jenis ROM (Read Only Memory) Dapat Dihapus Elektrik." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor) (MPU)?", "id": "CPU (Central Processing Unit) Dalam Satu Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller) (MCU)?", "id": "MPU + Memori + Peripheral I/O." },
  { "en": "Apa Itu Bus (Bus) Sistem?", "id": "Jalur Komunikasi Data, Alamat, Kontrol." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Mengubah Besaran Fisik Ke Sinyal Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Mengubah Sinyal Listrik Ke Gerakan Mekanis." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Level Tegangan AC." },
  { "en": "Apa Prinsip Kerja Transformator (Transformer)?", "id": "Induksi Mutual Elektromagnetik." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Mengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Mengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Motor Induksi (Induction Motor)?", "id": "Motor AC Asinkron Paling Umum." },
  { "en": "Apa Itu Slip (Slip) Motor Induksi?", "id": "Perbedaan Kecepatan Rotor, Medan Putar." },
  { "en": "Apa Itu Motor Sinkron (Synchronous Motor)?", "id": "Motor AC Berputar Kecepatan Sinkron." },
  { "en": "Apa Itu Motor DC (Direct Current)?", "id": "Motor Beroperasi Arus Searah." },
  { "en": "Apa Itu Komutator (Commutator) Motor DC?", "id": "Saklar Mekanis Pembalik Arah Arus Rotor." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Motor Bergerak Dalam Langkah Diskrit." },
  { "en": "Apa Itu Motor Servo (Servo Motor)?", "id": "Motor Kontrol Posisi Loop Tertutup." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Kontrol Konversi Daya Semikonduktor." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Konverter AC Ke DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Konverter DC Ke AC." },
  { "en": "Apa Itu Konverter (Converter) DC-DC?", "id": "Mengubah Level Tegangan DC." },
  { "en": "Apa Itu Thyristor (Thyristor) / SCR?", "id": "Saklar Daya Semikonduktor Terkontrol." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Saklar Daya Kecepatan Tinggi, Daya Tinggi." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Rata-Rata Daya Saklar." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Jaringan Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Itu Transmisi (Transmission) Tegangan Tinggi?", "id": "Penyaluran Daya Jarak Jauh Efisien." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Sistem Dari Gangguan." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Pelindung Arus Lebih/Gangguan." },
  { "en": "Apa Itu Rele (Relay) Proteksi?", "id": "Pendeteksi Kondisi Gangguan Sistem." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Bumi Untuk Keselamatan." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Kontrol Umpan Balik (Feedback Control)?", "id": "Menggunakan Sinyal Output Mengoreksi Input." },
  { "en": "Apa Itu Kontroler PID (PID Controller)?", "id": "Kontroler Umpan Balik Industri Populer." },
  { "en": "Apa Itu Pemrosesan Sinyal (Signal Processing)?", "id": "Analisis, Interpretasi, Manipulasi Sinyal." },
  { "en": "Apa Itu Sinyal Waktu Kontinu (Continuous Time Signal)?", "id": "Sinyal Terdefinisi Setiap Saat Waktu." },
  { "en": "Apa Itu Sinyal Waktu Diskrit (Discrete Time Signal)?", "id": "Sinyal Terdefinisi Hanya Waktu Tertentu." },
  { "en": "Apa Itu Pencuplikan (Sampling)?", "id": "Mengubah Sinyal Kontinu Ke Diskrit." },
  { "en": "Apa Teorema Sampling Nyquist (Nyquist Sampling Theorem)?", "id": "Laju Sampling Harus > 2x Frekuensi Max." },
  { "en": "Apa Itu Aliasing (Aliasing)?", "id": "Distorsi Frekuensi Akibat Sampling Kurang." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Mengubah Nilai Amplitudo Diskrit." },
  { "en": "Apa Itu Kesalahan Kuantisasi (Quantization Error)?", "id": "Error Akibat Pembulatan Nilai." },
  { "en": "Apa Itu Sistem Linear Time-Invariant (LTI)?", "id": "Sistem Linear, Respon Tak Berubah Waktu." },
  { "en": "Apa Itu Respon Impuls (Impulse Response)?", "id": "Output Sistem LTI Terhadap Input Impuls." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematis Output Sistem LTI." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Mengubah Sinyal Domain Waktu Ke Frekuensi." },
  { "en": "Apa Itu Spektrum Frekuensi (Frequency Spectrum)?", "id": "Representasi Amplitudo/Fasa Vs Frekuensi." },
  { "en": "Apa Itu Transformasi Fourier Diskrit (DFT)?", "id": "Transformasi Fourier Sinyal Waktu Diskrit." },
  { "en": "Apa Itu Transformasi Fourier Cepat (FFT)?", "id": "Algoritma Efisien Hitung DFT." },
  { "en": "Apa Itu Filter Digital (Digital Filter)?", "id": "Memodifikasi Spektrum Sinyal Digital." },
  { "en": "Apa Itu Filter FIR (Finite Impulse Response)?", "id": "Filter Respon Impuls Berhingga (Stabil)." },
  { "en": "Apa Itu Filter IIR (Infinite Impulse Response)?", "id": "Filter Respon Impuls Tak Hingga (Umpan Balik)." },
  { "en": "Apa Keuntungan Filter FIR (Finite Impulse Response)?", "id": "Fasa Linear Dapat Dirancang." },
  { "en": "Apa Keuntungan Filter IIR (Infinite Impulse Response)?", "id": "Lebih Efisien Komputasi Dari FIR." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Pertukaran Data." },
  { "en": "Apa Itu LAN (Local Area Network)?", "id": "Jaringan Area Lokal (Gedung)." },
  { "en": "Apa Itu WAN (Wide Area Network)?", "id": "Jaringan Area Luas (Antar Kota/Negara)." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Global Antar Jaringan." },
  { "en": "Apa Itu Protokol (Protocol)?", "id": "Aturan Komunikasi Jaringan." },
  { "en": "Apa Itu Model OSI (Open Systems Interconnection)?", "id": "Model Referensi Tujuh Lapisan Jaringan." },
  { "en": "Sebutkan Tujuh Lapisan OSI (OSI Layers)?", "id": "Fisik, Data Link, Network, Transport, Sesi, Presentasi, Aplikasi." },
  { "en": "Apa Itu Lapisan Fisik (Physical Layer)?", "id": "Transmisi Bit Mentah Lewat Medium." },
  { "en": "Apa Itu Lapisan Data Link (Data Link Layer)?", "id": "Transfer Data Antar Node Berdekatan (Frame)." },
  { "en": "Apa Itu Lapisan Jaringan (Network Layer)?", "id": "Pengalamatan Logis, Routing Paket (IP)." },
  { "en": "Apa Itu Lapisan Transport (Transport Layer)?", "id": "Komunikasi End-to-End (TCP/UDP)." },
  { "en": "Apa Itu Lapisan Sesi (Session Layer)?", "id": "Manajemen Dialog Antar Aplikasi." },
  { "en": "Apa Itu Lapisan Presentasi (Presentation Layer)?", "id": "Format Data, Enkripsi, Kompresi." },
  { "en": "Apa Itu Lapisan Aplikasi (Application Layer)?", "id": "Layanan Jaringan Langsung Ke Pengguna." },
  { "en": "Apa Itu Model TCP/IP (TCP/IP Model)?", "id": "Model Arsitektur Praktis Jaringan Internet." },
  { "en": "Apa Itu Alamat IP (IP Address)?", "id": "Alamat Logis Unik Perangkat Jaringan." },
  { "en": "Apa Itu IPv4 (Internet Protocol version 4)?", "id": "Alamat IP 32-Bit (Format Desimal Bertitik)." },
  { "en": "Apa Itu IPv6 (Internet Protocol version 6)?", "id": "Alamat IP 128-Bit (Menggantikan IPv4)." },
  { "en": "Apa Itu Subnet Mask (Subnet Mask)?", "id": "Memisahkan Bagian Jaringan, Host Alamat IP." },
  { "en": "Apa Itu Default Gateway (Gateway Default)?", "id": "Router Tujuan Paket Luar Jaringan Lokal." },
  { "en": "Apa Itu DNS (Domain Name System)?", "id": "Menerjemahkan Nama Domain Ke Alamat IP." },
  { "en": "Apa Itu DHCP (Dynamic Host Configuration Protocol)?", "id": "Memberikan Konfigurasi IP Otomatis." },
  { "en": "Apa Itu Alamat MAC (MAC Address)?", "id": "Alamat Fisik Unik Perangkat Keras Jaringan." },
  { "en": "Apa Itu ARP (Address Resolution Protocol)?", "id": "Memetakan Alamat IP Ke Alamat MAC." },
  { "en": "Apa Itu Switch (Switch)?", "id": "Menghubungkan Perangkat Dalam LAN (Lapisan 2)." },
  { "en": "Apa Itu Router (Router)?", "id": "Menghubungkan Jaringan Berbeda (Lapisan 3)." },
  { "en": "Apa Itu Firewall (Firewall)?", "id": "Sistem Keamanan Penyaring Lalu Lintas Jaringan." },
  { "en": "Apa Itu Ethernet (Ethernet)?", "id": "Standar Teknologi Jaringan LAN Kabel." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Standar Teknologi Jaringan LAN Nirkabel." },
  { "en": "Apa Itu Access Point (Titik Akses) Wi-Fi?", "id": "Perangkat Penghubung Nirkabel Ke Jaringan Kabel." },
  { "en": "Apa Itu SSID (Service Set Identifier)?", "id": "Nama Jaringan Wi-Fi." },
  { "en": "Apa Itu Protokol Keamanan (Security Protocol) Wi-Fi?", "id": "WEP (Wired Equivalent Privacy), WPA (Wi-Fi Protected Access), WPA2, WPA3." },
  { "en": "Mana Protokol Keamanan (Security Protocol) Wi-Fi Terkuat?", "id": "WPA3 (Wi-Fi Protected Access 3)." },
  { "en": "Apa Itu TCP (Transmission Control Protocol)?", "id": "Protokol Transport Andal Berorientasi Koneksi." },
  { "en": "Apa Ciri Utama TCP (Transmission Control Protocol)?", "id": "Pengecekan Error, Kontrol Aliran, Urutan Paket." },
  { "en": "Apa Itu UDP (User Datagram Protocol)?", "id": "Protokol Transport Cepat Tanpa Koneksi." },
  { "en": "Apa Ciri Utama UDP (User Datagram Protocol)?", "id": "Tidak Andal, Tidak Ada Jaminan Urutan." },
  { "en": "Apa Itu HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Transfer Data Web." },
  { "en": "Apa Itu HTTPS (HTTP Secure)?", "id": "HTTP (Hypertext Transfer Protocol) Dengan Enkripsi SSL/TLS." },
  { "en": "Apa Itu FTP (File Transfer Protocol)?", "id": "Protokol Transfer Berkas Antar Komputer." },
  { "en": "Apa Itu SMTP (Simple Mail Transfer Protocol)?", "id": "Protokol Pengiriman Email." },
  { "en": "Apa Itu POP3 (Post Office Protocol 3)?", "id": "Protokol Pengambilan Email Dari Server." },
  { "en": "Apa Itu IMAP (Internet Message Access Protocol)?", "id": "Protokol Akses Email Di Server." },
  { "en": "Apa Itu SSH (Secure Shell)?", "id": "Protokol Akses Remote Aman." },
  { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Membuat Koneksi Aman Jaringan Publik." },
  { "en": "Apa Itu NAT (Network Address Translation)?", "id": "Mengubah Alamat IP Privat Ke Publik." },
  { "en": "Apa Itu VLAN (Virtual LAN)?", "id": "Membagi LAN Fisik Jadi Beberapa LAN Logis." },
  { "en": "Apa Itu Routing (Routing)?", "id": "Proses Menentukan Jalur Terbaik Paket Jaringan." },
  { "en": "Apa Itu Protokol Routing (Routing Protocol)?", "id": "Algoritma Pertukaran Informasi Routing Antar Router." },
  { "en": "Contoh Protokol Routing (Routing Protocol) Internal (IGP)?", "id": "RIP (Routing Information Protocol), OSPF (Open Shortest Path First), EIGRP (Enhanced Interior Gateway Routing Protocol)." },
  { "en": "Contoh Protokol Routing (Routing Protocol) Eksternal (EGP)?", "id": "BGP (Border Gateway Protocol)." },
  { "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Perlindungan Sistem Digital Dari Serangan." },
  { "en": "Apa Itu Malware (Malicious Software)?", "id": "Perangkat Lunak Jahat (Virus, Worm, Trojan)." },
  { "en": "Apa Itu Phishing (Phishing)?", "id": "Upaya Penipuan Mendapatkan Informasi Sensitif." },
  { "en": "Apa Itu Ransomware (Ransomware)?", "id": "Malware Mengenkripsi Data Minta Tebusan." },
  { "en": "Apa Itu Serangan DDoS (Distributed Denial of Service)?", "id": "Membanjiri Target Lalu Lintas Jaringan." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Mengubah Data Jadi Tidak Terbaca (Ciphertext)." },
  { "en": "Apa Itu Kunci Kriptografi (Cryptographic Key)?", "id": "Parameter Rahasia Proses Enkripsi/Dekripsi." },
  { "en": "Apa Itu Kriptografi Simetris (Symmetric Cryptography)?", "id": "Kunci Sama Enkripsi Dan Dekripsi." },
  { "en": "Apa Itu Kriptografi Asimetris (Asymmetric Cryptography)?", "id": "Kunci Publik (Enkripsi), Kunci Privat (Dekripsi)." },
  { "en": "Apa Itu Tanda Tangan Digital (Digital Signature)?", "id": "Otentikasi, Integritas Pesan Kunci Asimetris." },
  { "en": "Apa Itu SSL/TLS (Secure Sockets Layer/Transport Layer Security)?", "id": "Protokol Enkripsi Koneksi Web." },
  { "en": "Apa Itu Bahan (Material) Teknik Elektro?", "id": "Konduktor, Isolator, Semikonduktor, Magnetik." },
  { "en": "Apa Sifat Utama Konduktor (Conductor)?", "id": "Resistivitas Rendah, Konduktivitas Tinggi." },
  { "en": "Apa Sifat Utama Isolator (Insulator)?", "id": "Resistivitas Tinggi, Konduktivitas Rendah." },
  { "en": "Apa Sifat Utama Semikonduktor (Semiconductor)?", "id": "Konduktivitas Dapat Dikontrol (Doping)." },
  { "en": "Apa Sifat Utama Bahan Magnetik (Magnetic)?", "id": "Kemampuan Berinteraksi Medan Magnet." },
  { "en": "Apa Itu Permitivitas (Permittivity) (ε)?", "id": "Kemampuan Bahan Simpan Energi Listrik." },
  { "en": "Apa Itu Permeabilitas (Permeability) (μ)?", "id": "Kemampuan Bahan Dukung Medan Magnet." },
  { "en": "Apa Itu Kekuatan Dielektrik (Dielectric Strength)?", "id": "Medan Listrik Maksimum Sebelum Breakdown." },
  { "en": "Apa Itu Piezoelektrik (Piezoelectricity)?", "id": "Listrik Dihasilkan Tekanan Mekanis." },
  { "en": "Apa Itu Termoelektrik (Thermoelectricity) (Efek Seebeck)?", "id": "Tegangan Dihasilkan Perbedaan Suhu." },
  { "en": "Apa Itu Efek Fotolistrik (Photoelectric Effect)?", "id": "Elektron Dipancarkan Cahaya Mengenai Logam." },
  { "en": "Apa Itu Superkonduktivitas (Superconductivity)?", "id": "Resistansi Listrik Nol Suhu Sangat Rendah." },
  { "en": "Apa Itu Rekayasa Perangkat Lunak (Software Engineering)?", "id": "Disiplin Pengembangan Perangkat Lunak." },
  { "en": "Apa Itu Siklus Hidup Pengembangan Perangkat Lunak (SDLC)?", "id": "Tahapan Proses Pengembangan Software." },
  { "en": "Apa Itu Model Air Terjun (Waterfall Model)?", "id": "Model SDLC (Software Development Lifecycle) Linear Sekuensial." },
  { "en": "Apa Itu Model Agile (Agile Model)?", "id": "Model SDLC (Software Development Lifecycle) Iteratif Inkremental." },
  { "en": "Apa Itu Scrum (Scrum)?", "id": "Kerangka Kerja Manajemen Proyek Agile." },
  { "en": "Apa Itu Pemrograman (Programming)?", "id": "Proses Menulis Instruksi Komputer." },
  { "en": "Apa Itu Bahasa Pemrograman (Programming Language)?", "id": "Bahasa Formal Komunikasi Dengan Komputer." },
  { "en": "Apa Itu Kompiler (Compiler)?", "id": "Menerjemahkan Kode Sumber Ke Kode Mesin." },
  { "en": "Apa Itu Interpreter (Interpreter)?", "id": "Eksekusi Kode Sumber Baris Per Baris." },
  { "en": "Apa Itu Struktur Data (Data Structure)?", "id": "Cara Mengorganisasi Data Komputer." },
  { "en": "Apa Itu Algoritma (Algorithm)?", "id": "Langkah-Langkah Solusi Masalah Komputasi." },
  { "en": "Apa Itu Basis Data (Database)?", "id": "Kumpulan Data Terstruktur Tersimpan." },
  { "en": "Apa Itu SQL (Structured Query Language)?", "id": "Bahasa Query Standar Basis Data Relasional." },
  { "en": "Apa Itu Sistem Operasi (Operating System)?", "id": "Perangkat Lunak Pengelola Sumber Daya Komputer." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit)?", "id": "Komponen Elektronik Dalam Satu Chip." },
  { "en": "Apa Bahan Dasar Sirkuit Terpadu?", "id": "Silikon (Silicon) Murni." },
  { "en": "Apa Itu Skala Integrasi (Scale Integration) IC?", "id": "Ukuran Kepadatan Transistor Per Chip." },
  { "en": "Apa Kepanjangan VLSI (Very Large Scale Integration)?", "id": "Integrasi Skala Sangat Besar." },
  { "en": "Apa Itu Fabrikasi (Fabrication) IC?", "id": "Proses Pembuatan Sirkuit Terpadu." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Proses Pencetakan Pola Sirkuit." },
  { "en": "Apa Itu Wafer (Wafer) Silikon?", "id": "Lembaran Tipis Bahan Dasar IC." },
  { "en": "Apa Itu Doping (Doping) Semikonduktor?", "id": "Penambahan Impuritas Mengatur Konduktivitas." },
  { "en": "Apa Itu Semikonduktor Tipe-N (N-Type)?", "id": "Kelebihan Elektron (Pembawa Muatan Negatif)." },
  { "en": "Apa Itu Semikonduktor Tipe-P (P-Type)?", "id": "Kekurangan Elektron (Pembawa Muatan Positif)." },
  { "en": "Apa Itu Lubang (Hole)?", "id": "Kekosongan Elektron Bertindak Muatan Positif." },
  { "en": "Apa Itu Sambungan P-N (P-N Junction)?", "id": "Pertemuan Semikonduktor Tipe-P, Tipe-N." },
  { "en": "Komponen Apa Berbasis Sambungan P-N?", "id": "Dioda Dan Transistor." },
  { "en": "Apa Itu Daerah Deplesi (Depletion Region)?", "id": "Daerah Sekitar Sambungan Kurang Muatan Bebas." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Penyearah Arus Satu Arah." },
  { "en": "Apa Itu Bias Maju (Forward Bias) Dioda?", "id": "Kondisi Dioda Dapat Menghantar Arus." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias) Dioda?", "id": "Kondisi Dioda Menghambat Arus." },
  { "en": "Apa Itu Tegangan Maju (Forward Voltage) (Vf)?", "id": "Jatuh Tegangan Saat Dioda Konduksi." },
  { "en": "Apa Tegangan Maju (Forward Voltage) (Vf) Dioda Silikon?", "id": "Sekitar Nol Koma Tujuh Volt (0.7 V)." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Rangkaian Pengubah Sinyal AC Ke DC." },
  { "en": "Apa Itu Dioda Zener (Zener Diode)?", "id": "Dioda Penstabil Tegangan Bias Mundur." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Yang Memancarkan Cahaya." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Yang Peka Terhadap Cahaya." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Saklar Atau Penguat Terkontrol Arus." },
  { "en": "Apa Tiga Terminal BJT (Bipolar Junction Transistor)?", "id": "Basis (Base), Kolektor (Collector), Emitor (Emitter)." },
  { "en": "Apa Itu Penguatan Arus (Beta/hFE) BJT?", "id": "Rasio Arus Kolektor Terhadap Basis." },
  { "en": "Apa Itu Daerah Aktif (Active Region) BJT?", "id": "Daerah Operasi BJT Sebagai Penguat." },
  { "en": "Apa Itu Daerah Saturasi (Saturation) BJT?", "id": "Daerah Operasi BJT Saklar Tertutup." },
  { "en": "Apa Itu Daerah Cutoff (Cutoff) BJT?", "id": "Daerah Operasi BJT Saklar Terbuka." },
  { "en": "Apa Itu FET (Field Effect Transistor)?", "id": "Transistor Efek Medan Terkontrol Tegangan." },
  { "en": "Apa Tiga Terminal FET (Field Effect Transistor)?", "id": "Gate (Gerbang), Drain (Saluran), Source (Sumber)." },
  { "en": "Apa Keunggulan FET (Field Effect Transistor)?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Jenis FET Gerbang Terisolasi." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Karakteristik Op-Amp (Operational Amplifier) Ideal?", "id": "Gain Tak Hingga, Input Z Tak Hingga." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Menstabilkan Penguatan Op-Amp." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Penguat Membalik Fasa Sinyal Output." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Penguat Tidak Membalik Fasa Sinyal." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Penguat Op-Amp Dengan Penguatan Satu." },
  { "en": "Apa Fungsi Pengikut Tegangan (Voltage Follower)?", "id": "Sebagai Penyangga (Buffer) Impedansi." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Rangkaian Op-Amp Pembanding Dua Tegangan." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Selektif Frekuensi Sinyal." },
  { "en": "Apa Itu Filter Lolos Rendah (Low Pass)?", "id": "Melewatkan Frekuensi Rendah." },
  { "en": "Apa Itu Filter Lolos Tinggi (High Pass)?", "id": "Melewatkan Frekuensi Tinggi." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Periodik." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Blok Dasar Sirkuit Digital." },
  { "en": "Apa Level Logika Biner (Binary Logic)?", "id": "Dua Level (Tinggi/1, Rendah/0)." },
  { "en": "Apa Fungsi Gerbang AND (AND Gate)?", "id": "Output 1 Jika Semua Input 1." },
  { "en": "Apa Fungsi Gerbang OR (OR Gate)?", "id": "Output 1 Jika Ada Input 1." },
  { "en": "Apa Fungsi Gerbang NOT (NOT Gate)?", "id": "Membalikkan Sinyal Input Logika." },
  { "en": "Apa Fungsi Gerbang NAND (NAND Gate)?", "id": "Kebalikan Dari Fungsi Gerbang AND." },
  { "en": "Apa Fungsi Gerbang NOR (NOR Gate)?", "id": "Kebalikan Dari Fungsi Gerbang OR." },
  { "en": "Apa Fungsi Gerbang XOR (XOR Gate)?", "id": "Output 1 Jika Input Berbeda." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Untuk Analisis Logika." },
  { "en": "Apa Itu Sirkuit Kombinasional (Combinational Circuit)?", "id": "Output Tergantung Input Saat Ini." },
  { "en": "Apa Itu Sirkuit Sekuensial (Sequential Circuit)?", "id": "Output Tergantung Input, Keadaan Terdahulu." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Dasar Sirkuit Sekuensial." },
  { "en": "Apa Beda Latch Dan Flip-Flop?", "id": "Latch (Sensitif Level), Flip-Flop (Sensitif Tepi)." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Penyimpan Data." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Pencacah Urutan Pulsa." },
  { "en": "Apa Itu Memori (Memory)?", "id": "Perangkat Penyimpanan Informasi Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Volatil (Data Hilang Tanpa Daya)." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Non-Volatil (Data Tetap)." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "CPU (Central Processing Unit) Dalam Satu Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Bus (Bus) Alamat?", "id": "Jalur Penentu Lokasi Memori/Periferal." },
  { "en": "Apa Itu Bus (Bus) Data?", "id": "Jalur Transfer Data Antar Komponen." },
  { "en": "Apa Itu Bus (Bus) Kontrol?", "id": "Jalur Sinyal Pengatur Operasi Sistem." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Mengubah Besaran Fisik Ke Sinyal Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Mengubah Sinyal Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Alat Pengubah Level Tegangan AC." },
  { "en": "Prinsip Kerja Transformator (Transformer)?", "id": "Induksi Mutual Elektromagnetik." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Pengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Pengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Itu Transmisi (Transmission) HV?", "id": "Penyaluran Daya Jarak Jauh Efisien." },
  { "en": "Apa Itu Distribusi (Distribution) LV?", "id": "Penyaluran Daya Ke Konsumen Akhir." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Pengubah Tegangan Jaringan." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Sistem Terhadap Gangguan." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Otomatis Pelindung Gangguan." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Tanah Untuk Keselamatan." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Aplikasi Semikonduktor Kontrol Daya." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah Daya AC Ke DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Mengubah Daya DC Ke AC." },
  { "en": "Apa Itu Konverter (Converter) DC-DC?", "id": "Mengubah Level Tegangan DC." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Modulasi Lebar Pulsa." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Menggunakan Sinyal Output Mengatur Input." },
  { "en": "Apa Itu Kontroler PID (PID Controller)?", "id": "Kontroler Umpan Balik Industri Umum." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Kontrol Industri Tangguh." },
  { "en": "Apa Itu Komunikasi Data (Data Communication)?", "id": "Transfer Data Antar Perangkat." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komunikasi." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Skala Global." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Kuantifikasi Besaran Fisik." },
  { "en": "Apa Itu Akurasi (Accuracy) Pengukuran?", "id": "Kedekatan Hasil Ukur Nilai Benar." },
  { "en": "Apa Itu Presisi (Precision) Pengukuran?", "id": "Keterulangan Hasil Pengukuran." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu Medan Elektromagnetik (Electromagnetic Field)?", "id": "Medan Kombinasi Listrik Dan Magnet." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Energi Medan Elektromagnetik." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Pancar/Terima Gelombang EM." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Proses Penumpangan Informasi Ke Pembawa." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Proses Ekstraksi Informasi Dari Pembawa." },
  { "en": "Apa Itu Sirkuit (Circuit) Elektronik?", "id": "Jalur Tertutup Aliran Elektron." },
  { "en": "Apa Dua Tipe Dasar Sirkuit?", "id": "Seri Dan Paralel." },
  { "en": "Apa Itu Resistansi (Resistance) Total Seri?", "id": "Jumlah Total Semua Resistor." },
  { "en": "Apa Itu Resistansi (Resistance) Total Paralel?", "id": "Lebih Kecil Dari Resistor Terkecil." },
  { "en": "Apa Itu Arus (Current) Searah?", "id": "Direct Current (DC)." },
  { "en": "Apa Itu Arus (Current) Bolak-Balik?", "id": "Alternating Current (AC)." },
  { "en": "Apa Satuan (Unit) Frekuensi?", "id": "Hertz (Hz)." },
  { "en": "Apa Itu Konduktor (Conductor)?", "id": "Bahan Penghantar Listrik Yang Baik." },
  { "en": "Apa Itu Isolator (Insulator)?", "id": "Bahan Penghambat Listrik Yang Baik." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Bahan Antara Konduktor, Isolator." },
  { "en": "Apa Komponen (Component) Penyimpan Muatan?", "id": "Kapasitor (Capacitor)." },
  { "en": "Apa Komponen (Component) Penyimpan Energi Magnet?", "id": "Induktor (Inductor)." },
  { "en": "Apa Fungsi (Function) Dioda?", "id": "Menyearahkan Arus Listrik (Satu Arah)." },
  { "en": "Apa Fungsi (Function) Transistor?", "id": "Penguat (Amplifier) Atau Saklar (Switch)." },
  { "en": "Apa Itu Penguatan (Gain)?", "id": "Rasio Sinyal Output Terhadap Input." },
  { "en": "Apa Itu Penguat (Amplifier) Operasional?", "id": "Op-Amp (Operational Amplifier)." },
  { "en": "Apa Karakteristik (Characteristic) Op-Amp Ideal?", "id": "Penguatan Tak Terhingga, Impedansi Input Tak Hingga." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Melewatkan Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Lolos Rendah (Low Pass)?", "id": "Melewatkan Frekuensi Rendah." },
  { "en": "Apa Itu Filter Lolos Tinggi (High Pass)?", "id": "Melewatkan Frekuensi Tinggi." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Gelombang Periodik." },
  { "en": "Apa Itu Logika (Logic) Biner?", "id": "Sistem Logika (0 Dan 1)." },
  { "en": "Apa Itu Gerbang (Gate) Logika?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
  { "en": "Gerbang (Gate) Apa Yang Membalik Sinyal?", "id": "Gerbang NOT (Inverter)." },
  { "en": "Gerbang (Gate) Apa Outputnya 1 Jika Semua 1?", "id": "Gerbang AND." },
  { "en": "Gerbang (Gate) Apa Outputnya 1 Jika Ada 1?", "id": "Gerbang OR." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Level Tegangan AC." },
  { "en": "Apa Prinsip (Principle) Kerja Transformator?", "id": "Induksi Mutual Elektromagnetik." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Ubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Ubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Sistem (System) Tenaga Listrik?", "id": "Jaringan Listrik (Pembangkit, Transmisi, Distribusi)." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Aman Ke Potensial Bumi." },
  { "en": "Apa Fungsi (Function) Sekring (Fuse)?", "id": "Pelindung Terhadap Arus Berlebih." },
  { "en": "Apa Itu Pemutus (Breaker) Sirkuit?", "id": "Saklar Pengaman Arus Lebih Otomatis." },
  { "en": "Apa Satuan (Unit) Kapasitansi?", "id": "Farad (F)." },
  { "en": "Apa Satuan (Unit) Induktansi?", "id": "Henry (H)." },
  { "en": "Apa Satuan (Unit) Impedansi?", "id": "Ohm (Ω)." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Non-Resistif Di AC." },
  { "en": "Apa Itu Daya (Power) Nyata?", "id": "Daya Yang Melakukan Kerja (Watt)." },
  { "en": "Apa Itu Daya (Power) Reaktif?", "id": "Daya Medan Magnet/Listrik (VAR)." },
  { "en": "Apa Itu Faktor (Factor) Daya?", "id": "Rasio Daya Nyata, Daya Semu." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Rangkaian Pengubah AC Ke DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Rangkaian Pengubah DC Ke AC." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Perangkat Deteksi Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Perangkat Penggerak Mekanis." },
  { "en": "Apa Itu Sistem (System) Kontrol?", "id": "Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Loop (Loop) Tertutup Kontrol?", "id": "Sistem Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Sinyal Output Kembali Ke Input." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Kuantifikasi Besaran Fisik." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Pengukuran Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Keterulangan Hasil Pengukuran." },
  { "en": "Apa Alat (Tool) Ukur Tegangan?", "id": "Voltmeter (Voltmeter)." },
  { "en": "Apa Alat (Tool) Ukur Arus?", "id": "Amperemeter (Ammeter)." },
  { "en": "Apa Alat (Tool) Ukur Resistansi?", "id": "Ohmmeter (Ohmmeter)." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Menampilkan Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu Medan (Field) Listrik?", "id": "Daerah Pengaruh Muatan Listrik." },
  { "en": "Apa Itu Medan (Field) Magnet?", "id": "Daerah Pengaruh Gaya Magnetik." },
  { "en": "Apa Itu Gelombang (Wave) Elektromagnetik?", "id": "Getaran Medan Listrik, Magnet Merambat." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Informasi Ke Sinyal Pembawa." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Memisahkan Informasi Dari Sinyal Pembawa." },
  { "en": "Apa Itu Jaringan (Network) Komputer?", "id": "Interkoneksi Perangkat Komunikasi Data." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Skala Global." },
  { "en": "Apa Itu Alamat (Address) IP?", "id": "Alamat Logis Perangkat Di Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Perangkat Penghubung Antar Jaringan." },
  { "en": "Apa Itu Elektronika (Electronics) Daya?", "id": "Kontrol Aliran Daya Listrik." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Modulasi Lebar Pulsa." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Kontrol Logika Industri." },
  { "en": "Apa Itu Bahasa (Language) Ladder Logic?", "id": "Bahasa Pemrograman Grafis PLC." },
  { "en": "Apa Itu HMI (Human Machine Interface)?", "id": "Antarmuka Grafis Manusia-Mesin." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pemantauan, Kontrol Terpusat." },
  { "en": "Apa Itu Listrik (Electricity) Statis?", "id": "Akumulasi Muatan Listrik Diam." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Muatan Listrik Statis." },
  { "en": "Apa Itu Resistivitas (Resistivity)?", "id": "Hambatan Spesifik Suatu Material." },
  { "en": "Apa Itu Konduktivitas (Conductivity)?", "id": "Kemudahan Material Hantarkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Dielektrik?", "id": "Bahan Isolator Dalam Kapasitor." },
  { "en": "Apa Itu Kekuatan (Strength) Dielektrik?", "id": "Medan Listrik Maksimum Sebelum Tembus." },
  { "en": "Apa Itu Bahan (Material) Magnetik?", "id": "Bahan Berinteraksi Dengan Medan Magnet." },
  { "en": "Apa Itu Histeresis (Hysteresis) Magnetik?", "id": "Ketergantungan Magnetisasi Riwayat Medan." },
  { "en": "Apa Itu Efek (Effect) Hall?", "id": "Tegangan Timbul Konduktor Medan Magnet." },
  { "en": "Apa Itu Efek (Effect) Termoelektrik?", "id": "Konversi Langsung Panas Ke Listrik." },
  { "en": "Apa Itu Efek (Effect) Piezoelektrik?", "id": "Listrik Dihasilkan Tekanan Mekanis." },
  { "en": "Apa Itu Efek (Effect) Fotolistrik?", "id": "Elektron Dipancarkan Akibat Cahaya." },
  { "en": "Apa Itu Sel (Cell) Surya?", "id": "Perangkat Pengubah Cahaya Matahari Listrik." },
  { "en": "Apa Itu Superkonduktor (Superconductor)?", "id": "Bahan Resistansi Nol Suhu Rendah." },
  { "en": "Apa Itu Sirkuit (Circuit) Terpadu (IC)?", "id": "Rangkaian Elektronik Dalam Satu Chip." },
  { "en": "Apa Itu Fabrikasi (Fabrication) IC?", "id": "Proses Pembuatan Sirkuit Terpadu." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Proses Pencetakan Pola Sirkuit IC." },
  { "en": "Apa Itu Doping (Doping) Semikonduktor?", "id": "Penambahan Impuritas Atur Konduktivitas." },
  { "en": "Apa Itu Sambungan (Junction) P-N?", "id": "Dasar Komponen Semikonduktor Dioda." },
  { "en": "Apa Itu Daerah (Region) Deplesi?", "id": "Area Sekitar Sambungan Kurang Muatan." },
  { "en": "Apa Itu Tegangan (Voltage) Maju (Vf) Dioda?", "id": "Jatuh Tegangan Saat Konduksi." },
  { "en": "Apa Itu Penyearah (Rectifier) Jembatan?", "id": "Empat Dioda Penyearah Gelombang Penuh." },
  { "en": "Apa Itu Dioda (Diode) Zener?", "id": "Dioda Penstabil Tegangan Mundur." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Yang Memancarkan Cahaya." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Sensor Intensitas Cahaya." },
  { "en": "Apa Itu Transistor (Transistor) BJT?", "id": "Penguat/Saklar Terkontrol Arus Basis." },
  { "en": "Apa Itu Transistor (Transistor) FET?", "id": "Penguat/Saklar Terkontrol Tegangan Gate." },
  { "en": "Apa Itu Penguat (Amplifier) Op-Amp?", "id": "Penguat Diferensial Gain Sangat Tinggi." }



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
