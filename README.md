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


    { "en": "Fungsi Menguatkan dan Membalik Fasa Sinyal?", "id": "Penguat Inverting (Inverting Amplifier)." },
    { "en": "Fungsi Menguatkan Tanpa Membalik Fasa Sinyal?", "id": "Penguat Non-Inverting." },
    { "en": "Fungsi Menjumlahkan Beberapa Sinyal Tegangan?", "id": "Penguat Penjumlah (Summing Amplifier)." },
    { "en": "Fungsi Menguatkan Selisih Antara Dua Sinyal?", "id": "Penguat Diferensial." },
    { "en": "Fungsi Menghasilkan Output Integral Dari Input?", "id": "Integrator." },
    { "en": "Fungsi Menghasilkan Output Turunan Dari Input?", "id": "Differentiator." },
    { "en": "Fungsi Membandingkan Dua Tegangan Input?", "id": "Komparator (Comparator)." },
    { "en": "Fungsi Memiliki Penguatan Tegangan Sama Dengan Satu?", "id": "Penyangga Tegangan (Voltage Follower)." },
    { "en": "Fungsi Melewatkan Sinyal Frekuensi Rendah?", "id": "Filter Lolos Bawah (Low-Pass Filter)." },
    { "en": "Fungsi Melewatkan Sinyal Frekuensi Tinggi?", "id": "Filter Lolos Atas (High-Pass Filter)." },
    { "en": "Fungsi Melewatkan Pita Frekuensi Tertentu?", "id": "Filter Lolos Pita (Band-Pass Filter)." },
    { "en": "Fungsi Menolak Pita Frekuensi Tertentu?", "id": "Filter Tolak Pita (Band-Stop Filter)." },
    { "en": "Fungsi Mengubah AC Menjadi DC Berdenyut?", "id": "Penyearah (Rectifier)." },
    { "en": "Fungsi Menyearahkan Setengah Gelombang Sinyal AC?", "id": "Penyearah Setengah Gelombang." },
    { "en": "Fungsi Menyearahkan Seluruh Gelombang Sinyal AC?", "id": "Penyearah Gelombang Penuh." },
    { "en": "Fungsi Meratakan Tegangan DC Berdenyut?", "id": "Filter Kapasitor." },
    { "en": "Fungsi Menjaga Tegangan Output Tetap Stabil?", "id": "Regulator Tegangan." },
    { "en": "Fungsi Mengubah Tegangan DC Menjadi AC?", "id": "Inverter." },
    { "en": "Fungsi Mengubah Tegangan DC Ke DC Lain?", "id": "Konverter DC-DC." },
    { "en": "Fungsi Menaikkan Tegangan DC?", "id": "Boost Converter." },
    { "en": "Fungsi Menurunkan Tegangan DC?", "id": "Buck Converter." },
    { "en": "Fungsi Menaikkan Atau Menurunkan Tegangan DC?", "id": "Buck-Boost Converter." },
    { "en": "Fungsi Menghasilkan Sinyal Periodik?", "id": "Osilator." },
    { "en": "Fungsi Osilator LC Menggunakan Pembagi Kapasitif?", "id": "Osilator Colpitts." },
    { "en": "Fungsi Osilator LC Menggunakan Pembagi Induktif?", "id": "Osilator Hartley." },
    { "en": "Fungsi Osilator Gelombang Sinus Menggunakan Jembatan?", "id": "Osilator Jembatan Wien." },
    { "en": "Fungsi Osilator Menggunakan Pergeseran Fasa RC?", "id": "Osilator Geser Fasa." },
    { "en": "Fungsi Osilator Berbasis Kristal Yang Stabil?", "id": "Osilator Kristal." },
    { "en": "Fungsi Tidak Memiliki Keadaan Stabil (Osilator Persegi)?", "id": "Multivibrator Astable." },
    { "en": "Fungsi Memiliki Satu Keadaan Stabil (Pemicu Pulsa)?", "id": "Multivibrator Monostable." },
    { "en": "Fungsi Memiliki Dua Keadaan Stabil (Penyimpan Bit)?", "id": "Multivibrator Bistable (Flip-Flop)." },
    { "en": "Fungsi Mengubah Sinyal Analog Ke Digital?", "id": "ADC (Analog-to-Digital Converter)." },
    { "en": "Fungsi Mengubah Sinyal Digital Ke Analog?", "id": "DAC (Digital-to-Analog Converter)." },
    { "en": "Fungsi Komparator Dengan Umpan Balik Positif?", "id": "Schmitt Trigger." },
    { "en": "Fungsi Menjumlahkan Dua Buah Bit Biner?", "id": "Half Adder." },
    { "en": "Fungsi Menjumlahkan Tiga Buah Bit Biner?", "id": "Full Adder." },
    { "en": "Fungsi Memilih Satu Dari Banyak Input?", "id": "Multiplexer (MUX)." },
    { "en": "Fungsi Mengirim Satu Input Ke Banyak Output?", "id": "Demultiplexer (DEMUX)." },
    { "en": "Fungsi Mengubah Kode Biner Ke Output Aktif?", "id": "Decoder." },
    { "en": "Fungsi Mengubah Input Aktif Menjadi Kode Biner?", "id": "Encoder." },
    { "en": "Fungsi Menggeser Data Bit Secara Berurutan?", "id": "Register Geser (Shift Register)." },
    { "en": "Fungsi Menghitung Jumlah Pulsa Masukan?", "id": "Pencacah (Counter)." },
    { "en": "Fungsi Mengukur Resistansi Tidak Diketahui?", "id": "Jembatan Wheatstone." },
    { "en": "Fungsi Menggabungkan Dua Transistor Untuk Gain Tinggi?", "id": "Pasangan Darlington (Darlington Pair)." },
    { "en": "Fungsi Rangkaian Resonansi LC Paralel?", "id": "Sirkuit Tangki (Tank Circuit)." },
    { "en": "Fungsi Melindungi Dari Lonjakan Tegangan Induktif?", "id": "Dioda Flyback (Flyback Diode)." },
    { "en": "Fungsi Meredam Lonjakan Tegangan Saat Pensaklaran?", "id": "Sirkuit Snubber." },
    { "en": "Fungsi Proteksi Tegangan Berlebih Hubung Singkat?", "id": "Sirkuit Crowbar." },
    { "en": "Fungsi Mengubah Sinyal Arus Menjadi Tegangan?", "id": "Penguat Transimpedansi." },
    { "en": "Fungsi Mengubah Sinyal Tegangan Menjadi Arus?", "id": "Penguat Transkonduktansi." },
    { "en": "Fungsi Mendeteksi Kapan Sinyal Melewati Nol?", "id": "Detektor Lintasan Nol." },
    { "en": "Fungsi Menangkap Dan Menahan Nilai Puncak Sinyal?", "id": "Detektor Puncak (Peak Detector)." },
    { "en": "Fungsi Menyearahkan Sinyal AC Level Sangat Rendah?", "id": "Penyearah Presisi." },
    { "en": "Fungsi Menggandakan Tegangan DC Input?", "id": "Pengganda Tegangan (Voltage Doubler)." },
    { "en": "Fungsi Menyalin Arus Dari Satu Jalur Ke Lainnya?", "id": "Cermin Arus (Current Mirror)." },
    { "en": "Fungsi Mengunci Fasa Osilator Ke Sinyal Referensi?", "id": "PLL (Phase-Locked Loop)." },
    { "en": "Fungsi Osilator Dengan Frekuensi Terkendali Tegangan?", "id": "VCO (Voltage-Controlled Oscillator)." },
    { "en": "Fungsi Menaikkan Tegangan DC Menggunakan Kapasitor?", "id": "Pompa Muatan (Charge Pump)." },
    { "en": "Fungsi Penguat Diferensial Presisi Tinggi?", "id": "Penguat Instrumentasi." },
    { "en": "Fungsi Memberikan Isolasi Listrik Antar Sirkuit?", "id": "Penguat Isolasi." },
    { "en": "Fungsi Menggeser Level Tegangan DC Suatu Sinyal?", "id": "Penggeser Level (Level Shifter)." },
    { "en": "Fungsi Rangkaian Penstabil Tegangan Menggunakan Dioda?", "id": "Regulator Zener." },
    { "en": "Fungsi Membentuk Pulsa Dengan Durasi Tertentu?", "id": "One-Shot Multivibrator." },
    { "en": "Fungsi Mengonversi Bentuk Gelombang Menjadi Gelombang Persegi?", "id": "Squaring Circuit (Schmitt Trigger)." },
    { "en": "Fungsi Menghasilkan Sinyal Gigi Gergaji?", "id": "Generator Ramp." },
    { "en": "Fungsi Mengontrol Kecerahan Lampu AC?", "id": "Sirkuit Dimmer." },
    { "en": "Fungsi Mengontrol Kecepatan Motor DC?", "id": "Kontroler PWM (Pulse Width Modulation)." },
    { "en": "Fungsi Mengontrol Motor Tiga Fasa?", "id": "Variable Frequency Drive (VFD)." },
    { "en": "Fungsi Penguat Dengan Umpan Balik Arus?", "id": "Current Feedback Amplifier (CFA)." },
    { "en": "Fungsi Saklar Elektromagnetik Untuk Beban Besar?", "id": "Kontaktor." },
    { "en": "Fungsi Rangkaian Penyimpan Energi Sementara?", "id": "Sirkuit RLC." },
    { "en": "Fungsi Penguat Dengan Beban Aktif?", "id": "Beban Aktif (Active Load)." },
    { "en": "Fungsi Menggabungkan Sinyal Audio Dari Banyak Sumber?", "id": "Mixer Audio." },
    { "en": "Fungsi Menyesuaikan Respon Frekuensi Audio (Bass/Treble)?", "id": "Kontrol Nada (Tone Control)." },
    { "en": "Fungsi Membagi Sinyal Audio Ke Woofer/Tweeter?", "id": "Jaringan Crossover." },
    { "en": "Fungsi Menguatkan Sinyal Lemah Dari Piringan Hitam?", "id": "Phono Preamplifier." },
    { "en": "Fungsi Menghilangkan Komponen DC Dari Sinyal?", "id": "Kapasitor Kopling (Coupling Capacitor)." },
    { "en": "Fungsi Menyediakan Jalur Impedansi Rendah Untuk Derau?", "id": "Kapasitor Bypass." },
    { "en": "Fungsi Mengisolasi Derau Antar Bagian Sirkuit?", "id": "Kapasitor Decoupling." },
    { "en": "Fungsi Mencuplik Dan Menahan Tegangan Analog?", "id": "Sample and Hold Circuit." },
    { "en": "Fungsi Menghasilkan Output Logaritma Dari Input?", "id": "Penguat Logaritmik." },
    { "en": "Fungsi Mengubah Sinyal Diferensial Ke Single-Ended?", "id": "Differential to Single-Ended Converter." },
    { "en": "Fungsi Mengubah Sinyal Single-Ended Ke Diferensial?", "id": "Single-Ended to Differential Converter." },
    { "en": "Fungsi Rangkaian Proteksi Dari Arus Berlebih?", "id": "Pemutus Sirkuit (Circuit Breaker)." },
    { "en": "Fungsi Mengurangi Rentang Dinamis Sinyal Audio?", "id": "Kompresor Audio." },
    { "en": "Fungsi Mencegah Sinyal Melebihi Level Tertentu?", "id": "Limiter Audio." },
    { "en": "Fungsi Mematikan Sinyal Audio Di Bawah Ambang?", "id": "Gerbang Derau (Noise Gate)." },
    { "en": "Fungsi Mengukur Impedansi Kompleks?", "id": "Jembatan Pengukur Impedansi." },
    { "en": "Fungsi Menghasilkan Sumber Arus Konstan?", "id": "Sumber Arus (Current Source)." },
    { "en": "Fungsi Mengubah Tegangan Menjadi Frekuensi?", "id": "VFC (Voltage-to-Frequency Converter)." },
    { "en": "Fungsi Mengubah Frekuensi Menjadi Tegangan?", "id": "FVC (Frequency-to-Voltage Converter)." },
    { "en": "Fungsi Mengalikan Dua Sinyal Analog?", "id": "Pengali Analog (Analog Multiplier)." },
    { "en": "Fungsi Menghasilkan Nilai RMS Sinyal AC?", "id": "RMS-to-DC Converter." },
    { "en": "Fungsi Saklar Analog Berkecepatan Tinggi?", "id": "Saklar CMOS (CMOS Switch)." },
    { "en": "Fungsi Mengonversi Impedansi?", "id": "Konverter Impedansi." },
    { "en": "Fungsi Mensimulasikan Induktor?", "id": "Gyrator." },
    { "en": "Fungsi Rangkaian Delay Line Analog?", "id": "Bucket-Brigade Device (BBD)." },
    { "en": "Fungsi Mencocokkan Impedansi Saluran Seimbang-Tak Seimbang?", "id": "Balun." },
    { "en": "Fungsi Menggeser Fasa Sinyal Tanpa Mengubah Amplitudo?", "id": "Filter All-Pass." },
    { "en": "Fungsi Mengukur Suhu Menggunakan Resistansi?", "id": "Jembatan RTD (Resistance Temperature Detector)." },
    { "en": "Fungsi Rangkaian Pengaman Hubung Terbalik Polaritas?", "id": "Dioda Proteksi Polaritas." },
    { "en": "Fungsi Topologi Filter Aktif Populer?", "id": "Topologi Sallen-Key." },
    { "en": "Fungsi Filter Dengan Respon Passband Paling Datar?", "id": "Filter Butterworth." },
    { "en": "Fungsi Filter Dengan Roll-off Tajam Tapi Beriak?", "id": "Filter Chebyshev." },
    { "en": "Fungsi Filter Dengan Respon Fasa Linear?", "id": "Filter Bessel." },
    { "en": "Fungsi Filter Universal (LP/HP/BP Output)?", "id": "Filter State-Variable." },
    { "en": "Fungsi Rangkaian Penguat Dengan Bandwidth Lebar?", "id": "Sirkuit Cascode." },
    { "en": "Fungsi Inti Dari Tahap Input Op-Amp?", "id": "Pasangan Ekor Panjang (Long-Tailed Pair)." },
    { "en": "Fungsi Cermin Arus Dengan Kinerja Tinggi?", "id": "Cermin Arus Wilson." },
    { "en": "Fungsi Menghasilkan Referensi Tegangan Sangat Stabil?", "id": "Referensi Bandgap." },
    { "en": "Fungsi Kontrol Umpan Balik Proporsional-Integral-Derivatif?", "id": "Kontroler PID." },
    { "en": "Fungsi Menghasilkan Gelombang Persegi Tanpa Input?", "id": "Osilator Relaksasi." },
    { "en": "Fungsi Menghasilkan Pulsa Tunggal Saat Dipicu?", "id": "Pemicu Monostable (Monostable Trigger)." },
    { "en": "Fungsi Mengisolasi Dua Bagian Sirkuit Menggunakan Cahaya?", "id": "Optocoupler (Opto-isolator)." },
    { "en": "Fungsi Mengubah Sinyal Listrik Menjadi Cahaya?", "id": "Driver LED (Light Emitting Diode)." },
    { "en": "Fungsi Mengubah Cahaya Menjadi Sinyal Listrik?", "id": "Penguat Fotodioda." },
    { "en": "Fungsi Konverter DC-DC Terisolasi Penyimpan Energi?", "id": "Konverter Flyback." },
    { "en": "Fungsi Konverter DC-DC Terisolasi Transfer Energi?", "id": "Konverter Forward." },
    { "en": "Fungsi Konverter Menggunakan Dua Transistor?", "id": "Konverter Setengah Jembatan." },
    { "en": "Fungsi Konverter Menggunakan Empat Transistor?", "id": "Konverter Jembatan Penuh." },
    { "en": "Fungsi Rangkaian Penyimpan Satu Bit Memori?", "id": "Flip-Flop." },
    { "en": "Fungsi Flip-Flop Yang Sensitif Terhadap Tepi Clock?", "id": "Edge-Triggered Flip-Flop." },
    { "en": "Fungsi Flip-Flop Yang Sensitif Terhadap Level Clock?", "id": "Level-Triggered Latch." },
    { "en": "Fungsi Mendeteksi Tegangan Dalam Rentang Tertentu?", "id": "Komparator Jendela (Window Comparator)." },
    { "en": "Fungsi Menghasilkan Nilai Absolut Dari Sinyal?", "id": "Penguat Nilai Absolut." },
    { "en": "Fungsi Mengunci Dan Mengikuti Sinyal FM?", "id": "Demodulator PLL (Phase-Locked Loop)." },
    { "en": "Fungsi Mengalikan Dua Sinyal (Modulasi Amplitudo)?", "id": "Modulator Seimbang (Balanced Modulator)." },
    { "en": "Fungsi Penguat Arus Dengan Output Arus?", "id": "OTA (Operational Transconductance Amplifier)." },
    { "en": "Fungsi Penguat Dengan Gain Terkendali Tegangan?", "id": "VCA (Voltage-Controlled Amplifier)." },
    { "en": "Fungsi Pelindung Lonjakan Tegangan Cepat?", "id": "Dioda TVS (Transient Voltage Suppressor)." },
    { "en": "Fungsi Pelindung Tegangan Berlebih Berbasis Keramik?", "id": "Varistor Oksida Logam (MOV)." },
    { "en": "Fungsi Menghilangkan Derau Common-Mode Dari Jalur?", "id": "Common-Mode Choke." },
    { "en": "Fungsi Penguat Awal Dengan Derau Sangat Rendah?", "id": "LNA (Low-Noise Amplifier)." },
    { "en": "Fungsi Mencampur Dua Sinyal Frekuensi?", "id": "Mixer (Pencampur)." },
    { "en": "Fungsi Menstabilkan Amplitudo Osilator?", "id": "Sirkuit AGC (Automatic Gain Control)." },
    { "en": "Fungsi Menghasilkan Dua Output Beda Fasa 180°?", "id": "Pemisah Fasa (Phase Splitter)." },
    { "en": "Fungsi Rangkaian Penunda Waktu Sederhana?", "id": "Sirkuit Pewaktu RC." },
    { "en": "Fungsi Membangkitkan Pulsa Clock Untuk Sirkuit Digital?", "id": "Generator Clock." },
    { "en": "Fungsi Rangkaian Untuk Mengukur Arus Tinggi?", "id": "Penguat Shunt Arus." },
    { "en": "Fungsi Mengukur Arus Di Sisi Positif Catu Daya?", "id": "High-Side Current Sense Amplifier." },
    { "en": "Fungsi Mengukur Arus Di Sisi Negatif Catu Daya?", "id": "Low-Side Current Sense Amplifier." },
    { "en": "Fungsi Menguatkan Sinyal Dari Sensor Piezoelektrik?", "id": "Penguat Muatan (Charge Amplifier)." },
    { "en": "Fungsi Rangkaian Dengan Resistansi Negatif?", "id": "Konverter Impedansi Negatif (NIC)." },
    { "en": "Fungsi Membagi Tegangan Input Secara Tepat?", "id": "Pembagi Tegangan (Voltage Divider)." },
    { "en": "Fungsi Membagi Arus Input?", "id": "Pembagi Arus (Current Divider)." },
    { "en": "Fungsi Saklar Yang Dioperasikan Medan Magnet?", "id": "Saklar Reed (Reed Switch)." },
    { "en": "Fungsi Saklar Solid-State Untuk Beban AC?", "id": "TRIAC (Triode for Alternating Current)." },
    { "en": "Fungsi Pemicu Untuk TRIAC?", "id": "DIAC (Diode for Alternating Current)." },
    { "en": "Fungsi Dioda Terkendali Untuk Pensaklaran Daya?", "id": "SCR (Silicon Controlled Rectifier)." },
    { "en": "Fungsi Rangkaian Otomatis Beralih Ke Baterai?", "id": "Sirkuit Catu Daya Tak Terputus (UPS)." },
    { "en": "Fungsi Mengisi Ulang Baterai Secara Aman?", "id": "Sirkuit Pengisi Baterai." },
    { "en": "Fungsi Melindungi Baterai Dari Pengisian Berlebih?", "id": "Sirkuit Proteksi Baterai." },
    { "en": "Fungsi Menyesuaikan Impedansi Sumber Dan Beban?", "id": "Jaringan Penyesuai Impedansi." },
    { "en": "Fungsi Mengubah Level Logika Tegangan?", "id": "Penerjemah Level Logika." },
    { "en": "Fungsi Menghilangkan Pantulan Kontak Saklar Mekanis?", "id": "Sirkuit Debouncer." },
    { "en": "Fungsi Mereset Sistem Jika Perangkat Lunak Macet?", "id": "Pewaktu Watchdog (Watchdog Timer)." },
    { "en": "Fungsi Menguatkan Sinyal Dari Jembatan Sensor?", "id": "Penguat Jembatan (Bridge Amplifier)." },
    { "en": "Fungsi Menggandakan Frekuensi Sinyal Input?", "id": "Pengganda Frekuensi (Frequency Doubler)." },
    { "en": "Fungsi Membagi Frekuensi Sinyal Input?", "id": "Pembagi Frekuensi (Frequency Divider)." },
    { "en": "Fungsi Membandingkan Fasa Dari Dua Sinyal?", "id": "Detektor Fasa (Phase Detector)." },
    { "en": "Fungsi Konverter DC-DC Terisolasi Efisiensi Tinggi?", "id": "Konverter Resonansi LLC." },
    { "en": "Fungsi Menyediakan Beberapa Output Tegangan Terisolasi?", "id": "Catu Daya Multi-Output." },
    { "en": "Fungsi Mengoreksi Faktor Daya Beban?", "id": "Sirkuit PFC (Power Factor Correction)." },
    { "en": "Fungsi Membatasi Arus Saat Awal Dihidupkan?", "id": "Pembatas Arus Inrush." },
    { "en": "Fungsi Mengaktifkan Beban Secara Perlahan?", "id": "Sirkuit Soft-Start." },
    { "en": "Fungsi Menggerakkan Gerbang MOSFET Dengan Cepat?", "id": "Driver Gerbang (Gate Driver)." },
    { "en": "Fungsi Menggerakkan Motor DC Dua Arah?", "id": "Jembatan-H (H-Bridge)." },
    { "en": "Fungsi Menggerakkan Motor Stepper?", "id": "Driver Motor Stepper." },
    { "en": "Fungsi Menggerakkan Motor Servo?", "id": "Kontroler Servo." },
    { "en": "Fungsi Menghasilkan Tegangan Negatif Dari Positif?", "id": "Generator Tegangan Negatif." },
    { "en": "Fungsi Menguatkan Sinyal Head Tape Magnetik?", "id": "Preamplifier Pita Magnetik." },
    { "en": "Fungsi Ekualisasi Standar Untuk Piringan Hitam?", "id": "Sirkuit Ekualisasi RIAA." },
    { "en": "Fungsi Rangkaian Penguat Dengan Gain Terkendali Digital?", "id": "PGA (Programmable Gain Amplifier)." },
    { "en": "Fungsi Menghasilkan Sinyal Video Komposit?", "id": "Encoder Video." },
    { "en": "Fungsi Memisahkan Sinyal Luma Dan Chroma?", "id": "Decoder Video." },
    { "en": "Fungsi Sinkronisasi Sinyal Video?", "id": "Pemisah Sinkronisasi (Sync Separator)." },
    { "en": "Fungsi Menggerakkan Tabung Sinar Katoda?", "id": "Sirkuit Defleksi CRT (Cathode Ray Tube)." },
    { "en": "Fungsi Menghasilkan Tegangan Sangat Tinggi?", "id": "Transformator Flyback (FBT)." },
    { "en": "Fungsi Mengemulasi Perilaku Komponen Lain?", "id": "Sirkuit Sintetis." },
    { "en": "Fungsi Mengukur Kapasitansi?", "id": "Jembatan Kapasitansi." },
    { "en": "Fungsi Mengukur Induktansi?", "id": "Jembatan Induktansi (Jembatan Maxwell)." },
    { "en": "Fungsi Menghasilkan Referensi Arus Stabil?", "id": "Referensi Arus (Current Reference)." },
    { "en": "Fungsi Membatasi Amplitudo Sinyal Secara Simetris?", "id": "Sirkuit Clipper Simetris." },
    { "en": "Fungsi Menjepit Sinyal Ke Level DC Tertentu?", "id": "Sirkuit Clamper." },
    { "en": "Fungsi Rangkaian Penunda Waktu Analog?", "id": "Garis Penunda (Delay Line)." },
    { "en": "Fungsi Menguatkan Sinyal Audio Untuk Headphone?", "id": "Penguat Headphone." },
    { "en": "Fungsi Melindungi Speaker Dari Kerusakan DC?", "id": "Sirkuit Proteksi Speaker." },
    { "en": "Fungsi Penyearah Gelombang Penuh Menggunakan Jembatan?", "id": "Penyearah Jembatan (Bridge Rectifier)." },
    { "en": "Fungsi Mengatur Kecerahan LED Dengan Arus Konstan?", "id": "Driver LED Arus Konstan." },
    { "en": "Fungsi Menyediakan Daya Ke Mikrofon Kondensor?", "id": "Catu Daya Phantom." },
    { "en": "Fungsi Mengubah Sinyal Instrumen Ke Sinyal Seimbang?", "id": "Kotak DI (Direct Injection)." },
    { "en": "Fungsi Menghilangkan Derau Akibat Ground Loop?", "id": "Isolator Ground Loop." },
    { "en": "Fungsi Membagi Sinyal Gitar Ke Banyak Penguat?", "id": "Pembagi Sinyal Gitar." },
    { "en": "Fungsi Sirkuit Efek Gitar Fuzz?", "id": "Sirkuit Fuzz Face." },
    { "en": "Fungsi Sirkuit Efek Gitar Wah-wah?", "id": "Sirkuit Wah Pedal." },
    { "en": "Fungsi Rangkaian Pengontrol Filter Dengan Tegangan?", "id": "VCF (Voltage-Controlled Filter)." },
    { "en": "Fungsi Rangkaian Amplifikasi Terkendali Tegangan?", "id": "VCA (Voltage-Controlled Amplifier)." },
    { "en": "Fungsi Rangkaian Pembangkit Envelope Sinyal?", "id": "Generator Envelope (ADSR)." },
    { "en": "Fungsi Osilator Frekuensi Rendah Untuk Modulasi?", "id": "LFO (Low-Frequency Oscillator)." },
    { "en": "Fungsi Penguat Dasar Menggunakan Transistor BJT?", "id": "Penguat Common-Emitter." },
    { "en": "Fungsi Penguat BJT Sebagai Penyangga Arus?", "id": "Penguat Common-Base." },
    { "en": "Fungsi Penguat BJT Sebagai Penyangga Tegangan?", "id": "Penguat Common-Collector (Emitter Follower)." },
    { "en": "Fungsi Konverter DC-DC Dengan Polaritas Terbalik?", "id": "Konverter Cuk." },
    { "en": "Fungsi Konverter DC-DC Non-inverting Buck-Boost?", "id": "Konverter SEPIC." },
    { "en": "Fungsi Penyearah Presisi Untuk Sinyal Gelombang Penuh?", "id": "Penguat Nilai Absolut." },
    { "en": "Fungsi Menampilkan Level Sinyal Audio Rata-rata?", "id": "Sirkuit VU Meter." },
    { "en": "Fungsi Topologi Kontrol Nada Bass/Treble Klasik?", "id": "Kontrol Nada Baxandall." },
    { "en": "Fungsi Mendeteksi Sampul Sinyal Modulasi AM?", "id": "Detektor Sampul (Envelope Detector)." },
    { "en": "Fungsi Mendeteksi Pergeseran Frekuensi Sinyal FM?", "id": "Diskriminator Frekuensi." },
    { "en": "Fungsi Antena Dengan Penguat Terintegrasi?", "id": "Antena Aktif." },
    { "en": "Fungsi Encoder Yang Mengutamakan Input Prioritas Tertinggi?", "id": "Priority Encoder." },
    { "en": "Fungsi Register Geser Dengan Output Terhubung Ke Input?", "id": "Ring Counter." },
    { "en": "Fungsi Ring Counter Dengan Feedback Terbalik?", "id": "Johnson Counter." },
    { "en": "Fungsi Mematikan Sistem Saat Tegangan Terlalu Rendah?", "id": "Sirkuit UVLO (Under-Voltage Lockout)." },
    { "en": "Fungsi Mematikan Sistem Saat Tegangan Terlalu Tinggi?", "id": "Sirkuit OVLO (Over-Voltage Lockout)." },
    { "en": "Fungsi Mengizinkan Papan Sirkuit Dimasukkan Saat Hidup?", "id": "Kontroler Hot-Swap." },
    { "en": "Fungsi Pembatasan Arus Yang Menurunkan Tegangan?", "id": "Pembatas Arus Foldback." },
    { "en": "Fungsi Sekring Yang Dapat Pulih Setelah Trip?", "id": "Sekring PTC (Polymeric PTC Resettable Fuse)." },
    { "en": "Fungsi Mengubah Kode BCD Ke Tampilan 7-Segmen?", "id": "BCD to 7-Segment Decoder." },
    { "en": "Fungsi Menghasilkan Tegangan Output Proporsional Suhu?", "id": "Sensor Suhu Analog." },
    { "en": "Fungsi Menguatkan Sinyal AC Dan Menolak DC?", "id": "Penguat Kopling-AC." },
    { "en": "Fungsi Menghilangkan Offset DC Menggunakan Umpan Balik?", "id": "DC Servo." },
    { "en": "Fungsi Rangkaian Yang Mengalikan Sinyal Analog?", "id": "Gilbert Cell Mixer." },
    { "en": "Fungsi Osilator Yang Menghasilkan Dua Fasa (Quadrature)?", "id": "Osilator Quadrature." },
    { "en": "Fungsi Menghasilkan Sinyal Gigi Gergaji?", "id": "Generator Gelombang Gigi Gergaji." },
    { "en": "Fungsi Penguat Dengan Umpan Balik Positif?", "id": "Komparator Dengan Histeresis." },
    { "en": "Fungsi Sirkuit Yang Membagi Tegangan Referensi?", "id": "Jaringan Pembagi String." },
    { "en": "Fungsi ADC Sangat Cepat Menggunakan Banyak Komparator?", "id": "Flash ADC." },
    { "en": "Fungsi ADC Berbasis Pencarian Biner?", "id": "SAR ADC (Successive Approximation Register)." },
    { "en": "Fungsi ADC Resolusi Tinggi Dengan Oversampling?", "id": "Delta-Sigma ADC." },
    { "en": "Fungsi DAC Menggunakan Jaringan Resistor R-2R?", "id": "R-2R Ladder DAC." },
    { "en": "Fungsi Rangkaian Penstabil Tegangan Menggunakan Transistor?", "id": "Regulator Tegangan Seri." },
    { "en": "Fungsi Regulator Dengan Elemen Paralel Dengan Beban?", "id": "Regulator Tegangan Shunt." },
    { "en": "Fungsi Saklar Analog Solid-State?", "id": "Gerbang Transmisi CMOS." },
    { "en": "Fungsi Mengontrol Kecerahan LED Secara Digital?", "id": "Driver LED PWM." },
    { "en": "Fungsi Mengukur Sinyal Sangat Kecil Di Tengah Derau?", "id": "Lock-In Amplifier." },
    { "en": "Fungsi Menghasilkan Arus Output Konstan?", "id": "Widlar Current Source." },
    { "en": "Fungsi Penyearah Dengan Jatuh Tegangan Sangat Rendah?", "id": "Penyearah Sinkron." },
    { "en": "Fungsi Pembangkit Sinyal Noise Putih?", "id": "Generator Derau Zener." },
    { "en": "Fungsi Menghasilkan Sinyal Sinus Dari Gelombang Persegi?", "id": "Filter Pasif Orde Tinggi." },
    { "en": "Fungsi Membatasi Sinyal Tanpa Menggunakan Dioda?", "id": "Sirkuit Pembatas Berbasis Op-Amp." },
    { "en": "Fungsi Menguatkan Beda Tegangan Antara Dua Titik?", "id": "Penguat Diferensial Seimbang." },
    { "en": "Fungsi Mengukur Arus AC Tanpa Kontak?", "id": "Current Clamp (Trafo Arus)." },
    { "en": "Fungsi Sirkuit Penyimpan Nilai Puncak Sinyal RF?", "id": "Detektor Dioda." },
    { "en": "Fungsi Mengemulasi Resistor Negatif Tergantung Frekuensi?", "id": "FDNR (Frequency-Dependent Negative Resistor)." },
    { "en": "Fungsi Topologi Filter Biquadratic?", "id": "Filter Biquad." },
    { "en": "Fungsi Osilator Yang Amplitudonya Stabil?", "id": "Osilator Amplitudo Stabil." },
    { "en": "Fungsi Menggandakan Tegangan Dengan Dioda-Kapasitor?", "id": "Pengganda Tegangan Cockcroft-Walton." },
    { "en": "Fungsi Osilator Dengan Dua Jaringan RC-T?", "id": "Osilator Twin-T." },
    { "en": "Fungsi Penyearah Presisi Respon Sangat Cepat?", "id": "Penyearah Presisi Berbasis Komparator." },
    { "en": "Fungsi Membatasi Arus Ke Nilai Aman?", "id": "Sirkuit Pembatas Arus." },
    { "en": "Fungsi Memberi Penundaan Waktu Yang Dapat Diatur?", "id": "Timer 555 Monostable." },
    { "en": "Fungsi Menghasilkan Frekuensi Clock Yang Dapat Diatur?", "id": "Timer 555 Astable." },
    { "en": "Fungsi Menggeser Fase Sinyal Tepat 90 Derajat?", "id": "Quadrature Phase Shifter." },
    { "en": "Fungsi Mengukur Kelembaban Udara Secara Elektronik?", "id": "Hygrometer Kapasitif." },
    { "en": "Fungsi Mengukur Tekanan Menggunakan Sensor Piezoelektrik?", "id": "Transduser Tekanan Piezoelektrik." },
    { "en": "Fungsi Mengukur Laju Aliran Cairan?", "id": "Flowmeter Ultrasonik." },
    { "en": "Fungsi Mengisolasi Jalur Sinyal Frekuensi Tinggi?", "id": "Isolator RF." },
    { "en": "Fungsi Mengarahkan Sinyal RF Searah Sirkulasi?", "id": "Sirkulator RF." },
    { "en": "Fungsi Pembangkit Sinyal Dengan Bentuk Apapun?", "id": "AWG (Arbitrary Waveform Generator)." },
    { "en": "Fungsi Menguatkan Sinyal Dan Menggerakkan Koil?", "id": "Driver Solenoida." },
    { "en": "Fungsi Mengontrol Posisi Sudut Motor Secara Presisi?", "id": "Driver Motor Servo." },
    { "en": "Fungsi Menghasilkan Medan Magnet Kuat Sesaat?", "id": "Sirkuit Pemicu Koil." },
    { "en": "Fungsi Penguat Audio Berdaya Sangat Tinggi?", "id": "Penguat Daya Kelas-D." },
    { "en": "Fungsi Catu Daya Dengan Efisiensi Sangat Tinggi?", "id": "SMPS (Switch-Mode Power Supply)." },
    { "en": "Fungsi Mengukur Sinyal Optik Lemah?", "id": "Penerima Optik APD (Avalanche Photodiode)." },
    { "en": "Fungsi Memodulasi Sinar Laser?", "id": "Driver Dioda Laser." },
    { "en": "Fungsi Menstabilkan Suhu Dioda Laser?", "id": "Kontroler TEC (Thermoelectric Cooler)." },
    { "en": "Fungsi Menghasilkan Pulsa Laser Sangat Pendek?", "id": "Laser Mode-Locked." },
    { "en": "Fungsi Memperkuat Pulsa Laser?", "id": "Penguat Serat Optik." },
    { "en": "Fungsi Menghasilkan Tegangan Output Terisolasi?", "id": "Konverter Flyback Terisolasi." },
    { "en": "Fungsi Mengukur Resistansi Isolasi Sangat Tinggi?", "id": "Megohmmeter." },
    { "en": "Fungsi Menghasilkan Tegangan Tinggi Untuk Pengujian?", "id": "Generator Uji Hipot." },
    { "en": "Fungsi Menghasilkan Tegangan Referensi Presisi?", "id": "Referensi Tegangan Kelvin." },
    { "en": "Fungsi Saklar Analog Dengan Distorsi Rendah?", "id": "Saklar Relai Solid-State." },
    { "en": "Fungsi Mengurangi Derau Akibat Getaran Mekanis?", "id": "Filter Anti-Getaran (Anti-Vibration)." },
    { "en": "Fungsi Mengubah Impedansi Speaker?", "id": "Transformator Pencocok Impedansi Audio." },
    { "en": "Fungsi Mengontrol Banyak LED Secara Individual?", "id": "Driver LED Matriks." },
    { "en": "Fungsi Menguatkan Sinyal Dari Sensor Strain Gauge?", "id": "Penguat Strain Gauge." },
    { "en": "Fungsi Mengubah Posisi Mekanis Menjadi Sinyal Listrik?", "id": "LVDT (Linear Variable Differential Transformer)." },
    { "en": "Fungsi Membandingkan Dua Frekuensi?", "id": "Pencacah Frekuensi Diferensial." },
    { "en": "Fungsi Mengukur Waktu Antara Dua Pulsa?", "id": "Intervalometer Waktu." },
    { "en": "Fungsi Sirkuit Yang Mengunci Data Saat Diperintahkan?", "id": "Latch Transparan." },
    { "en": "Fungsi Memori Penyimpan Data Sementara?", "id": "RAM (Random Access Memory)." },
    { "en": "Fungsi Memori Penyimpan Data Permanen?", "id": "ROM (Read-Only Memory)." },
    { "en": "Fungsi Mengontrol Aliran Data Dalam Sistem Digital?", "id": "Unit Kontrol." },
    { "en": "Fungsi Melakukan Operasi Aritmatika Dan Logika?", "id": "ALU (Arithmetic Logic Unit)." },
    { "en": "Fungsi Menghasilkan Pola Waktu Yang Kompleks?", "id": "Sequencer Logika." },
    { "en": "Fungsi Mendeteksi Urutan Bit Tertentu?", "id": "Detektor Urutan." },
    { "en": "Fungsi Memeriksa Kesalahan Data Dengan Paritas?", "id": "Generator/Pemeriksa Paritas." },
    { "en": "Fungsi Memeriksa Kesalahan Data Dengan Metode CRC?", "id": "Sirkuit CRC (Cyclic Redundancy Check)." },
    { "en": "Fungsi Sirkuit Untuk Menghitung Akar Kuadrat?", "id": "Kalkulator Akar Kuadrat Analog." },
    { "en": "Fungsi Sirkuit Dengan Output Tergantung Keadaan Sebelumnya?", "id": "Mesin Keadaan Terbatas (Finite State Machine)." },
    { "en": "Fungsi Rangkaian Penunda Pulsa Digital?", "id": "Generator Penunda Pulsa." },
    { "en": "Fungsi Osilator Digital Berbasis Gerbang Logika?", "id": "Osilator Cincin (Ring Oscillator)." },
    { "en": "Fungsi Menghasilkan Suara Musik Elektronik?", "id": "Chip Synthesizer Suara." },
    { "en": "Fungsi Mengontrol Akses Ke Memori Bersama?", "id": "Arbiter Memori." },
    { "en": "Fungsi Mengubah Format Data Serial Ke Paralel?", "id": "Konverter Serial-ke-Paralel." },
    { "en": "Fungsi Mengubah Format Data Paralel Ke Serial?", "id": "Konverter Paralel-ke-Serial." },
    { "en": "Fungsi Menggeser Bit Dalam Satu Siklus Clock?", "id": "Barrel Shifter." },
    { "en": "Fungsi Menjumlahkan Banyak Bilangan Secara Cepat?", "id": "Wallace Tree Multiplier." },
    { "en": "Fungsi Menerapkan Koreksi Kecerahan Non-Linear?", "id": "Sirkuit Koreksi Gamma." },
    { "en": "Fungsi Mengontrol Pembelokan Berkas Elektron Vertikal?", "id": "Sirkuit Defleksi Vertikal." },
    { "en": "Fungsi Mengontrol Pembelokan Berkas Elektron Horizontal?", "id": "Sirkuit Defleksi Horizontal." },
    { "en": "Fungsi Menghasilkan Output Rasio Logaritmik Input?", "id": "Log Ratio Amplifier." },
    { "en": "Fungsi Penguat Push-Pull Tanpa Trafo Output?", "id": "Penguat OTL (Output Transformerless)." },
    { "en": "Fungsi Penguat Push-Pull Tanpa Kapasitor Output?", "id": "Penguat OCL (Output Capacitorless)." },
    { "en": "Fungsi Konverter DC-DC Berbasis Induktor-Kapasitor?", "id": "Konverter Zeta." },
    { "en": "Fungsi Filter Dengan Roll-off Paling Tajam?", "id": "Filter Eliptik (Cauer Filter)." },
    { "en": "Fungsi Filter Menggunakan Kapasitor Dan Saklar?", "id": "Filter Kapasitor-Saklar." },
    { "en": "Fungsi Mengekstrak Sinyal Clock Dari Data?", "id": "Sirkuit Pemulihan Clock (Clock Recovery)." },
    { "en": "Fungsi Menginterpolasi Fasa Antara Dua Sinyal Clock?", "id": "Phase Interpolator." },
    { "en": "Fungsi Sel Memori Statis Menggunakan Flip-Flop?", "id": "Sel SRAM (Static RAM)." },
    { "en": "Fungsi Sel Memori Dinamis Menggunakan Kapasitor?", "id": "Sel DRAM (Dynamic RAM)." },
    { "en": "Fungsi Menguatkan Sinyal AC Lemah Secara Sinkron?", "id": "Detektor Homodyne." },
    { "en": "Fungsi Mendeteksi Tegangan Melintasi Resistor Shunt?", "id": "Current Sense Amplifier." },
    { "en": "Fungsi Mengontrol Arus Melalui Beban?", "id": "Regulator Arus." },
    { "en": "Fungsi Menghasilkan Tegangan Output Terprogram?", "id": "Catu Daya Terprogram." },
    { "en": "Fungsi Menghasilkan Pulsa Sangat Sempit?", "id": "Generator Pulsa Jarum." },
    { "en": "Fungsi Menghasilkan Penundaan Waktu Presisi?", "id": "Generator Penundaan Digital." },
    { "en": "Fungsi Osilator Dengan Amplitudo Terkendali?", "id": "Osilator Terkontrol Amplitudo." },
    { "en": "Fungsi Menghasilkan Tegangan Tinggi Dari Rendah?", "id": "Generator Marx." },
    { "en": "Fungsi Mengukur Resistansi Rendah Secara Akurat?", "id": "Koneksi Kelvin (Empat Kawat)." },
    { "en": "Fungsi Membatalkan Kapasitansi Input Penguat?", "id": "Sirkuit Netralisasi." },
    { "en": "Fungsi Mengkompensasi Drift Termal?", "id": "Sirkuit Kompensasi Suhu." },
    { "en": "Fungsi Menguatkan Dan Mencampur Sinyal RF?", "id": "Penguat Mixer." },
    { "en": "Fungsi Mengubah Impedansi Tanpa Penguatan?", "id": "Pengikut Impedansi (Impedance Follower)." },
    { "en": "Fungsi Mengubah Fase Sinyal Secara Variabel?", "id": "Phase Shifter Variabel." },
    { "en": "Fungsi Menghasilkan Sinyal Noise Dengan Distribusi Gaussian?", "id": "Generator Derau Gaussian." },
    { "en": "Fungsi Rangkaian Yang Menguji Sirkuit Logika?", "id": "Tester Logika." },
    { "en": "Fungsi Mengemulasi Resistor Dengan Nilai Negatif?", "id": "Konverter Impedansi Negatif." },
    { "en": "Fungsi Menguatkan Sinyal Dari Sensor Cahaya?", "id": "Penguat Fotosensor." },
    { "en": "Fungsi Menggerakkan Layar LCD?", "id": "Driver LCD." },
    { "en": "Fungsi Menggerakkan Layar VFD (Vacuum Fluorescent Display)?", "id": "Driver VFD." },
    { "en": "Fungsi Menghasilkan Frekuensi Referensi Yang Akurat?", "id": "Osilator Kristal Terkontrol Oven (OCXO)." },
    { "en": "Fungsi Osilator Kristal Dengan Kompensasi Suhu?", "id": "Osilator TCXO." },
    { "en": "Fungsi Menggerakkan Motor Brushless DC?", "id": "Kontroler BLDC." },
    { "en": "Fungsi Mengontrol Torsi Motor Listrik?", "id": "Kontroler Torsi." },
    { "en": "Fungsi Melakukan Pengereman Motor Dengan Energi Balik?", "id": "Pengereman Regeneratif." },
    { "en": "Fungsi Mengukur Posisi Sudut Poros?", "id": "Resolver." },
    { "en": "Fungsi Mengubah Sinyal Resolver Ke Digital?", "id": "Resolver-to-Digital Converter." },
    { "en": "Fungsi Sirkuit Yang Mengunci Data?", "id": "Latch." },
    { "en": "Fungsi Menghasilkan Pulsa Tunggal Dari Input?", "id": "Pulse Shaper." },
    { "en": "Fungsi Memperpanjang Durasi Pulsa?", "id": "Pulse Stretcher." },
    { "en": "Fungsi Menggabungkan Beberapa Sinyal Digital?", "id": "Gerbang Logika." },
    { "en": "Fungsi Mengubah Level Logika TTL Ke CMOS?", "id": "Penerjemah Level TTL-ke-CMOS." },
    { "en": "Fungsi Mengubah Level Logika CMOS Ke TTL?", "id": "Penerjemah Level CMOS-ke-TTL." },
    { "en": "Fungsi Menggerakkan Bus Komunikasi Dengan Banyak Perangkat?", "id": "Bus Driver." },
    { "en": "Fungsi Menerima Sinyal Dari Bus Komunikasi?", "id": "Bus Receiver." },
    { "en": "Fungsi Gabungan Driver Dan Receiver Bus?", "id": "Bus Transceiver." },
    { "en": "Fungsi Sirkuit Anti-Paralel LED?", "id": "Indikator Polaritas." },
    { "en": "Fungsi Mendeteksi Kehadiran Objek Tanpa Sentuhan?", "id": "Sensor Jarak (Proximity Sensor)." },
    { "en": "Fungsi Mengukur Jarak Menggunakan Suara?", "id": "Sensor Ultrasonik." },
    { "en": "Fungsi Mendeteksi Gerakan Berbasis Inframerah?", "id": "Sensor PIR (Passive Infrared)." },
    { "en": "Fungsi Memancarkan Dan Menerima Sinyal Inframerah?", "id": "Transceiver Inframerah (IrDA)." },
    { "en": "Fungsi Menghasilkan Tegangan Tinggi Untuk Lampu Neon?", "id": "Ballast Elektronik." },
    { "en": "Fungsi Menghasilkan Tegangan Tinggi Untuk Strobe?", "id": "Sirkuit Pemicu Flash." },
    { "en": "Fungsi Menghasilkan Suara Sirene Elektronik?", "id": "Generator Sirene." },
    { "en": "Fungsi Membangkitkan Musik Sederhana?", "id": "Chip Melodi." },
    { "en": "Fungsi Merekam Dan Memutar Ulang Suara?", "id": "Chip Perekam Suara." },
    { "en": "Fungsi Mengontrol Volume Secara Digital?", "id": "Potensiometer Digital." },
    { "en": "Fungsi Mengontrol Saklar Secara Digital?", "id": "Saklar Analog." },
    { "en": "Fungsi Sirkuit Yang Mensimulasikan Dadu Elektronik?", "id": "Dadu Elektronik." },
    { "en": "Fungsi Menghitung Waktu Putaran?", "id": "Lap Timer." },
    { "en": "Fungsi Mengukur Kecepatan Putaran?", "id": "Tachometer Elektronik." },
    { "en": "Fungsi Mendeteksi Level Air?", "id": "Sensor Level Air." },
    { "en": "Fungsi Mendeteksi Kebocoran Gas?", "id": "Sensor Gas." },
    { "en": "Fungsi Membuka Kunci Pintu Secara Elektronik?", "id": "Sirkuit Kunci Elektronik." },
    { "en": "Fungsi Mengatur Suhu Secara Otomatis?", "id": "Termostat Elektronik." },
    { "en": "Fungsi Mengukur Intensitas Cahaya?", "id": "Light Meter (Lux Meter)." },
    { "en": "Fungsi Menunjukkan Kekuatan Sinyal Radio?", "id": "S-Meter (Signal Strength Meter)." },
    { "en": "Fungsi Menguji Kabel Jaringan?", "id": "LAN Tester." },
    { "en": "Fungsi Menguji Dioda, Transistor, Kapasitor?", "id": "Tester Komponen LCR-T4." },
    { "en": "Fungsi Menghilangkan Derau Listrik Dari Jaringan?", "id": "Filter Jaringan Listrik." },
    { "en": "Fungsi Menstabilkan Tegangan AC Dari Jaringan?", "id": "AVR (Automatic Voltage Regulator)." },
    { "en": "Fungsi Mengisolasi Jaringan Listrik Secara Aman?", "id": "Transformator Isolasi." },
    { "en": "Fungsi Mengurangi Arus Start Motor Listrik?", "id": "Soft Starter." },
    { "en": "Fungsi Mengatur Putaran Motor Induksi?", "id": "Inverter VFD." },
    { "en": "Fungsi Menguatkan Sinyal Dari Mikrofon Dinamis?", "id": "Preamplifier Mikrofon." },
    { "en": "Fungsi Menguatkan Sinyal Dari Gitar Listrik?", "id": "Preamplifier Gitar." },
    { "en": "Fungsi Sirkuit Yang Memisahkan Ground Audio-Digital?", "id": "Isolator Audio Digital." },
    { "en": "Fungsi Sirkuit Yang Menggabungkan Sinyal Stereo-Mono?", "id": "Stereo-to-Mono Converter." },
    { "en": "Fungsi Sirkuit Yang Membuat Efek Stereo Palsu?", "id": "Stereo Synthesizer." },
    { "en": "Fungsi Sirkuit Yang Mengompres Rentang Dinamis?", "id": "Kompresor." },
    { "en": "Fungsi Menghasilkan Tegangan Yang Dapat Diatur?", "id": "Regulator Tegangan Variabel (LM317)." },
    { "en": "Fungsi Menghasilkan Tegangan Negatif Yang Dapat Diatur?", "id": "Regulator Tegangan Negatif (LM337)." },
    { "en": "Fungsi Menghasilkan Referensi Tegangan Presisi?", "id": "Referensi Tegangan Presisi (TL431)." },
    { "en": "Fungsi Menguatkan Daya Audio Dengan IC?", "id": "Penguat Daya Audio Monolitik." },
    { "en": "Fungsi Menguatkan Sinyal TV Kabel?", "id": "Penguat Distribusi CATV." },
    { "en": "Fungsi Memisahkan Sinyal Antena Ke TV/Radio?", "id": "Pemisah Antena (Antenna Splitter)." },
    { "en": "Fungsi Menggabungkan Sinyal Dari Beberapa Antena?", "id": "Penggabung Antena (Antenna Combiner)." },
    { "en": "Fungsi Sirkuit Pembangkit Jam Waktu Nyata?", "id": "RTC (Real-Time Clock)." },
    { "en": "Fungsi Sirkuit Antarmuka Keyboard?", "id": "Encoder Keyboard." },
    { "en": "Fungsi Sirkuit Antarmuka Mouse?", "id": "Kontroler Mouse." },
    { "en": "Fungsi Mengubah Standar Sinyal RS-232?", "id": "Transceiver RS-232 (MAX232)." },
    { "en": "Fungsi Mengubah Standar Sinyal USB Ke Serial?", "id": "Konverter USB-to-Serial." },
    { "en": "Fungsi Mengontrol Perangkat Melalui Jaringan Ethernet?", "id": "Kontroler Ethernet." },
    { "en": "Fungsi Menambahkan Kemampuan Nirkabel?", "id": "Modul Wi-Fi/Bluetooth." },
    { "en": "Fungsi Menambahkan Kemampuan Komunikasi Nirkabel Jarak Dekat?", "id": "Modul Transceiver RF." },
    { "en": "Fungsi Membaca Dan Menulis Kartu Memori?", "id": "Antarmuka Kartu SD/MMC." },
    { "en": "Fungsi Sirkuit Yang Menyimpan Konfigurasi Sistem?", "id": "EEPROM (Electrically Erasable Programmable ROM) Serial." },
    { "en": "Fungsi Mengirimkan Data Audio Digital?", "id": "Transmitter I2S (Inter-IC Sound)." },
    { "en": "Fungsi Menerima Data Audio Digital?", "id": "Receiver I2S." },
    { "en": "Fungsi Mengkodekan Sinyal Audio Untuk Transmisi?", "id": "Audio Codec." },
    { "en": "Fungsi Menguatkan Sinyal Daya Rendah Ke Daya Tinggi?", "id": "Power Amplifier Stage." },
    { "en": "Fungsi Menguatkan Tegangan Tanpa Penguatan Arus?", "id": "Voltage Amplifier Stage." },
    { "en": "Fungsi Menguatkan Arus Tanpa Penguatan Tegangan?", "id": "Current Amplifier (Buffer)." },
    { "en": "Fungsi Mengubah Sinyal Digital Menjadi Pulsa Analog?", "id": "Pulse Generator." },
    { "en": "Fungsi Menghasilkan Pulsa Dengan Lebar Terkontrol?", "id": "PWM (Pulse Width Modulator) Generator." },
    { "en": "Fungsi Menghasilkan Sinyal Dengan Frekuensi Terkontrol?", "id": "Frequency Synthesizer." },
    { "en": "Fungsi Mendeteksi Urutan Fasa Tiga Jaringan Listrik?", "id": "Phase Sequence Detector." },
    { "en": "Fungsi Mendeteksi Kegagalan Fasa?", "id": "Phase Failure Detector." },
    { "en": "Fungsi Melindungi Motor Dari Beban Berlebih Termal?", "id": "Thermal Overload Relay." },
    { "en": "Fungsi Mengaktifkan Sirkuit Dengan Sentuhan?", "id": "Saklar Sentuh Kapasitif." },
    { "en": "Fungsi Mengaktifkan Sirkuit Dengan Suara?", "id": "Saklar Teraktivasi Suara." },
    { "en": "Fungsi Mengaktifkan Lampu Saat Gelap?", "id": "Saklar Cahaya Otomatis (LDR)." },
    { "en": "Fungsi Mematikan Pompa Air Saat Tangki Penuh?", "id": "Kontrol Level Air Otomatis." },
    { "en": "Fungsi Menghidupkan Kipas Saat Suhu Tinggi?", "id": "Kontrol Kipas Termostatik." },
    { "en": "Fungsi Mengubah Tegangan Listrik AC Ke Level Lain?", "id": "Transformator." },
    { "en": "Fungsi Mengubah Gerakan Menjadi Sinyal Listrik?", "id": "Akselerometer." },
    { "en": "Fungsi Mengukur Kecepatan Sudut?", "id": "Giroskop." },
    { "en": "Fungsi Menentukan Arah Medan Magnet?", "id": "Kompas Elektronik (Magnetometer)." },
    { "en": "Fungsi Mengukur Ketinggian Diatas Permukaan Laut?", "id": "Altimeter (Sensor Tekanan Barometrik)." },
    { "en": "Fungsi Sirkuit Yang Menguji Kualitas Baterai?", "id": "Penganalisis Baterai." },
    { "en": "Fungsi Menyeimbangkan Tegangan Sel Baterai Seri?", "id": "Penyeimbang Sel (Cell Balancer)." },
    { "en": "Fungsi Mengukur Konsumsi Energi Listrik?", "id": "Watt-Hour Meter." },
    { "en": "Fungsi Mengukur Faktor Daya Listrik?", "id": "Power Factor Meter." },
    { "en": "Fungsi Menghasilkan Sinyal Audio Untuk Pengujian?", "id": "Generator Sinyal Audio." },
    { "en": "Fungsi Menganalisis Spektrum Sinyal Audio?", "id": "Penganalisis Spektrum Audio." },
    { "en": "Fungsi Mengukur Distorsi Dalam Sinyal Audio?", "id": "Distortion Meter." },
    { "en": "Fungsi Rangkaian Yang Menguji Kabel Audio?", "id": "Cable Tester." },
    { "en": "Fungsi Menguji Operasi Remote Control Inframerah?", "id": "Tester Remote IR." },
    { "en": "Fungsi Mengemulasi Sinyal Sensor Untuk Pengujian ECU?", "id": "Simulator Sinyal Sensor." },
    { "en": "Fungsi Memprogram Mikrokontroler Di Dalam Sistem?", "id": "ISP (In-System Programmer)." },
    { "en": "Fungsi Menghapus Memori EPROM Dengan Sinar UV?", "id": "Penghapus EPROM UV." },
    { "en": "Fungsi Menggandakan Chip ROM atau EPROM?", "id": "Duplikator ROM." },
    { "en": "Fungsi Menguji Chip Logika Digital?", "id": "Penganalisis Logika (Logic Analyzer)." },
    { "en": "Fungsi Menampilkan Bentuk Gelombang Sinyal Listrik?", "id": "Osiloskop." },
    { "en": "Fungsi Menghasilkan Berbagai Bentuk Gelombang Standar?", "id": "Generator Fungsi." },
    { "en": "Fungsi Mengukur Frekuensi Sinyal Secara Akurat?", "id": "Frequency Counter." },
    { "en": "Fungsi Mengukur Induktansi, Kapasitansi, dan Resistansi?", "id": "LCR Meter." },
    { "en": "Fungsi Mengukur Resistansi Tanah?", "id": "Earth Ground Resistance Tester." },
    { "en": "Fungsi Menguatkan Sinyal Sangat Kecil Dari Otak?", "id": "Penguat EEG (Electroencephalogram)." },
    { "en": "Fungsi Menguatkan Sinyal Sangat Kecil Dari Jantung?", "id": "Penguat EKG (Electrocardiogram)." },
    { "en": "Fungsi Menguatkan Sinyal Sangat Kecil Dari Otot?", "id": "Penguat EMG (Electromyogram)." },
    { "en": "Fungsi Menghasilkan Denyut Listrik Untuk Jantung?", "id": "Alat Pacu Jantung (Pacemaker)." },
    { "en": "Fungsi Memberi Kejutan Listrik Untuk Resusitasi Jantung?", "id": "Defibrilator." },
    { "en": "Fungsi Memonitor Tingkat Oksigen Dalam Darah?", "id": "Pulse Oximeter." },
    { "en": "Fungsi Mengukur Tekanan Darah Secara Elektronik?", "id": "Sphygmomanometer Digital." },
    { "en": "Fungsi Mengirimkan Sinyal Pengukuran Jarak Jauh?", "id": "Sistem Telemetri." },
    { "en": "Fungsi Mengontrol Perangkat Dari Jarak Jauh?", "id": "Sistem Remote Control." },
    { "en": "Fungsi Menggerakkan Katup Secara Elektronik?", "id": "Driver Katup Solenoida." },
    { "en": "Fungsi Memanaskan Elemen Secara Terkontrol?", "id": "Kontroler Pemanas." },
    { "en": "Fungsi Sirkuit Yang Menggabungkan Daya RF?", "id": "Wilkinson Power Divider." },
    { "en": "Fungsi Mixer RF Dengan Penekanan Port Baik?", "id": "Double-Balanced Mixer." },
    { "en": "Fungsi Penguat RF Dengan Umpan Balik Negatif?", "id": "Penguat Umpan Balik RF." },
    { "en": "Fungsi Penguat RF Dengan Distribusi Daya?", "id": "Penguat Terdistribusi." },
    { "en": "Fungsi Penguat RF Berdaya Tinggi?", "id": "RF Power Amplifier." },
    { "en": "Fungsi Melindungi Input Penerima Dari Daya Berlebih?", "id": "Limiter Penerima." },
    { "en": "Fungsi Menyesuaikan Penguatan Penerima Secara Otomatis?", "id": "AGC/AVC (Automatic Gain/Volume Control)." },
    { "en": "Fungsi Mematikan Audio Saat Sinyal Lemah?", "id": "Sirkuit Squelch." },
    { "en": "Fungsi Mengindikasikan Frekuensi Radio Yang Tepat?", "id": "Indikator Tuning." },
    { "en": "Fungsi Mengubah Sinyal Stereo FM Menjadi Audio?", "id": "Dekoder Stereo FM." },
    { "en": "Fungsi Memproses Sinyal Audio Untuk Radio FM?", "id": "Prosesor Audio Siaran." },
    { "en": "Fungsi Membandingkan Besaran Dua Bilangan Biner?", "id": "Komparator Magnitudo." },
    { "en": "Fungsi Menambahkan Penundaan Waktu Ke Sinyal?", "id": "Sirkuit Penunda (Delay Circuit)." },
    { "en": "Fungsi Rangkaian Yang Menguji Dirinya Sendiri?", "id": "BIST (Built-In Self-Test) Circuit." },
    { "en": "Fungsi Mengontrol Akses Ke Bus Bersama?", "id": "Arbiter Bus." },
    { "en": "Fungsi Menghitung Jumlah Kejadian?", "id": "Event Counter." },
    { "en": "Fungsi Mengukur Durasi Waktu Suatu Kejadian?", "id": "Time Duration Counter." },
    { "en": "Fungsi Sirkuit Logika Yang Dapat Diprogram Ulang?", "id": "FPGA/CPLD." },
    { "en": "Fungsi Sirkuit Logika Khusus Untuk Aplikasi Tertentu?", "id": "ASIC (Application-Specific Integrated Circuit)." },
    { "en": "Fungsi Sirkuit Yang Menggabungkan Prosesor Dan Periferal?", "id": "SoC (System on a Chip)." },
    { "en": "Fungsi Sirkuit Yang Memproses Sinyal Digital?", "id": "DSP (Digital Signal Processor)." },
    { "en": "Fungsi Sirkuit Untuk Menghasilkan Warna Video Uji?", "id": "Generator Pola Color Bar." },
    { "en": "Fungsi Menggabungkan Teks Ke Sinyal Video?", "id": "OSD (On-Screen Display) Generator." },
    { "en": "Fungsi Menstabilkan Gambar Video Analog?", "id": "TBC (Time Base Corrector)." },
    { "en": "Fungsi Mengubah Sinyal Audio Menjadi Spektrum Cahaya?", "id": "Spectrum Analyzer Display." },
    { "en": "Fungsi Menghasilkan Efek Cahaya Berlari?", "id": "LED Chaser." },
    { "en": "Fungsi Mengontrol Kecerahan Banyak LED?", "id": "Multiplexed LED Display Driver." },
    { "en": "Fungsi Menggerakkan Panel Dot Matrix?", "id": "Dot Matrix Controller." },
    { "en": "Fungsi Menggerakkan Motor DC Berdasarkan Umpan Balik?", "id": "Kontrol Loop Tertutup Motor DC." },
    { "en": "Fungsi Menghasilkan Tegangan Tinggi Untuk Pagar Listrik?", "id": "Electric Fence Energizer." },
    { "en": "Fungsi Menghasilkan Ion Negatif Untuk Pembersih Udara?", "id": "Generator Ion Negatif." },
    { "en": "Fungsi Menghasilkan Ozon Untuk Sterilisasi?", "id": "Generator Ozon." },
    { "en": "Fungsi Mengusir Hama Menggunakan Frekuensi Ultrasonik?", "id": "Pengusir Hama Ultrasonik." },
    { "en": "Fungsi Mendeteksi Logam Di Dekatnya?", "id": "Detektor Logam." },
    { "en": "Fungsi Mengukur Medan Elektromagnetik?", "id": "EMF Meter." },
    { "en": "Fungsi Menguji Transistor Di Dalam Sirkuit?", "id": "In-Circuit Transistor Tester." },
    { "en": "Fungsi Mengukur ESR Kapasitor?", "id": "ESR Meter." },
    { "en": "Fungsi Menguji Kristal Kuarsa?", "id": "Crystal Tester." },
    { "en": "Fungsi Sirkuit Yang Menguji Diri Saat Dihidupkan?", "id": "POST (Power-On Self-Test) Circuit." },
    { "en": "Fungsi Membaca Kode Batang (Barcode)?", "id": "Barcode Scanner Circuit." },
    { "en": "Fungsi Membaca Kartu Magnetik?", "id": "Magnetic Stripe Reader." },
    { "en": "Fungsi Berkomunikasi Menggunakan Frekuensi Radio?", "id": "Modul RFID (Radio-Frequency Identification)." },
    { "en": "Fungsi Berkomunikasi Jarak Sangat Dekat?", "id": "Sirkuit NFC (Near Field Communication)." },
    { "en": "Fungsi Mengirimkan Sinyal Melalui Jalur Listrik AC?", "id": "Transceiver Power-Line Carrier (PLC)." },
    { "en": "Fungsi Sirkuit Logika Yang Dapat Dikonfigurasi Ulang?", "id": "FPGA (Field-Programmable Gate Array)." },
    { "en": "Fungsi Buffer Data First-In, First-Out?", "id": "Memori FIFO." },
    { "en": "Fungsi Buffer Data Last-In, First-Out?", "id": "Memori LIFO (Stack)." },
    { "en": "Fungsi Memori Yang Mencari Berdasarkan Konten?", "id": "CAM (Content-Addressable Memory)." },
    { "en": "Fungsi Mendeteksi Dan Memperbaiki Kesalahan Memori?", "id": "Sirkuit ECC (Error Correction Code)." },
    { "en": "Fungsi Mengontrol Sudut Konduksi Beban AC?", "id": "Kontroler Fasa (Phase Controller)." },
    { "en": "Fungsi Mengganti Dioda Penyearah Dengan MOSFET?", "id": "Penyearah Sinkron." },
    { "en": "Fungsi Memodulasi Sinyal Pada Dua Pembawa Kuadratur?", "id": "Modulator Kuadratur (I/Q Modulator)." },
    { "en": "Fungsi Demodulasi Sinyal Dari Dua Pembawa Kuadratur?", "id": "Demodulator Kuadratur (I/Q Demodulator)." },
    { "en": "Fungsi Membandingkan Fasa Dan Frekuensi Dua Sinyal?", "id": "Detektor Fasa-Frekuensi (PFD)." },
    { "en": "Fungsi Menjaga Frekuensi Penerima Tetap Terkunci?", "id": "Sirkuit AFC (Automatic Frequency Control)." },
    { "en": "Fungsi Menghasilkan Output DC Sesuai Nilai RMS?", "id": "True RMS-to-DC Converter." },
    { "en": "Fungsi Menjepit (Clamp) Sinyal Pada Level Presisi?", "id": "Precision Clamper." },
    { "en": "Fungsi Menahan Nilai Puncak Sinyal?", "id": "Sirkuit Peak-Hold." },
    { "en": "Fungsi Penguat Dengan Bandwidth Sangat Lebar?", "id": "Penguat Video." },
    { "en": "Fungsi Memisahkan Sinyal Sinkronisasi Dari Video?", "id": "Sync Separator." },
    { "en": "Fungsi Mengarahkan Banyak Input Ke Banyak Output?", "id": "Matrix Switcher." },
    { "en": "Fungsi Menguatkan Sinyal Dari Sensor Timbangan?", "id": "Penguat Load Cell." },
    { "en": "Fungsi Mengkondisikan Sinyal Dari Sensor LVDT?", "id": "LVDT Signal Conditioner." },
    { "en": "Fungsi Sekring Elektronik Yang Dapat Direset?", "id": "eFuse (Electronic Fuse)." },
    { "en": "Fungsi Jalur Penunda Dengan Waktu Terkontrol Tegangan?", "id": "Voltage-Controlled Delay Line." },
    { "en": "Fungsi Menggerakkan Tampilan Tabung Nixie?", "id": "Nixie Tube Driver." },
    { "en": "Fungsi Membagi Sinyal Secara Analog?", "id": "Pembagi Analog." },
    { "en": "Fungsi Konverter DC-DC Berbasis Kapasitor-Saklar?", "id": "Konverter Kapasitor-Saklar." },
    { "en": "Fungsi Mengurangi Distorsi Akibat Pensaklaran?", "id": "Sirkuit Dead-Time Control." },
    { "en": "Fungsi Menghasilkan Sinyal Referensi Presisi?", "id": "Sumber Referensi Tegangan." },
    { "en": "Fungsi Rangkaian Penyala Lampu Flas Xenon?", "id": "Sirkuit Pemicu Xenon." },
    { "en": "Fungsi Menghasilkan Efek Suara Gema (Reverb)?", "id": "Sirkuit Reverb Digital." },
    { "en": "Fungsi Menghasilkan Efek Suara Tunda (Delay)?", "id": "Sirkuit Delay Audio." },
    { "en": "Fungsi Menghasilkan Suara Drum Elektronik?", "id": "Sirkuit Drum Machine." },
    { "en": "Fungsi Mengukur Medan Magnet?", "id": "Sensor Efek Hall." },
    { "en": "Fungsi Mengukur Arus Melalui Efek Magnet?", "id": "Sensor Arus Efek Hall." },
    { "en": "Fungsi Mengukur Getaran?", "id": "Vibration Sensor Interface." },
    { "en": "Fungsi Menghasilkan Tegangan Output Terisolasi?", "id": "Catu Daya Terisolasi." },
    { "en": "Fungsi Menghasilkan Sinyal Clock Dua Fasa?", "id": "Generator Clock Non-Overlapping." },
    { "en": "Fungsi Menguji Operasionalitas Op-Amp?", "id": "Tester Op-Amp." },
    { "en": "Fungsi Menguji Dioda Zener?", "id": "Tester Dioda Zener." },
    { "en": "Fungsi Mengukur Kapasitansi Dengan Akurat?", "id": "Capacitance Meter." },
    { "en": "Fungsi Menghasilkan Tegangan Tinggi Untuk Pengion Udara?", "id": "Ionizer Circuit." },
    { "en": "Fungsi Mengontrol Kipas Pendingin Berdasarkan Suhu?", "id": "Kontroler Kipas PWM." },
    { "en": "Fungsi Menghasilkan Sinyal Uji TV?", "id": "Generator Pola Video." },
    { "en": "Fungsi Memisahkan Sinyal Audio Dari Siaran TV?", "id": "Detektor SIF (Sound Intermediate Frequency)." },
    { "en": "Fungsi Memilih Sumber Audio/Video?", "id": "AV Switch." },
    { "en": "Fungsi Menguatkan Sinyal Head Pembaca Hard Disk?", "id": "Preamplifier Hard Disk." },
    { "en": "Fungsi Mengontrol Posisi Lengan Hard Disk?", "id": "Kontroler Voice Coil Motor (VCM)." },
    { "en": "Fungsi Membaca Data Dari Trek CD/DVD?", "id": "Preamplifier Optical Pickup Unit (OPU)." },
    { "en": "Fungsi Mengontrol Fokus Dan Tracking Laser CD?", "id": "Kontroler Servo CD/DVD." },
    { "en": "Fungsi Menggerakkan Motor Spindle CD/DVD?", "id": "Driver Motor Spindle." },
    { "en": "Fungsi Mengubah Sinyal RF Dari CD Menjadi Data?", "id": "Prosesor Sinyal Digital CD." },
    { "en": "Fungsi Sirkuit Logika Yang Bekerja Asinkron?", "id": "Logika Asinkron." },
    { "en": "Fungsi Sinkronisasi Sinyal Antara Domain Clock Berbeda?", "id": "Sinkronizer." },
    { "en": "Fungsi Mencegah Kondisi Metastability?", "id": "Sinkronizer Dua Flip-Flop." },
    { "en": "Fungsi Memilih Antara Dua Sinyal Clock?", "id": "Clock Mux (Multiplexer)." },
    { "en": "Fungsi Mengaktifkan Atau Menonaktifkan Sinyal Clock?", "id": "Clock Gating Circuit." },
    { "en": "Fungsi Membagi Frekuensi Clock Dengan Bilangan Bulat?", "id": "Pembagi Frekuensi Integer-N." },
    { "en": "Fungsi Membagi Frekuensi Clock Dengan Bilangan Pecahan?", "id": "Pembagi Frekuensi Fractional-N." },
    { "en": "Fungsi Mendistribusikan Sinyal Clock Dengan Skew Rendah?", "id": "Jaringan Distribusi Clock (Clock Tree)." },
    { "en": "Fungsi Penyangga Sinyal Clock?", "id": "Clock Buffer." },
    { "en": "Fungsi Menghasilkan Sinyal Clock Dari Kristal?", "id": "Osilator Pierce." },
    { "en": "Fungsi Menghilangkan Jitter Dari Sinyal Clock?", "id": "Clock De-jitter Circuit." },
    { "en": "Fungsi Mengukur Jitter Sinyal?", "id": "Time Interval Analyzer." },
    { "en": "Fungsi Mengontrol Impedansi Output Driver?", "id": "Digitally Controlled Impedance (DCI)." },
    { "en": "Fungsi Menyesuaikan Waktu Sinyal Data?", "id": "Data Slicer." },
    { "en": "Fungsi Menyamakan Waktu Tiba Sinyal Paralel?", "id": "De-skew Circuit." },
    { "en": "Fungsi Mengubah Data Biner Menjadi Kode Gray?", "id": "Binary to Gray Code Converter." },
    { "en": "Fungsi Mengubah Kode Gray Menjadi Data Biner?", "id": "Gray to Binary Code Converter." },
    { "en": "Fungsi Melakukan Penjumlahan BCD (Binary-Coded Decimal)?", "id": "BCD Adder." },
    { "en": "Fungsi Menghasilkan Sinyal Kontrol Untuk DRAM?", "id": "Kontroler Memori DRAM." },
    { "en": "Fungsi Menyegarkan Isi Memori DRAM?", "id": "Refresh Controller." },
    { "en": "Fungsi Mengelola Akses Langsung Periferal Ke Memori?", "id": "Kontroler DMA (Direct Memory Access)." },
    { "en": "Fungsi Mengelola Sinyal Interupsi Dari Periferal?", "id": "Interrupt Controller." },
    { "en": "Fungsi Timer Yang Dapat Diprogram?", "id": "Programmable Interval Timer (PIT)." },
    { "en": "Fungsi Port Input/Output Paralel?", "id": "PIO (Parallel Input/Output)." },
    { "en": "Fungsi Antarmuka Komunikasi Serial?", "id": "UART/USART." },
    { "en": "Fungsi Antarmuka Komunikasi Serial Sinkron?", "id": "SPI (Serial Peripheral Interface)." },
    { "en": "Fungsi Antarmuka Komunikasi Dua Kawat?", "id": "I2C (Inter-Integrated Circuit)." },
    { "en": "Fungsi Antarmuka Jaringan Otomotif?", "id": "Kontroler CAN (Controller Area Network)." },
    { "en": "Fungsi Sirkuit Pembangkit Tegangan Reset?", "id": "Power-on Reset (POR) Circuit." },
    { "en": "Fungsi Mereset Sistem Saat Tegangan Turun?", "id": "Brown-out Detector (BOD)." },
    { "en": "Fungsi Menguji Batas-batas Sirkuit Terintegrasi?", "id": "Boundary Scan (JTAG)." },
    { "en": "Fungsi Penguat Audio Yang Menghasilkan Panas Rendah?", "id": "Penguat Kelas-D." },
    { "en": "Fungsi Mengontrol Motor Menggunakan Gelombang Sinus?", "id": "Kontrol FOC (Field-Oriented Control)." },
    { "en": "Fungsi Mengukur Arus Tanpa Memutus Sirkuit?", "id": "Sensor Arus Non-Invasif." },
    { "en": "Fungsi Mengirimkan Daya Listrik Tanpa Kabel?", "id": "Wireless Power Transfer Circuit." },
    { "en": "Fungsi Memanen Energi Dari Lingkungan?", "id": "Energy Harvesting Circuit." },
    { "en": "Fungsi Menguatkan Sinyal Biologis?", "id": "Bio-Amplifier." },
    { "en": "Fungsi Mencegah Efek Pemuatan Antar Tahap?", "id": "Sirkuit Penyangga (Buffer Circuit)." },
    { "en": "Fungsi Menghasilkan Tegangan Negatif Sama Besar?", "id": "Inverting Charge Pump." },
    { "en": "Fungsi Mengubah Level Tegangan Baterai?", "id": "Regulator Baterai." },
    { "en": "Fungsi Menghasilkan Tegangan Tinggi Untuk Backlight LCD?", "id": "Inverter Backlight CCFL." },
    { "en": "Fungsi Mengatur Kecerahan Backlight LED?", "id": "Driver Backlight LED." },
    { "en": "Fungsi Menghasilkan Tegangan Bias Untuk Panel LCD?", "id": "Generator Tegangan Bias LCD." },
    { "en": "Fungsi Mengontrol Kontras Tampilan LCD?", "id": "Kontrol Kontras Digital." },
    { "en": "Fungsi Menguji Seluruh Piksel Layar?", "id": "Generator Pola Uji Layar." },
    { "en": "Fungsi Sirkuit Yang Mengaktifkan Pompa Secara Periodik?", "id": "Timer Pompa Intermiten." },
    { "en": "Fungsi Mengukur Waktu Nyala Suatu Alat?", "id": "Hour Meter Circuit." },
    { "en": "Fungsi Mengontrol Kecerahan Lampu Pijar AC?", "id": "Phase Control Dimmer." },
    { "en": "Fungsi Mengontrol Peralatan Listrik Melalui Telepon?", "id": "DTMF (Dual-Tone Multi-Frequency) Controller." },
    { "en": "Fungsi Mengirimkan Sinyal Audio Melalui Sinar Laser?", "id": "Laser Audio Communicator." },
    { "en": "Fungsi Sirkuit Yang Merespon Suara Tepukan Tangan?", "id": "Clap Switch." },
    { "en": "Fungsi Menghasilkan Suara Mirip Kicauan Burung?", "id": "Electronic Bird Chirp Generator." },
    { "en": "Fungsi Mendeteksi Kebohongan Berdasarkan Respon Kulit?", "id": "Lie Detector Circuit." },
    { "en": "Fungsi Menghasilkan Tegangan Busur Untuk Pengapian?", "id": "CDI (Capacitor Discharge Ignition) Circuit." },
    { "en": "Fungsi Mengontrol Solenoida Dengan Akurat?", "id": "Solenoid Driver Circuit." },
    { "en": "Fungsi Menguatkan Sinyal Dari Sensor Getaran?", "id": "Vibration Amplifier." },
    { "en": "Fungsi Membatasi Amplitudo Sinyal RF?", "id": "RF Limiter." },
    { "en": "Fungsi Menggeser Level DC Sinyal Video?", "id": "Video DC Clamp." },
    { "en": "Fungsi Menambahkan Data Ke Sinyal Video?", "id": "Video Data Inserter." },
    { "en": "Fungsi Mendeteksi Ada Atau Tidaknya Sinyal Video?", "id": "Video Presence Detector." },
    { "en": "Fungsi Menguatkan Sinyal Diferensial Kecepatan Tinggi?", "id": "High-Speed Differential Amplifier." },
    { "en": "Fungsi Menerima Sinyal Diferensial Kecepatan Tinggi?", "id": "LVDS (Low-Voltage Differential Signaling) Receiver." },
    { "en": "Fungsi Mengirimkan Sinyal Diferensial Kecepatan Tinggi?", "id": "LVDS (Low-Voltage Differential Signaling) Transmitter." },
    { "en": "Fungsi Menyamakan Impedansi Jalur Transmisi?", "id": "Termination Circuit." },
    { "en": "Fungsi Menekan Derau Pada Jalur Catu Daya?", "id": "Power Supply Filter." },
    { "en": "Fungsi Memisahkan Jalur Analog Dan Digital?", "id": "Analog/Digital Ground Isolation." },
    { "en": "Fungsi Melindungi Dari Tegangan Balik Baterai?", "id": "Reverse Polarity Protection." },
    { "en": "Fungsi Menghasilkan Tegangan Tinggi Dari Baterai?", "id": "Inverter DC-AC." },
    { "en": "Fungsi Mengisi Kapasitor Besar Secara Terkontrol?", "id": "Capacitor Charger Circuit." },
    { "en": "Fungsi Melepaskan Muatan Kapasitor Dengan Cepat?", "id": "Capacitor Discharge Circuit." },
    { "en": "Fungsi Sirkuit Yang Menguji Sirkuit Lain?", "id": "Test Jig." },
    { "en": "Fungsi Mengisolasi Sinyal Bus I2C?", "id": "I2C Isolator." },
    { "en": "Fungsi Memperpanjang Jarak Bus I2C?", "id": "I2C Bus Extender." },
    { "en": "Fungsi Mengemulasi Perangkat I2C?", "id": "I2C Slave Emulator." },
    { "en": "Fungsi Menganalisis Lalu Lintas Data Bus I2C?", "id": "I2C Bus Analyzer." },
    { "en": "Fungsi Mengisolasi Jalur Komunikasi CAN Bus?", "id": "CAN Bus Isolator." },
    { "en": "Fungsi Menerima Dan Mengirim Data CAN Bus?", "id": "CAN Transceiver." },
    { "en": "Fungsi Menghasilkan Bentuk Gelombang Sinus Murni?", "id": "Pure Sine Wave Inverter." },
    { "en": "Fungsi Menghasilkan Bentuk Gelombang Kotak?", "id": "Modified Sine Wave Inverter." },
    { "en": "Fungsi Memilih Antara Dua Sumber Daya?", "id": "Power Path Controller." },
    { "en": "Fungsi Mengukur Resistansi Secara Tidak Langsung?", "id": "Ohmmeter (V-I Method)." },
    { "en": "Fungsi Mengukur Arus Tanpa Memutus Sirkuit?", "id": "Inductive Current Sensor." },
    { "en": "Fungsi Mengukur Suhu Tanpa Sentuhan?", "id": "Infrared Thermometer Circuit." },
    { "en": "Fungsi Mengukur Kelembaban Tanah?", "id": "Soil Moisture Sensor Circuit." },
    { "en": "Fungsi Mendeteksi Hujan?", "id": "Rain Sensor Circuit." },
    { "en": "Fungsi Mendeteksi Asap?", "id": "Smoke Detector Circuit." },
    { "en": "Fungsi Mendeteksi Api (Nyala)?", "id": "Flame Detector Circuit." },
    { "en": "Fungsi Menghasilkan Suara Alarm Keras?", "id": "Siren Driver." },
    { "en": "Fungsi Mengedipkan Lampu Strobo?", "id": "Strobe Light Driver." },
    { "en": "Fungsi Mengontrol Lampu Lalu Lintas?", "id": "Traffic Light Controller." },
    { "en": "Fungsi Menampilkan Angka Pada Papan Skor?", "id": "Scoreboard Display Driver." },
    { "en": "Fungsi Sirkuit Yang Mengocok Angka Acak?", "id": "Random Number Generator." },
    { "en": "Fungsi Menguatkan Sinyal Dari Sel Surya?", "id": "Solar Cell Amplifier." },
    { "en": "Fungsi Mengisi Baterai Menggunakan Tenaga Surya?", "id": "Solar Charge Controller." },
    { "en": "Fungsi Melacak Titik Daya Maksimum Panel Surya?", "id": "MPPT (Maximum Power Point Tracking) Controller." },
    { "en": "Fungsi Mengubah Energi Angin Menjadi Listrik DC?", "id": "Wind Turbine Rectifier." },
    { "en": "Fungsi Melindungi Baterai Dari Pelepasan Berlebih?", "id": "Low Voltage Disconnect (LVD)." },
    { "en": "Fungsi Mengubah Satu Fasa Listrik Menjadi Tiga Fasa?", "id": "Phase Converter." },
    { "en": "Fungsi Sinkronisasi Generator Ke Jaringan Listrik?", "id": "Generator Synchronizer." },
    { "en": "Fungsi Membagi Beban Antar Generator?", "id": "Load Sharing Controller." },
    { "en": "Fungsi Mengukur Energi Reaktif?", "id": "VAR-Hour Meter." },
    { "en": "Fungsi Memperbaiki Faktor Daya?", "id": "Power Factor Correction (PFC) Circuit." },
    { "en": "Fungsi Menekan Harmonik Dalam Jaringan Listrik?", "id": "Harmonic Filter." },
    { "en": "Fungsi Mengukur Kualitas Daya Listrik?", "id": "Power Quality Analyzer." },
    { "en": "Fungsi Mendeteksi Gangguan Busur Api Listrik?", "id": "Arc Fault Circuit Interrupter (AFCI)." },
    { "en": "Fungsi Mendeteksi Kebocoran Arus Ke Tanah?", "id": "Ground Fault Circuit Interrupter (GFCI)." },
    { "en": "Fungsi Mengemulasi Perilaku Baterai?", "id": "Battery Simulator." },
    { "en": "Fungsi Mengemulasi Beban Listrik?", "id": "Electronic Load." },
    { "en": "Fungsi Menguji Catu Daya Secara Otomatis?", "id": "Automated Test Equipment (ATE)." },
    { "en": "Fungsi Sirkuit Logika Berbasis Relai?", "id": "Relay Logic Circuit." },
    { "en": "Fungsi Logika Yang Dapat Diprogram Secara Fisik?", "id": "Programmable Logic Array (PLA)." },
    { "en": "Fungsi Sirkuit Logika Dengan Struktur Generik?", "id": "Generic Array Logic (GAL)." },
    { "en": "Fungsi Sirkuit Digital Yang Bekerja Berdasarkan Clock?", "id": "Synchronous Logic Circuit." },
    { "en": "Fungsi Sirkuit Digital Tanpa Clock Terpusat?", "id": "Asynchronous Logic Circuit." },
    { "en": "Fungsi Menghasilkan Pulsa Clock Tunggal?", "id": "Single Pulse Generator." },
    { "en": "Fungsi Menghasilkan Sinyal 'Power Good'?", "id": "Power Good Signal Generator." },
    { "en": "Fungsi Mengurutkan Waktu Nyala Beberapa Catu Daya?", "id": "Power Sequencer." },
    { "en": "Fungsi Sirkuit Yang Menggabungkan Beberapa Fungsi Analog?", "id": "Analog Front-End (AFE)." },
    { "en": "Fungsi Sirkuit Yang Menggabungkan Beberapa Fungsi RF?", "id": "RF Front-End." },
    { "en": "Fungsi Mengubah Sinyal Digital Ke Format Optik?", "id": "Optical Transmitter." },
    { "en": "Fungsi Mengubah Sinyal Optik Kembali Ke Digital?", "id": "Optical Receiver." },
    { "en": "Fungsi Menguatkan Sinyal Optik Lemah?", "id": "Optical Amplifier." },
    { "en": "Fungsi Mengulang Dan Menguatkan Sinyal Optik?", "id": "Optical Repeater." },
    { "en": "Fungsi Membagi Sinar Optik?", "id": "Optical Splitter." },
    { "en": "Fungsi Menggabungkan Sinar Optik?", "id": "Optical Combiner." },
    { "en": "Fungsi Sirkuit Yang Mengontrol Kamera?", "id": "Camera Control Unit (CCU)." },
    { "en": "Fungsi Menguatkan Sinyal Dari Sensor Gambar?", "id": "Image Sensor Amplifier." },
    { "en": "Fungsi Memproses Data Mentah Dari Sensor Gambar?", "id": "Image Signal Processor (ISP)." },
    { "en": "Fungsi Mengontrol Fokus Lensa Secara Otomatis?", "id": "Autofocus Circuit." },
    { "en": "Fungsi Mengontrol Iris Lensa Secara Otomatis?", "id": "Auto-Iris Circuit." },
    { "en": "Fungsi Menstabilkan Gambar Dari Getaran?", "id": "Image Stabilization Circuit." },
    { "en": "Fungsi Menghasilkan Suara Shutter Kamera?", "id": "Camera Shutter Sound Generator." },
    { "en": "Fungsi Mengontrol Flash Kamera?", "id": "Flash Controller." },
    { "en": "Fungsi Mengukur Cahaya Untuk Fotografi?", "id": "Exposure Meter Circuit." },
    { "en": "Fungsi Menghasilkan Suara Metronom Elektronik?", "id": "Electronic Metronome." },
    { "en": "Fungsi Menyetem Alat Musik Secara Elektronik?", "id": "Electronic Tuner." },
    { "en": "Fungsi Menciptakan Efek Gema Pada Audio?", "id": "Echo Chamber Circuit." },
    { "en": "Fungsi Menciptakan Efek Vibrato Pada Audio?", "id": "Vibrato Circuit." },
    { "en": "Fungsi Menciptakan Efek Tremolo Pada Audio?", "id": "Tremolo Circuit." },
    { "en": "Fungsi Sirkuit Yang Membatasi Kebisingan Audio?", "id": "Noise Reduction Circuit (Dolby/DBX)." },
    { "en": "Fungsi Menghilangkan Vokal Dari Lagu?", "id": "Vocal Remover Circuit." },
    { "en": "Fungsi Menggeser Pitch Suara?", "id": "Pitch Shifter Circuit." },
    { "en": "Fungsi Mengubah Suara Manusia?", "id": "Voice Changer Circuit." },
    { "en": "Fungsi Menguatkan Sinyal Head Pembaca Kaset?", "id": "Tape Head Preamplifier." },
    { "en": "Fungsi Sirkuit Yang Mengoreksi Respon Frekuensi Kaset?", "id": "NAB Equalization Circuit." },
    { "en": "Fungsi Menghasilkan Frekuensi Bias Untuk Merekam?", "id": "Bias Oscillator." },
    { "en": "Fungsi Menghapus Rekaman Pada Pita Magnetik?", "id": "Erase Head Driver." },
    { "en": "Fungsi Sirkuit Yang Menerima Sinyal Remote Control?", "id": "IR Receiver Module." },
    { "en": "Fungsi Sirkuit Yang Mengirim Sinyal Remote Control?", "id": "IR Transmitter Circuit." },
    { "en": "Fungsi Menguatkan Dan Menyaring Sinyal IR?", "id": "Infrared Signal Processor." },
    { "en": "Fungsi Mengubah Kode Remote Menjadi Perintah?", "id": "Remote Control Decoder." },
    { "en": "Fungsi Menghasilkan Sinyal Pembawa Untuk Transmisi RF?", "id": "RF Carrier Oscillator." },
    { "en": "Fungsi Menguatkan Sinyal RF Sebelum Transmisi?", "id": "RF Power Amplifier (PA)." },
    { "en": "Fungsi Menyaring Harmonik Dari Output Pemancar?", "id": "Harmonic Filter." },
    { "en": "Fungsi Mengukur Daya Maju Dan Mundur RF?", "id": "SWR (Standing Wave Ratio) Meter." },
    { "en": "Fungsi Melindungi Pemancar Dari SWR Tinggi?", "id": "SWR Protection Circuit." },
    { "en": "Fungsi Memodulasi Suara Ke Sinyal AM?", "id": "AM Modulator." },
    { "en": "Fungsi Memodulasi Suara Ke Sinyal FM?", "id": "FM Modulator." },
    { "en": "Fungsi Menstabilkan Frekuensi Pemancar FM?", "id": "AFC (Automatic Frequency Control) Circuit." },
    { "en": "Fungsi Sirkuit Yang Menguji TV Dan Monitor?", "id": "Video Pattern Generator." },
    { "en": "Fungsi Mengemulasi Joystick Untuk Pengujian?", "id": "Joystick Simulator." },
    { "en": "Fungsi Mengontrol Kecerahan Tampilan VFD?", "id": "VFD Dimmer Circuit." },
    { "en": "Fungsi Mengemulasi Suara Mesin Uap?", "id": "Steam Engine Sound Simulator." },
    { "en": "Fungsi Menghasilkan Nada Panggilan Telepon?", "id": "Telephone Tone Ringer." },
    { "en": "Fungsi Mendeteksi Dering Telepon?", "id": "Ring Detector Circuit." },
    { "en": "Fungsi Menahan Jalur Telepon (Hold)?", "id": "Telephone Hold Circuit." },
    { "en": "Fungsi Mencampur Suara Dengan Jalur Telepon?", "id": "Telephone Audio Mixer." },
    { "en": "Fungsi Antarmuka Radio Dua Arah Ke Jalur Telepon?", "id": "Phone Patch Circuit." },
    { "en": "Fungsi Mengubah Sinyal Modem Ke Audio?", "id": "Modem Modulator/Demodulator." },
    { "en": "Fungsi Menerima Sinyal Kode Morse?", "id": "Morse Code Reader." },
    { "en": "Fungsi Latihan Mengirim Kode Morse?", "id": "Morse Code Practice Oscillator." },
    { "en": "Fungsi Mengontrol Perangkat Listrik Melalui Komputer?", "id": "PC Parallel Port Controller." },
    { "en": "Fungsi Mengukur Suhu Menggunakan Antarmuka USB?", "id": "USB Thermometer." },
    { "en": "Fungsi Sirkuit Yang Mengisolasi Port USB?", "id": "USB Isolator." },
    { "en": "Fungsi Melindungi Port USB Dari Arus Berlebih?", "id": "USB Power Protection Circuit." },
    { "en": "Fungsi Sirkuit Yang Berbagi Perangkat USB?", "id": "USB Switch." },
    { "en": "Fungsi Memperpanjang Jarak Kabel USB?", "id": "USB Repeater/Extender." },
    { "en": "Fungsi Mengisi Daya Perangkat Melalui USB?", "id": "USB Charging Circuit." },
    { "en": "Fungsi Mendeteksi Tipe Charger USB?", "id": "USB Charger Detection Circuit." },
    { "en": "Fungsi Mengubah Sinyal VGA Ke Video Komposit?", "id": "VGA to Composite Converter." },
    { "en": "Fungsi Mengubah Sinyal HDMI Ke VGA?", "id": "HDMI to VGA Converter." },
    { "en": "Fungsi Memisahkan Audio Dari Sinyal HDMI?", "id": "HDMI Audio Extractor." },
    { "en": "Fungsi Memilih Di Antara Beberapa Input HDMI?", "id": "HDMI Switcher." },
    { "en": "Fungsi Mengirim Satu Sinyal HDMI Ke Banyak Layar?", "id": "HDMI Splitter." },
    { "en": "Fungsi Menguatkan Dan Menyamakan Sinyal HDMI?", "id": "HDMI Equalizer/Repeater." },
    { "en": "Fungsi Sirkuit Yang Melindungi Peralatan Dari Petir?", "id": "Lightning Arrester." },
    { "en": "Fungsi Menghasilkan Tegangan Negatif Untuk Op-Amp?", "id": "Split Rail Power Supply." },
    { "en": "Fungsi Mengaktifkan Catu Daya Secara Elektronik?", "id": "Soft Power Switch." },
    { "en": "Fungsi Mematikan Catu Daya Setelah Waktu Tertentu?", "id": "Auto Power-Off Circuit." },
    { "en": "Fungsi Mengukur Arus Tanpa Resistansi Shunt?", "id": "Fluxgate Magnetometer Current Sensor." },
    { "en": "Fungsi Mengukur Tegangan Sangat Tinggi?", "id": "High Voltage Probe/Divider." },
    { "en": "Fungsi Mengukur Arus Sangat Kecil?", "id": "Picoammeter." },
    { "en": "Fungsi Mengukur Muatan Listrik?", "id": "Electrometer." },
    { "en": "Fungsi Mengukur Medan Listrik Statis?", "id": "Static Field Meter." },
    { "en": "Fungsi Menghilangkan Muatan Statis?", "id": "Static Eliminator (Ionizer)." },
    { "en": "Fungsi Mendeteksi Urutan Logika Digital?", "id": "Digital Logic Sequence Detector." },
    { "en": "Fungsi Mengubah Level Sinyal ECL ke TTL?", "id": "ECL to TTL Translator." },
    { "en": "Fungsi Mengubah Level Sinyal PECL ke LVDS?", "id": "PECL to LVDS Translator." },
    { "en": "Fungsi Menggerakkan Jalur Transmisi Terterminasi?", "id": "Line Driver." },
    { "en": "Fungsi Menerima Sinyal Dari Jalur Transmisi?", "id": "Line Receiver." },
    { "en": "Fungsi Menyamakan Penundaan Sinyal?", "id": "Delay Locked Loop (DLL)." },
    { "en": "Fungsi Mengalikan Frekuensi Clock?", "id": "Frequency Multiplier." },
    { "en": "Fungsi Memulihkan Sinyal Digital Yang Terdistorsi?", "id": "Retimer/Reclocker." },
    { "en": "Fungsi Menambahkan Pre-emphasis Ke Sinyal?", "id": "Pre-emphasis Circuit." },
    { "en": "Fungsi Menyamakan Respon Frekuensi Kabel?", "id": "Cable Equalizer." },
    { "en": "Fungsi Melindungi Dari Kondisi 'Latch-Up'?", "id": "Latch-Up Protection Circuit." },
    { "en": "Fungsi Sirkuit Yang Dapat Menahan Nilai Analog?", "id": "Analog Memory." },
    { "en": "Fungsi Penguat Dengan Noise Sangat Rendah?", "id": "Ultra-Low Noise Amplifier." },
    { "en": "Fungsi Penguat Dengan Drift Sangat Rendah?", "id": "Zero-Drift Amplifier." },
    { "en": "Fungsi Penguat Dengan Arus Input Sangat Rendah?", "id": "Electrometer-Grade Amplifier." },
    { "en": "Fungsi Penguat Dengan Distorsi Sangat Rendah?", "id": "Ultra-Low Distortion Amplifier." },
    { "en": "Fungsi Penguat Dengan Bandwidth Sangat Lebar?", "id": "Wideband Amplifier." },
    { "en": "Fungsi Penguat Dengan Tegangan Catu Daya Tinggi?", "id": "High-Voltage Amplifier." },
    { "en": "Fungsi Penguat Yang Dapat Menghasilkan Arus Tinggi?", "id": "High-Current Amplifier." },
    { "en": "Fungsi Penguat Dengan Waktu Settling Sangat Cepat?", "id": "High-Speed Settling Amplifier." },
    { "en": "Fungsi Regulator Dengan Respon Transien Cepat?", "id": "Fast Transient Response LDO." },
    { "en": "Fungsi Regulator Dengan Arus Diam Sangat Rendah?", "id": "Nano-power Quiescent Current Regulator." },
    { "en": "Fungsi Regulator Dengan Noise Sangat Rendah?", "id": "Ultra-Low Noise LDO." },
    { "en": "Fungsi Regulator Dengan PSRR Sangat Tinggi?", "id": "High PSRR LDO." },
    { "en": "Fungsi Catu Daya Laboratorium?", "id": "Bench Power Supply." },
    { "en": "Fungsi Menguji Baterai Di Bawah Beban?", "id": "Battery Load Tester." },
    { "en": "Fungsi Memprogram IC Mikrokontroler Universal?", "id": "Universal Device Programmer." },
    { "en": "Fungsi Debugging Kode Pada Mikrokontroler?", "id": "In-Circuit Debugger/Emulator." },
    { "en": "Fungsi Mengendus Lalu Lintas Bus USB?", "id": "USB Protocol Analyzer." },
    { "en": "Fungsi Mengendus Lalu Lintas Jaringan Ethernet?", "id": "Network Sniffer/Packet Analyzer." },
    { "en": "Fungsi Menghasilkan Sinyal Uji Untuk Jaringan?", "id": "Network Test Signal Generator." },
    { "en": "Fungsi Sirkuit Yang Menguji Kabel Serat Optik?", "id": "Optical Fiber Tester (OTDR)." },
    { "en": "Fungsi Mengukur Daya Sinyal Optik?", "id": "Optical Power Meter." },
    { "en": "Fungsi Sirkuit Yang Menghasilkan Cahaya Terlihat?", "id": "Visible Light Source." },
    { "en": "Fungsi Sirkuit Yang Mensimulasikan Kegagalan?", "id": "Fault Injection Circuit." },
    { "en": "Fungsi Mengontrol Peralatan Menggunakan Suara?", "id": "Voice Recognition Controller." },
    { "en": "Fungsi Mengubah Teks Menjadi Ucapan?", "id": "Text-to-Speech (TTS) Synthesizer." },
    { "en": "Fungsi Sirkuit Yang Memutar File MP3?", "id": "MP3 Player Circuit." },
    { "en": "Fungsi Sirkuit Yang Merekam Video Digital?", "id": "Digital Video Recorder (DVR) Circuit." },
    { "en": "Fungsi Menentukan Lokasi Menggunakan Satelit?", "id": "GPS (Global Positioning System) Receiver." },
    { "en": "Fungsi Mengirimkan Data Melalui Jaringan Seluler?", "id": "Cellular Modem Circuit." },
    { "en": "Fungsi Sirkuit Yang Mengontrol Layar Sentuh?", "id": "Touchscreen Controller." },
    { "en": "Fungsi Mengukur Detak Jantung?", "id": "Heart Rate Monitor Circuit." },
    { "en": "Fungsi Sirkuit Yang Membaca Sidik Jari?", "id": "Fingerprint Scanner Circuit." },
    { "en": "Fungsi Menggerakkan Cermin Kecil Secara Cepat?", "id": "MEMS (Micro-Electro-Mechanical Systems) Mirror Driver." },
    { "en": "Fungsi Menghasilkan Plasma Dingin?", "id": "Cold Plasma Generator." },
    { "en": "Fungsi Mengukur Aliran Udara Dalam Pipa?", "id": "Mass Airflow Sensor (MAF) Circuit." },
    { "en": "Fungsi Mendeteksi Ketukan Pada Mesin?", "id": "Knock Sensor Amplifier." },
    { "en": "Fungsi Mengontrol Injektor Bahan Bakar?", "id": "Fuel Injector Driver." },
    { "en": "Fungsi Mengontrol Koil Pengapian?", "id": "Ignition Coil Driver." },
    { "en": "Fungsi Mengontrol Kipas Radiator Mobil?", "id": "Radiator Fan Controller." },
    { "en": "Fungsi Menstabilkan Putaran Mesin Saat Idle?", "id": "Idle Air Control (IAC) Circuit." },
    { "en": "Fungsi Mengontrol Sirkulasi Ulang Gas Buang?", "id": "EGR (Exhaust Gas Recirculation) Valve Controller." },
    { "en": "Fungsi Mengukur Kadar Oksigen Gas Buang?", "id": "Oxygen (O2) Sensor Amplifier." },
    { "en": "Fungsi Antarmuka Ke Jaringan Diagnostik Mobil?", "id": "OBD-II (On-Board Diagnostics) Interface." },
    { "en": "Fungsi Mengontrol Transmisi Otomatis?", "id": "Transmission Control Unit (TCU)." },
    { "en": "Fungsi Mengontrol Sistem Rem Anti-Terkunci?", "id": "ABS (Anti-lock Braking System) Controller." },
    { "en": "Fungsi Mengontrol Kantung Udara (Airbag)?", "id": "Airbag Control Module." },
    { "en": "Fungsi Mengontrol Lampu Depan Mobil?", "id": "Headlight Control Module." },
    { "en": "Fungsi Sirkuit Yang Mengedipkan Lampu Sein?", "id": "Flasher Relay Circuit." },
    { "en": "Fungsi Menghapus Embun Kaca Belakang?", "id": "Rear Window Defogger Timer." },
    { "en": "Fungsi Mengontrol Jeda Wiper Kaca Depan?", "id": "Intermittent Wiper Controller." },
    { "en": "Fungsi Mengontrol Kunci Pintu Mobil?", "id": "Central Locking System." },
    { "en": "Fungsi Mengontrol Jendela Mobil?", "id": "Power Window Control Circuit." },
    { "en": "Fungsi Sirkuit Alarm Mobil?", "id": "Car Alarm System." },
    { "en": "Fungsi Melacak Posisi Kendaraan?", "id": "GPS Vehicle Tracker." },
    { "en": "Fungsi Sirkuit Yang Memutar Audio Dari USB?", "id": "USB Audio Player." },
    { "en": "Fungsi Menampilkan Informasi Di Kaca Depan?", "id": "Heads-Up Display (HUD) Controller." },
    { "en": "Fungsi Mengukur Suhu Di Dalam Dan Luar Mobil?", "id": "Dual Thermometer Circuit." },
    { "en": "Fungsi Mengontrol Sistem Pendingin Udara (AC)?", "id": "Climate Control System." },
    { "en": "Fungsi Menguatkan Sinyal Antena Radio Mobil?", "id": "Antenna Booster." },
    { "en": "Fungsi Menggabungkan Video Dari Beberapa Kamera?", "id": "360-Degree Camera Stitching Unit." },
    { "en": "Fungsi Membantu Parkir Dengan Sensor Ultrasonik?", "id": "Parking Sensor Circuit." },
    { "en": "Fungsi Mengontrol Tekanan Angin Ban?", "id": "TPMS (Tire Pressure Monitoring System)." },
    { "en": "Fungsi Mengontrol Kestabilan Kendaraan?", "id": "ESC (Electronic Stability Control) System." },
    { "en": "Fungsi Mengontrol Distribusi Tenaga Penggerak Roda?", "id": "Traction Control System." },
    { "en": "Fungsi Sirkuit Pengapian Berbasis Transistor?", "id": "Transistorized Ignition System." },
    { "en": "Fungsi Mengukur Aliran Bahan Bakar?", "id": "Fuel Flow Meter." },
    { "en": "Fungsi Menampilkan Kecepatan Secara Digital?", "id": "Digital Speedometer." },
    { "en": "Fungsi Menampilkan Putaran Mesin (RPM)?", "id": "Digital Tachometer." },
    { "en": "Fungsi Sirkuit Yang Mengukur Percepatan?", "id": "Accelerometer Interface." },
    { "en": "Fungsi Menghasilkan Suara Klakson Elektronik?", "id": "Electronic Horn Circuit." },
    { "en": "Fungsi Mengontrol Lampu Interior Mobil?", "id": "Dome Light Controller." },
    { "en": "Fungsi Sirkuit Yang Menguji Aki Mobil?", "id": "Car Battery Tester." },
    { "en": "Fungsi Sirkuit Yang Mengisi Aki Mobil?", "id": "Car Battery Charger." },
    { "en": "Fungsi Melindungi Sistem Kelistrikan Dari Lonjakan?", "id": "Load Dump Protection Circuit." },
    { "en": "Fungsi Menyaring Derau Dari Alternator?", "id": "Alternator Noise Filter." },
    { "en": "Fungsi Mengontrol Output Tegangan Alternator?", "id": "Voltage Regulator (Alternator)." },
    { "en": "Fungsi Sirkuit Yang Menggerakkan Papan Iklan LED?", "id": "LED Signboard Controller." },
    { "en": "Fungsi Sirkuit Jam Digital Besar?", "id": "Jumbo Digital Clock." },
    { "en": "Fungsi Timer Untuk Papan Catur?", "id": "Chess Clock Circuit." },
    { "en": "Fungsi Sirkuit Yang Menghitung Jumlah Pengunjung?", "id": "Visitor Counter." },
    { "en": "Fungsi Sirkuit Yang Mensimulasikan Suara Dadu?", "id": "Electronic Dice." },
    { "en": "Fungsi Sirkuit Roda Roulette Elektronik?", "id": "Electronic Roulette Wheel." },
    { "en": "Fungsi Sirkuit Yang Mengukur Jarak?", "id": "Ultrasonic Distance Meter." },
    { "en": "Fungsi Sirkuit Yang Mengukur Kecepatan Objek?", "id": "Radar Speed Gun." },
    { "en": "Fungsi Sirkuit Yang Mendeteksi Kebohongan?", "id": "Polygraph Circuit." },
    { "en": "Fungsi Sirkuit Yang Mendeteksi Napas Beralkohol?", "id": "Breathalyzer Circuit." },
    { "en": "Fungsi Sirkuit Untuk Terapi Medan Magnet?", "id": "Pulsed Electromagnetic Field (PEMF) Generator." },
    { "en": "Fungsi Sirkuit Perangsang Saraf Listrik?", "id": "TENS (Transcutaneous Electrical Nerve Stimulation) Unit." },
    { "en": "Fungsi Sirkuit Pemijat Elektronik?", "id": "Electronic Massager Circuit." },
    { "en": "Fungsi Mengukur Detak Jantung Dengan Jari?", "id": "Finger-Based Heart Rate Monitor." },
    { "en": "Fungsi Mengukur Konduktivitas Kulit?", "id": "Galvanic Skin Response (GSR) Circuit." },
    { "en": "Fungsi Sirkuit Yang Mendeteksi Tingkat Stres?", "id": "Stress Meter." },
    { "en": "Fungsi Mengukur pH Larutan?", "id": "pH Meter." },
    { "en": "Fungsi Mengukur Konduktivitas Cairan?", "id": "Conductivity Meter." },
    { "en": "Fungsi Mengukur Kekeruhan Air?", "id": "Turbidity Meter." },
    { "en": "Fungsi Mengukur Kadar Oksigen Terlarut?", "id": "Dissolved Oxygen (DO) Meter." },
    { "en": "Fungsi Mengukur Intensitas Suara (Kebisingan)?", "id": "Sound Level Meter." },
    { "en": "Fungsi Sirkuit Yang Menguji Pendengaran?", "id": "Audiometer." },
    { "en": "Fungsi Alat Bantu Dengar?", "id": "Hearing Aid Circuit." },
    { "en": "Fungsi Mengurangi Derau Pada Alat Bantu Dengar?", "id": "Digital Noise Reduction (DNR) Circuit." },
    { "en": "Fungsi Mencegah Feedback Pada Alat Bantu Dengar?", "id": "Feedback Cancellation Circuit." },
    { "en": "Fungsi Sirkuit Yang Membaca Kartu Pintar (Smart Card)?", "id": "Smart Card Reader." },
    { "en": "Fungsi Mengontrol Akses Menggunakan Kartu?", "id": "RFID Access Control System." },
    { "en": "Fungsi Sistem Absensi Sidik Jari?", "id": "Fingerprint Attendance System." },
    { "en": "Fungsi Mengontrol Gerbang Otomatis?", "id": "Automatic Gate Controller." },
    { "en": "Fungsi Mengontrol Palang Parkir?", "id": "Barrier Gate Controller." },
    { "en": "Fungsi Sistem Antrian Otomatis?", "id": "Queue Management System." },
    { "en": "Fungsi Menggerakkan Lengan Robot?", "id": "Robotic Arm Controller." },
    { "en": "Fungsi Robot Yang Mengikuti Garis?", "id": "Line Follower Robot." },
    { "en": "Fungsi Robot Yang Menghindari Rintangan?", "id": "Obstacle Avoiding Robot." },
    { "en": "Fungsi Robot Yang Menyeimbangkan Diri?", "id": "Self-Balancing Robot." },
    { "en": "Fungsi Mengontrol Drone/Quadcopter?", "id": "Flight Controller." },
    { "en": "Fungsi Menstabilkan Orientasi Drone?", "id": "Inertial Measurement Unit (IMU)." },
    { "en": "Fungsi Mengirimkan Video Dari Drone?", "id": "Video Transmitter (VTX)." },
    { "en": "Fungsi Menerima Sinyal Kontrol Drone?", "id": "Radio Control (RC) Receiver." },
    { "en": "Fungsi Mengontrol Kecepatan Motor Drone?", "id": "ESC (Electronic Speed Controller)." },
    { "en": "Fungsi Mendistribusikan Daya Pada Drone?", "id": "Power Distribution Board (PDB)." },
    { "en": "Fungsi Mengontrol Lampu Navigasi Pesawat Model?", "id": "RC Navigation Light Controller." },
    { "en": "Fungsi Sirkuit Yang Menggerakkan Otot Buatan?", "id": "Shape-Memory Alloy (SMA) Driver." },
    { "en": "Fungsi Menghasilkan Getaran Haptic?", "id": "Haptic Feedback Driver." },
    { "en": "Fungsi Menggerakkan Penutur Piezoelektrik?", "id": "Piezo Driver." },
    { "en": "Fungsi Sirkuit Untuk Pengelasan Titik?", "id": "Spot Welder Controller." },
    { "en": "Fungsi Mengontrol Suhu Solder?", "id": "Soldering Iron Temperature Controller." },
    { "en": "Fungsi Memanaskan Benda Secara Induksi?", "id": "Induction Heater." },
    { "en": "Fungsi Sirkuit Untuk Pelapisan Logam?", "id": "Electroplating Rectifier." },
    { "en": "Fungsi Sirkuit Untuk Pemotongan Logam Plasma?", "id": "Plasma Cutter Inverter." },
    { "en": "Fungsi Menghasilkan Ultrasonik Untuk Pembersihan?", "id": "Ultrasonic Cleaner Driver." },
    { "en": "Fungsi Mengusir Nyamuk Dengan Suara?", "id": "Mosquito Repellent Circuit." },
    { "en": "Fungsi Mengionisasi Udara?", "id": "Air Ionizer Circuit." },
    { "en": "Fungsi Sirkuit Yang Memperkuat Sinyal Seismik?", "id": "Geophone Amplifier." },
    { "en": "Fungsi Mendeteksi Sambaran Petir?", "id": "Lightning Detector." },
    { "en": "Fungsi Menghitung Tetesan Infus?", "id": "IV Drip Counter." },
    { "en": "Fungsi Mengukur Laju Pernapasan?", "id": "Respiration Rate Monitor." },
    { "en": "Fungsi Sirkuit Yang Menguji Refleks?", "id": "Reflex Tester." },
    { "en": "Fungsi Mengukur Waktu Reaksi Manusia?", "id": "Human Reaction Timer." },
    { "en": "Fungsi Sirkuit Yang Menguji Penglihatan Warna?", "id": "Color Blindness Test Circuit." },
    { "en": "Fungsi Sirkuit Yang Mensimulasikan Suara Hujan?", "id": "Rain Sound Simulator." },
    { "en": "Fungsi Sirkuit Yang Mensimulasikan Suara Ombak?", "id": "Ocean Wave Sound Simulator." },
    { "en": "Fungsi Sirkuit Yang Mensimulasikan Detak Jantung?", "id": "Heartbeat Simulator." },
    { "en": "Fungsi Mengontrol Kecerahan Lampu Secara Bertahap?", "id": "Soft Fade Lamp Dimmer." },
    { "en": "Fungsi Lampu Yang Berkedip Secara Acak?", "id": "Random Flashing Light Circuit." },
    { "en": "Fungsi Lampu Darurat Yang Menyala Saat Listrik Padam?", "id": "Automatic Emergency Light." },
    { "en": "Fungsi Mengisi Baterai Cadangan Secara Otomatis?", "id": "Backup Battery Charger." },
    { "en": "Fungsi Memilih Sumber Daya Terbaik Secara Otomatis?", "id": "Automatic Power Source Selector." },
    { "en": "Fungsi Mengisolasi Catu Daya USB?", "id": "USB Power Isolator." },
    { "en": "Fungsi Menekan Derau Pada Jalur Data USB?", "id": "USB Data Line Filter." },
    { "en": "Fungsi Mengontrol Beberapa Perangkat Dengan Satu Tombol?", "id": "Single-Button Multi-Device Controller." },
    { "en": "Fungsi Sirkuit Pengingat Minum Obat?", "id": "Medicine Reminder Alarm." },
    { "en": "Fungsi Menghitung Mundur Waktu?", "id": "Countdown Timer." },
    { "en": "Fungsi Menghitung Waktu Maju (Stopwatch)?", "id": "Stopwatch Circuit." },
    { "en": "Fungsi Jam Digital Dengan Alarm?", "id": "Digital Alarm Clock." },
    { "en": "Fungsi Kalender Digital?", "id": "Digital Calendar Circuit." },
    { "en": "Fungsi Termometer Digital?", "id": "Digital Thermometer." },
    { "en": "Fungsi Barometer Digital?", "id": "Digital Barometer." },
    { "en": "Fungsi Pengukur Kelembaban Digital?", "id": "Digital Hygrometer." },
    { "en": "Fungsi Stasiun Cuaca Mini?", "id": "Mini Weather Station." },
    { "en": "Fungsi Sirkuit Yang Menampilkan Pesan Berjalan?", "id": "Scrolling Message Display." },
    { "en": "Fungsi Sirkuit Papan Tulis Elektronik?", "id": "Electronic Drawing Board." },
    { "en": "Fungsi Mengubah Suara Menjadi Pola Cahaya?", "id": "Color Organ Circuit." },
    { "en": "Fungsi Lampu Disko Sederhana?", "id": "Simple Disco Light Circuit." },
    { "en": "Fungsi Mengontrol Laser Untuk Pertunjukan Cahaya?", "id": "Laser Show Controller." },
    { "en": "Fungsi Mengontrol Mesin Asap?", "id": "Fog Machine Controller." },
    { "en": "Fungsi Mengontrol Mesin Gelembung?", "id": "Bubble Machine Controller." },
    { "en": "Fungsi Menggerakkan Bola Cermin?", "id": "Mirror Ball Motor Controller." },
    { "en": "Fungsi Sirkuit Piano Elektronik Sederhana?", "id": "Simple Electronic Piano." },
    { "en": "Fungsi Sirkuit Organ Elektronik?", "id": "Electronic Organ Circuit." },
    { "en": "Fungsi Gitar Efek Tremolo?", "id": "Tremolo Effect Pedal." },
    { "en": "Fungsi Gitar Efek Chorus?", "id": "Chorus Effect Pedal." },
    { "en": "Fungsi Gitar Efek Phaser?", "id": "Phaser Effect Pedal." },
    { "en": "Fungsi Gitar Efek Flanger?", "id": "Flanger Effect Pedal." },
    { "en": "Fungsi Gitar Efek Delay/Echo?", "id": "Delay/Echo Pedal." },
    { "en": "Fungsi Gitar Efek Reverb?", "id": "Reverb Pedal." },
    { "en": "Fungsi Gitar Efek Compressor?", "id": "Compressor Pedal." },
    { "en": "Fungsi Gitar Efek Noise Gate?", "id": "Noise Gate Pedal." },
    { "en": "Fungsi Gitar Efek Distortion/Overdrive?", "id": "Distortion/Overdrive Pedal." },
    { "en": "Fungsi Gitar Efek Fuzz?", "id": "Fuzz Pedal." },
    { "en": "Fungsi Memilih Di Antara Beberapa Efek Gitar?", "id": "Effects Loop Switcher." },
    { "en": "Fungsi Catu Daya Untuk Beberapa Efek Pedal?", "id": "Pedalboard Power Supply." },
    { "en": "Fungsi Sirkuit Penyangga Sinyal Gitar?", "id": "Guitar Buffer Circuit." },
    { "en": "Fungsi Membagi Sinyal Gitar Tanpa Kehilangan Sinyal?", "id": "Buffered Signal Splitter." },
    { "en": "Fungsi Mengubah Impedansi Pickup Gitar?", "id": "Pickup Impedance Converter." },
    { "en": "Fungsi Menguatkan Sinyal Pickup Piezo?", "id": "Piezo Pickup Preamp." },
    { "en": "Fungsi Mengontrol Volume Tanpa Mengubah Tone?", "id": "Treble Bleed Circuit." },
    { "en": "Fungsi Sirkuit Untuk Menyetem Gitar?", "id": "Guitar Tuner Circuit." },
    { "en": "Fungsi Sirkuit Untuk Latihan Ritme?", "id": "Metronome Circuit." },
    { "en": "Fungsi Merekam Loop Audio Singkat?", "id": "Looper Pedal." },
    { "en": "Fungsi Sirkuit Yang Mengubah Level Sinyal?", "id": "Attenuator Circuit." },
    { "en": "Fungsi Menguatkan Sinyal Mikrofon Untuk Mixer?", "id": "Microphone Preamplifier." },
    { "en": "Fungsi Mengatur Keseimbangan Stereo?", "id": "Balance Control Circuit." },
    { "en": "Fungsi Mengatur Lebar Gambar Stereo?", "id": "Stereo Width Control." },
    { "en": "Fungsi Membalik Fasa Salah Satu Kanal Audio?", "id": "Phase Inverter Switch." },
    { "en": "Fungsi Mengirim Sinyal Ke Headphone?", "id": "Headphone Distribution Amplifier." },
    { "en": "Fungsi Interkomunikasi Antar Ruangan?", "id": "Intercom System." },
    { "en": "Fungsi Sistem Panggilan Publik?", "id": "Public Address (PA) System." },
    { "en": "Fungsi Sirkuit Yang Mencegah Feedback Akustik?", "id": "Acoustic Feedback Suppressor." },
    { "en": "Fungsi Mengukur Waktu Gema Ruangan?", "id": "Reverberation Time Analyzer." },
    { "en": "Fungsi Menganalisis Frekuensi Ruangan Secara Real-time?", "id": "Real-Time Analyzer (RTA)." },
    { "en": "Fungsi Menghasilkan Sinyal Pink Noise?", "id": "Pink Noise Generator." },
    { "en": "Fungsi Menghasilkan Sinyal White Noise?", "id": "White Noise Generator." },
    { "en": "Fungsi Menghasilkan Sinyal Sweep Frekuensi?", "id": "Sweep Frequency Generator." },
    { "en": "Fungsi Mengukur Respons Frekuensi Speaker?", "id": "Speaker Frequency Response Tester." },
    { "en": "Fungsi Mengukur Impedansi Speaker?", "id": "Speaker Impedance Analyzer." },
    { "en": "Fungsi Mengukur Parameter Thiele/Small Speaker?", "id": "Thiele/Small Parameter Tester." },
    { "en": "Fungsi Menguji Polaritas Speaker?", "id": "Speaker Polarity Tester." },
    { "en": "Fungsi Sirkuit Untuk Menghilangkan Magnetisasi Head Tape?", "id": "Tape Head Degausser." },
    { "en": "Fungsi Menggerakkan Motor Kaset Dengan Kecepatan Stabil?", "id": "Capstan Motor Speed Controller." },
    { "en": "Fungsi Sirkuit Yang Menerima Sinyal Waktu Radio?", "id": "Radio Time Signal Receiver (WWV/DCF77)." },
    { "en": "Fungsi Jam Yang Sinkron Dengan Sinyal Waktu Atom?", "id": "Radio-Controlled Clock." },
    { "en": "Fungsi Mengukur Medan Magnet Bumi?", "id": "Magnetometer." },
    { "en": "Fungsi Mendeteksi Radiasi Nuklir?", "id": "Geiger Counter." },
    { "en": "Fungsi Menghasilkan Tegangan Tinggi Untuk Tabung Geiger?", "id": "High-Voltage Supply for Geiger Tube." },
    { "en": "Fungsi Mendeteksi Partikel Alfa, Beta, Gamma?", "id": "Radiation Detector." },
    { "en": "Fungsi Sirkuit Untuk Eksperimen Pertumbuhan Tanaman?", "id": "Plant Growth Monitoring System." },
    { "en": "Fungsi Sistem Irigasi Otomatis?", "id": "Automatic Irrigation System." },
    { "en": "Fungsi Mengontrol Suhu Dan Kelembaban Rumah Kaca?", "id": "Greenhouse Controller." },
    { "en": "Fungsi Memberi Makan Ikan Secara Otomatis?", "id": "Automatic Fish Feeder." },
    { "en": "Fungsi Mengontrol Pompa Dan Filter Akuarium?", "id": "Aquarium Controller." },
    { "en": "Fungsi Mengontrol Pemanas Akuarium?", "id": "Aquarium Heater Thermostat." },
    { "en": "Fungsi Sirkuit Penerangan Akuarium?", "id": "Aquarium Lighting Controller." },
    { "en": "Fungsi Sirkuit Penangkal Petir?", "id": "Lightning Rod System." },
    { "en": "Fungsi Sirkuit Penangkal Karat Elektronik?", "id": "Electronic Rust Protection." },
    { "en": "Fungsi Menghasilkan Medan Elektromagnetik Berdenyut?", "id": "Pulsed Electromagnetic Field (PEMF) Circuit." },
    { "en": "Fungsi Sirkuit Untuk Terapi Cahaya?", "id": "Light Therapy Circuit." },
    { "en": "Fungsi Mengukur Indeks UV Matahari?", "id": "UV Index Meter." },
    { "en": "Fungsi Sirkuit Yang Menguji Kualitas Air?", "id": "Water Quality Tester." },
    { "en": "Fungsi Mengukur TDS (Total Dissolved Solids) Air?", "id": "TDS Meter." },
    { "en": "Fungsi Membunuh Bakteri Dengan Sinar UV?", "id": "UV Water Sterilizer." },
    { "en": "Fungsi Memisahkan Air Menjadi Hidrogen Dan Oksigen?", "id": "Water Electrolysis Circuit." },
    { "en": "Fungsi Menghasilkan Hidrogen Untuk Sel Bahan Bakar?", "id": "Hydrogen Generator." },
    { "en": "Fungsi Mengubah Energi Kimia Menjadi Listrik?", "id": "Fuel Cell Control Circuit." },
    { "en": "Fungsi Sirkuit Yang Menggerakkan Kereta Maglev?", "id": "Maglev Propulsion System." },
    { "en": "Fungsi Mengangkat Beban Dengan Elektromagnet?", "id": "Electromagnetic Lifter." },
    { "en": "Fungsi Mengontrol Coil Relai Tanpa Kontak Mekanis?", "id": "Solid-State Relay (SSR) Driver." },
    { "en": "Fungsi Sirkuit Yang Mengikuti Kontur Amplop Sinyal?", "id": "Envelope Follower." },
    { "en": "Fungsi Menciptakan Suara Robotik Dengan Modulasi Suara?", "id": "Vocoder Circuit." },
    { "en": "Fungsi Mengalikan Dua Sinyal Audio (Efek Metalik)?", "id": "Ring Modulator." },
    { "en": "Fungsi Mengemulasi Speaker Leslie Berputar?", "id": "Leslie Speaker Simulator." },
    { "en": "Fungsi Menyesuaikan Banyak Pita Frekuensi Audio Tetap?", "id": "Graphic Equalizer." },
    { "en": "Fungsi Menyesuaikan Frekuensi, Gain, dan Q Audio?", "id": "Parametric Equalizer." },
    { "en": "Fungsi Penerima Radio Dengan Umpan Balik Positif?", "id": "Regenerative Receiver." },
    { "en": "Fungsi Penerima Radio Tanpa Frekuensi Menengah?", "id": "Direct Conversion Receiver." },
    { "en": "Fungsi Meningkatkan Ketajaman Filter LC?", "id": "Q-Multiplier Circuit." },
    { "en": "Fungsi Filter Frekuensi Sangat Sempit Menggunakan Kristal?", "id": "Crystal Filter." },
    { "en": "Fungsi Mengubah Frekuensi Radio Ke Band Lain?", "id": "Transverter." },
    { "en": "Fungsi Mengukur Frekuensi Resonansi Sirkuit LC?", "id": "Grid Dip Meter." },
    { "en": "Fungsi Menghilangkan Sulfat Dari Aki Timbal?", "id": "Battery Desulfator." },
    { "en": "Fungsi Mengarahkan Panel Surya Ke Matahari?", "id": "Solar Tracker Circuit." },
    { "en": "Fungsi Menghasilkan Pulsa Berdasarkan Laju Input?", "id": "Binary Rate Multiplier." },
    { "en": "Fungsi Encoder Yang Hanya Mengaktifkan Satu Output?", "id": "One-Hot Encoder." },
    { "en": "Fungsi Mengukur Resistansi Sangat Rendah?", "id": "Milliohmmeter Circuit." },
    { "en": "Fungsi Menunjukkan Level Logika (High/Low/Pulsating)?", "id": "Logic Probe." },
    { "en": "Fungsi Menghasilkan Frekuensi Sangat Stabil Dan Akurat?", "id": "Frequency Standard." },
    { "en": "Fungsi Menghasilkan Busur Listrik Tegangan Tinggi?", "id": "Jacob's Ladder Driver." },
    { "en": "Fungsi Menghasilkan Suara Dari Plasma?", "id": "Plasma Speaker Driver." },
    { "en": "Fungsi Menghasilkan Resonansi Tegangan Sangat Tinggi?", "id": "Tesla Coil Driver." },
    { "en": "Fungsi Sirkuit Fotografi Tegangan Tinggi?", "id": "Kirlian Photography Circuit." },
    { "en": "Fungsi Mengontrol Motor Mesin CNC?", "id": "CNC Motor Driver." },
    { "en": "Fungsi Papan Kontrol Utama Printer 3D?", "id": "3D Printer Controller Board." },
    { "en": "Fungsi Menguatkan Dan Mendistribusikan Sinyal Video?", "id": "Video Distribution Amplifier." },
    { "en": "Fungsi Menghasilkan Karakter Teks Di Layar?", "id": "Character Generator." },
    { "en": "Fungsi Menghilangkan Derau Impulsif Dari Sinyal?", "id": "Noise Blanker." },
    { "en": "Fungsi Mengontrol Penguatan Penguat Mikrofon Dari Jauh?", "id": "Remote Microphone Preamp." },
    { "en": "Fungsi Sirkuit Yang Menguji Kabel Listrik?", "id": "Continuity Tester." },
    { "en": "Fungsi Sirkuit Yang Menemukan Kabel Di Dinding?", "id": "Tone Generator and Probe." },
    { "en": "Fungsi Mengukur Medan Magnet?", "id": "Gaussmeter Circuit." },
    { "en": "Fungsi Mengukur Induktansi?", "id": "Inductance Meter." },
    { "en": "Fungsi Mengukur Kapasitansi?", "id": "Capacitance Meter." },
    { "en": "Fungsi Sirkuit Yang Mengukur Kecepatan Angin?", "id": "Anemometer Circuit." },
    { "en": "Fungsi Sirkuit Yang Menghasilkan Uap Air?", "id": "Ultrasonic Humidifier/Mist Maker." },
    { "en": "Fungsi Sirkuit Penggerak Aktuator Piezoelektrik?", "id": "Piezo Actuator Driver." },
    { "en": "Fungsi Sirkuit Penggerak Relai Latching?", "id": "Latching Relay Driver." },
    { "en": "Fungsi Menghasilkan Tegangan Bias Negatif?", "id": "Negative Bias Generator." },
    { "en": "Fungsi Catu Daya Dengan Pelacakan Tegangan?", "id": "Tracking Power Supply." },
    { "en": "Fungsi Catu Daya Untuk Laboratorium?", "id": "Bench Power Supply." },
    { "en": "Fungsi Sirkuit Penunda Penyalaan Daya?", "id": "Power-On Delay Circuit." },
    { "en": "Fungsi Sirkuit Penunda Pemutusan Daya?", "id": "Power-Off Delay Circuit." },
    { "en": "Fungsi Melindungi Dari Tegangan Transien Listrik Mobil?", "id": "Load Dump Protection." },
    { "en": "Fungsi Mengontrol Katup Proporsional?", "id": "Proportional Valve Driver." },
    { "en": "Fungsi Sirkuit Antarmuka Sensor Jembatan?", "id": "Bridge Sensor Interface." },
    { "en": "Fungsi Kompensasi Suhu Untuk Sensor?", "id": "Temperature Compensation Circuit." },
    { "en": "Fungsi Linearisasi Sinyal Sensor?", "id": "Sensor Linearization Circuit." },
    { "en": "Fungsi Menguatkan Sinyal Diferensial Dengan Common Mode?", "id": "Instrumentation Amplifier." },
    { "en": "Fungsi Penguat Dengan Penguatan Terkendali Secara Digital?", "id": "Digitally Programmable Gain Amplifier." },
    { "en": "Fungsi Mengubah Sinyal Digital Menjadi Sinyal Sinkron?", "id": "Synchro-to-Digital Converter." },
    { "en": "Fungsi Mengubah Sinyal Digital Menjadi Sinyal Resolver?", "id": "Digital-to-Resolver Converter." },
    { "en": "Fungsi Mengemulasi Perilaku Resolver?", "id": "Resolver Simulator." },
    { "en": "Fungsi Sirkuit Untuk Menyeimbangkan Jembatan Secara Otomatis?", "id": "Auto-Balancing Bridge." },
    { "en": "Fungsi Mengukur Pergeseran Fasa Antara Dua Sinyal?", "id": "Phase Meter." },
    { "en": "Fungsi Mengukur Faktor Daya?", "id": "Power Factor Meter." },
    { "en": "Fungsi Mengukur Watt-jam Secara Digital?", "id": "Digital Energy Meter." },
    { "en": "Fungsi Sirkuit Antarmuka Untuk Termokopel?", "id": "Thermocouple Amplifier." },
    { "en": "Fungsi Kompensasi Sambungan Dingin Termokopel?", "id": "Cold Junction Compensation." },
    { "en": "Fungsi Sirkuit Antarmuka Untuk Termistor?", "id": "Thermistor Interface Circuit." },
    { "en": "Fungsi Sirkuit Antarmuka Untuk RTD?", "id": "RTD Interface Circuit." },
    { "en": "Fungsi Sirkuit Penggerak Pemanas Berbasis Termistor?", "id": "Thermistor-Based Heater Controller." },
    { "en": "Fungsi Sirkuit Pengukur Kelembaban?", "id": "Humidity Sensor Circuit." },
    { "en": "Fungsi Sirkuit Pengukur Tekanan?", "id": "Pressure Sensor Amplifier." },
    { "en": "Fungsi Mengukur Laju Aliran Udara/Gas?", "id": "Airflow Sensor Circuit." },
    { "en": "Fungsi Sirkuit Antarmuka Sensor pH?", "id": "pH Sensor Amplifier." },
    { "en": "Fungsi Mengukur Konsentrasi Karbon Dioksida?", "id": "CO2 Sensor Circuit." },
    { "en": "Fungsi Sirkuit Yang Mendeteksi Alkohol?", "id": "Alcohol Sensor Circuit." },
    { "en": "Fungsi Sirkuit Yang Mendeteksi Gas Mudah Terbakar?", "id": "Combustible Gas Detector." },
    { "en": "Fungsi Sirkuit Yang Mendeteksi Getaran?", "id": "Vibration Switch." },
    { "en": "Fungsi Sirkuit Yang Mendeteksi Kemiringan?", "id": "Tilt Sensor Circuit." },
    { "en": "Fungsi Mengukur Percepatan Dan Getaran?", "id": "Accelerometer Amplifier." },
    { "en": "Fungsi Mengukur Kecepatan Rotasi?", "id": "Rotational Speed Sensor Circuit." },
    { "en": "Fungsi Mendeteksi Kehadiran Cairan?", "id": "Liquid Level Sensor." },
    { "en": "Fungsi Sirkuit Yang Mendeteksi Benda Logam?", "id": "Inductive Proximity Sensor." },
    { "en": "Fungsi Sirkuit Yang Mendeteksi Benda Apapun?", "id": "Capacitive Proximity Sensor." },
    { "en": "Fungsi Mendeteksi Objek Dengan Sinar Cahaya?", "id": "Photoelectric Sensor." },
    { "en": "Fungsi Sirkuit Yang Mengukur Jarak Dengan Laser?", "id": "Laser Distance Meter." },
    { "en": "Fungsi Sirkuit Pembaca Kode QR?", "id": "QR Code Reader Circuit." },
    { "en": "Fungsi Sirkuit Pembaca RFID?", "id": "RFID Reader." },
    { "en": "Fungsi Sirkuit Tag Aktif RFID?", "id": "Active RFID Tag." },
    { "en": "Fungsi Sirkuit Tag Pasif RFID?", "id": "Passive RFID Tag." },
    { "en": "Fungsi Sirkuit Antarmuka Layar Sentuh Resistif?", "id": "Resistive Touchscreen Controller." },
    { "en": "Fungsi Sirkuit Antarmuka Layar Sentuh Kapasitif?", "id": "Capacitive Touchscreen Controller." },
    { "en": "Fungsi Sirkuit Penggerak Motor Getar?", "id": "Vibration Motor Driver." },
    { "en": "Fungsi Sirkuit Penggerak Tampilan E-Ink?", "id": "E-Ink Display Driver." },
    { "en": "Fungsi Menghasilkan Suara Polifonik?", "id": "Polyphonic Sound Generator." },
    { "en": "Fungsi Sirkuit Untuk Efek Vokal Talk Box?", "id": "Talk Box Circuit." },
    { "en": "Fungsi Sirkuit Yang Mengontrol Pedal Sustain Piano?", "id": "Sustain Pedal Interface." },
    { "en": "Fungsi Mengubah Sinyal MIDI Ke Tegangan Kontrol?", "id": "MIDI to CV Converter." },
    { "en": "Fungsi Mengubah Tegangan Kontrol Ke Sinyal MIDI?", "id": "CV to MIDI Converter." },
    { "en": "Fungsi Sirkuit Clock Untuk Synthesizer Modular?", "id": "Modular Synth Clock Generator." },
    { "en": "Fungsi Sirkuit Sequencer Untuk Synthesizer?", "id": "Analog Sequencer." },
    { "en": "Fungsi Sirkuit Yang Mencuplik Dan Menahan Tegangan CV?", "id": "Sample and Hold (S&H) Module." },
    { "en": "Fungsi Sirkuit Yang Membatasi Laju Perubahan Tegangan?", "id": "Slew Limiter (Lag Processor)." },
    { "en": "Fungsi Menguatkan Sinyal Eksternal Ke Level Modular?", "id": "External Input Module." },
    { "en": "Fungsi Mengirim Sinyal Modular Ke Peralatan Eksternal?", "id": "Output Module." },
    { "en": "Fungsi Sirkuit Mixer Untuk Tegangan Kontrol?", "id": "CV Mixer." },
    { "en": "Fungsi Sirkuit Pengganda Tegangan Kontrol?", "id": "Voltage Multiplier." },
    { "en": "Fungsi Sirkuit Pembalik Polaritas Tegangan?", "id": "Polarity Inverter." },
    { "en": "Fungsi Sirkuit Pembagi Tegangan Kontrol?", "id": "Attenuator." },
    { "en": "Fungsi Sirkuit Yang Menggeser Level Tegangan Kontrol?", "id": "Voltage Offset (DC Offset)." },
    { "en": "Fungsi Memilih Di Antara Beberapa Sumber CV?", "id": "CV Switch." },
    { "en": "Fungsi Memuluskan Transisi Antara Tegangan Kontrol?", "id": "Slew Limiter (Portamento/Glide)." },
    { "en": "Fungsi Menghasilkan Tegangan Acak?", "id": "Random Voltage Generator." },
    { "en": "Fungsi Logika 'AND' Untuk Sinyal Gerbang/Pemicu?", "id": "Logic AND Gate." },
    { "en": "Fungsi Logika 'OR' Untuk Sinyal Gerbang/Pemicu?", "id": "Logic OR Gate." },
    { "en": "Fungsi Menghasilkan Kelipatan Frekuensi Clock?", "id": "Clock Multiplier." },
    { "en": "Fungsi Menghasilkan Pembagian Frekuensi Clock?", "id": "Clock Divider." },
    { "en": "Fungsi Menggeser Waktu Sinyal Pemicu?", "id": "Trigger Delay." },
    { "en": "Fungsi Mengirim Sinyal Audio Ke Perangkat Eksternal?", "id": "Send/Return Module." },
    { "en": "Fungsi Mengukur Tegangan Dengan Sangat Presisi?", "id": "Precision Voltmeter." },
    { "en": "Fungsi Sirkuit Yang Menguji Kabel MIDI?", "id": "MIDI Cable Tester." },
    { "en": "Fungsi Memantau Aktivitas Sinyal MIDI?", "id": "MIDI Monitor." },
    { "en": "Fungsi Menggabungkan Beberapa Aliran Sinyal MIDI?", "id": "MIDI Merger." },
    { "en": "Fungsi Membagi Satu Aliran MIDI Ke Banyak Output?", "id": "MIDI Thru Box." },
    { "en": "Fungsi Mengisolasi Jalur MIDI Secara Listrik?", "id": "MIDI Isolator." },
    { "en": "Fungsi Mengontrol Kecerahan Lampu Dengan Sinyal DMX?", "id": "DMX Dimmer Pack." },
    { "en": "Fungsi Mengontrol Gerakan Lampu Panggung?", "id": "Moving Head Controller." },
    { "en": "Fungsi Menerima Sinyal Kontrol DMX?", "id": "DMX Receiver." },
    { "en": "Fungsi Mengirim Sinyal Kontrol DMX?", "id": "DMX Transmitter." },
    { "en": "Fungsi Memisahkan Dan Memperkuat Sinyal DMX?", "id": "DMX Splitter/Booster." },
    { "en": "Fungsi Menguji Jalur Sinyal DMX?", "id": "DMX Tester." },
    { "en": "Fungsi Antarmuka DMX Ke Komputer?", "id": "USB to DMX Interface." },
    { "en": "Fungsi Sirkuit Pemindai Kode Batang?", "id": "Barcode Reader." },
    { "en": "Fungsi Sirkuit Pembaca Kartu Cerdas?", "id": "Smart Card Reader." },
    { "en": "Fungsi Mengemulasi Kartu Cerdas?", "id": "Smart Card Emulator." },
    { "en": "Fungsi Menganalisis Komunikasi Kartu Cerdas?", "id": "Smart Card Protocol Analyzer." },
    { "en": "Fungsi Menguji Fungsionalitas Keyboard?", "id": "Keyboard Tester." },
    { "en": "Fungsi Mengemulasi Penekanan Tombol Keyboard?", "id": "Keyboard Emulator." },
    { "en": "Fungsi Sirkuit Yang Menerjemahkan Protokol Keyboard?", "id": "Keyboard Protocol Converter." },
    { "en": "Fungsi Sirkuit Yang Menerima Input Dari Mouse?", "id": "Mouse Interface." },
    { "en": "Fungsi Sirkuit Yang Menggerakkan Kursor Mouse?", "id": "Mouse Cursor Controller." },
    { "en": "Fungsi Mengemulasi Gerakan Mouse?", "id": "Mouse Jiggler/Emulator." },
    { "en": "Fungsi Sirkuit Yang Menguji Joystick?", "id": "Joystick Tester." },
    { "en": "Fungsi Sirkuit Yang Menguji Monitor Komputer?", "id": "Monitor Tester." },
    { "en": "Fungsi Sirkuit Yang Menguji Catu Daya Komputer?", "id": "Power Supply Tester." },
    { "en": "Fungsi Sirkuit Yang Menguji RAM Komputer?", "id": "RAM Tester." },
    { "en": "Fungsi Sirkuit Yang Menguji Hard Drive?", "id": "Hard Drive Tester." },
    { "en": "Fungsi Sirkuit Yang Memulihkan Data?", "id": "Data Recovery Circuit." },
    { "en": "Fungsi Menghapus Data Secara Aman?", "id": "Secure Data Eraser." },
    { "en": "Fungsi Sirkuit Yang Menyadap Komunikasi Serial?", "id": "Serial Port Sniffer." },
    { "en": "Fungsi Mengontrol Sistem Akuisisi Data?", "id": "Data Acquisition (DAQ) System." },
    { "en": "Fungsi Mengkondisikan Sinyal Sebelum Akuisisi Data?", "id": "Signal Conditioning Front-End." },
    { "en": "Fungsi Membangkitkan Sinyal Uji Arbitrer?", "id": "Arbitrary Function Generator." },
    { "en": "Fungsi Sirkuit Yang Menganalisis Respon Sistem?", "id": "System Identification Circuit." },
    { "en": "Fungsi Mengontrol Proses Industri Secara Otomatis?", "id": "Programmable Logic Controller (PLC)." },
    { "en": "Fungsi Antarmuka Antara PLC Dan Manusia?", "id": "Human-Machine Interface (HMI)." },
    { "en": "Fungsi Sirkuit Yang Mengontrol Proses Batch?", "id": "Batch Process Controller." },
    { "en": "Fungsi Sirkuit Yang Mengontrol Gerakan Presisi?", "id": "Motion Controller." },
    { "en": "Fungsi Sirkuit Yang Mengontrol Pompa?", "id": "Pump Controller." },
    { "en": "Fungsi Sirkuit Yang Mengontrol Kompresor?", "id": "Compressor Controller." },
    { "en": "Fungsi Sirkuit Yang Mengontrol Konveyor?", "id": "Conveyor Belt Controller." },
    { "en": "Fungsi Sistem Penglihatan Untuk Mesin?", "id": "Machine Vision System." },
    { "en": "Fungsi Menerangi Objek Untuk Penglihatan Mesin?", "id": "Machine Vision Lighting Controller." },
    { "en": "Fungsi Sirkuit Pemicu Kamera Kecepatan Tinggi?", "id": "High-Speed Camera Trigger." },
    { "en": "Fungsi Sinkronisasi Beberapa Kamera?", "id": "Multi-Camera Synchronization." },
    { "en": "Fungsi Sirkuit Yang Mendeteksi Cacat Produk?", "id": "Automated Optical Inspection (AOI)." },
    { "en": "Fungsi Sirkuit Yang Mengukur Dimensi Objek?", "id": "Optical Measurement System." },
    { "en": "Fungsi Sirkuit Untuk Menimbang Benda Bergerak?", "id": "In-Motion Weighing System." },
    { "en": "Fungsi Sirkuit Pengisi Botol Otomatis?", "id": "Automatic Bottle Filling System." },
    { "en": "Fungsi Sirkuit Pemasang Tutup Botol Otomatis?", "id": "Automatic Capping Machine." },
    { "en": "Fungsi Sirkuit Pelabelan Produk Otomatis?", "id": "Automatic Labeling Machine." },
    { "en": "Fungsi Sistem Penyortiran Otomatis?", "id": "Automatic Sorting System." },
    { "en": "Fungsi Sirkuit Untuk Pengelasan Otomatis?", "id": "Robotic Welding Controller." },
    { "en": "Fungsi Sirkuit Untuk Pengecatan Otomatis?", "id": "Robotic Painting Controller." },
    { "en": "Fungsi Sistem Penyimpanan Dan Pengambilan Otomatis?", "id": "AS/RS (Automated Storage/Retrieval System)." },
    { "en": "Fungsi Kendaraan Pemandu Otomatis?", "id": "AGV (Automated Guided Vehicle) Controller." },
    { "en": "Fungsi Sirkuit Yang Menguji Kekuatan Material?", "id": "Material Tensile Strength Tester." },
    { "en": "Fungsi Sirkuit Yang Menguji Kekerasan Material?", "id": "Hardness Tester." },
    { "en": "Fungsi Sirkuit Untuk Analisis Getaran?", "id": "Vibration Analyzer." },
    { "en": "Fungsi Sirkuit Untuk Analisis Akustik?", "id": "Acoustic Analyzer." },
    { "en": "Fungsi Sirkuit Untuk Analisis Termal?", "id": "Thermal Imaging System." },
    { "en": "Fungsi Sirkuit Pengukur Ketebalan Lapisan?", "id": "Coating Thickness Gauge." },
    { "en": "Fungsi Sirkuit Pengukur Kekasaran Permukaan?", "id": "Surface Roughness Tester." },
    { "en": "Fungsi Sirkuit Untuk Analisis Kimia?", "id": "Spectrometer." },
    { "en": "Fungsi Sirkuit Untuk Kromatografi?", "id": "Chromatography Controller." },
    { "en": "Fungsi Sirkuit Untuk Pengurutan DNA?", "id": "DNA Sequencer." },
    { "en": "Fungsi Sirkuit Untuk Reaksi Berantai Polimerase?", "id": "PCR (Polymerase Chain Reaction) Machine." },
    { "en": "Fungsi Sirkuit Untuk Elektroforesis?", "id": "Electrophoresis Power Supply." },
    { "en": "Fungsi Sirkuit Pemintal Sentrifugal?", "id": "Centrifuge Controller." },
    { "en": "Fungsi Sirkuit Pengaduk Magnetik?", "id": "Magnetic Stirrer." },
    { "en": "Fungsi Sirkuit Pompa Peristaltik?", "id": "Peristaltic Pump Controller." },
    { "en": "Fungsi Sirkuit Pompa Vakum?", "id": "Vacuum Pump Controller." },
    { "en": "Fungsi Sirkuit Pengontrol Aliran Massa?", "id": "Mass Flow Controller." },
    { "en": "Fungsi Sirkuit Pengontrol Tekanan?", "id": "Pressure Controller." },
    { "en": "Fungsi Sirkuit Pengontrol Suhu?", "id": "Temperature Controller." },
    { "en": "Fungsi Sirkuit Pengontrol Kelembaban?", "id": "Humidity Controller." },
    { "en": "Fungsi Sirkuit Yang Mengemulasi Panel Surya?", "id": "Solar Array Simulator." },
    { "en": "Fungsi Sirkuit Yang Mengemulasi Jaringan Listrik?", "id": "Grid Simulator." },
    { "en": "Fungsi Sirkuit Yang Mengemulasi Gangguan Jaringan?", "id": "Grid Fault Simulator." },
    { "en": "Fungsi Sirkuit Yang Menguji Inverter Panel Surya?", "id": "Solar Inverter Tester." },
    { "en": "Fungsi Sirkuit Yang Menguji Pengisi Daya Baterai?", "id": "Battery Charger Tester." },
    { "en": "Fungsi Sirkuit Yang Menguji Efisiensi Motor?", "id": "Motor Efficiency Tester." },
    { "en": "Fungsi Sirkuit Yang Mengukur Torsi Dan Kecepatan?", "id": "Dynamometer." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Getaran?", "id": "Vibration Test System (Shaker)." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Iklim?", "id": "Environmental Chamber Controller." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Semprotan Garam?", "id": "Salt Spray Test Controller." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Hujan?", "id": "Rain Test Chamber Controller." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Debu?", "id": "Dust Test Chamber Controller." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Kejut Termal?", "id": "Thermal Shock Chamber Controller." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Ketinggian?", "id": "Altitude Chamber Controller." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Tekanan Berlebih?", "id": "Overpressure Test System." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Ledakan?", "id": "Explosion-Proof Test System." },
    { "en": "Fungsi Sirkuit Untuk Pengujian EMC?", "id": "EMC Test System." },
    { "en": "Fungsi Ruangan Yang Melindungi Dari Sinyal RF?", "id": "Anechoic Chamber." },
    { "en": "Fungsi Sirkuit Untuk Pengujian ESD?", "id": "ESD Gun/Simulator." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Lonjakan (Surge)?", "id": "Surge Generator." },
    { "en": "Fungsi Sirkuit Untuk Pengujian EFT (Electrical Fast Transient)?", "id": "EFT Burst Generator." },
    { "en": "Fungsi Antena Untuk Pengujian Emisi Radiasi?", "id": "Broadband Antenna." },
    { "en": "Fungsi Penjepit Untuk Pengujian Emisi Konduksi?", "id": "LISN (Line Impedance Stabilization Network)." },
    { "en": "Fungsi Sirkuit Untuk Mengukur Intensitas Medan RF?", "id": "Field Strength Meter." },
    { "en": "Fungsi Sirkuit Yang Menguji Peralatan Medis?", "id": "Medical Device Tester." },
    { "en": "Fungsi Sirkuit Yang Menguji Defibrilator?", "id": "Defibrillator Analyzer." },
    { "en": "Fungsi Sirkuit Yang Menguji Mesin EKG?", "id": "ECG Simulator." },
    { "en": "Fungsi Sirkuit Yang Menguji Pompa Infus?", "id": "Infusion Pump Analyzer." },
    { "en": "Fungsi Sirkuit Yang Menguji Ventilator?", "id": "Ventilator Tester." },
    { "en": "Fungsi Sirkuit Yang Menguji Mesin Anestesi?", "id": "Anesthesia Machine Tester." },
    { "en": "Fungsi Sirkuit Yang Menguji Unit Bedah Listrik?", "id": "Electrosurgical Unit (ESU) Analyzer." },
    { "en": "Fungsi Sirkuit Yang Menguji Monitor Pasien?", "id": "Patient Simulator." },
    { "en": "Fungsi Sirkuit Untuk Kalibrasi Peralatan Medis?", "id": "Medical Calibration System." },
    { "en": "Fungsi Sistem Peringatan Panggilan Perawat?", "id": "Nurse Call System." },
    { "en": "Fungsi Sistem Pemantauan Pasien Jarak Jauh?", "id": "Remote Patient Monitoring System." },
    { "en": "Fungsi Sirkuit Yang Mengukur Glukosa Darah?", "id": "Blood Glucose Meter." },
    { "en": "Fungsi Sirkuit Yang Menganalisis Sampel Darah?", "id": "Blood Analyzer." },
    { "en": "Fungsi Sirkuit Mesin Dialisis (Cuci Darah)?", "id": "Dialysis Machine Controller." },
    { "en": "Fungsi Sirkuit Pompa Jantung-Paru?", "id": "Heart-Lung Machine Controller." },
    { "en": "Fungsi Sirkuit Inkubator Bayi?", "id": "Infant Incubator Controller." },
    { "en": "Fungsi Sirkuit Untuk Terapi Oksigen?", "id": "Oxygen Concentrator." },
    { "en": "Fungsi Sirkuit Untuk Alat Bantu Pernapasan?", "id": "CPAP/BiPAP Machine." },
    { "en": "Fungsi Sirkuit Untuk Endoskopi?", "id": "Endoscopy Light Source and Camera." },
    { "en": "Fungsi Sirkuit Untuk Operasi Laser?", "id": "Surgical Laser Controller." },
    { "en": "Fungsi Sirkuit Untuk Pencitraan Ultrasound?", "id": "Ultrasound Imaging System." },
    { "en": "Fungsi Sirkuit Untuk Pencitraan Sinar-X?", "id": "X-Ray Machine Controller." },
    { "en": "Fungsi Sirkuit Untuk CT Scan?", "id": "CT Scanner Control System." },
    { "en": "Fungsi Sirkuit Untuk MRI?", "id": "MRI Control System." },
    { "en": "Fungsi Sirkuit Untuk PET Scan?", "id": "PET Scanner Electronics." },
    { "en": "Fungsi Sirkuit Untuk Terapi Radiasi?", "id": "Linear Accelerator (LINAC) Controller." },
    { "en": "Fungsi Sirkuit Penggerak Kursi Roda Listrik?", "id": "Electric Wheelchair Controller." },
    { "en": "Fungsi Sirkuit Tempat Tidur Rumah Sakit?", "id": "Hospital Bed Controller." },
    { "en": "Fungsi Sirkuit Pengangkat Pasien?", "id": "Patient Lift Controller." },
    { "en": "Fungsi Sirkuit Untuk Mikroskop Elektron?", "id": "Electron Microscope Controller." },
    { "en": "Fungsi Sirkuit Untuk Spektrometer Massa?", "id": "Mass Spectrometer Electronics." },
    { "en": "Fungsi Sirkuit Untuk Mesin PCR?", "id": "PCR Thermal Cycler." },
    { "en": "Fungsi Sirkuit Untuk Penganalisis Genetik?", "id": "Genetic Analyzer." },
    { "en": "Fungsi Sirkuit Untuk Laboratorium-di-atas-Chip?", "id": "Lab-on-a-Chip (LOC) Controller." },
    { "en": "Fungsi Sirkuit Untuk Pencetakan 3D Bio?", "id": "Bioprinter Controller." },
    { "en": "Fungsi Sirkuit Untuk Mikromanipulator?", "id": "Micromanipulator Controller." },
    { "en": "Fungsi Sirkuit Untuk Sistem Optogenetik?", "id": "Optogenetics Control System." },
    { "en": "Fungsi Sirkuit Untuk Pencitraan Kalsium?", "id": "Calcium Imaging System." },
    { "en": "Fungsi Sirkuit Untuk Patch Clamp?", "id": "Patch Clamp Amplifier." },
    { "en": "Fungsi Sirkuit Untuk Neurostimulasi?", "id": "Neurostimulator." },
    { "en": "Fungsi Sirkuit Untuk Antarmuka Otak-Komputer?", "id": "Brain-Computer Interface (BCI)." },
    { "en": "Fungsi Sirkuit Yang Mengemulasi Neuron?", "id": "Neuromorphic Circuit." },
    { "en": "Fungsi Sirkuit Untuk Komputasi Kuantum?", "id": "Quantum Computer Control System." },
    { "en": "Fungsi Sirkuit Untuk Mengontrol Qubit?", "id": "Qubit Controller." },
    { "en": "Fungsi Menguatkan Sinyal Kuantum Yang Sangat Lemah?", "id": "Quantum Amplifier." },
    { "en": "Fungsi Sirkuit Yang Mengukur Keadaan Kuantum?", "id": "Quantum Measurement Circuit." },
    { "en": "Fungsi Sirkuit Untuk Komunikasi Kuantum?", "id": "Quantum Communication System." },
    { "en": "Fungsi Sirkuit Untuk Distribusi Kunci Kuantum?", "id": "Quantum Key Distribution (QKD) System." },
    { "en": "Fungsi Sirkuit Detektor Foton Tunggal?", "id": "Single-Photon Detector." },
    { "en": "Fungsi Sirkuit Untuk Menghitung Foton?", "id": "Photon Counter." },
    { "en": "Fungsi Sirkuit Untuk Menghasilkan Foton Terkait?", "id": "Entangled Photon Source." },
    { "en": "Fungsi Sirkuit Untuk Jam Atom?", "id": "Atomic Clock." },
    { "en": "Fungsi Sirkuit Untuk Mendinginkan Atom Dengan Laser?", "id": "Laser Cooling System." },
    { "en": "Fungsi Sirkuit Untuk Perangkap Magneto-Optik?", "id": "Magneto-Optical Trap (MOT) Controller." },
    { "en": "Fungsi Sirkuit Untuk Akselerator Partikel?", "id": "Particle Accelerator Control System." },
    { "en": "Fungsi Sirkuit Untuk Detektor Partikel?", "id": "Particle Detector Readout." },
    { "en": "Fungsi Sirkuit Untuk Reaktor Fusi?", "id": "Fusion Reactor Control System (Tokamak)." },
    { "en": "Fungsi Sirkuit Untuk Teleskop Radio?", "id": "Radio Telescope Receiver." },
    { "en": "Fungsi Sirkuit Untuk Interferometri Astronomi?", "id": "Astronomical Interferometer." },
    { "en": "Fungsi Sirkuit Untuk Optik Adaptif?", "id": "Adaptive Optics Control System." },
    { "en": "Fungsi Sirkuit Untuk Memandu Teleskop?", "id": "Telescope Autoguider." },
    { "en": "Fungsi Sirkuit Penggerak Pemasangan Teleskop?", "id": "Telescope Mount Controller." },
    { "en": "Fungsi Sirkuit Untuk Astrofotografi?", "id": "Astrophotography Camera Controller." },
    { "en": "Fungsi Sirkuit Untuk Spektroskopi Astronomi?", "id": "Astronomical Spectrograph." },
    { "en": "Fungsi Sirkuit Untuk Fotometri Astronomi?", "id": "Astronomical Photometer." },
    { "en": "Fungsi Sirkuit Untuk Mendeteksi Sinar Kosmik?", "id": "Cosmic Ray Detector." },
    { "en": "Fungsi Sirkuit Untuk Mendeteksi Neutrino?", "id": "Neutrino Detector." },
    { "en": "Fungsi Sirkuit Untuk Mendeteksi Gelombang Gravitasi?", "id": "Gravitational Wave Detector (LIGO/Virgo)." },
    { "en": "Fungsi Sirkuit Untuk Komunikasi Satelit?", "id": "Satellite Transponder." },
    { "en": "Fungsi Sirkuit Untuk Stasiun Bumi Satelit?", "id": "Satellite Ground Station." },
    { "en": "Fungsi Sirkuit Untuk Panel Surya Satelit?", "id": "Satellite Solar Panel Regulator." },
    { "en": "Fungsi Sirkuit Sistem Daya Satelit?", "id": "Satellite Power Control Unit." },
    { "en": "Fungsi Sirkuit Kontrol Sikap Satelit?", "id": "Attitude Control System (ACS)." },
    { "en": "Fungsi Sirkuit Untuk Komando Dan Telemetri Satelit?", "id": "Telemetry and Command (T&C) System." },
    { "en": "Fungsi Sirkuit Pendorong Ion?", "id": "Ion Thruster Controller." },
    { "en": "Fungsi Sirkuit Untuk Rover Planet?", "id": "Planetary Rover Electronics." },
    { "en": "Fungsi Sirkuit Untuk Pendarat Planet?", "id": "Planetary Lander Electronics." },
    { "en": "Fungsi Sirkuit Untuk Instrumen Luar Angkasa?", "id": "Space Instrument Electronics." },
    { "en": "Fungsi Sirkuit Tahan Radiasi?", "id": "Radiation-Hardened (Rad-Hard) Circuit." },
    { "en": "Fungsi Sirkuit Yang Mengoreksi Kesalahan Akibat Radiasi?", "id": "SEU (Single-Event Upset) Mitigation Circuit." },
    { "en": "Fungsi Sistem Navigasi Inersia?", "id": "Inertial Navigation System (INS)." },
    { "en": "Fungsi Sirkuit Untuk Radar Cuaca?", "id": "Weather Radar System." },
    { "en": "Fungsi Sirkuit Untuk Radar Penembus Tanah?", "id": "Ground-Penetrating Radar (GPR)." },
    { "en": "Fungsi Sirkuit Untuk Sonar Pencitraan?", "id": "Imaging Sonar System." },
    { "en": "Fungsi Sirkuit Untuk Kendaraan Bawah Air?", "id": "AUV/ROV (Autonomous/Remotely Operated Vehicle) Electronics." },
    { "en": "Fungsi Sirkuit Untuk Komunikasi Akustik Bawah Air?", "id": "Underwater Acoustic Modem." },
    { "en": "Fungsi Sirkuit Untuk Seismograf?", "id": "Seismograph Amplifier and Digitizer." },
    { "en": "Fungsi Sirkuit Untuk Hidrofon?", "id": "Hydrophone Preamplifier." },
    { "en": "Fungsi Sirkuit Untuk Pelampung Oseanografi?", "id": "Oceanographic Buoy Electronics." },
    { "en": "Fungsi Sirkuit Untuk Alat Survei Geofisika?", "id": "Geophysical Survey Equipment." },
    { "en": "Fungsi Sirkuit Untuk Logging Sumur Minyak?", "id": "Well Logging Tool Electronics." },
    { "en": "Fungsi Sirkuit Untuk Kendaraan Inspeksi Pipa?", "id": "Pipeline Inspection Gauge (PIG) Electronics." },
    { "en": "Fungsi Sirkuit Untuk Sistem Kontrol Kereta Api?", "id": "Railway Signaling System." },
    { "en": "Fungsi Sirkuit Deteksi Kereta Api Di Rel?", "id": "Track Circuit." },
    { "en": "Fungsi Sirkuit Pengontrol Palang Pintu Kereta?", "id": "Railway Crossing Controller." },
    { "en": "Fungsi Sirkuit 'Orang Mati' (Dead Man's Switch)?", "id": "Dead Man's Switch." },
    { "en": "Fungsi Sirkuit Perekam Data Perjalanan?", "id": "Event Data Recorder (Black Box)." },
    { "en": "Fungsi Sirkuit Untuk Sistem Kontrol Lift?", "id": "Elevator Control System." },
    { "en": "Fungsi Sirkuit Untuk Sistem Kontrol Eskalator?", "id": "Escalator Control System." },
    { "en": "Fungsi Sirkuit Untuk Pintu Geser Otomatis?", "id": "Automatic Sliding Door Controller." },
    { "en": "Fungsi Sirkuit Untuk Gerbang Putar (Turnstile)?", "id": "Turnstile Control Circuit." },
    { "en": "Fungsi Sirkuit Untuk Mesin Penjual Otomatis?", "id": "Vending Machine Controller." },
    { "en": "Fungsi Sirkuit Untuk Mesin Kopi Otomatis?", "id": "Coffee Machine Controller." },
    { "en": "Fungsi Sirkuit Untuk Mesin Cuci?", "id": "Washing Machine Controller." },
    { "en": "Fungsi Sirkuit Untuk Pengering Pakaian?", "id": "Clothes Dryer Controller." },
    { "en": "Fungsi Sirkuit Untuk Kulkas?", "id": "Refrigerator Control Board." },
    { "en": "Fungsi Sirkuit Untuk Pendingin Ruangan (AC)?", "id": "Air Conditioner Controller." },
    { "en": "Fungsi Sirkuit Untuk Oven Microwave?", "id": "Microwave Oven Controller." },
    { "en": "Fungsi Sirkuit Untuk Kompor Induksi?", "id": "Induction Cooktop Driver." },
    { "en": "Fungsi Sirkuit Untuk Pemanas Air Listrik?", "id": "Electric Water Heater Thermostat." },
    { "en": "Fungsi Sirkuit Untuk Pembersih Vakum?", "id": "Vacuum Cleaner Motor Controller." },
    { "en": "Fungsi Sirkuit Untuk Penanak Nasi?", "id": "Rice Cooker Control Circuit." },
    { "en": "Fungsi Sirkuit Untuk Blender?", "id": "Blender Speed Controller." },
    { "en": "Fungsi Sirkuit Untuk Kipas Angin?", "id": "Fan Speed Regulator." },
    { "en": "Fungsi Sirkuit Untuk Sikat Gigi Listrik?", "id": "Electric Toothbrush Charger and Controller." },
    { "en": "Fungsi Sirkuit Untuk Alat Cukur Listrik?", "id": "Electric Shaver Control Circuit." },
    { "en": "Fungsi Sirkuit Untuk Pengering Rambut?", "id": "Hair Dryer Control Circuit." },
    { "en": "Fungsi Sirkuit Untuk Timbangan Digital?", "id": "Digital Scale Circuit." },
    { "en": "Fungsi Sirkuit Untuk Jam Tangan Digital?", "id": "Digital Watch Circuit." },
    { "en": "Fungsi Sirkuit Untuk Kalkulator?", "id": "Calculator Circuit." },
    { "en": "Fungsi Sirkuit Untuk Mainan Elektronik?", "id": "Electronic Toy Circuit." },
    { "en": "Fungsi Sirkuit Untuk Konsol Game Video?", "id": "Video Game Console." },
    { "en": "Fungsi Sirkuit Untuk Kontroler Game?", "id": "Game Controller Interface." },
    { "en": "Fungsi Sirkuit Untuk Efek Getar Pada Kontroler?", "id": "Rumble/Haptic Feedback Driver." },
    { "en": "Fungsi Sirkuit Untuk Mouse Komputer?", "id": "Optical Mouse Sensor Circuit." },
    { "en": "Fungsi Sirkuit Untuk Keyboard Komputer?", "id": "Keyboard Matrix Scanner." },
    { "en": "Fungsi Sirkuit Untuk Webcam?", "id": "Webcam Image Processor." },
    { "en": "Fungsi Sirkuit Untuk Mikrofon USB?", "id": "USB Microphone Preamp and ADC." },
    { "en": "Fungsi Sirkuit Untuk Headset Bluetooth?", "id": "Bluetooth Headset Circuit." },
    { "en": "Fungsi Sirkuit Untuk Speaker Bluetooth?", "id": "Bluetooth Speaker Circuit." },
    { "en": "Fungsi Sirkuit Untuk Router Wi-Fi?", "id": "Wi-Fi Router." },
    { "en": "Fungsi Sirkuit Untuk Modem?", "id": "Modem Circuit." },
    { "en": "Fungsi Sirkuit Untuk Switch Jaringan?", "id": "Network Switch." },
    { "en": "Fungsi Sirkuit Untuk Kartu Jaringan?", "id": "Network Interface Card (NIC)." },
    { "en": "Fungsi Sirkuit Untuk Titik Akses Nirkabel?", "id": "Wireless Access Point (WAP)." },
    { "en": "Fungsi Sirkuit Untuk Repeater Wi-Fi?", "id": "Wi-Fi Range Extender/Repeater." },
    { "en": "Fungsi Sirkuit Untuk Firewall Perangkat Keras?", "id": "Hardware Firewall." },
    { "en": "Fungsi Sirkuit Untuk Sistem Deteksi Intrusi?", "id": "Intrusion Detection System (IDS)." },
    { "en": "Fungsi Sirkuit Untuk Sistem Pencegahan Intrusi?", "id": "Intrusion Prevention System (IPS)." },
    { "en": "Fungsi Sirkuit Untuk Kriptografi Perangkat Keras?", "id": "Hardware Security Module (HSM)." },
    { "en": "Fungsi Sirkuit Untuk Akselerasi Kriptografi?", "id": "Cryptographic Accelerator." },
    { "en": "Fungsi Sirkuit Untuk Komputasi Performa Tinggi?", "id": "High-Performance Computing (HPC) Cluster." },
    { "en": "Fungsi Sirkuit Untuk Server?", "id": "Server Motherboard." },
    { "en": "Fungsi Sirkuit Manajemen Server Jarak Jauh?", "id": "Baseboard Management Controller (BMC)." },
    { "en": "Fungsi Catu Daya Redundan Untuk Server?", "id": "Redundant Power Supply." },
    { "en": "Fungsi Sirkuit Untuk Sistem Penyimpanan Jaringan?", "id": "Network-Attached Storage (NAS)." },
    { "en": "Fungsi Sirkuit Untuk Jaringan Area Penyimpanan?", "id": "Storage Area Network (SAN) Switch." },
    { "en": "Fungsi Sirkuit Untuk Kontroler RAID?", "id": "RAID Controller." },
    { "en": "Fungsi Sirkuit Untuk Drive Solid-State?", "id": "SSD Controller." },
    { "en": "Fungsi Sirkuit Untuk Drive Hard Disk?", "id": "Hard Drive Controller." },
    { "en": "Fungsi Sirkuit Untuk Drive Optik?", "id": "Optical Drive Controller." },
    { "en": "Fungsi Sirkuit Untuk Papan Induk Komputer?", "id": "Motherboard." },
    { "en": "Fungsi Sirkuit Untuk Unit Pemrosesan Pusat?", "id": "CPU (Central Processing Unit)." },
    { "en": "Fungsi Sirkuit Untuk Unit Pemrosesan Grafis?", "id": "GPU (Graphics Processing Unit)." },
    { "en": "Fungsi Sirkuit Yang Mengelola Komunikasi Papan Induk?", "id": "Chipset." },
    { "en": "Fungsi Sirkuit Untuk Menginisialisasi Perangkat Keras?", "id": "BIOS/UEFI." },
    { "en": "Fungsi Sirkuit Untuk Kartu Grafis?", "id": "Graphics Card." },
    { "en": "Fungsi Sirkuit Untuk Kartu Suara?", "id": "Sound Card." },
    { "en": "Fungsi Sirkuit Untuk Catu Daya Komputer?", "id": "Power Supply Unit (PSU)." },
    { "en": "Fungsi Sirkuit Pendingin CPU?", "id": "CPU Cooler Fan Controller." },
    { "en": "Fungsi Sirkuit Pendingin Casing?", "id": "Case Fan Controller." },
    { "en": "Fungsi Sirkuit Untuk Lampu RGB Komputer?", "id": "RGB Lighting Controller." },
    { "en": "Fungsi Sirkuit Untuk Laptop?", "id": "Laptop Motherboard." },
    { "en": "Fungsi Sirkuit Untuk Tablet?", "id": "Tablet Mainboard." },
    { "en": "Fungsi Sirkuit Untuk Ponsel Pintar?", "id": "Smartphone Mainboard." },
    { "en": "Fungsi Sirkuit Untuk Jam Tangan Pintar?", "id": "Smartwatch Circuit." },
    { "en": "Fungsi Sirkuit Untuk Pelacak Kebugaran?", "id": "Fitness Tracker Circuit." },
    { "en": "Fungsi Sirkuit Untuk Headset Realitas Virtual?", "id": "Virtual Reality (VR) Headset." },
    { "en": "Fungsi Sirkuit Untuk Headset Realitas Tertambah?", "id": "Augmented Reality (AR) Headset." },
    { "en": "Fungsi Sirkuit Untuk Pena Digital?", "id": "Digital Pen/Stylus." },
    { "en": "Fungsi Sirkuit Untuk Pemindai Dokumen?", "id": "Document Scanner." },
    { "en": "Fungsi Sirkuit Untuk Printer?", "id": "Printer Controller." },
    { "en": "Fungsi Sirkuit Untuk Printer 3D?", "id": "3D Printer Controller." },
    { "en": "Fungsi Sirkuit Untuk Pemotong Laser?", "id": "Laser Cutter Controller." },
    { "en": "Fungsi Sirkuit Untuk Mesin Ukir CNC?", "id": "CNC Engraver Controller." },
    { "en": "Fungsi Sirkuit Untuk Plotter?", "id": "Plotter Controller." },
    { "en": "Fungsi Sirkuit Untuk Proyektor Digital?", "id": "Digital Projector." },
    { "en": "Fungsi Sirkuit Untuk Sistem Audio Mobil?", "id": "Car Audio Head Unit." },
    { "en": "Fungsi Sirkuit Untuk Penguat Audio Mobil?", "id": "Car Audio Amplifier." },
    { "en": "Fungsi Sirkuit Untuk Equalizer Audio Mobil?", "id": "Car Audio Equalizer." },
    { "en": "Fungsi Sirkuit Untuk Crossover Audio Mobil?", "id": "Car Audio Crossover." },
    { "en": "Fungsi Sirkuit Untuk Prosesor Sinyal Digital Mobil?", "id": "Car Audio DSP." },
    { "en": "Fungsi Sirkuit Untuk Sistem Navigasi Mobil?", "id": "Car Navigation System." },
    { "en": "Fungsi Sirkuit Untuk Sistem Alarm Kebakaran?", "id": "Fire Alarm Control Panel." },
    { "en": "Fungsi Sirkuit Untuk Detektor Asap?", "id": "Smoke Detector." },
    { "en": "Fungsi Sirkuit Untuk Detektor Panas?", "id": "Heat Detector." },
    { "en": "Fungsi Sirkuit Untuk Detektor Karbon Monoksida?", "id": "Carbon Monoxide (CO) Detector." },
    { "en": "Fungsi Sirkuit Untuk Sprinkler Pemadam Kebakaran?", "id": "Fire Sprinkler Control System." },
    { "en": "Fungsi Sirkuit Untuk Sistem Keamanan Rumah?", "id": "Home Security Alarm System." },
    { "en": "Fungsi Sirkuit Untuk Sensor Gerak?", "id": "Motion Detector." },
    { "en": "Fungsi Sirkuit Untuk Sensor Pintu/Jendela?", "id": "Door/Window Reed Switch Sensor." },
    { "en": "Fungsi Sirkuit Untuk Sensor Pecah Kaca?", "id": "Glass Break Detector." },
    { "en": "Fungsi Sirkuit Untuk Keypad Alarm?", "id": "Alarm Keypad." },
    { "en": "Fungsi Sirkuit Untuk Sirene Alarm?", "id": "Alarm Siren Driver." },
    { "en": "Fungsi Sirkuit Untuk Pemanggil Telepon Otomatis?", "id": "Automatic Telephone Dialer." },
    { "en": "Fungsi Sirkuit Untuk Kamera Keamanan?", "id": "CCTV (Closed-Circuit Television) Camera." },
    { "en": "Fungsi Sirkuit Untuk Perekam Video Digital?", "id": "Digital Video Recorder (DVR)." },
    { "en": "Fungsi Sirkuit Untuk Perekam Video Jaringan?", "id": "Network Video Recorder (NVR)." },
    { "en": "Fungsi Sirkuit Untuk Sistem Kontrol Akses?", "id": "Access Control System." },
    { "en": "Fungsi Sirkuit Untuk Pembaca Kartu Akses?", "id": "Access Card Reader." },
    { "en": "Fungsi Sirkuit Untuk Kunci Pintu Elektromagnetik?", "id": "Electromagnetic Lock Driver." },
    { "en": "Fungsi Sirkuit Untuk Kunci Pintu Electric Strike?", "id": "Electric Strike Driver." },
    { "en": "Fungsi Sirkuit Untuk Tombol Keluar (Exit Button)?", "id": "Exit Button Interface." },
    { "en": "Fungsi Sirkuit Untuk Sensor Biometrik?", "id": "Biometric Sensor Interface." },
    { "en": "Fungsi Sirkuit Untuk Sistem Interkom Video?", "id": "Video Intercom System." },
    { "en": "Fungsi Sirkuit Untuk Sistem Otomasi Rumah?", "id": "Home Automation Controller." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Pencahayaan Cerdas?", "id": "Smart Lighting Controller." },
    { "en": "Fungsi Sirkuit Untuk Termostat Cerdas?", "id": "Smart Thermostat." },
    { "en": "Fungsi Sirkuit Untuk Tirai Jendela Cerdas?", "id": "Smart Blinds/Curtains Controller." },
    { "en": "Fungsi Sirkuit Untuk Colokan Listrik Cerdas?", "id": "Smart Plug." },
    { "en": "Fungsi Sirkuit Untuk Irigasi Taman Cerdas?", "id": "Smart Sprinkler Controller." },
    { "en": "Fungsi Sirkuit Untuk Bel Pintu Cerdas?", "id": "Smart Doorbell." },
    { "en": "Fungsi Sirkuit Untuk Kunci Pintu Cerdas?", "id": "Smart Lock." },
    { "en": "Fungsi Sirkuit Untuk Asisten Suara?", "id": "Voice Assistant (e.g., Alexa, Google Home)." },
    { "en": "Fungsi Sirkuit Untuk Jembatan Otomasi Rumah?", "id": "Home Automation Hub/Bridge." },
    { "en": "Fungsi Sirkuit Protokol Z-Wave?", "id": "Z-Wave Controller." },
    { "en": "Fungsi Sirkuit Protokol Zigbee?", "id": "Zigbee Coordinator." },
    { "en": "Fungsi Sirkuit Protokol Bluetooth Mesh?", "id": "Bluetooth Mesh Network." },
    { "en": "Fungsi Sirkuit Untuk Robot Penyedot Debu?", "id": "Robotic Vacuum Cleaner." },
    { "en": "Fungsi Sirkuit Untuk Robot Pemotong Rumput?", "id": "Robotic Lawn Mower." },
    { "en": "Fungsi Sirkuit Untuk Robot Pembersih Kolam?", "id": "Robotic Pool Cleaner." },
    { "en": "Fungsi Sirkuit Untuk Sistem Pakan Hewan Peliharaan Otomatis?", "id": "Automatic Pet Feeder." },
    { "en": "Fungsi Sirkuit Untuk Pintu Hewan Peliharaan Elektronik?", "id": "Electronic Pet Door." },
    { "en": "Fungsi Sirkuit Untuk Pelacak Hewan Peliharaan GPS?", "id": "GPS Pet Tracker." },
    { "en": "Fungsi Sirkuit Untuk Kandang Hewan Peliharaan Terkontrol Iklim?", "id": "Climate-Controlled Pet House." },
    { "en": "Fungsi Sirkuit Untuk Mainan Laser Hewan Peliharaan Otomatis?", "id": "Automatic Laser Pet Toy." },
    { "en": "Fungsi Sirkuit Untuk Air Mancur Hewan Peliharaan?", "id": "Pet Water Fountain." },
    { "en": "Fungsi Sirkuit Papan Pengembangan Mikrokontroler?", "id": "Microcontroller Development Board (e.g., Arduino)." },
    { "en": "Fungsi Sirkuit Komputer Papan Tunggal?", "id": "Single-Board Computer (e.g., Raspberry Pi)." },
    { "en": "Fungsi Sirkuit Modul Tambahan (Shield/HAT)?", "id": "Expansion Board (Shield/HAT)." },
    { "en": "Fungsi Sirkuit Prototipe Tanpa Solder?", "id": "Breadboard/Protoboard." },
    { "en": "Fungsi Sirkuit Papan Cetak Kustom?", "id": "Printed Circuit Board (PCB)." },
    { "en": "Fungsi Sirkuit Untuk Pemrograman IC?", "id": "Device Programmer." },
    { "en": "Fungsi Sirkuit Untuk Debugging Perangkat Keras?", "id": "Hardware Debugger (e.g., JTAG/SWD)." },
    { "en": "Fungsi Sirkuit Untuk Analisis Protokol?", "id": "Protocol Analyzer." },
    { "en": "Fungsi Sirkuit Untuk Mengisolasi Sinyal Digital?", "id": "Digital Isolator." },
    { "en": "Fungsi Sirkuit Untuk Mengisolasi Catu Daya?", "id": "Isolated DC-DC Converter." },
    { "en": "Fungsi Sirkuit Untuk Mengisolasi Komunikasi Serial?", "id": "Isolated RS-232/RS-485 Transceiver." },
    { "en": "Fungsi Sirkuit Untuk Mengemudi Motor DC Dengan Sikat?", "id": "Brushed DC Motor Driver." },
    { "en": "Fungsi Sirkuit Untuk Mengemudi Motor DC Tanpa Sikat?", "id": "Brushless DC (BLDC) Motor Driver." },
    { "en": "Fungsi Sirkuit Untuk Mengemudi Motor Stepper?", "id": "Stepper Motor Driver." },
    { "en": "Fungsi Sirkuit Untuk Mengemudi Motor Servo?", "id": "Servo Motor Driver." },
    { "en": "Fungsi Sirkuit Untuk Umpan Balik Posisi Motor?", "id": "Encoder Interface." },
    { "en": "Fungsi Sirkuit Untuk Umpan Balik Arus Motor?", "id": "Motor Current Sense Amplifier." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Kecepatan Loop Tertutup?", "id": "Closed-Loop Speed Controller." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Posisi Loop Tertutup?", "id": "Closed-Loop Position Controller." },
    { "en": "Fungsi Sirkuit Untuk Pengereman Dinamis Motor?", "id": "Dynamic Braking Circuit." },
    { "en": "Fungsi Sirkuit Untuk Pengereman Regeneratif Motor?", "id": "Regenerative Braking Circuit." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Motor Universal?", "id": "Universal Motor Speed Controller." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Motor Induksi Satu Fasa?", "id": "Single-Phase Induction Motor Controller." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Motor Induksi Tiga Fasa?", "id": "Three-Phase Induction Motor Controller (VFD)." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Motor Sinkron?", "id": "Synchronous Motor Controller." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Motor Reluktansi?", "id": "Switched Reluctance Motor (SRM) Driver." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Aktuator Linear?", "id": "Linear Actuator Controller." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Voice Coil?", "id": "Voice Coil Motor (VCM) Driver." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Transformator?", "id": "Transformer Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Relai?", "id": "Relay Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Saklar?", "id": "Switch Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian LED?", "id": "LED Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Kabel?", "id": "Cable Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Sirkuit Terpadu?", "id": "Integrated Circuit (IC) Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Osilator Kristal?", "id": "Crystal Oscillator Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Sumber Daya?", "id": "Power Supply Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Beban Elektronik?", "id": "Electronic Load." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Baterai?", "id": "Battery Analyzer." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Amplifier Audio?", "id": "Audio Amplifier Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Speaker?", "id": "Speaker Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Antena?", "id": "Antenna Analyzer." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Kabel Koaksial?", "id": "Coaxial Cable Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Serat Optik?", "id": "Optical Fiber Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Jaringan Komputer?", "id": "Network Cable Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Kualitas Udara?", "id": "Air Quality Monitor." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Kualitas Air?", "id": "Water Quality Monitor." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Tingkat Kebisingan?", "id": "Noise Dosimeter." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Tingkat Getaran?", "id": "Vibration Meter." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Tingkat Cahaya?", "id": "Lux Meter." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Warna?", "id": "Colorimeter." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Jarak?", "id": "Distance Meter." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Kecepatan?", "id": "Speed Meter (Tachometer)." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Akselerasi?", "id": "Accelerometer." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Tekanan?", "id": "Pressure Gauge." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Aliran?", "id": "Flow Meter." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Suhu?", "id": "Thermometer." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Kelembaban?", "id": "Hygrometer." },
    { "en": "Fungsi Sirkuit Untuk Pengujian pH?", "id": "pH Meter." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Konduktivitas?", "id": "Conductivity Meter." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Salinitas?", "id": "Salinity Meter." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Radiasi Elektromagnetik?", "id": "EMF/RF Field Strength Meter." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Tahanan Isolasi?", "id": "Insulation Resistance Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Loop Tanah?", "id": "Earth Loop Impedance Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian RCD/GFCI?", "id": "RCD/GFCI Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Urutan Fasa?", "id": "Phase Sequence Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Motor?", "id": "Motor Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Transformator?", "id": "Transformer Winding Analyzer." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Baterai?", "id": "Battery Impedance Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Panel Surya?", "id": "Solar Panel I-V Curve Tracer." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Pompa Bahan Bakar?", "id": "Fuel Pump Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Injektor Bahan Bakar?", "id": "Fuel Injector Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Sistem Pengapian?", "id": "Ignition System Analyzer." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Emisi Gas Buang?", "id": "Exhaust Gas Analyzer." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Sistem AC Mobil?", "id": "AC Manifold Gauge Set (Electronic)." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Sensor Otomotif?", "id": "Automotive Sensor Simulator." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Lampu Mobil?", "id": "Automotive Light Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Sistem Pengisian Mobil?", "id": "Alternator/Charging System Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Sistem Starter Mobil?", "id": "Starter Motor Tester." },
    { "en": "Fungsi Sirkuit Untuk Pengujian Jaringan CAN Bus?", "id": "CAN Bus Analyzer." },
    { "en": "Fungsi Sirkuit Untuk Pemindai Diagnostik Mobil?", "id": "OBD-II Scanner." },
    { "en": "Fungsi Sirkuit Untuk Memprogram Ulang ECU Mobil?", "id": "ECU Reflashing/Programming Tool." },
    { "en": "Fungsi Sirkuit Untuk Kalibrasi Sistem ADAS?", "id": "ADAS (Advanced Driver-Assistance Systems) Calibration." },
    { "en": "Fungsi Sirkuit Untuk Sistem Peringatan Tabrakan?", "id": "Collision Avoidance System." },
    { "en": "Fungsi Sirkuit Untuk Cruise Control Adaptif?", "id": "Adaptive Cruise Control (ACC) System." },
    { "en": "Fungsi Sirkuit Untuk Peringatan Keluar Jalur?", "id": "Lane Departure Warning System." },
    { "en": "Fungsi Sirkuit Untuk Deteksi Titik Buta?", "id": "Blind Spot Detection System." },
    { "en": "Fungsi Sirkuit Untuk Kamera Parkir?", "id": "Parking Camera System." },
    { "en": "Fungsi Sirkuit Untuk Sistem Parkir Otomatis?", "id": "Automatic Parking Assist." },
    { "en": "Fungsi Sirkuit Untuk Perekam Data Kecelakaan?", "id": "Crash Data Recorder." },
    { "en": "Fungsi Sirkuit Untuk Sistem Infotainment Mobil?", "id": "Car Infotainment System." },
    { "en": "Fungsi Sirkuit Untuk Tampilan Head-Up?", "id": "Head-Up Display (HUD)." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Suara Di Mobil?", "id": "In-Car Voice Control System." },
    { "en": "Fungsi Sirkuit Untuk Pembatalan Kebisingan Aktif?", "id": "Active Noise Cancellation (ANC)." },
    { "en": "Fungsi Sirkuit Untuk Peningkatan Suara Mesin?", "id": "Engine Sound Enhancement." },
    { "en": "Fungsi Sirkuit Untuk Penerima Radio Satelit?", "id": "Satellite Radio Receiver." },
    { "en": "Fungsi Sirkuit Untuk Penerima Radio Digital?", "id": "Digital Radio (DAB/HD Radio) Receiver." },
    { "en": "Fungsi Sirkuit Untuk Pemancar FM Bluetooth?", "id": "Bluetooth FM Transmitter." },
    { "en": "Fungsi Sirkuit Untuk Pengisi Daya Nirkabel Di Mobil?", "id": "In-Car Wireless Charger." },
    { "en": "Fungsi Sirkuit Untuk Pemantik Rokok?", "id": "Cigarette Lighter Socket." },
    { "en": "Fungsi Sirkuit Untuk Inverter Daya Mobil?", "id": "Car Power Inverter." },
    { "en": "Fungsi Sirkuit Untuk Starter Lompat (Jump Starter)?", "id": "Jump Starter." },
    { "en": "Fungsi Sirkuit Untuk Pompa Ban Portabel?", "id": "Portable Tire Inflator." },
    { "en": "Fungsi Sirkuit Untuk Kulkas Portabel?", "id": "Portable Refrigerator/Cooler." },
    { "en": "Fungsi Sirkuit Untuk Pemanas Kursi Mobil?", "id": "Seat Heater Control." },
    { "en": "Fungsi Sirkuit Untuk Ventilasi Kursi Mobil?", "id": "Seat Ventilation Control." },
    { "en": "Fungsi Sirkuit Untuk Kursi Pijat Mobil?", "id": "Massage Seat Controller." },
    { "en": "Fungsi Sirkuit Untuk Kaca Spion Peredupan Otomatis?", "id": "Auto-Dimming Mirror." },
    { "en": "Fungsi Sirkuit Untuk Wiper Sensor Hujan?", "id": "Rain-Sensing Wiper Controller." },
    { "en": "Fungsi Sirkuit Untuk Lampu Depan Otomatis?", "id": "Automatic Headlight Controller." },
    { "en": "Fungsi Sirkuit Untuk Lampu Jauh Otomatis?", "id": "Automatic High-Beam Assist." },
    { "en": "Fungsi Sirkuit Untuk Lampu Belok Adaptif?", "id": "Adaptive Front-Lighting System (AFS)." },
    { "en": "Fungsi Sirkuit Untuk Lampu Kabut?", "id": "Fog Light Relay Circuit." },
    { "en": "Fungsi Sirkuit Untuk Lampu Rem?", "id": "Brake Light Switch Circuit." },
    { "en": "Fungsi Sirkuit Untuk Klakson?", "id": "Horn Relay Circuit." },
    { "en": "Fungsi Sirkuit Untuk Sistem Injeksi Bahan Bakar?", "id": "Electronic Fuel Injection (EFI) System." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Waktu Katup Variabel?", "id": "Variable Valve Timing (VVT) Controller." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Turbocharger?", "id": "Turbo Boost Controller." },
    { "en": "Fungsi Sirkuit Untuk Pemanas Awal Mesin Diesel?", "id": "Glow Plug Controller." },
    { "en": "Fungsi Sirkuit Untuk Knalpot Performa?", "id": "Exhaust Valve Controller." },
    { "en": "Fungsi Sirkuit Untuk Sistem Penggerak Empat Roda?", "id": "4WD/AWD Control Module." },
    { "en": "Fungsi Sirkuit Untuk Suspensi Udara?", "id": "Air Suspension Controller." },
    { "en": "Fungsi Sirkuit Untuk Kemudi Daya Listrik?", "id": "Electric Power Steering (EPS) Module." },
    { "en": "Fungsi Sirkuit Untuk Rem Parkir Elektronik?", "id": "Electronic Parking Brake (EPB)." },
    { "en": "Fungsi Sirkuit Untuk Sistem Start-Stop?", "id": "Start-Stop System Controller." },
    { "en": "Fungsi Sirkuit Untuk Pengisian Baterai Hibrida/EV?", "id": "On-Board Charger (OBC)." },
    { "en": "Fungsi Sirkuit Manajemen Baterai Hibrida/EV?", "id": "Battery Management System (BMS)." },
    { "en": "Fungsi Sirkuit Kontrol Motor Traksi EV?", "id": "Traction Motor Inverter." },
    { "en": "Fungsi Sirkuit Pemanas Kabin EV?", "id": "PTC (Positive Temperature Coefficient) Heater Controller." },
    { "en": "Fungsi Sirkuit Pendingin Baterai EV?", "id": "Battery Thermal Management System." },
    { "en": "Fungsi Sirkuit Komunikasi Pengisian EV?", "id": "Charging Communication Interface (PLC/CAN)." },
    { "en": "Fungsi Sirkuit Untuk Stasiun Pengisian EV?", "id": "EVSE (Electric Vehicle Supply Equipment)." },
    { "en": "Fungsi Sirkuit Untuk Pengisian Cepat DC?", "id": "DC Fast Charger." },
    { "en": "Fungsi Sirkuit Untuk Pengisian Nirkabel EV?", "id": "Wireless EV Charging System." },
    { "en": "Fungsi Sirkuit Vehicle-to-Grid (V2G)?", "id": "V2G Bidirectional Charger." },
    { "en": "Fungsi Sirkuit Suara Peringatan Pejalan Kaki EV?", "id": "Acoustic Vehicle Alerting System (AVAS)." },
    { "en": "Fungsi Sirkuit Pengukur Energi Baterai EV?", "id": "State of Charge (SoC) Estimator." },
    { "en": "Fungsi Sirkuit Prediksi Jarak Tempuh EV?", "id": "Range Estimation Algorithm." },
    { "en": "Fungsi Sirkuit Untuk Diagnostik Baterai EV?", "id": "Battery Diagnostics System." },
    { "en": "Fungsi Sirkuit Untuk Menyeimbangkan Sel Baterai?", "id": "Active/Passive Cell Balancing." },
    { "en": "Fungsi Sirkuit Pengaman Tegangan Tinggi EV?", "id": "High-Voltage Interlock Loop (HVIL)." },
    { "en": "Fungsi Sirkuit Deteksi Isolasi Baterai EV?", "id": "Insulation Monitoring Device (IMD)." },
    { "en": "Fungsi Sirkuit Kontak Tegangan Tinggi EV?", "id": "High-Voltage Contactor Driver." },
    { "en": "Fungsi Sirkuit Untuk Prapengisian (Pre-charge) EV?", "id": "Pre-charge Circuit." },
    { "en": "Fungsi Sirkuit Konverter DC-DC Tegangan Tinggi EV?", "id": "High-Voltage DC-DC Converter." },
    { "en": "Fungsi Sirkuit Untuk Kompresor AC Listrik?", "id": "Electric AC Compressor Driver." },
    { "en": "Fungsi Sirkuit Untuk Pompa Vakum Rem Listrik?", "id": "Electric Brake Booster Vacuum Pump." },
    { "en": "Fungsi Sirkuit Untuk Sistem Audio Peringatan?", "id": "Acoustic Warning System." },
    { "en": "Fungsi Sirkuit Untuk Panel Instrumen Digital?", "id": "Digital Instrument Cluster." },
    { "en": "Fungsi Sirkuit Untuk Tampilan Konsol Tengah?", "id": "Center Console Display." },
    { "en": "Fungsi Sirkuit Untuk Sistem Hiburan Kursi Belakang?", "id": "Rear Seat Entertainment System." },
    { "en": "Fungsi Sirkuit Untuk Antena Sirip Hiu?", "id": "Shark Fin Antenna." },
    { "en": "Fungsi Sirkuit Untuk Penerima Radio Satelit?", "id": "Satellite Digital Audio Radio Service (SDARS)." },
    { "en": "Fungsi Sirkuit Untuk Panggilan Darurat Otomatis?", "id": "eCall System." },
    { "en": "Fungsi Sirkuit Untuk Sistem Telematika?", "id": "Telematics Control Unit (TCU)." },
    { "en": "Fungsi Sirkuit Untuk Pencahayaan Ambient Interior?", "id": "Ambient Lighting Controller." },
    { "en": "Fungsi Sirkuit Untuk Kontrol Gestur?", "id": "Gesture Control System." },
    { "en": "Fungsi Sirkuit Untuk Pengenalan Wajah Pengemudi?", "id": "Driver Recognition System." }



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
