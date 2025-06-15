<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Arduino Dasar</title>
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
        <h1>Pengetahuan Arduino Dasar</h1>
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
            // Kosakata 1-308 (dari PDF pertama)disini yahhhhh
        // Set 1: 100 Pertanyaan Elektronika Dasar

  { "en": "Kerajaan Hindu tertua di Indonesia?", "id": "Kutai Martadipura." },
  { "en": "Di mana letak Kerajaan Kutai?", "id": "Kalimantan Timur." },
  { "en": "Siapa raja Kutai yang terkenal?", "id": "Mulawarman." },
  { "en": "Apa bukti keberadaan Kerajaan Kutai?", "id": "Prasasti Yupa." },
  { "en": "Bahasa apa yang digunakan Prasasti Yupa?", "id": "Sansekerta." },
  { "en": "Huruf apa yang digunakan Prasasti Yupa?", "id": "Pallawa." },
  { "en": "Kerajaan bercorak Hindu di Jawa Barat?", "id": "Tarumanegara." },
  { "en": "Siapa raja Tarumanegara yang terkenal?", "id": "Purnawarman." },
  { "en": "Apa peninggalan terkenal Purnawarman?", "id": "Prasasti Ciaruteun." },
  { "en": "Prasasti Ciaruteun berukir telapak kaki siapa?", "id": "Purnawarman." },
  { "en": "Kerajaan maritim besar di Sumatra?", "id": "Sriwijaya." },
  { "en": "Di mana pusat Kerajaan Sriwijaya?", "id": "Palembang." },
  { "en": "Kapan Sriwijaya mencapai puncak kejayaan?", "id": "Abad ke-9." },
  { "en": "Siapa raja Sriwijaya yang terkenal?", "id": "Balaputradewa." },
  { "en": "Sriwijaya dikenal sebagai pusat agama apa?", "id": "Buddha." },
  { "en": "Candi Buddha terbesar peninggalan Syailendra?", "id": "Borobudur." },
  { "en": "Dinasti apa yang membangun Borobudur?", "id": "Syailendra." },
  { "en": "Di mana letak Candi Borobudur?", "id": "Magelang." },
  { "en": "Dinasti Hindu yang berkuasa di Mataram Kuno?", "id": "Sanjaya." },
  { "en": "Candi Hindu terbesar di Indonesia?", "id": "Prambanan." },
  { "en": "Candi Prambanan dipersembahkan untuk siapa?", "id": "Trimurti." },
  { "en": "Siapa Trimurti dalam Hindu?", "id": "Brahma, Wisnu, Siwa." },
  { "en": "Kerajaan apa penerus Mataram Kuno di Jawa Timur?", "id": "Medang." },
  { "en": "Siapa pendiri Dinasti Isyana di Medang?", "id": "Mpu Sindok." },
  { "en": "Kerajaan apa yang terkenal dengan Raja Jayabaya?", "id": "Kediri." },
  { "en": "Apa ramalan Jayabaya yang terkenal?", "id": "Jangka Jayabaya." },
  { "en": "Kerajaan apa yang didirikan Ken Arok?", "id": "Singasari." },
  { "en": "Siapa raja Singasari terakhir?", "id": "Kertanegara." },
  { "en": "Ekspedisi Kertanegara untuk memperluas wilayah?", "id": "Pamalayu." },
  { "en": "Kerajaan Hindu-Buddha terbesar dalam sejarah Indonesia?", "id": "Majapahit." },
  { "en": "Siapa pendiri Kerajaan Majapahit?", "id": "Raden Wijaya." },
  { "en": "Kapan Majapahit berdiri?", "id": "Tahun 1293." },
  { "en": "Siapa patih Majapahit yang terkenal?", "id": "Gajah Mada." },
  { "en": "Sumpah Gajah Mada yang terkenal?", "id": "Sumpah Palapa." },
  { "en": "Apa isi Sumpah Palapa?", "id": "Menyatukan Nusantara." },
  { "en": "Siapa raja Majapahit saat Gajah Mada bersumpah?", "id": "Tribhuwana Wijayatunggadewi." },
  { "en": "Siapa raja Majapahit terbesar?", "id": "Hayam Wuruk." },
  { "en": "Kitab apa yang menceritakan kebesaran Majapahit?", "id": "Nagarakretagama." },
  { "en": "Siapa penulis Kitab Nagarakretagama?", "id": "Mpu Prapanca." },
  { "en": "Kitab lain yang penting dari Majapahit?", "id": "Sutasoma." },
  { "en": "Siapa penulis Kitab Sutasoma?", "id": "Mpu Tantular." },
  { "en": "Apa semboyan dari Kitab Sutasoma?", "id": "Bhinneka Tunggal Ika." },
  { "en": "Apa arti Bhinneka Tunggal Ika?", "id": "Berbeda-beda tetap satu." },
  { "en": "Kapan Majapahit mencapai puncak kejayaan?", "id": "Abad ke-14." },
  { "en": "Apa penyebab kemunduran Majapahit?", "id": "Perang Paregreg." },
  { "en": "Kapan Islam mulai masuk Indonesia?", "id": "Abad ke-7." },
  { "en": "Melalui jalur apa Islam masuk Indonesia?", "id": "Perdagangan." },
  { "en": "Dari mana pedagang Islam berasal?", "id": "Gujarat, Persia, Arab." },
  { "en": "Kerajaan Islam pertama di Indonesia?", "id": "Samudera Pasai." },
  { "en": "Di mana letak Samudera Pasai?", "id": "Aceh." },
  { "en": "Siapa pendiri Samudera Pasai?", "id": "Marah Silu." },
  { "en": "Gelar Marah Silu setelah masuk Islam?", "id": "Sultan Malik As-Saleh." },
  { "en": "Kerajaan Islam besar di Aceh?", "id": "Kesultanan Aceh." },
  { "en": "Siapa sultan Aceh terbesar?", "id": "Sultan Iskandar Muda." },
  { "en": "Kapan Sultan Iskandar Muda berkuasa?", "id": "Awal abad ke-17." },
  { "en": "Kerajaan Islam pertama di Jawa?", "id": "Demak." },
  { "en": "Siapa pendiri Kesultanan Demak?", "id": "Raden Patah." },
  { "en": "Siapa wali yang mendukung Demak?", "id": "Walisongo." },
  { "en": "Berapa jumlah Walisongo?", "id": "Sembilan." },
  { "en": "Siapa saja anggota Walisongo yang terkenal?", "id": "Sunan Kalijaga, Sunan Bonang." },
  { "en": "Apa peran Walisongo?", "id": "Menyebarkan Islam di Jawa." },
  { "en": "Siapa raja Demak ketiga yang terkenal?", "id": "Sultan Trenggana." },
  { "en": "Pelabuhan penting Kerajaan Demak?", "id": "Jepara." },
  { "en": "Kerajaan apa yang dikalahkan Demak?", "id": "Majapahit." },
  { "en": "Kerajaan Islam penerus Demak di Pajang?", "id": "Kesultanan Pajang." },
  { "en": "Siapa pendiri Kesultanan Pajang?", "id": "Jaka Tingkir." },
  { "en": "Gelar Jaka Tingkir sebagai raja?", "id": "Sultan Hadiwijaya." },
  { "en": "Kerajaan Islam besar di Jawa setelah Pajang?", "id": "Mataram Islam." },
  { "en": "Siapa pendiri Kesultanan Mataram Islam?", "id": "Panembahan Senopati." },
  { "en": "Di mana pusat Mataram Islam awalnya?", "id": "Kotagede." },
  { "en": "Siapa sultan Mataram Islam terbesar?", "id": "Sultan Agung." },
  { "en": "Kapan Sultan Agung menyerang Batavia?", "id": "1628 dan 1629." },
  { "en": "Apa hasil serangan Sultan Agung ke Batavia?", "id": "Gagal." },
  { "en": "Perjanjian apa yang memecah Mataram Islam?", "id": "Perjanjian Giyanti." },
  { "en": "Mataram Islam terpecah menjadi apa saja?", "id": "Kasunanan Surakarta, Kasultanan Yogyakarta." },
  { "en": "Kerajaan Islam di Banten?", "id": "Kesultanan Banten." },
  { "en": "Siapa sultan Banten yang terkenal?", "id": "Sultan Ageng Tirtayasa." },
  { "en": "Kerajaan Islam di Sulawesi Selatan?", "id": "Gowa dan Tallo." },
  { "en": "Siapa raja Gowa yang terkenal dijuluki Ayam Jantan?", "id": "Sultan Hasanuddin." },
  { "en": "Kerajaan Islam di Maluku Utara?", "id": "Ternate dan Tidore." },
  { "en": "Apa komoditas utama Ternate dan Tidore?", "id": "Cengkih." },
  { "en": "Bangsa Eropa pertama tiba di Indonesia?", "id": "Portugis." },
  { "en": "Kapan Portugis pertama kali datang?", "id": "Tahun 1511." },
  { "en": "Di mana Portugis pertama mendarat?", "id": "Malaka." },
  { "en": "Siapa pemimpin ekspedisi Portugis ke Malaka?", "id": "Alfonso de Albuquerque." },
  { "en": "Apa tujuan utama Portugis datang?", "id": "Mencari rempah-rempah." },
  { "en": "Selain Portugis, bangsa Eropa mana lagi?", "id": "Spanyol, Belanda, Inggris." },
  { "en": "Kapan Belanda pertama kali datang?", "id": "Tahun 1596." },
  { "en": "Di mana kapal Belanda pertama mendarat?", "id": "Banten." },
  { "en": "Siapa pemimpin ekspedisi Belanda pertama?", "id": "Cornelis de Houtman." },
  { "en": "Perusahaan dagang Belanda yang dibentuk?", "id": "VOC." },
  { "en": "Kapan VOC didirikan?", "id": "Tahun 1602." },
  { "en": "Apa kepanjangan VOC?", "id": "Vereenigde Oostindische Compagnie." },
  { "en": "Siapa gubernur jenderal VOC pertama?", "id": "Pieter Both." },
  { "en": "Di mana VOC membangun kantor dagangnya?", "id": "Batavia." },
  { "en": "Siapa yang menaklukkan Jayakarta dan mengganti nama Batavia?", "id": "Jan Pieterszoon Coen." },
  { "en": "Apa hak istimewa VOC?", "id": "Hak monopoli dagang." },
  { "en": "Kebijakan VOC yang merugikan rakyat?", "id": "Tanam paksa." },
  { "en": "Kapan VOC dibubarkan?", "id": "Tahun 1799." },
  { "en": "Apa penyebab VOC bubar?", "id": "Korupsi dan utang." },
  { "en": "Siapa yang mengambil alih kekuasaan VOC?", "id": "Pemerintah Hindia Belanda." },
  { "en": "Gubernur Jenderal Hindia Belanda yang membangun Jalan Raya Pos?", "id": "Herman Willem Daendels." },
  { "en": "Jalan apa yang dibangun Daendels?", "id": "Anyer sampai Panarukan." },
  { "en": "Siapa Letnan Gubernur Inggris di Jawa?", "id": "Thomas Stamford Raffles." },
  { "en": "Apa karya terkenal Raffles tentang Jawa?", "id": "History of Java." },
  { "en": "Kebijakan Raffles yang terkenal?", "id": "Sistem sewa tanah." },
  { "en": "Kapan Belanda kembali berkuasa setelah Inggris?", "id": "Tahun 1816." },
  { "en": "Sistem apa yang diterapkan Van den Bosch?", "id": "Cultuurstelsel (Tanam Paksa)." },
  { "en": "Kapan Tanam Paksa diterapkan?", "id": "Tahun 1830." },
  { "en": "Apa tujuan Tanam Paksa?", "id": "Mengisi kas Belanda." },
  { "en": "Siapa tokoh Belanda yang menentang Tanam Paksa?", "id": "Douwes Dekker." },
  { "en": "Apa nama samaran Douwes Dekker?", "id": "Multatuli." },
  { "en": "Apa buku karya Multatuli?", "id": "Max Havelaar." },
  { "en": "Perang besar melawan Belanda di Jawa?", "id": "Perang Diponegoro." },
  { "en": "Kapan Perang Diponegoro terjadi?", "id": "1825-1830." },
  { "en": "Siapa pemimpin Perang Diponegoro?", "id": "Pangeran Diponegoro." },
  { "en": "Perang besar di Sumatra Barat?", "id": "Perang Padri." },
  { "en": "Siapa tokoh utama Perang Padri?", "id": "Tuanku Imam Bonjol." },
  { "en": "Perang melawan Belanda di Aceh?", "id": "Perang Aceh." },
  { "en": "Tokoh pejuang wanita dari Aceh?", "id": "Cut Nyak Dien." },
  { "en": "Tokoh pejuang pria dari Aceh?", "id": "Teuku Umar." },
  { "en": "Kebijakan Belanda setelah Tanam Paksa?", "id": "Politik Pintu Terbuka." },
  { "en": "Kebijakan etis Belanda untuk kesejahteraan pribumi?", "id": "Politik Etis." },
  { "en": "Apa tiga program Politik Etis?", "id": "Irigasi, Edukasi, Emigrasi." },
  { "en": "Apa dampak positif Politik Etis?", "id": "Melahirkan kaum terpelajar." },
  { "en": "Organisasi pergerakan nasional pertama?", "id": "Budi Utomo." },
  { "en": "Kapan Budi Utomo didirikan?", "id": "20 Mei 1908." },
  { "en": "Siapa pendiri Budi Utomo?", "id": "Dr. Sutomo dan mahasiswa STOVIA." },
  { "en": "Apa tujuan awal Budi Utomo?", "id": "Kemajuan Jawa dan Madura." },
  { "en": "Tanggal berdirinya Budi Utomo diperingati sebagai hari apa?", "id": "Hari Kebangkitan Nasional." },
  { "en": "Organisasi dagang Islam yang kemudian menjadi partai politik?", "id": "Sarekat Islam." },
  { "en": "Siapa pendiri Sarekat Dagang Islam?", "id": "H. Samanhudi." },
  { "en": "Siapa tokoh sentral Sarekat Islam?", "id": "H.O.S. Cokroaminoto." },
  { "en": "Organisasi yang berhaluan sosialis-komunis?", "id": "ISDV." },
  { "en": "Siapa pendiri ISDV?", "id": "Henk Sneevliet." },
  { "en": "ISDV kemudian berubah menjadi apa?", "id": "Partai Komunis Indonesia (PKI)." },
  { "en": "Organisasi pemuda pertama bersifat kedaerahan?", "id": "Tri Koro Dharmo." },
  { "en": "Tri Koro Dharmo kemudian menjadi apa?", "id": "Jong Java." },
  { "en": "Sebutkan organisasi pemuda kedaerahan lain!", "id": "Jong Sumatranen Bond, Jong Ambon." },
  { "en": "Organisasi wanita pertama di Indonesia?", "id": "Putri Mardika." },
  { "en": "Tokoh wanita pelopor emansipasi?", "id": "R.A. Kartini." },
  { "en": "Apa judul buku kumpulan surat Kartini?", "id": "Habis Gelap Terbitlah Terang." },
  { "en": "Organisasi pergerakan nasional yang radikal?", "id": "Indische Partij." },
  { "en": "Siapa tiga serangkai pendiri Indische Partij?", "id": "Douwes Dekker, Cipto Mangunkusumo, Suwardi Suryaningrat." },
  { "en": "Nama lain Suwardi Suryaningrat?", "id": "Ki Hajar Dewantara." },
  { "en": "Apa kritikan Suwardi terhadap Belanda?", "id": "Als ik een Nederlander was." },
  { "en": "Lembaga pendidikan yang didirikan Ki Hajar Dewantara?", "id": "Taman Siswa." },
  { "en": "Di mana Taman Siswa didirikan?", "id": "Yogyakarta." },
  { "en": "Apa semboyan pendidikan Taman Siswa?", "id": "Ing ngarsa sung tuladha." },
  { "en": "Semboyan lain Taman Siswa?", "id": "Ing madya mangun karsa." },
  { "en": "Semboyan ketiga Taman Siswa?", "id": "Tut wuri handayani." },
  { "en": "Kongres Pemuda I diadakan tahun berapa?", "id": "Tahun 1926." },
  { "en": "Di mana Kongres Pemuda I diadakan?", "id": "Jakarta (Batavia)." },
  { "en": "Kongres Pemuda II diadakan tahun berapa?", "id": "Tahun 1928." },
  { "en": "Di mana Kongres Pemuda II diadakan?", "id": "Jakarta (Batavia)." },
  { "en": "Apa hasil penting Kongres Pemuda II?", "id": "Sumpah Pemuda." },
  { "en": "Kapan Sumpah Pemuda diikrarkan?", "id": "28 Oktober 1928." },
  { "en": "Apa isi pertama Sumpah Pemuda?", "id": "Satu tanah air, Indonesia." },
  { "en": "Apa isi kedua Sumpah Pemuda?", "id": "Satu bangsa, Indonesia." },
  { "en": "Apa isi ketiga Sumpah Pemuda?", "id": "Satu bahasa, Indonesia." },
  { "en": "Siapa yang menciptakan lagu Indonesia Raya?", "id": "Wage Rudolf Supratman." },
  { "en": "Kapan lagu Indonesia Raya pertama kali diperdengarkan?", "id": "Kongres Pemuda II." },
  { "en": "Partai politik yang didirikan Soekarno?", "id": "Partai Nasional Indonesia (PNI)." },
  { "en": "Kapan PNI didirikan?", "id": "4 Juli 1927." },
  { "en": "Apa tujuan PNI?", "id": "Indonesia merdeka." },
  { "en": "Apa pembelaan Soekarno di pengadilan Belanda?", "id": "Indonesia Menggugat." },
  { "en": "Badan penyelidik usaha kemerdekaan bentukan Jepang?", "id": "BPUPKI." },
  { "en": "Apa kepanjangan BPUPKI?", "id": "Badan Penyelidik Usaha-usaha Persiapan Kemerdekaan Indonesia." },
  { "en": "Siapa ketua BPUPKI?", "id": "Dr. K.R.T. Radjiman Wedyodiningrat." },
  { "en": "Kapan sidang pertama BPUPKI?", "id": "29 Mei - 1 Juni 1945." },
  { "en": "Apa agenda sidang pertama BPUPKI?", "id": "Dasar negara Indonesia." },
  { "en": "Apa sebutan untuk piagam kerajaan Kutai?", "id": "Yupa." },
  { "en": "Siapa dewa utama yang dipuja di Kutai?", "id": "Dewa Siwa." },
  { "en": "Kerajaan Buddha awal lain di Jawa Tengah selain Mataram Kuno?", "id": "Kalingga." },
  { "en": "Siapa ratu Kalingga yang terkenal tegas?", "id": "Ratu Shima." },
  { "en": "Prasasti apa yang menyebutkan nama \"Indonesia\" pertama kali?", "id": "Belum ada saat itu." },
  { "en": "Apa nama pelabuhan penting Sriwijaya di Selat Malaka?", "id": "Palembang (pusatnya)." },
  { "en": "Candi Buddha lain peninggalan Sriwijaya di Muaro Jambi?", "id": "Candi Muaro Jambi." },
  { "en": "Apa komoditas utama yang diperdagangkan Sriwijaya selain rempah?", "id": "Emas, lada." },
  { "en": "Siapa raja Airlangga yang terkenal dari Medang Kamulan?", "id": "Airlangga." },
  { "en": "Kerajaan apa yang dibagi Airlangga menjadi dua?", "id": "Kahuripan (Janggala, Panjalu)." },
  { "en": "Apa karya sastra terkenal dari masa Kediri?", "id": "Kakawin Bharatayuddha." },
  { "en": "Siapa yang menggubah Kakawin Bharatayuddha?", "id": "Mpu Sedah, Mpu Panuluh." },
  { "en": "Patung terkenal dari Singasari yang merupakan perwujudan Ken Dedes?", "id": "Prajnaparamita." },
  { "en": "Gelar Raden Wijaya setelah menjadi raja Majapahit?", "id": "Kertarajasa Jayawardhana." },
  { "en": "Siapa istri Raden Wijaya yang terkenal?", "id": "Gayatri Rajapatni." },
  { "en": "Selain Nagarakretagama, kitab apa yang menceritakan raja-raja Singasari-Majapahit?", "id": "Pararaton." },
  { "en": "Apa nama armada laut Majapahit?", "id": "Junggala." },
  { "en": "Perang saudara terkenal di Majapahit?", "id": "Perang Paregreg." },
  { "en": "Siapa tokoh Walisongo yang dikenal sebagai arsitek Masjid Agung Demak?", "id": "Sunan Kalijaga." },
  { "en": "Sunan mana yang sering berdakwah lewat wayang?", "id": "Sunan Kalijaga." },
  { "en": "Gelar Raden Fatah lainnya?", "id": "Sultan Alam Akbar Al-Fatah." },
  { "en": "Siapa Pati Unus yang memimpin serangan ke Malaka?", "id": "Adipati Unus (putra Raden Patah)." },
  { "en": "Apa julukan Pati Unus?", "id": "Pangeran Sabrang Lor." },
  { "en": "Siapa sultan Demak yang memperluas wilayah ke Jawa Barat?", "id": "Sultan Trenggana." },
  { "en": "Siapa yang menggantikan Sultan Trenggana dan memindahkan pusat ke Pajang?", "id": "Jaka Tingkir (Sultan Hadiwijaya)." },
  { "en": "Siapa ayah Panembahan Senopati?", "id": "Ki Ageng Pemanahan." },
  { "en": "Apa karya sastra terkenal dari masa Sultan Agung?", "id": "Serat Sastra Gending." },
  { "en": "Siapa arsitek Taman Sari Yogyakarta?", "id": "Tumenggung Mangundipuro (atas perintah Sultan)." },
  { "en": "Perjanjian apa yang membagi Mataram menjadi Surakarta dan Yogyakarta?", "id": "Perjanjian Giyanti (1755)." },
  { "en": "Perjanjian apa yang membentuk Mangkunegaran?", "id": "Perjanjian Salatiga (1757)." },
  { "en": "Siapa pangeran dari Mataram yang mendirikan Mangkunegaran?", "id": "Raden Mas Said (Pangeran Sambernyawa)." },
  { "en": "Siapa Sultan Baabullah dari Ternate yang mengusir Portugis?", "id": "Sultan Baabullah." },
  { "en": "Benteng Portugis yang direbut Sultan Baabullah?", "id": "Sao Paulo (Gamalama)." },
  { "en": "Siapa yang memberi hak oktroi kepada VOC?", "id": "Staten-Generaal (Parlemen Belanda)." },
  { "en": "Apa sebutan untuk kerja paksa menanam kopi di Priangan?", "id": "Preangerstelsel." },
  { "en": "Siapa gubernur jenderal VOC yang membangun kota Batavia?", "id": "Jan Pieterszoon Coen." },
  { "en": "Apa nama lain Batavia pada masa pendudukan Jepang?", "id": "Djakarta." },
  { "en": "Kebijakan VOC untuk menebang tanaman rempah berlebih?", "id": "Ekstirpasi." },
  { "en": "Hak VOC untuk mengirim kapal ke wilayah lain?", "id": "Pelayaran Hongi (Hongitochten)." },
  { "en": "Siapa pahlawan wanita dari Maluku selain Martha Christina Tiahahu?", "id": "Kapitan Pattimura (laki-laki)." },
  { "en": "Siapa nama asli Kapitan Pattimura?", "id": "Thomas Matulessy." },
  { "en": "Perlawanan Trunojoyo berasal dari kerajaan mana?", "id": "Madura." },
  { "en": "Siapa Untung Suropati yang melawan VOC?", "id": "Mantan budak VOC." },
  { "en": "Perang apa yang melibatkan Pangeran Mangkubumi dan Mas Said?", "id": "Perang Tahta Jawa Ketiga." },
  { "en": "Apa tujuan utama Daendels membangun Jalan Raya Pos?", "id": "Pertahanan militer." },
  { "en": "Apa nama sistem pajak tanah Raffles?", "id": "Landrent." },
  { "en": "Di mana Raffles menemukan bunga bangkai raksasa?", "id": "Bengkulu." },
  { "en": "Apa nama ilmiah bunga bangkai yang ditemukan Raffles?", "id": "Rafflesia arnoldii." },
  { "en": "Konvensi apa yang mengembalikan Hindia Belanda dari Inggris ke Belanda?", "id": "Konvensi London (1814)." },
  { "en": "Siapa tokoh Belanda yang memperkenalkan Politik Etis?", "id": "Conrad Theodore van Deventer." },
  { "en": "Apa judul artikel Van Deventer yang menginspirasi Politik Etis?", "id": "Een Eereschuld (Utang Kehormatan)." },
  { "en": "Sekolah kedokteran untuk pribumi pada masa Hindia Belanda?", "id": "STOVIA." },
  { "en": "Apa kepanjangan STOVIA?", "id": "School tot Opleiding van Indische Artsen." },
  { "en": "Sekolah pamong praja untuk pribumi?", "id": "OSVIA/MOSVIA." },
  { "en": "Siapa tokoh yang memimpin Perang Banjar?", "id": "Pangeran Antasari." },
  { "en": "Di mana pusat perlawanan Pangeran Antasari?", "id": "Pedalaman Kalimantan Selatan." },
  { "en": "Siapa raja Bali yang melawan Belanda dalam Perang Puputan Margarana?", "id": "I Gusti Ngurah Rai (Perang Kemerdekaan)." },
  { "en": "Perang puputan terkenal di Bali melawan Belanda (awal abad 20)?", "id": "Puputan Badung (1906)." },
  { "en": "Siapa raja Sisingamangaraja XII yang melawan Belanda?", "id": "Sisingamangaraja XII." },
  { "en": "Dari daerah mana Sisingamangaraja XII berasal?", "id": "Tanah Batak, Tapanuli." },
  { "en": "Siapa penulis buku \"Habis Gelap Terbitlah Terang\"?", "id": "R.A. Kartini (kumpulan surat)." },
  { "en": "Siapa tokoh wanita dari Jawa Barat yang mendirikan sekolah?", "id": "Dewi Sartika." },
  { "en": "Apa nama sekolah yang didirikan Dewi Sartika?", "id": "Sakola Kautamaan Istri." },
  { "en": "Siapa tokoh wanita dari Minahasa yang memperjuangkan pendidikan?", "id": "Maria Walanda Maramis." },
  { "en": "Organisasi pergerakan yang berfokus pada studi dan kebudayaan Jawa?", "id": "Budi Utomo (fokus awal)." },
  { "en": "Siapa penggagas utama Budi Utomo selain Dr. Sutomo?", "id": "Dr. Wahidin Sudirohusodo." },
  { "en": "Apa nama surat kabar pertama yang dimiliki pribumi?", "id": "Medan Prijaji." },
  { "en": "Siapa pendiri Medan Prijaji?", "id": "Tirto Adhi Soerjo." },
  { "en": "Volksraad adalah lembaga apa pada masa Hindia Belanda?", "id": "Dewan Rakyat." },
  { "en": "Apakah Volksraad memiliki kekuasaan legislatif penuh?", "id": "Tidak." },
  { "en": "Siapa tokoh PNI yang ditangkap bersama Soekarno?", "id": "Gatot Mangkoepradja, Maskoen Soemadiredja." },
  { "en": "Di penjara mana Soekarno menulis \"Indonesia Menggugat\"?", "id": "Penjara Banceuy, Bandung." },
  { "en": "Petisi apa yang diajukan Soetardjo Kartohadikoesoemo ke Volksraad?", "id": "Petisi Soetardjo." },
  { "en": "Apa isi Petisi Soetardjo?", "id": "Otonomi Indonesia dalam Kerajaan Belanda." },
  { "en": "Organisasi gabungan partai politik Indonesia?", "id": "GAPI (Gabungan Politik Indonesia)." },
  { "en": "Apa tuntutan utama GAPI?", "id": "Indonesia Berparlemen." },
  { "en": "Kapan Jepang pertama kali mendarat di Indonesia?", "id": "11 Januari 1942." },
  { "en": "Di mana Jepang pertama mendarat?", "id": "Tarakan, Kalimantan Timur." },
  { "en": "Kapan Belanda menyerah tanpa syarat kepada Jepang di Kalijati?", "id": "8 Maret 1942." },
  { "en": "Apa semboyan Jepang untuk menarik simpati rakyat Indonesia?", "id": "Asia untuk bangsa Asia." },
  { "en": "Gerakan Tiga A yang dipropagandakan Jepang?", "id": "Nippon Pemimpin Asia, Pelindung Asia, Cahaya Asia." },
  { "en": "Siapa pemimpin Gerakan Tiga A?", "id": "Mr. Syamsuddin." },
  { "en": "Organisasi massa bentukan Jepang pengganti Gerakan Tiga A?", "id": "PUTERA." },
  { "en": "Apa kepanjangan PUTERA?", "id": "Pusat Tenaga Rakyat." },
  { "en": "Siapa empat serangkai pemimpin PUTERA?", "id": "Soekarno, Hatta, Ki Hajar Dewantara, K.H. Mas Mansyur." },
  { "en": "Organisasi Islam bentukan Jepang?", "id": "MIAI (kemudian Masyumi)." },
  { "en": "Apa kepanjangan MIAI?", "id": "Majelis Islam A'la Indonesia." },
  { "en": "Organisasi semi-militer pemuda bentukan Jepang?", "id": "Seinendan." },
  { "en": "Barisan pembantu polisi bentukan Jepang?", "id": "Keibodan." },
  { "en": "Barisan wanita bentukan Jepang?", "id": "Fujinkai." },
  { "en": "Pasukan sukarela pembela tanah air bentukan Jepang?", "id": "PETA." },
  { "en": "Apa kepanjangan PETA?", "id": "Pembela Tanah Air." },
  { "en": "Siapa tokoh PETA yang terkenal dari Blitar?", "id": "Supriyadi." },
  { "en": "Apa itu Romusha?", "id": "Pekerja paksa zaman Jepang." },
  { "en": "Apa tujuan Jepang membentuk Romusha?", "id": "Membangun infrastruktur perang." },
  { "en": "Apa nama mata uang yang berlaku zaman Jepang?", "id": "Uang invasi Jepang." },
  { "en": "Lagu apa yang wajib dinyanyikan selain Kimigayo?", "id": "Indonesia Raya (kemudian dibatasi)." },
  { "en": "Apa janji yang diberikan Perdana Menteri Koiso?", "id": "Kemerdekaan Indonesia kelak." },
  { "en": "Kapan janji Koiso diucapkan?", "id": "September 1944." },
  { "en": "Siapa nama lain BPUPKI dalam bahasa Jepang?", "id": "Dokuritsu Junbi Cosakai." },
  { "en": "Berapa jumlah anggota BPUPKI awalnya?", "id": "62 orang." },
  { "en": "Sidang BPUPKI kedua membahas apa?", "id": "Rancangan Undang-Undang Dasar." },
  { "en": "Siapa yang mengusulkan rumusan Pancasila pada 1 Juni 1945?", "id": "Soekarno." },
  { "en": "Panitia kecil yang dibentuk BPUPKI untuk merumuskan dasar negara?", "id": "Panitia Sembilan." },
  { "en": "Apa hasil kerja Panitia Sembilan?", "id": "Piagam Jakarta." },
  { "en": "Apa sila pertama dalam Piagam Jakarta?", "id": "Ketuhanan dengan kewajiban menjalankan syariat Islam bagi pemeluknya." },
  { "en": "Badan yang dibentuk setelah BPUPKI dibubarkan?", "id": "PPKI." },
  { "en": "Apa kepanjangan PPKI?", "id": "Panitia Persiapan Kemerdekaan Indonesia." },
  { "en": "Siapa ketua PPKI?", "id": "Ir. Soekarno." },
  { "en": "Siapa wakil ketua PPKI?", "id": "Drs. Mohammad Hatta." },
  { "en": "Nama lain PPKI dalam bahasa Jepang?", "id": "Dokuritsu Junbi Inkai." },
  { "en": "Kota mana di Jepang yang dibom atom sekutu pertama?", "id": "Hiroshima." },
  { "en": "Kapan Hiroshima dibom?", "id": "6 Agustus 1945." },
  { "en": "Kota kedua yang dibom atom?", "id": "Nagasaki." },
  { "en": "Kapan Nagasaki dibom?", "id": "9 Agustus 1945." },
  { "en": "Apa dampak bom atom bagi Jepang?", "id": "Menyerah kepada Sekutu." },
  { "en": "Kapan Jepang menyerah kepada Sekutu?", "id": "15 Agustus 1945." },
  { "en": "Siapa yang mendengar berita kekalahan Jepang lewat radio?", "id": "Sutan Sjahrir." },
  { "en": "Peristiwa apa yang terjadi pada 16 Agustus 1945 dini hari?", "id": "Peristiwa Rengasdengklok." },
  { "en": "Siapa yang diculik ke Rengasdengklok?", "id": "Soekarno dan Hatta." },
  { "en": "Siapa golongan yang menculik Soekarno-Hatta?", "id": "Golongan muda." },
  { "en": "Apa tujuan penculikan ke Rengasdengklok?", "id": "Mendesak proklamasi segera." },
  { "en": "Siapa tokoh golongan tua yang menengahi di Rengasdengklok?", "id": "Ahmad Soebardjo." },
  { "en": "Di mana naskah proklamasi dirumuskan?", "id": "Rumah Laksamana Tadashi Maeda." },
  { "en": "Siapa Laksamana Maeda?", "id": "Perwira Angkatan Laut Jepang." },
  { "en": "Siapa saja yang menyusun konsep naskah proklamasi?", "id": "Soekarno, Hatta, Ahmad Soebardjo." },
  { "en": "Siapa yang mengetik naskah proklamasi?", "id": "Sayuti Melik." },
  { "en": "Perubahan penting apa dalam naskah ketikan proklamasi?", "id": "Kata \"wakil-wakil bangsa Indonesia\"." },
  { "en": "Kapan Proklamasi Kemerdekaan Indonesia dibacakan?", "id": "17 Agustus 1945." },
  { "en": "Di mana Proklamasi Kemerdekaan Indonesia dibacakan?", "id": "Jalan Pegangsaan Timur No. 56, Jakarta." },
  { "en": "Siapa yang membacakan teks Proklamasi?", "id": "Ir. Soekarno." },
  { "en": "Siapa yang mendampingi Soekarno saat proklamasi?", "id": "Drs. Mohammad Hatta." },
  { "en": "Siapa pengibar bendera pertama saat proklamasi?", "id": "Latief Hendraningrat, Suhud Sastrosoeparto." },
  { "en": "Siapa yang menjahit bendera pusaka?", "id": "Fatmawati." },
  { "en": "Apa warna bendera pusaka?", "id": "Merah Putih." },
  { "en": "Kapan sidang PPKI pertama setelah proklamasi?", "id": "18 Agustus 1945." },
  { "en": "Apa hasil sidang PPKI pertama?", "id": "Mengesahkan UUD, memilih Presiden/Wapres." },
  { "en": "Siapa Presiden pertama Indonesia?", "id": "Ir. Soekarno." },
  { "en": "Siapa Wakil Presiden pertama Indonesia?", "id": "Drs. Mohammad Hatta." },
  { "en": "Berapa jumlah provinsi yang dibentuk awal kemerdekaan?", "id": "Delapan provinsi." },
  { "en": "Badan Keamanan Rakyat dibentuk untuk apa?", "id": "Menjaga keamanan umum." },
  { "en": "Komite Nasional Indonesia Pusat (KNIP) berfungsi sebagai apa?", "id": "Parlemen sementara." },
  { "en": "Siapa ketua KNIP pertama?", "id": "Kasman Singodimedjo." },
  { "en": "Apa tugas utama pasukan Sekutu (AFNEI) di Indonesia?", "id": "Melucuti tentara Jepang." },
  { "en": "Siapa pemimpin AFNEI?", "id": "Letnan Jenderal Sir Philip Christison." },
  { "en": "Apa NICA itu?", "id": "Netherlands Indies Civil Administration." },
  { "en": "Tujuan NICA datang ke Indonesia?", "id": "Memulihkan kekuasaan Belanda." },
  { "en": "Pertempuran besar pertama melawan Sekutu di mana?", "id": "Surabaya." },
  { "en": "Kapan pertempuran Surabaya terjadi?", "id": "Oktober-November 1945." },
  { "en": "Siapa tokoh pemuda yang membakar semangat arek-arek Suroboyo?", "id": "Bung Tomo." },
  { "en": "Siapa Brigadir Jenderal Inggris yang tewas di Surabaya?", "id": "A.W.S. Mallaby." },
  { "en": "Tanggal berapa Hari Pahlawan diperingati (mengenang pertempuran Surabaya)?", "id": "10 November." },
  { "en": "Pertempuran di Ambarawa dipimpin oleh siapa?", "id": "Kolonel Soedirman." },
  { "en": "Peristiwa heroik di Bandung Selatan dikenal sebagai?", "id": "Bandung Lautan Api." },
  { "en": "Kapan Bandung Lautan Api terjadi?", "id": "Maret 1946." },
  { "en": "Pertempuran di Medan dikenal sebagai?", "id": "Medan Area." },
  { "en": "Agresi Militer Belanda I dilancarkan kapan?", "id": "21 Juli 1947." },
  { "en": "Apa nama lain Agresi Militer Belanda I?", "id": "Operatie Product." },
  { "en": "Perjanjian apa yang mengakhiri Agresi Militer Belanda I?", "id": "Perjanjian Renville." },
  { "en": "Di atas kapal apa Perjanjian Renville ditandatangani?", "id": "USS Renville." },
  { "en": "Siapa delegasi Indonesia dalam Perjanjian Renville?", "id": "Amir Sjarifuddin." },
  { "en": "Apa isi merugikan dari Perjanjian Renville?", "id": "Wilayah RI semakin sempit." },
  { "en": "Agresi Militer Belanda II dilancarkan kapan?", "id": "19 Desember 1948." },
  { "en": "Apa sasaran utama Agresi Militer Belanda II?", "id": "Yogyakarta, ibu kota RI." },
  { "en": "Siapa pemimpin RI yang ditawan saat Agresi Militer II?", "id": "Soekarno, Hatta, Sjahrir." },
  { "en": "Pemerintahan Darurat Republik Indonesia (PDRI) dibentuk di mana?", "id": "Bukittinggi, Sumatra Barat." },
  { "en": "Siapa ketua PDRI?", "id": "Sjafruddin Prawiranegara." },
  { "en": "Serangan Umum 1 Maret 1949 terjadi di kota mana?", "id": "Yogyakarta." },
  { "en": "Siapa yang memprakarsai Serangan Umum 1 Maret?", "id": "Sri Sultan Hamengkubuwono IX." },
  { "en": "Siapa komandan lapangan Serangan Umum 1 Maret?", "id": "Letkol Soeharto." },
  { "en": "Perjanjian apa yang mengawali penarikan pasukan Belanda dari Yogyakarta?", "id": "Perjanjian Roem-Royen." },
  { "en": "Konferensi Meja Bundar (KMB) diadakan di mana?", "id": "Den Haag, Belanda." },
  { "en": "Kapan KMB dilaksanakan?", "id": "Agustus-November 1949." },
  { "en": "Siapa ketua delegasi Indonesia di KMB?", "id": "Mohammad Hatta." },
  { "en": "Apa hasil utama KMB?", "id": "Pengakuan kedaulatan Indonesia." },
  { "en": "Kapan Belanda secara resmi mengakui kedaulatan Indonesia?", "id": "27 Desember 1949." },
  { "en": "Bentuk negara Indonesia hasil KMB?", "id": "Republik Indonesia Serikat (RIS)." },
  { "en": "Siapa Presiden RIS?", "id": "Ir. Soekarno." },
  { "en": "Siapa Perdana Menteri RIS pertama?", "id": "Mohammad Hatta." },
  { "en": "Masalah apa yang belum selesai dalam KMB?", "id": "Masalah Irian Barat." },
  { "en": "Kapan RIS bubar dan Indonesia kembali ke NKRI?", "id": "17 Agustus 1950." },
  { "en": "UUD apa yang digunakan pada masa Demokrasi Parlementer?", "id": "UUDS 1950." },
  { "en": "Sistem pemerintahan pada masa Demokrasi Parlementer?", "id": "Parlementer." },
  { "en": "Siapa kepala pemerintahan dalam sistem parlementer?", "id": "Perdana Menteri." },
  { "en": "Apa ciri utama masa Demokrasi Parlementer?", "id": "Sering berganti kabinet." },
  { "en": "Kabinet pertama setelah kembali ke NKRI?", "id": "Kabinet Natsir." },
  { "en": "Pemilihan Umum pertama di Indonesia diadakan tahun berapa?", "id": "Tahun 1955." },
  { "en": "Apa yang dipilih dalam Pemilu 1955?", "id": "Anggota DPR dan Konstituante." },
  { "en": "Siapa Perdana Menteri saat Pemilu 1955?", "id": "Burhanuddin Harahap." },
  { "en": "Empat partai besar pemenang Pemilu 1955?", "id": "PNI, Masyumi, NU, PKI." },
  { "en": "Apa tugas utama Konstituante?", "id": "Menyusun UUD baru." },
  { "en": "Mengapa Konstituante gagal menyusun UUD baru?", "id": "Perdebatan dasar negara." },
  { "en": "Dekrit Presiden dikeluarkan pada tanggal berapa?", "id": "5 Juli 1959." },
  { "en": "Apa isi utama Dekrit Presiden 5 Juli 1959?", "id": "Kembali ke UUD 1945." },
  { "en": "Era setelah Dekrit Presiden 1959 disebut apa?", "id": "Demokrasi Terpimpin." },
  { "en": "Siapa pemimpin utama era Demokrasi Terpimpin?", "id": "Presiden Soekarno." },
  { "en": "Apa konsep politik Soekarno yang menyatukan Nasionalis, Agama, Komunis?", "id": "NASAKOM." },
  { "en": "Apa kepanjangan Manipol USDEK?", "id": "Manifesto Politik, UUD 45, Sosialisme Indonesia, Demokrasi Terpimpin, Ekonomi Terpimpin, Kepribadian Indonesia." },
  { "en": "Operasi pembebasan Irian Barat disebut apa?", "id": "Operasi Trikora." },
  { "en": "Apa kepanjangan Trikora?", "id": "Tri Komando Rakyat." },
  { "en": "Siapa tokoh yang memimpin Komando Mandala pembebasan Irian Barat?", "id": "Mayor Jenderal Soeharto." },
  { "en": "Perjanjian apa yang mengakhiri sengketa Irian Barat?", "id": "Perjanjian New York." },
  { "en": "Kapan Irian Barat resmi kembali ke Indonesia?", "id": "1 Mei 1963." },
  { "en": "Penentuan Pendapat Rakyat (Pepera) di Irian Barat tahun berapa?", "id": "Tahun 1969." },
  { "en": "Politik konfrontasi Indonesia terhadap Malaysia disebut apa?", "id": "Dwikora (Ganyang Malaysia)." },
  { "en": "Kapan Gerakan 30 September (G30S) terjadi?", "id": "30 September 1965." },
  { "en": "Siapa saja jenderal yang menjadi korban G30S?", "id": "Ahmad Yani, S. Parman, dll." },
  { "en": "Siapa satu-satunya jenderal yang selamat dari G30S?", "id": "A.H. Nasution." },
  { "en": "Siapa putri A.H. Nasution yang menjadi korban?", "id": "Ade Irma Suryani Nasution." },
  { "en": "Surat Perintah Sebelas Maret (Supersemar) dikeluarkan kapan?", "id": "11 Maret 1966." },
  { "en": "Siapa yang mengeluarkan Supersemar?", "id": "Presiden Soekarno." },
  { "en": "Kepada siapa Supersemar diberikan?", "id": "Letjen Soeharto." },
  { "en": "Apa isi pokok Supersemar?", "id": "Kewenangan memulihkan keamanan." },
  { "en": "Era pemerintahan setelah G30S dikenal sebagai?", "id": "Orde Baru." },
  { "en": "Siapa pemimpin utama Orde Baru?", "id": "Jenderal Soeharto." },
  { "en": "Kapan Soeharto resmi menjadi Presiden menggantikan Soekarno?", "id": "Maret 1968 (definitif)." },
  { "en": "Apa fokus utama pembangunan Orde Baru?", "id": "Ekonomi (Trilogi Pembangunan)." },
  { "en": "Apa isi Trilogi Pembangunan?", "id": "Stabilitas, Pertumbuhan, Pemerataan." },
  { "en": "Rencana pembangunan lima tahunan Orde Baru disebut?", "id": "Repelita." },
  { "en": "Apa kepanjangan Repelita?", "id": "Rencana Pembangunan Lima Tahun." },
  { "en": "Partai politik dominan pada masa Orde Baru?", "id": "Golongan Karya (Golkar)." },
  { "en": "Penyederhanaan partai politik menghasilkan berapa partai?", "id": "Tiga (Golkar, PPP, PDI)." },
  { "en": "Apa konsep dwifungsi ABRI?", "id": "Peran sosial-politik dan pertahanan." },
  { "en": "Program keluarga berencana yang digalakkan Orde Baru?", "id": "KB." },
  { "en": "Semboyan KB yang terkenal?", "id": "Dua anak cukup." },
  { "en": "Timor Timur berintegrasi dengan Indonesia tahun berapa?", "id": "Tahun 1976." },
  { "en": "Apa nama operasi militer Indonesia di Timor Timur?", "id": "Operasi Seroja." },
  { "en": "Peristiwa demonstrasi mahasiswa besar yang menuntut reformasi?", "id": "Mei 1998." },
  { "en": "Apa pemicu utama gerakan reformasi 1998?", "id": "Krisis moneter Asia." },
  { "en": "Tragedi penembakan mahasiswa di Jakarta pada Mei 1998?", "id": "Tragedi Trisakti." },
  { "en": "Kapan Soeharto mengundurkan diri dari jabatan Presiden?", "id": "21 Mei 1998." },
  { "en": "Siapa yang menggantikan Soeharto sebagai Presiden?", "id": "B.J. Habibie." },
  { "en": "Apa agenda utama gerakan Reformasi?", "id": "Demokratisasi, supremasi hukum, pemberantasan KKN." },
  { "en": "Apa kepanjangan KKN?", "id": "Korupsi, Kolusi, Nepotisme." },
  { "en": "Kebijakan penting Habibie terkait Timor Timur?", "id": "Referendum (Penentuan Pendapat)." },
  { "en": "Kapan referendum Timor Timur dilaksanakan?", "id": "30 Agustus 1999." },
  { "en": "Apa hasil referendum Timor Timur?", "id": "Memilih merdeka." },
  { "en": "Pemilu pertama era Reformasi diadakan tahun berapa?", "id": "Tahun 1999." },
  { "en": "Siapa Presiden terpilih hasil Pemilu 1999 (melalui MPR)?", "id": "Abdurrahman Wahid (Gus Dur)." },
  { "en": "Siapa Wakil Presiden pada masa Gus Dur?", "id": "Megawati Soekarnoputri." },
  { "en": "Kebijakan Gus Dur yang kontroversial terkait MPR/DPR?", "id": "Pembekuan MPR/DPR (Dekrit)." },
  { "en": "Siapa yang menggantikan Gus Dur setelah Sidang Istimewa MPR?", "id": "Megawati Soekarnoputri." },
  { "en": "Kapan Megawati menjadi Presiden?", "id": "Juli 2001." },
  { "en": "Siapa Wakil Presiden pada masa Megawati?", "id": "Hamzah Haz." },
  { "en": "Pemilihan Presiden secara langsung pertama kali tahun berapa?", "id": "Tahun 2004." },
  { "en": "Siapa Presiden terpilih hasil Pemilu 2004?", "id": "Susilo Bambang Yudhoyono (SBY)." },
  { "en": "Siapa Wakil Presiden SBY periode pertama?", "id": "Jusuf Kalla." },
  { "en": "Lembaga pemberantasan korupsi yang dibentuk era Reformasi?", "id": "KPK (Komisi Pemberantasan Korupsi)." },
  { "en": "Amandemen UUD 1945 dilakukan berapa kali selama Reformasi?", "id": "Empat kali." },
  { "en": "Otonomi daerah mulai diberlakukan secara luas pada era?", "id": "Reformasi." },
  { "en": "Kitab Arjunawiwaha ditulis oleh siapa?", "id": "Mpu Kanwa." },
  { "en": "Candi bercorak Hindu di Jawa Timur peninggalan Singasari?", "id": "Candi Jago, Candi Kidal." },
  { "en": "Masjid tertua di Jawa yang masih berdiri?", "id": "Masjid Agung Demak (dianggap)." },
  { "en": "Seni ukir kayu terkenal dari daerah mana?", "id": "Jepara." },
  { "en": "Tarian Saman berasal dari provinsi mana?", "id": "Aceh." },
  { "en": "Alat musik tradisional Jawa berupa gamelan terdiri dari apa saja?", "id": "Gong, bonang, saron." },
  { "en": "Wayang kulit menggunakan bahan apa?", "id": "Kulit kerbau/sapi." },
  { "en": "Cerita Ramayana dan Mahabharata berasal dari negara mana?", "id": "India." },
  { "en": "Siapa pelukis Indonesia yang terkenal dengan aliran romantisisme?", "id": "Raden Saleh." },
  { "en": "Siapa sastrawan angkatan Pujangga Baru yang terkenal?", "id": "Sutan Takdir Alisjahbana." },
  { "en": "Karya Sutan Takdir Alisjahbana yang terkenal?", "id": "Layar Terkembang." },
  { "en": "Siapa penyair yang dijuluki \"Si Burung Merak\"?", "id": "W.S. Rendra." },
  { "en": "Siapa novelis wanita terkenal pengarang \"Saman\" dan \"Larung\"?", "id": "Ayu Utami." },
  { "en": "Sistem kekerabatan matrilineal dianut suku mana?", "id": "Minangkabau." },
  { "en": "Sistem kasta di Bali merupakan pengaruh agama apa?", "id": "Hindu." },
  { "en": "Tradisi lompat batu di Nias disebut apa?", "id": "Fahombo." },
  { "en": "Rumah adat Sumatra Barat disebut apa?", "id": "Rumah Gadang." },
  { "en": "Rumah adat Toraja disebut apa?", "id": "Tongkonan." },
  { "en": "Upacara pembakaran mayat di Bali disebut?", "id": "Ngaben." },
  { "en": "Perayaan Sekaten di Yogyakarta dan Surakarta untuk memperingati apa?", "id": "Maulid Nabi Muhammad SAW." },
  { "en": "Tokoh pendidikan yang dikenal sebagai Bapak Pendidikan Nasional?", "id": "Ki Hadjar Dewantara." },
  { "en": "Universitas tertua di Indonesia?", "id": "Universitas Indonesia (cikal bakal STOVIA)." },
  { "en": "Bahasa persatuan Indonesia sebelum resmi Bahasa Indonesia?", "id": "Melayu (Pasar)." },
  { "en": "Film Indonesia pertama yang diproduksi tahun 1926?", "id": "Loetoeng Kasaroeng." },
  { "en": "Stasiun radio pertama milik pribumi?", "id": "Solosche Radio Vereeniging (SRV)." },
  { "en": "Siapa tokoh pers yang juga sastrawan angkatan Balai Pustaka?", "id": "Marah Roesli." },
  { "en": "Karya Marah Roesli yang terkenal?", "id": "Sitti Nurbaya." },
  { "en": "Organisasi kepanduan pertama di Indonesia?", "id": "Javaansche Padvinders Organisatie." },
  { "en": "Siapa tokoh yang mempopulerkan istilah \"Indonesia\" secara politik?", "id": "Indische Partij (Suwardi dkk)." },
  { "en": "Istilah \"Indonesia\" pertama kali digunakan secara ilmiah oleh siapa?", "id": "James Richardson Logan." },
  { "en": "Peristiwa penting apa yang menyatukan berbagai kerajaan kecil di Bali?", "id": "Perang Jagaraga." },
  { "en": "Perang antara Kesultanan Gowa melawan VOC dikenal sebagai?", "id": "Perang Makassar." },
  { "en": "Siapa pahlawan dari Maluku Utara yang memimpin perlawanan terhadap VOC?", "id": "Sultan Nuku." },
  { "en": "Kebijakan \"Devide et Impera\" artinya apa?", "id": "Politik adu domba." },
  { "en": "Jalur perdagangan laut kuno yang menghubungkan Asia, Afrika, Eropa?", "id": "Jalur Sutra Maritim." },
  { "en": "Komoditas penting dari Kepulauan Banda selain cengkeh?", "id": "Pala." },
  { "en": "Sistem irigasi kuno di Bali disebut apa?", "id": "Subak." },
  { "en": "Candi Prambanan dibangun pada masa dinasti apa?", "id": "Sanjaya." },
  { "en": "Kitab yang berisi ajaran moral dan etika Jawa kuno?", "id": "Serat Wulangreh." },
  { "en": "Siapa penulis Serat Wulangreh?", "id": "Pakubuwana IV." },
  { "en": "Tokoh wanita pejuang dari Sulawesi Selatan?", "id": "Opu Daeng Risaju." },
  { "en": "Perlawanan rakyat Banten terhadap VOC dipimpin oleh siapa (selain Sultan Ageng)?", "id": "Kiai Tapa." },
  { "en": "Siapa gubernur jenderal VOC yang terakhir?", "id": "Pieter Gerardus van Overstraten." },
  { "en": "Undang-Undang Agraria tahun 1870 bertujuan untuk apa?", "id": "Membuka investasi swasta asing." },
  { "en": "Undang-Undang Gula (Suikerwet) 1870 mengatur apa?", "id": "Industri gula." },
  { "en": "Sekolah guru untuk pribumi pada masa kolonial?", "id": "Kweekschool." },
  { "en": "Siapa tokoh Muhammadiyah yang juga pahlawan nasional?", "id": "K.H. Ahmad Dahlan." },
  { "en": "Siapa pendiri Nahdlatul Ulama (NU)?", "id": "K.H. Hasyim Asy'ari." },
  { "en": "Pemberontakan PKI pertama terhadap Belanda tahun berapa?", "id": "Tahun 1926-1927." },
  { "en": "Di mana pemberontakan PKI 1926/1927 terjadi?", "id": "Jawa Barat, Sumatra Barat." },
  { "en": "Konferensi Asia-Afrika diadakan di kota mana?", "id": "Bandung." },
  { "en": "Kapan Konferensi Asia-Afrika dilaksanakan?", "id": "April 1955." },
  { "en": "Apa hasil utama Konferensi Asia-Afrika?", "id": "Dasasila Bandung." },
  { "en": "Gerakan Non-Blok (GNB) salah satunya diprakarsai oleh siapa dari Indonesia?", "id": "Soekarno." },
  { "en": "Pemberontakan DI/TII dipimpin oleh siapa?", "id": "Sekarmadji Maridjan Kartosoewirjo." },
  { "en": "Di mana pusat pemberontakan DI/TII Kartosoewirjo?", "id": "Jawa Barat." },
  { "en": "Pemberontakan PRRI/Permesta terjadi di daerah mana?", "id": "Sumatra dan Sulawesi." },
  { "en": "Apa kepanjangan PRRI?", "id": "Pemerintah Revolusioner Republik Indonesia." },
  { "en": "Peristiwa Malari terjadi pada tahun berapa?", "id": "1974." },
  { "en": "Apa kepanjangan Malari?", "id": "Malapetaka Lima Belas Januari." },
  { "en": "Penataran P4 bertujuan untuk apa pada masa Orde Baru?", "id": "Indoktrinasi Pancasila." },
  { "en": "Apa kepanjangan P4?", "id": "Pedoman Penghayatan dan Pengamalan Pancasila." },
  { "en": "Peristiwa Tanjung Priok terjadi tahun berapa?", "id": "1984." },
  { "en": "Undang-Undang Antisubversi digunakan untuk apa oleh Orde Baru?", "id": "Membungkam lawan politik." },
  { "en": "Siapa aktivis HAM yang dibunuh pada masa Orde Baru?", "id": "Marsinah (buruh)." },
  { "en": "Badan Penyangga dan Pemasaran Cengkeh (BPPC) terkait dengan siapa?", "id": "Tommy Soeharto." },
  { "en": "Bank Indonesia dilikuidasi pada krisis moneter tahun berapa?", "id": "Tahun 1997-1998 (banyak bank)." },
  { "en": "Siapa yang menandatangani Letter of Intent dengan IMF?", "id": "Presiden Soeharto." },
  { "en": "Apa nama gerakan mahasiswa yang menumbangkan Orde Baru?", "id": "Gerakan Reformasi 1998." },
  { "en": "Amien Rais adalah tokoh dari organisasi apa saat Reformasi?", "id": "Muhammadiyah." },
  { "en": "Pemilu Legislatif 2004 dimenangkan partai apa?", "id": "Partai Golkar." },
  { "en": "Bencana tsunami besar melanda Aceh tahun berapa?", "id": "Desember 2004." },
  { "en": "Perjanjian damai antara RI dan GAM ditandatangani di mana?", "id": "Helsinki, Finlandia." },
  { "en": "Kapan perjanjian damai Helsinki ditandatangani?", "id": "Agustus 2005." },
  { "en": "Siapa Presiden Indonesia yang menjabat dua periode hasil pemilu langsung?", "id": "Susilo Bambang Yudhoyono." },
  { "en": "Program pemerintah SBY yang terkenal untuk bantuan langsung tunai?", "id": "BLT." },
  { "en": "Siapa Presiden Indonesia ketujuh?", "id": "Joko Widodo." },
  { "en": "Kabinet kerja pertama Joko Widodo bernama?", "id": "Kabinet Kerja." },
  { "en": "Ibukota Nusantara (IKN) direncanakan berlokasi di provinsi mana?", "id": "Kalimantan Timur." },
  { "en": "Siapa pendiri kerajaan Demak yang merupakan putra Brawijaya V?", "id": "Raden Patah." },
  { "en": "Ekspedisi militer Majapahit untuk menaklukkan Bali dipimpin oleh siapa?", "id": "Gajah Mada." },
  { "en": "Perang Bubat adalah perselisihan antara Majapahit dengan kerajaan apa?", "id": "Sunda." },
  { "en": "Pelabuhan utama Kesultanan Aceh pada masa jayanya?", "id": "Banda Aceh." },
  { "en": "Siapa tokoh Sarekat Islam yang kemudian berhaluan kiri?", "id": "Semaun, Darsono." },
  { "en": "Pemogokan buruh kereta api besar tahun 1923 diorganisir oleh?", "id": "VSTP (PKI)." },
  { "en": "Majalah pergerakan nasional yang terkenal pada tahun 1930-an?", "id": "Daulat Ra'jat." },
  { "en": "Siapa tokoh pendidikan wanita dari Minangkabau selain Rohana Kudus?", "id": "Rahmah El Yunusiyah." },
  { "en": "Apa nama sekolah yang didirikan Rahmah El Yunusiyah?", "id": "Diniyah Putri Padang Panjang." },
  { "en": "Siapa tokoh yang memimpin pemberontakan PETA di Gumilir, Cilacap?", "id": "Kusaeri." },
  { "en": "Perundingan Linggarjati menghasilkan kesepakatan tentang apa?", "id": "Pengakuan de facto RI." },
  { "en": "Siapa Perdana Menteri Indonesia saat Perundingan Linggarjati?", "id": "Sutan Sjahrir." },
  { "en": "Komisi Tiga Negara (KTN) dibentuk oleh PBB untuk apa?", "id": "Menengahi sengketa Indonesia-Belanda." },
  { "en": "Anggota KTN terdiri dari negara mana saja?", "id": "Australia, Belgia, Amerika Serikat." },
  { "en": "Sistem ekonomi Ali-Baba bertujuan untuk apa?", "id": "Memajukan pengusaha pribumi." },
  { "en": "Kebijakan Gunting Sjafruddin bertujuan untuk apa?", "id": "Mengurangi jumlah uang beredar." },
  { "en": "Proyek Mercusuar Soekarno yang terkenal?", "id": "Monas, Stadion Senayan." },
  { "en": "Siapa arsitek Masjid Istiqlal Jakarta?", "id": "Frederich Silaban." },
  { "en": "Operasi Woyla dilakukan untuk membebaskan sandera pesawat apa?", "id": "Garuda Indonesia DC-9." },
  { "en": "Insiden Santa Cruz terjadi di kota mana di Timor Timur?", "id": "Dili." },
  { "en": "Komisi Nasional Hak Asasi Manusia (Komnas HAM) dibentuk tahun?", "id": "Tahun 1993." },
  { "en": "Siapa Ketua MPR yang melantik Abdurrahman Wahid dan Megawati?", "id": "Amien Rais." },
  { "en": "Badan Rekonstruksi dan Rehabilitasi (BRR) dibentuk untuk menangani pasca-bencana apa?", "id": "Tsunami Aceh-Nias." },
  { "en": "Siapa yang menjabat sebagai Wakil Presiden pada periode kedua SBY?", "id": "Boediono." },
  { "en": "Program Kartu Indonesia Pintar (KIP) diluncurkan oleh Presiden siapa?", "id": "Joko Widodo." },
  { "en": "Siapa pencipta lambang negara Garuda Pancasila?", "id": "Sultan Hamid II." },
  { "en": "Prasasti Tarumanegara yang berisi tentang penggalian Sungai Gomati?", "id": "Prasasti Tugu." },
  { "en": "Berapa lama penggalian Sungai Gomati menurut Prasasti Tugu?", "id": "21 hari." },
  { "en": "Apa nama ibu kota Kerajaan Kalingga menurut catatan Tiongkok?", "id": "She-po (diperkirakan Jawa)." },
  { "en": "Siapa biksu Tiongkok yang singgah di Kalingga untuk menerjemahkan kitab?", "id": "Hwuning." },
  { "en": "Prasasti Sriwijaya yang berisi kutukan bagi yang tidak patuh?", "id": "Prasasti Kota Kapur." },
  { "en": "Apa nama lain untuk wilayah kekuasaan Sriwijaya?", "id": "Bhumi Sriwijaya." },
  { "en": "Candi Buddha di Thailand Selatan yang diduga peninggalan Sriwijaya?", "id": "Candi Wat Phra Borommathat Chaiya." },
  { "en": "Siapa raja Mataram Kuno yang memindahkan pusat kerajaan ke Jawa Timur?", "id": "Mpu Sindok." },
  { "en": "Apa gelar Mpu Sindok setelah menjadi raja Medang?", "id": "Sri Isyana Tunggawijaya." },
  { "en": "Prasasti Mpu Sindok yang terkenal dari daerah Nganjuk?", "id": "Prasasti Anjukladang." },
  { "en": "Apa isi penting Prasasti Anjukladang?", "id": "Penetapan Sima untuk bangunan suci." },
  { "en": "Siapa Dharmawangsa Teguh, raja Medang yang menyerang Sriwijaya?", "id": "Cucu Mpu Sindok." },
  { "en": "Peristiwa apa yang meruntuhkan Kerajaan Medang pada masa Dharmawangsa?", "id": "Pralaya Medang (Serangan Wurawari)." },
  { "en": "Kitab sastra yang berisi nasihat perkawinan dari masa Kediri?", "id": "Kakawin Smaradahana." },
  { "en": "Tokoh Panji dalam cerita Panji berasal dari kerajaan mana?", "id": "Janggala atau Kediri." },
  { "en": "Siapa Tunggul Ametung yang dibunuh Ken Arok?", "id": "Akuwu Tumapel." },
  { "en": "Siapa Anusapati, putra Ken Dedes dengan Tunggul Ametung?", "id": "Raja kedua Singasari." },
  { "en": "Siapa Tohjaya yang merebut tahta Singasari dari Anusapati?", "id": "Putra Ken Arok (selir)." },
  { "en": "Candi Jawi di Pasuruan merupakan pendharmaan raja Singasari siapa?", "id": "Kertanegara." },
  { "en": "Siapa Jayakatwang yang memberontak dan meruntuhkan Singasari?", "id": "Bupati Gelang-gelang (Kediri)." },
  { "en": "Pasukan Mongol yang datang ke Jawa bertujuan menghukum siapa?", "id": "Kertanegara." },
  { "en": "Nama lain Raden Wijaya saat muda?", "id": "Nararya Sanggramawijaya." },
  { "en": "Siapa Lembu Sora yang membantu Raden Wijaya?", "id": "Pengikut setia." },
  { "en": "Kitab Pararaton juga dikenal dengan nama apa?", "id": "Katuturanira Ken Angrok." },
  { "en": "Siapa Ra Kuti yang memberontak pada masa Jayanegara di Majapahit?", "id": "Dharmaputra Majapahit." },
  { "en": "Siapa yang menyelamatkan Jayanegara saat pemberontakan Ra Kuti?", "id": "Gajah Mada (Bhayangkara)." },
  { "en": "Perang Sadeng dan Keta terjadi pada masa pemerintahan siapa di Majapahit?", "id": "Tribhuwana Wijayatunggadewi." },
  { "en": "Nama pelabuhan penting Majapahit di muara Sungai Brantas?", "id": "Canggu." },
  { "en": "Istilah \"Mitreka Satata\" dalam politik Majapahit artinya apa?", "id": "Persahabatan setara (dengan negara lain)." },
  { "en": "Siapa Wikramawardhana yang terlibat Perang Paregreg?", "id": "Suami Kusumawardhani (putri Hayam Wuruk)." },
  { "en": "Siapa Syekh Ibrahim Asmarakandi, ayah Sunan Ampel?", "id": "Ulama asal Samarkand." },
  { "en": "Makam Fatimah binti Maimun di Gresik berasal dari abad berapa?", "id": "Abad ke-11." },
  { "en": "Siapa yang dianggap sebagai sultan pertama Kesultanan Cirebon?", "id": "Sunan Gunung Jati." },
  { "en": "Masjid Menara Kudus memadukan arsitektur Islam dengan apa?", "id": "Hindu-Buddha (pra-Islam)." },
  { "en": "Siapa Ki Ageng Gribig yang menyebarkan Islam di Jatinom, Klaten?", "id": "Ulama keturunan Majapahit." },
  { "en": "Tradisi \"Yaqowiyu\" di Jatinom terkait dengan tokoh siapa?", "id": "Ki Ageng Gribig." },
  { "en": "Siapa penulis Babad Tanah Jawi yang terkenal?", "id": "Carik Braja (versi awal, banyak versi)." },
  { "en": "Perjanjian Jepara (1677) antara Amangkurat II dan VOC berisi apa?", "id": "VOC bantu Amangkurat II, dapat wilayah." },
  { "en": "Siapa Arung Palakka dari Bone yang bersekutu dengan VOC?", "id": "Pangeran Bone." },
  { "en": "Komoditas utama yang dicari Portugis di Timor?", "id": "Kayu Cendana." },
  { "en": "Perusahaan dagang Inggris saingan VOC di Asia?", "id": "EIC (East India Company)." },
  { "en": "Pembantaian Amboyna (Ambon Massacre) tahun 1623 melibatkan VOC dengan siapa?", "id": "Pedagang Inggris (EIC)." },
  { "en": "Siapa Cornelis Speelman yang berperan dalam penaklukan Gowa?", "id": "Gubernur Jenderal VOC." },
  { "en": "\"Bataviase Nouvelles\" adalah surat kabar pertama di Hindia Belanda, tahun berapa?", "id": "Tahun 1744." },
  { "en": "Siapa Dirk van Hogendorp yang mengkritik VOC?", "id": "Residen, pengkritik feodalisme VOC." },
  { "en": "Undang-undang Agraria (Agrarische Wet) 1870 dikeluarkan oleh Menteri siapa?", "id": "Engelbertus de Waal." },
  { "en": "Apa tujuan utama UU Agraria 1870 bagi pengusaha swasta?", "id": "Memudahkan sewa tanah." },
  { "en": "Koelie Ordonnantie mengatur tentang apa?", "id": "Perburuhan kontrak (kuli)." },
  { "en": "Di mana Koelie Ordonnantie banyak diterapkan?", "id": "Perkebunan Sumatra Timur." },
  { "en": "Siapa Van Heutsz yang dikenal sebagai \"penakluk\" Aceh?", "id": "Gubernur Jenderal Hindia Belanda." },
  { "en": "\"Pax Neerlandica\" adalah istilah untuk apa?", "id": "Klaim perdamaian di bawah Belanda." },
  { "en": "Sekolah dokter Jawa selain STOVIA di Surabaya?", "id": "NIAS (Nederlandsch Indische Artsen School)." },
  { "en": "Siapa tokoh Tionghoa yang mendirikan Tiong Hoa Hwee Koan (THHK)?", "id": "Phoa Keng Hek, Lie Kim Hok." },
  { "en": "Apa tujuan awal pendirian THHK?", "id": "Reformasi adat, pendidikan Tionghoa." },
  { "en": "Siapa Ernest Douwes Dekker yang mengkritik Politik Etis?", "id": "Indo-Eropa, pendiri Indische Partij." },
  { "en": "\"Max Havelaar\" pertama kali diterbitkan dalam bahasa apa?", "id": "Belanda." },
  { "en": "Perlawanan rakyat Batak melawan Belanda sebelum Sisingamangaraja XII?", "id": "Perang Padri (keterlibatan Batak)." },
  { "en": "Siapa tokoh wanita dari Tapanuli yang membantu perjuangan Sisingamangaraja XII?", "id": "Ibu Sisingamangaraja XII (boru Sagala)." },
  { "en": "Perang Bayu di Blambangan tahun 1771 melawan siapa?", "id": "VOC." },
  { "en": "Siapa pemimpin Perang Bayu?", "id": "Mas Rempeg (Pangeran Jagapati)." },
  { "en": "Sekolah OSVIA melahirkan golongan apa dalam masyarakat?", "id": "Pamong praja (priyayi baru)." },
  { "en": "Siapa Marco Kartodikromo, jurnalis dan penulis yang kritis?", "id": "Tokoh pergerakan awal." },
  { "en": "Surat kabar \"Sinar Djawa\" terkait dengan organisasi apa?", "id": "Sarekat Islam Semarang (kiri)." },
  { "en": "Apa itu \"Volkslectuur\" yang didirikan pemerintah kolonial?", "id": "Balai Pustaka (Taman Bacaan Rakyat)." },
  { "en": "Tujuan awal Balai Pustaka?", "id": "Menyediakan bacaan \"aman\" bagi pribumi." },
  { "en": "Siapa Noto Soeroto, penyair Jawa yang menulis dalam bahasa Belanda?", "id": "Tokoh pergerakan budaya." },
  { "en": "\"Comité van Aanbeveling voor de Opleiding van Inlandsche Meisjes\" dipimpin siapa?", "id": "R.A. Kartini (komite pendukung)." },
  { "en": "Perlawanan di Jambi terhadap Belanda dipimpin oleh siapa?", "id": "Sultan Thaha Syaifuddin." },
  { "en": "Apa nama benteng Belanda di Muara Kumpeh, Jambi?", "id": "Benteng Muara Kumpeh." },
  { "en": "Siapa Asisten Residen Belanda yang mengawasi Pangeran Diponegoro di Manado?", "id": "Kapten J.A. Victor de Stuers." },
  { "en": "Perkebunan tembakau Deli di Sumatra Timur menggunakan tenaga kerja dari mana?", "id": "Jawa, Tiongkok, India." },
  { "en": "Di kota mana Budi Utomo pertama kali mengadakan kongres?", "id": "Yogyakarta." },
  { "en": "Siapa ketua pertama Budi Utomo?", "id": "Dr. Soetomo (namun lebih sebagai motor)." },
  { "en": "Tujuan awal Sarekat Islam Semarang yang dipimpin Semaun?", "id": "Memperjuangkan nasib buruh." },
  { "en": "Organisasi buruh kereta api yang dipengaruhi ISDV/PKI?", "id": "VSTP (Vereniging van Spoor-en Tramwegpersoneel)." },
  { "en": "Siapa Alimin Prawirodirdjo?", "id": "Tokoh komunis, terlibat pemberontakan 1926." },
  { "en": "Studieclub umum yang didirikan Soekarno di Bandung?", "id": "Algemeene Studieclub." },
  { "en": "Siapa yang membacakan teks Sumpah Pemuda saat Kongres Pemuda II?", "id": "Tidak ada pembacaan formal (rumusan)." },
  { "en": "Selain \"Indonesia Raya\", lagu apa lagi yang populer di kalangan pergerakan?", "id": "\"Bangun Pemudi Pemuda\"." },
  { "en": "Siapa pencipta lagu \"Bangun Pemudi Pemuda\"?", "id": "Alfred Simanjuntak." },
  { "en": "Pembelaan Hatta di pengadilan Belanda berjudul apa?", "id": "\"Indonesia Vrij\" (Indonesia Merdeka)." },
  { "en": "Siapa tokoh wanita yang ikut mendirikan PNI Pendidikan?", "id": "Tidak ada yang menonjol sebagai pendiri utama." },
  { "en": "Majalah yang diterbitkan Perhimpunan Indonesia di Belanda?", "id": "\"Indonesia Merdeka\" (Hindia Poetra)." },
  { "en": "Siapa ketua Perhimpunan Indonesia yang terkenal sebelum Hatta?", "id": "Nazir Pamuntjak, Ahmad Subardjo." },
  { "en": "Apa nama gerakan bawah tanah Sutan Sjahrir pada masa Jepang?", "id": "Kelompok Sjahrir." },
  { "en": "Siapa tokoh pers nasional yang mendirikan kantor berita ANTARA?", "id": "Adam Malik, Soemanang, A.M. Sipahoetar, Pandoe Kartawigoena." },
  { "en": "Kapan ANTARA didirikan?", "id": "13 Desember 1937." },
  { "en": "Fraksi Nasional dalam Volksraad dipimpin oleh siapa?", "id": "Mohammad Husni Thamrin." },
  { "en": "Apa tuntutan utama Fraksi Nasional di Volksraad?", "id": "Perubahan ketatanegaraan, Indonesia berparlemen." },
  { "en": "Siapa Abikusno Tjokrosujoso, tokoh Sarekat Islam dan BPUPKI?", "id": "Adik H.O.S. Cokroaminoto." },
  { "en": "Panitia Persiapan Perniagaan dan Perekonomian Indonesia (P4I) dibentuk untuk apa?", "id": "Persiapan ekonomi pasca-kemerdekaan." },
  { "en": "Siapa Mr. A.A. Maramis, anggota BPUPKI dari Kristen Minahasa?", "id": "Menteri Keuangan pertama RI." },
  { "en": "Badan apa yang dibentuk Jepang untuk mengawasi aktivitas keagamaan Islam?", "id": "Shumubu." },
  { "en": "Siapa K.H. Wahid Hasyim, tokoh NU di BPUPKI dan PPKI?", "id": "Ayah Gus Dur." },
  { "en": "Laskar Hizbullah dan Sabilillah adalah bentukan siapa?", "id": "Masyumi (era Jepang dan revolusi)." },
  { "en": "Siapa Chaerul Saleh, tokoh pemuda radikal masa proklamasi?", "id": "Pemimpin Menteng 31." },
  { "en": "Tonarigumi adalah sistem RT/RW versi Jepang, untuk apa?", "id": "Pengawasan dan mobilisasi penduduk." },
  { "en": "\"Kinrohoshi\" adalah kerja bakti sukarela versi Jepang, seringkali untuk apa?", "id": "Kepentingan perang Jepang." },
  { "en": "Siapa sastrawan yang terpaksa menulis untuk propaganda Jepang?", "id": "Usmar Ismail, Armijn Pane." },
  { "en": "\"Keimin Bunka Shidosho\" adalah lembaga kebudayaan bentukan Jepang untuk apa?", "id": "Mengontrol dan mengarahkan seniman." },
  { "en": "Siapa Iwa Kusumasumantri, tokoh nasionalis yang bekerjasama dengan Jepang?", "id": "Anggota PPKI, Menteri Sosial pertama." },
  { "en": "Peristiwa Cumbok di Aceh adalah perlawanan ulama terhadap siapa?", "id": "Uleebalang yang dianggap pro-Belanda/Jepang." },
  { "en": "Kapan pertempuran pertama antara pejuang Indonesia dengan Jepang pasca-proklamasi?", "id": "Beragam di banyak daerah (awal September 1945)." },
  { "en": "Siapa Komandan Kempetai (polisi militer Jepang) yang terkenal kejam?", "id": "Bervariasi di tiap daerah." },
  { "en": "Pertemuan antara Soekarno-Hatta dengan Jenderal Nishimura membahas apa?", "id": "Keinginan proklamasi, sikap Jepang." },
  { "en": "Di mana lokasi pusat komando pasukan Sekutu (AFNEI) di Jakarta?", "id": "Hotel des Indes (sekarang bagian dari Pertamina)." },
  { "en": "Insiden Bendera di Hotel Yamato, Surabaya, terjadi tanggal berapa?", "id": "19 September 1945." },
  { "en": "Siapa Ploegman yang menaikkan bendera Belanda di Hotel Yamato?", "id": "Pimpinan tentara Belanda/NICA." },
  { "en": "Pertempuran Lima Hari di Semarang melibatkan pemuda melawan siapa?", "id": "Tentara Jepang (Kidobutai)." },
  { "en": "Siapa dr. Kariadi yang gugur dalam Pertempuran Lima Hari Semarang?", "id": "Kepala Laboratorium Rumah Sakit." },
  { "en": "Maklumat Pemerintah 14 November 1945 mengubah sistem kabinet menjadi?", "id": "Kabinet parlementer." },
  { "en": "Siapa Perdana Menteri pertama RI setelah sistem parlementer?", "id": "Sutan Sjahrir." },
  { "en": "Laskar Rakyat muncul sebagai bentuk perlawanan bersenjata, contohnya?", "id": "Pesindo, Barisan Banteng." },
  { "en": "Agresi Militer Belanda I juga dikenal dengan sebutan Belanda apa?", "id": "Eerste Politionele Actie." },
  { "en": "Siapa Jenderal Spoor, Panglima Tertinggi Tentara Belanda di Hindia Belanda?", "id": "Simon Hendrik Spoor." },
  { "en": "Resolusi Dewan Keamanan PBB No. 27 (1 Agustus 1947) berisi apa?", "id": "Seruan gencatan senjata (Agresi I)." },
  { "en": "Siapa Frank Graham, wakil Amerika Serikat dalam Komisi Tiga Negara?", "id": "Anggota KTN." },
  { "en": "Garis demarkasi Renville memisahkan wilayah RI dengan wilayah apa?", "id": "Daerah pendudukan Belanda." },
  { "en": "Siapa Mr. Assaat yang menjadi Pemangku Jabatan Presiden RI saat PDRI?", "id": "Ketua KNIP (Presiden Sementara RI di Yogyakarta)." },
  { "en": "Konferensi Inter-Indonesia menghasilkan kesepakatan apa?", "id": "Pembentukan RIS, UUD RIS." },
  { "en": "Siapa Anak Agung Gde Agung, tokoh BFO dari Negara Indonesia Timur?", "id": "Perdana Menteri NIT." },
  { "en": "Uni Indonesia-Belanda diketuai oleh siapa secara formal?", "id": "Ratu Belanda." },
  { "en": "Penyerahan kedaulatan di Belanda dilakukan antara siapa dan siapa?", "id": "Mohammad Hatta dan Ratu Juliana." },
  { "en": "Siapa L.N. Palar yang gigih memperjuangkan Indonesia di PBB?", "id": "Diplomat Indonesia." },
  { "en": "Kabinet Sukiman dikenal karena menandatangani perjanjian apa dengan AS?", "id": "Mutual Security Act (MSA)." },
  { "en": "Mengapa MSA dianggap merugikan Indonesia?", "id": "Melanggar politik bebas aktif." },
  { "en": "Siapa Menteri Luar Negeri Kabinet Sukiman yang terkait MSA?", "id": "Achmad Soebardjo." },
  { "en": "Kabinet Wilopo jatuh karena peristiwa apa?", "id": "Peristiwa Tanjung Morawa (konflik tanah)." },
  { "en": "Siapa Menteri Pertahanan pada masa Kabinet Ali Sastroamidjojo I?", "id": "Iwa Kusumasumantri." },
  { "en": "Badan Perancang Negara (BPN) pada masa Demokrasi Parlementer diketuai siapa?", "id": "Djuanda Kartawidjaja." },
  { "en": "Deklarasi Djuanda 13 Desember 1957 berisi tentang apa?", "id": "Penentuan wilayah laut teritorial Indonesia." },
  { "en": "Berapa lebar laut teritorial Indonesia menurut Deklarasi Djuanda?", "id": "12 mil laut." },
  { "en": "Nasionalisasi perusahaan-perusahaan Belanda dilakukan pada tahun berapa?", "id": "Tahun 1957-1958." },
  { "en": "Apa alasan utama nasionalisasi perusahaan Belanda?", "id": "Sengketa Irian Barat." },
  { "en": "Siapa Jenderal A.H. Nasution yang menjadi KSAD dan Menko Hankam?", "id": "Tokoh militer berpengaruh." },
  { "en": "Konsep \"Jalan Tengah\" A.H. Nasution terkait peran apa?", "id": "Peran militer dalam politik." },
  { "en": "Piagam Perjuangan Semesta (Permesta) dideklarasikan di kota mana?", "id": "Makassar." },
  { "en": "Siapa Soemitro Djojohadikoesoemo yang terlibat PRRI?", "id": "Ahli ekonomi, Menteri Keuangan PRRI." },
  { "en": "Operasi militer penumpasan PRRI di Sumatra Barat disebut apa?", "id": "Operasi 17 Agustus." },
  { "en": "Operasi militer penumpasan Permesta di Sulawesi Utara disebut apa?", "id": "Operasi Merdeka." },
  { "en": "Apa isi pokok UU Keadaan Bahaya (SOB) yang sering digunakan?", "id": "Memberi wewenang luas pada militer." },
  { "en": "Pembentukan Dewan Perancang Nasional (Depernas) bertujuan untuk apa?", "id": "Merencanakan pembangunan semesta." },
  { "en": "Depernas kemudian diganti menjadi badan apa?", "id": "Bappenas (Badan Perencanaan Pembangunan Nasional)." },
  { "en": "Siapa Chaerul Saleh, tokoh Murba yang menjadi Wakil PM era Terpimpin?", "id": "Tokoh kiri non-PKI." },
  { "en": "Politik \"Berdikari\" Soekarno artinya apa?", "id": "Berdiri di atas kaki sendiri." },
  { "en": "Lembaga internasional apa yang ingin ditandingi Soekarno dengan CONEFO?", "id": "Perserikatan Bangsa-Bangsa (PBB)." },
  { "en": "Apa kepanjangan CONEFO?", "id": "Conference of the New Emerging Forces." },
  { "en": "Siapa Njoto, Wakil Ketua II CC PKI yang juga Menteri Negara?", "id": "Intelektual PKI." },
  { "en": "\"Dokumen Gilchrist\" yang memicu isu Dewan Jenderal berasal dari kedutaan mana?", "id": "Kedutaan Besar Inggris." },
  { "en": "Siapa Brigjen Soepardjo, tokoh militer yang terlibat langsung G30S?", "id": "Komandan pasukan di sekitar Istana." },
  { "en": "Mahkamah Militer Luar Biasa (Mahmilub) dibentuk untuk mengadili siapa?", "id": "Tokoh-tokoh G30S/PKI." },
  { "en": "Siapa Oei Tjoe Tat, Menteri Negara era Soekarno yang dituduh terlibat G30S?", "id": "Tokoh Tionghoa di kabinet." },
  { "en": "Kesatuan Aksi Mahasiswa Indonesia (KAMI) diketuai oleh siapa?", "id": "Cosmas Batubara (salah satu presidium)." },
  { "en": "Tap MPRS No. XXXIII/MPRS/1967 berisi tentang apa?", "id": "Pencabutan jabatan Presiden dari Soekarno." },
  { "en": "\"Surat Perintah Sebelas Maret\" secara de facto mengakhiri kekuasaan siapa?", "id": "Presiden Soekarno." },
  { "en": "Siapa Adam Malik yang menjadi Menteri Luar Negeri awal Orde Baru?", "id": "Tokoh senior, diplomat." },
  { "en": "Siapa Sri Sultan Hamengkubuwono IX yang menjadi Wakil Presiden Soeharto?", "id": "Tokoh penting transisi kekuasaan." },
  { "en": "Konsep \"Pembangunan Manusia Indonesia Seutuhnya\" dicanangkan pada Repelita ke berapa?", "id": "Repelita II." },
  { "en": "Badan Koordinasi Intelijen Negara (BAKIN) dipimpin oleh siapa pada masa Orde Baru?", "id": "Yoga Soegama (lama menjabat)." },
  { "en": "Operasi Tertib (Opstib) dan Pemberantasan Pungli (Lakhar Pungli) bertujuan untuk apa?", "id": "Memberantas korupsi (awal Orba)." },
  { "en": "Apa itu \"Bedol Desa\" terkait proyek pembangunan Orde Baru?", "id": "Pemindahan paksa seluruh desa." },
  { "en": "Waduk Kedung Ombo yang kontroversial karena Bedol Desa terletak di mana?", "id": "Jawa Tengah." },
  { "en": "Siapa Romo Mangunwijaya yang membela korban Kedung Ombo?", "id": "Rohaniwan, arsitek, aktivis." },
  { "en": "Program \"Safari Ramadhan\" Soeharto bertujuan untuk apa?", "id": "Pendekatan kepada umat Islam." },
  { "en": "Ikatan Cendekiawan Muslim Indonesia (ICMI) dibentuk tahun berapa?", "id": "Tahun 1990." },
  { "en": "Siapa ketua pertama ICMI?", "id": "B.J. Habibie." },
  { "en": "Asas \"Kekeluargaan\" dalam Koperasi sering ditekankan Orde Baru, maksudnya?", "id": "Harmoni, menghindari konflik kelas." },
  { "en": "Siapa Mochtar Riady, pengusaha yang sukses di era Orde Baru?", "id": "Pendiri Lippo Group." },
  { "en": "\"Petrus\" (Penembakan Misterius) tahun 1980-an menyasar siapa?", "id": "Preman dan residivis." },
  { "en": "Siapa yang dianggap bertanggung jawab atas kebijakan Petrus?", "id": "Aparat keamanan (L.B. Moerdani)." },
  { "en": "Insiden Talangsari di Lampung tahun 1989 melibatkan kelompok apa?", "id": "Kelompok Islam Warsidi." },
  { "en": "Siapa Jenderal Try Sutrisno, Panglima ABRI yang kemudian menjadi Wapres?", "id": "Tokoh militer." },
  { "en": "UU Subversi sering digunakan untuk menindak siapa?", "id": "Aktivis politik, pengkritik pemerintah." },
  { "en": "Siapa Nurcholish Madjid (Cak Nur), cendekiawan Muslim pembaharu?", "id": "Penggagas \"Islam Yes, Partai Islam No\"." },
  { "en": "Lembaga Swadaya Masyarakat (LSM) apa yang kritis terhadap Orde Baru?", "id": "YLBHI, Walhi." },
  { "en": "Siapa Adnan Buyung Nasution, pendiri YLBHI?", "id": "Pengacara, aktivis HAM." },
  { "en": "Siapa sastrawan yang karyanya dilarang beredar oleh Orde Baru?", "id": "Pramoedya Ananta Toer." },
  { "en": "\"Pancasila sebagai satu-satunya asas\" dihapuskan pada era siapa?", "id": "Abdurrahman Wahid (Reformasi)." },
  { "en": "Siapa Harmoko, Menteri Penerangan yang lama menjabat era Orde Baru?", "id": "Tokoh Golkar." },
  { "en": "Pidato \"Saya akan gebuk\" terkait dengan siapa dan peristiwa apa?", "id": "Soeharto (merespons kritik)." },
  { "en": "Komisi Penyelidik Pelanggaran HAM (KPP HAM) Trisakti, Semanggi I & II dibentuk untuk apa?", "id": "Menyelidiki kasus penembakan mahasiswa." },
  { "en": "Siapa Jenderal Wiranto, Panglima ABRI saat transisi ke Reformasi?", "id": "Tokoh militer kunci." },
  { "en": "Bank Bali Skandal mencuat melibatkan siapa dengan tim sukses Habibie?", "id": "Setya Novanto (disebut-sebut)." },
  { "en": "Otonomi Khusus Papua diberikan melalui UU Nomor berapa?", "id": "UU No. 21 Tahun 2001." },
  { "en": "Apa nama lain dari Otonomi Khusus Papua?", "id": "Otsus Papua." },
  { "en": "Siapa Theys Hiyo Eluay, tokoh Papua yang terbunuh?", "id": "Ketua Presidium Dewan Papua." },
  { "en": "Siapa Akbar Tandjung, Ketua DPR era Megawati dan Ketua Golkar?", "id": "Tokoh politik senior." },
  { "en": "Gerakan Aceh Merdeka (GAM) dipimpin oleh siapa saat Perjanjian Helsinki?", "id": "Malik Mahmud (PM GAM)." },
  { "en": "Siapa Hasan di Tiro, pendiri GAM?", "id": "Tokoh diaspora Aceh." },
  { "en": "Badan Rehabilitasi dan Rekonstruksi (BRR) Aceh-Nias diketuai oleh siapa?", "id": "Kuntoro Mangkusubroto." },
  { "en": "Pemilihan Kepala Daerah (Pilkada) secara langsung pertama kali tahun berapa?", "id": "Tahun 2005." },
  { "en": "Densus 88 Antiteror dibentuk setelah peristiwa apa?", "id": "Bom Bali I (2002)." },
  { "en": "Siapa Dr. Azahari dan Noordin M. Top?", "id": "Teroris buronan asal Malaysia." },
  { "en": "Satuan Tugas Pemberantasan Mafia Hukum dibentuk oleh Presiden siapa?", "id": "Susilo Bambang Yudhoyono." },
  { "en": "Siapa Sri Mulyani Indrawati, Menteri Keuangan yang terkenal?", "id": "Ekonom, teknokrat." },
  { "en": "Program Masterplan Percepatan dan Perluasan Pembangunan Ekonomi Indonesia (MP3EI) dicanangkan siapa?", "id": "Presiden SBY." },
  { "en": "Revolusi Mental adalah slogan kampanye Presiden siapa?", "id": "Joko Widodo." },
  { "en": "Program \"Tol Laut\" bertujuan untuk apa?", "id": "Memperlancar konektivitas maritim." },
  { "en": "Siapa Basuki Tjahaja Purnama (Ahok), Gubernur DKI Jakarta yang kontroversial?", "id": "Politisi." },
  { "en": "Gerakan 212 adalah aksi protes terkait dugaan apa?", "id": "Penistaan agama." },
  { "en": "Prasasti Tarumanegara yang berisi dua telapak kaki gajah?", "id": "Prasasti Kebon Kopi." },
  { "en": "Sungai apa yang penting bagi Kerajaan Tarumanegara?", "id": "Sungai Citarum." },
  { "en": "Kerajaan mana yang dianggap sebagai pendahulu Sriwijaya di Sumatra Selatan?", "id": "Kerajaan Melayu (Dharmasraya)." },
  { "en": "Prasasti Sriwijaya yang menyebutkan usaha membuat taman?", "id": "Prasasti Talang Tuo." },
  { "en": "Selain Palembang, pelabuhan Sriwijaya penting lainnya di Jambi?", "id": "Muaro Jambi." },
  { "en": "Siapa pendeta Buddha terkenal dari India yang mengunjungi Sriwijaya?", "id": "Atisha." },
  { "en": "Dinasti apa di Jawa yang bersaing dengan Sriwijaya?", "id": "Syailendra dan Sanjaya." },
  { "en": "Candi Dieng dibangun oleh dinasti apa?", "id": "Sanjaya." },
  { "en": "Apa nama candi utama di kompleks Candi Dieng?", "id": "Candi Arjuna." },
  { "en": "Gelar raja-raja Mataram Kuno (Syailendra) yang berarti \"raja gunung\"?", "id": "Sailaraja." },
  { "en": "Siapa raja Mataram Kuno yang memulai pembangunan Borobudur?", "id": "Samaratungga." },
  { "en": "Relief apa yang menceritakan kehidupan Sang Buddha di Borobudur?", "id": "Lalitavistara." },
  { "en": "Relief cerita hewan di Borobudur disebut apa?", "id": "Jataka atau Avadana." },
  { "en": "Permaisuri Samaratungga yang juga berpengaruh?", "id": "Dewi Tara." },
  { "en": "Kitab hukum yang digunakan pada masa Majapahit?", "id": "Kutaramanawa." },
  { "en": "Siapa Adityawarman yang berkuasa di Malayapura (Sumatra)?", "id": "Kerabat Majapahit." },
  { "en": "Prasasti Adityawarman yang terkenal di Sumatra Barat?", "id": "Prasasti Kuburajo." },
  { "en": "Pelabuhan utama Majapahit di pantai utara Jawa?", "id": "Tuban, Gresik, Surabaya." },
  { "en": "Upacara keagamaan penting Majapahit yang disebut dalam Nagarakretagama?", "id": "Upacara Sraddha." },
  { "en": "Siapa Bhre Wirabhumi yang terlibat Perang Paregreg?", "id": "Putra Hayam Wuruk." },
  { "en": "Setelah runtuhnya Majapahit, kerajaan Hindu apa yang bertahan di Jawa Timur?", "id": "Blambangan." },
  { "en": "Tokoh Walisongo yang dianggap berasal dari Campa?", "id": "Sunan Ampel (Raden Rahmat)." },
  { "en": "Siapa putra Sunan Ampel yang juga Walisongo?", "id": "Sunan Bonang, Sunan Drajat." },
  { "en": "Siapa Sunan Giri yang mendirikan pesantren Giri Kedaton?", "id": "Raden Paku." },
  { "en": "Wilayah dakwah Sunan Gunung Jati?", "id": "Jawa Barat (Cirebon, Banten)." },
  { "en": "Siapa nama asli Sunan Gunung Jati?", "id": "Syarif Hidayatullah." },
  { "en": "Fatahillah adalah panglima Demak yang mengusir Portugis dari mana?", "id": "Sunda Kelapa." },
  { "en": "Sunda Kelapa kemudian diubah namanya menjadi apa oleh Fatahillah?", "id": "Jayakarta." },
  { "en": "Siapa sultan Demak terakhir sebelum pindah ke Pajang?", "id": "Arya Penangsang." },
  { "en": "Pusat pemerintahan Mataram Islam setelah Kotagede?", "id": "Karta, Plered." },
  { "en": "Siapa Amangkurat I yang terkenal kejam?", "id": "Putra Sultan Agung." },
  { "en": "Pemberontakan besar terhadap Amangkurat I dipimpin oleh siapa?", "id": "Trunojoyo." },
  { "en": "Siapa Pangeran Puger yang kemudian bergelar Pakubuwana I?", "id": "Adik Amangkurat II." },
  { "en": "Perjanjian apa antara VOC dan Mataram yang sangat merugikan Mataram?", "id": "Banyak, misal Perjanjian 1677." },
  { "en": "Komoditas utama Kesultanan Banten selain lada?", "id": "Cengkih (transit)." },
  { "en": "Siapa Sultan Haji yang merebut tahta dari Sultan Ageng Tirtayasa?", "id": "Putranya (dibantu VOC)." },
  { "en": "Perjanjian Bongaya ditandatangani antara VOC dengan kerajaan mana?", "id": "Gowa." },
  { "en": "Apa julukan Sultan Hasanuddin karena keberaniannya?", "id": "Ayam Jantan dari Timur." },
  { "en": "Persaingan utama Ternate dan Tidore dalam perdagangan apa?", "id": "Rempah-rempah (cengkih, pala)." },
  { "en": "Benteng Oranje dibangun VOC di pulau mana?", "id": "Ternate." },
  { "en": "Siapa penjelajah Portugis pertama yang mencapai Kepulauan Banda?", "id": "António de Abreu." },
  { "en": "Apa sebutan untuk hak VOC mencetak uang sendiri?", "id": "Hak Oktroi." },
  { "en": "Sistem pemerintahan tidak langsung VOC melalui siapa?", "id": "Penguasa lokal (raja/bupati)." },
  { "en": "Siapa Gubernur Jenderal VOC yang dikenal korup dan kaya raya?", "id": "Banyak, misal Willem van Outhoorn." },
  { "en": "Wabah penyakit apa yang sering melanda Batavia pada masa VOC?", "id": "Malaria, disentri." },
  { "en": "Apa sebutan untuk tentara sewaan VOC dari berbagai bangsa?", "id": "Serdadu Kompeni." },
  { "en": "Perlawanan Pangeran Nuku dari Tidore disebut perang apa?", "id": "Perang Nuku (Perang Kei)." },
  { "en": "Siapa tokoh Tionghoa yang memimpin pemberontakan di Batavia (1740)?", "id": "Souw Pan Chiang (Oei Pandjang)." },
  { "en": "Pembantaian orang Tionghoa di Batavia tahun 1740 disebut?", "id": "Geger Pacinan." },
  { "en": "Siapa yang menggantikan Daendels sebagai Gubernur Jenderal?", "id": "Jan Willem Janssens." },
  { "en": "Di mana Janssens menyerah kepada Inggris?", "id": "Tuntang." },
  { "en": "Kebun Raya Bogor didirikan pada masa siapa?", "id": "Thomas Stamford Raffles." },
  { "en": "Nama awal Kebun Raya Bogor?", "id": "'s Lands Plantentuin te Buitenzorg." },
  { "en": "Siapa yang membantu Raffles dalam penelitian di Jawa?", "id": "John Crawfurd, Colin Mackenzie." },
  { "en": "Buku Raffles \"History of Java\" pertama terbit tahun berapa?", "id": "Tahun 1817." },
  { "en": "Apa nama lain Cultuurstelsel yang lebih netral?", "id": "Sistem Penanaman Wajib." },
  { "en": "Komoditas utama yang diwajibkan dalam Cultuurstelsel?", "id": "Kopi, tebu, nila (indigo)." },
  { "en": "Apa \"cultuurprocenten\" itu?", "id": "Bonus bagi pejabat pelaksana Tanam Paksa." },
  { "en": "Dampak negatif Tanam Paksa bagi rakyat?", "id": "Kelaparan, kemiskinan." },
  { "en": "Siapa bupati Lebak yang dikritik Multatuli dalam Max Havelaar?", "id": "Raden Adipati Karta Natanagara." },
  { "en": "Siapa nama tokoh utama dalam novel Max Havelaar?", "id": "Max Havelaar (Saidjah, Adinda)." },
  { "en": "Apa taktik perang yang digunakan Pangeran Diponegoro?", "id": "Perang gerilya." },
  { "en": "Siapa Sentot Alibasyah Prawirodirdjo?", "id": "Panglima perang Diponegoro." },
  { "en": "Di mana Pangeran Diponegoro ditangkap Belanda?", "id": "Magelang." },
  { "en": "Ke mana Pangeran Diponegoro diasingkan hingga wafat?", "id": "Makassar." },
  { "en": "Kaum Padri di Sumatra Barat ingin memurnikan ajaran apa?", "id": "Islam." },
  { "en": "Siapa pemimpin Kaum Adat dalam Perang Padri?", "id": "Yang Dipertuan Pagaruyung." },
  { "en": "Benteng Belanda yang terkenal dalam Perang Padri?", "id": "Fort de Kock (Bukittinggi)." },
  { "en": "Apa nama lain Perang Aceh?", "id": "Perang Sabil." },
  { "en": "Jenderal Belanda yang tewas dalam Perang Aceh?", "id": "Jenderal Köhler." },
  { "en": "Taktik Belanda untuk memenangkan Perang Aceh?", "id": "Konsentrasi Stelsel, Korps Marechaussee." },
  { "en": "Siapa Snouck Hurgronje?", "id": "Penasihat Belanda urusan Islam Aceh." },
  { "en": "Apa isi Plakat Pendek dalam Perang Aceh?", "id": "Pernyataan tunduk raja-raja Aceh." },
  { "en": "Perang Puputan di Klungkung, Bali, tahun berapa?", "id": "Tahun 1908." },
  { "en": "Siapa I Gusti Ketut Jelantik yang memimpin perlawanan di Bali Utara?", "id": "Patih Kerajaan Buleleng." },
  { "en": "Sekolah untuk anak-anak Eropa dan elit pribumi?", "id": "ELS (Europeesche Lagere School)." },
  { "en": "Sekolah lanjutan setelah HIS untuk pribumi?", "id": "MULO (Meer Uitgebreid Lager Onderwijs)." },
  { "en": "Sekolah menengah atas umum pada masa kolonial?", "id": "AMS (Algemeene Middelbare School)." },
  { "en": "Tokoh pers wanita selain Rohana Kudus?", "id": "Siti Soendari." },
  { "en": "Surat kabar wanita pertama yang dipimpin Rohana Kudus?", "id": "Soenting Melajoe." },
  { "en": "Tokoh Serikat Islam yang awalnya guru Cokroaminoto?", "id": "Tirto Adhi Soerjo." },
  { "en": "Apa nama awal Sarekat Islam sebelum berubah?", "id": "Sarekat Dagang Islam." },
  { "en": "Perpecahan dalam Sarekat Islam menjadi SI Merah dan SI Putih karena apa?", "id": "Pengaruh komunisme." },
  { "en": "Siapa tokoh SI Merah yang kemudian memimpin PKI?", "id": "Semaun, Darsono, Alimin." },
  { "en": "Organisasi pemuda Batak bernama apa?", "id": "Jong Bataks Bond." },
  { "en": "Siapa yang mengusulkan nama \"Indonesia\" dalam Kongres Pemuda II?", "id": "Muhammad Yamin (dalam pidato)." },
  { "en": "Lagu \"Indonesia Raya\" pertama kali dimainkan dengan alat musik apa?", "id": "Biola (oleh W.R. Supratman)." },
  { "en": "Siapa ketua Kongres Pemuda II?", "id": "Sugondo Djojopuspito." },
  { "en": "Siapa sekretaris Kongres Pemuda II?", "id": "Muhammad Yamin." },
  { "en": "Selain PNI, partai nasionalis radikal lain yang didirikan Soekarno?", "id": "Partindo (setelah PNI dibubarkan)." },
  { "en": "Pendidikan Nasional Indonesia (PNI Baru) dipimpin oleh siapa?", "id": "Mohammad Hatta, Sutan Sjahrir." },
  { "en": "Apa fokus PNI Baru?", "id": "Pendidikan kader." },
  { "en": "Siapa yang menggantikan Soekarno sebagai ketua PNI saat Soekarno dipenjara?", "id": "Sartono." },
  { "en": "Di mana Soekarno diasingkan setelah \"Indonesia Menggugat\"?", "id": "Ende, Flores." },
  { "en": "Setelah Ende, Soekarno diasingkan ke mana lagi?", "id": "Bengkulu." },
  { "en": "Apa tuntutan GAPI selain Indonesia Berparlemen?", "id": "Pemerintahan bertanggung jawab pada parlemen." },
  { "en": "Komisi Visman dibentuk Belanda untuk apa?", "id": "Menyelidiki keinginan rakyat Indonesia." },
  { "en": "Apa kesimpulan Komisi Visman?", "id": "Rakyat ingin perubahan, bukan merdeka." },
  { "en": "Selain Gerakan Tiga A dan Putera, propaganda Jepang lainnya?", "id": "Jawa Hokokai." },
  { "en": "Apa arti Jawa Hokokai?", "id": "Himpunan Kebaktian Jawa." },
  { "en": "Siapa pemimpin tertinggi Jawa Hokokai?", "id": "Gunseikan (Kepala Pemerintahan Militer Jepang)." },
  { "en": "Apa tujuan utama Jepang membentuk PETA?", "id": "Membantu Jepang melawan Sekutu." },
  { "en": "Latihan militer PETA menggunakan bahasa apa?", "id": "Bahasa Jepang dan Indonesia." },
  { "en": "Pemberontakan PETA di Blitar dipicu oleh apa?", "id": "Penderitaan rakyat akibat Romusha." },
  { "en": "Selain Supriyadi, tokoh PETA Blitar lainnya?", "id": "Muradi." },
  { "en": "Apa nasib Supriyadi setelah pemberontakan?", "id": "Tidak diketahui (hilang)." },
  { "en": "Heiho adalah pasukan pribumi yang digabungkan ke unit mana?", "id": "Angkatan Darat dan Laut Jepang." },
  { "en": "Selain Seinendan, organisasi barisan pemuda lainnya?", "id": "Gakukotai (pelajar)." },
  { "en": "Pengendalian ekonomi Jepang yang ketat disebut apa?", "id": "Ekonomi perang." },
  { "en": "Apa dampak ekonomi perang Jepang bagi rakyat?", "id": "Kekurangan pangan, sandang." },
  { "en": "Siapa tokoh yang menolak kerjasama dengan Jepang secara terbuka?", "id": "Amir Sjarifuddin (awalnya rahasia)." },
  { "en": "Sidang BPUPKI dilaksanakan di gedung mana?", "id": "Gedung Chuo Sangi In (Volksraad)." },
  { "en": "Selain Soekarno, Moh. Yamin, siapa lagi yang menyampaikan usulan dasar negara?", "id": "Mr. Soepomo." },
  { "en": "Usulan Soepomo menekankan konsep negara apa?", "id": "Negara integralistik." },
  { "en": "Apa nama lain dari \"Piagam Jakarta\"?", "id": "Jakarta Charter." },
  { "en": "Siapa tokoh Kristen yang keberatan dengan tujuh kata dalam Piagam Jakarta?", "id": "Johannes Latuharhary." },
  { "en": "Kapan tujuh kata dalam Piagam Jakarta diubah?", "id": "18 Agustus 1945 (Sidang PPKI)." },
  { "en": "Berapa anggota PPKI yang ditunjuk Jepang awalnya?", "id": "21 orang." },
  { "en": "Siapa yang menambah anggota PPKI tanpa sepengetahuan Jepang?", "id": "Soekarno." },
  { "en": "Pertemuan Soekarno, Hatta, Radjiman dengan Marsekal Terauchi di mana?", "id": "Dalat, Vietnam." },
  { "en": "Apa yang disampaikan Terauchi di Dalat?", "id": "Jepang akan beri kemerdekaan." },
  { "en": "Siapa tokoh pemuda yang mendesak Soekarno di Rengasdengklok?", "id": "Sukarni, Wikana, Chaerul Saleh." },
  { "en": "Di rumah siapa Soekarno-Hatta singgah setelah dari Rengasdengklok sebelum ke Maeda?", "id": "Tidak ada, langsung ke Maeda." },
  { "en": "Mengapa rumah Laksamana Maeda dipilih untuk merumuskan proklamasi?", "id": "Dianggap aman (hak imunitas)." },
  { "en": "Selain tiga perumus utama, siapa saja yang hadir saat perumusan naskah?", "id": "Sayuti Melik, B.M. Diah, Sukarni." },
  { "en": "B.M. Diah berperan dalam hal apa terkait naskah proklamasi?", "id": "Menyebarkan berita proklamasi." },
  { "en": "Perubahan kata \"tempoh\" menjadi \"tempo\" dilakukan oleh siapa?", "id": "Soekarno (saat didiktekan)." },
  { "en": "Bendera yang dikibarkan saat proklamasi awalnya disebut apa?", "id": "Sang Saka Merah Putih." },
  { "en": "Siapa yang menyediakan tiang bendera darurat saat proklamasi?", "id": "Suhud Sastrosoeparto." },
  { "en": "Berita proklamasi disebarkan melalui media apa saja?", "id": "Radio (Hoso Kyoku), surat kabar, pamflet." },
  { "en": "Siapa penyiar radio yang berani menyiarkan proklamasi?", "id": "Jusuf Ronodipuro." },
  { "en": "Maklumat Pemerintah 3 November 1945 berisi tentang apa?", "id": "Pembentukan partai-partai politik." },
  { "en": "Maklumat Wakil Presiden No. X (16 Oktober 1945) berisi apa?", "id": "KNIP diberi kekuasaan legislatif." },
  { "en": "Tentara Keamanan Rakyat (TKR) dibentuk tanggal berapa?", "id": "5 Oktober 1945." },
  { "en": "Siapa yang diangkat menjadi Panglima Besar TKR pertama?", "id": "Jenderal Soedirman." },
  { "en": "Peristiwa 10 November di Surabaya dipicu oleh tewasnya siapa?", "id": "Brigjen A.W.S. Mallaby." },
  { "en": "Ultimatum Sekutu di Surabaya berisi perintah apa?", "id": "Menyerahkan senjata, menghentikan perlawanan." },
  { "en": "Pidato Bung Tomo yang terkenal membakar semangat disiarkan lewat apa?", "id": "Radio Pemberontakan." },
  { "en": "Pertempuran Palagan Ambarawa menggunakan taktik supit urang, artinya?", "id": "Pengepungan dari dua sisi." },
  { "en": "Hari Juang Kartika diperingati berdasarkan peristiwa apa?", "id": "Kemenangan di Ambarawa." },
  { "en": "Tokoh pejuang wanita dalam Pertempuran Medan Area?", "id": "Tidak ada yang sangat menonjol (lebih kolektif)." },
  { "en": "Perjanjian Linggarjati ditandatangani di kota mana?", "id": "Linggarjati, Kuningan, Jawa Barat." },
  { "en": "Siapa Perdana Menteri Belanda saat Perjanjian Linggarjati?", "id": "Willem Schermerhorn." },
  { "en": "Apa hasil utama Perjanjian Linggarjati bagi Indonesia?", "id": "Pengakuan de facto atas Jawa, Sumatra, Madura." },
  { "en": "Mengapa banyak pihak di RI menentang Perjanjian Linggarjati?", "id": "Dianggap terlalu menguntungkan Belanda." },
  { "en": "Ibu kota RI sebelum Agresi Militer Belanda I?", "id": "Jakarta, lalu Yogyakarta." },
  { "en": "Wilayah mana saja yang dikuasai Belanda setelah Agresi Militer I?", "id": "Kota-kota besar, pelabuhan, perkebunan." },
  { "en": "Resolusi Dewan Keamanan PBB setelah Agresi Militer I berisi apa?", "id": "Seruan gencatan senjata." },
  { "en": "Komisi Tiga Negara (KTN) dibentuk oleh siapa?", "id": "Dewan Keamanan PBB." },
  { "en": "Siapa wakil Australia di KTN yang pro-Indonesia?", "id": "Richard Kirby." },
  { "en": "Siapa wakil Belgia di KTN yang pro-Belanda?", "id": "Paul van Zeeland." },
  { "en": "Perundingan Renville menghasilkan garis demarkasi apa?", "id": "Garis Van Mook." },
  { "en": "Mengapa Perjanjian Renville sangat merugikan RI?", "id": "Wilayah RI jadi sangat kecil." },
  { "en": "Siapa yang jatuh dari kabinet setelah Perjanjian Renville?", "id": "Amir Sjarifuddin." },
  { "en": "Pemberontakan PKI Madiun 1948 dipimpin oleh siapa?", "id": "Musso dan Amir Sjarifuddin." },
  { "en": "Tujuan pemberontakan PKI Madiun?", "id": "Mendirikan negara komunis." },
  { "en": "Bagaimana nasib Musso setelah pemberontakan Madiun?", "id": "Tewas tertembak." },
  { "en": "Serangan Belanda ke Yogyakarta saat Agresi Militer II dikenal sebagai?", "id": "Operatie Kraai (Operasi Gagak)." },
  { "en": "Selain Soekarno-Hatta, menteri siapa yang ikut ditawan Belanda?", "id": "Sutan Sjahrir, Agus Salim." },
  { "en": "Siapa yang memberi mandat kepada Sjafruddin Prawiranegara untuk membentuk PDRI?", "id": "Soekarno-Hatta (sebelum ditawan)." },
  { "en": "Tujuan utama Serangan Umum 1 Maret 1949?", "id": "Menunjukkan eksistensi RI ke dunia." },
  { "en": "Siapa wakil Indonesia dalam Perjanjian Roem-Royen?", "id": "Mohammad Roem." },
  { "en": "Siapa wakil Belanda dalam Perjanjian Roem-Royen?", "id": "Herman van Roijen." },
  { "en": "Isi penting Perjanjian Roem-Royen?", "id": "Penghentian permusuhan, kembalinya Yogya." },
  { "en": "Sebelum KMB, diadakan konferensi antar-Indonesia untuk apa?", "id": "Menyatukan sikap delegasi RI & BFO." },
  { "en": "Apa itu BFO (Bijeenkomst voor Federaal Overleg)?", "id": "Majelis Permusyawaratan Federal (negara boneka Belanda)." },
  { "en": "Siapa tokoh BFO yang kemudian mendukung RI?", "id": "Sultan Hamid II (meski kontroversial)." },
  { "en": "Selain pengakuan kedaulatan, apa lagi hasil KMB?", "id": "Pembentukan Uni Indonesia-Belanda." },
  { "en": "Hutang Hindia Belanda yang harus ditanggung RIS sebesar berapa?", "id": "4,3 miliar gulden." },
  { "en": "Kapan penyerahan kedaulatan di Jakarta dilaksanakan?", "id": "Istana Merdeka (dulu Istana Gambir)." },
  { "en": "Siapa yang mewakili Belanda dalam penyerahan kedaulatan di Jakarta?", "id": "A.H.J. Lovink." },
  { "en": "Siapa yang menerima kedaulatan atas nama RIS di Jakarta?", "id": "Sri Sultan Hamengkubuwono IX." },
  { "en": "Berapa lama usia Republik Indonesia Serikat (RIS)?", "id": "Kurang dari setahun." },
  { "en": "Apa alasan utama negara-negara bagian RIS ingin kembali ke NKRI?", "id": "Keinginan rakyat, tidak sesuai cita-cita." },
  { "en": "Siapa Perdana Menteri terakhir RIS?", "id": "Mohammad Hatta (juga Wapres)." },
  { "en": "Kabinet siapa yang dikenal sebagai Kabinet Zaken (ahli)?", "id": "Kabinet Wilopo." },
  { "en": "Peristiwa 17 Oktober 1952 melibatkan demonstrasi siapa?", "id": "Tentara dan rakyat." },
  { "en": "Apa tuntutan Peristiwa 17 Oktober 1952?", "id": "Pembubaran parlemen, profesionalisasi tentara." },
  { "en": "Kabinet Ali Sastroamidjojo I berhasil menyelenggarakan apa?", "id": "Konferensi Asia-Afrika." },
  { "en": "Sistem kepartaian pada masa Demokrasi Parlementer disebut?", "id": "Sistem multipartai." },
  { "en": "Pemberontakan DI/TII di Aceh dipimpin oleh siapa?", "id": "Daud Beureu'eh." },
  { "en": "Penyebab pemberontakan DI/TII Aceh?", "id": "Kekecewaan status Aceh jadi karesidenan." },
  { "en": "Pemberontakan DI/TII di Sulawesi Selatan dipimpin oleh siapa?", "id": "Kahar Muzakkar." },
  { "en": "Pemberontakan DI/TII di Kalimantan Selatan dipimpin oleh siapa?", "id": "Ibnu Hadjar." },
  { "en": "Apa tujuan utama PRRI/Permesta?", "id": "Koreksi kebijakan pusat, otonomi daerah." },
  { "en": "Siapa tokoh militer yang terlibat PRRI?", "id": "Kolonel Ahmad Husein, Kolonel Simbolon." },
  { "en": "Siapa tokoh sipil penting dalam PRRI?", "id": "Sjafruddin Prawiranegara (Perdana Menteri PRRI)." },
  { "en": "Bantuan asing kepada PRRI/Permesta datang dari mana?", "id": "Amerika Serikat (CIA)." },
  { "en": "Siapa pilot Amerika yang tertembak jatuh saat membantu Permesta?", "id": "Allen Pope." },
  { "en": "Isi pidato Soekarno yang menjadi dasar Demokrasi Terpimpin?", "id": "Penemuan Kembali Revolusi Kita." },
  { "en": "Lembaga perwakilan rakyat pada masa Demokrasi Terpimpin?", "id": "DPR-GR (Gotong Royong)." },
  { "en": "Lembaga tertinggi negara pada masa Demokrasi Terpimpin?", "id": "MPRS (Sementara)." },
  { "en": "Front Nasional dibentuk untuk apa?", "id": "Menghimpun potensi nasional." },
  { "en": "Manipol USDEK dijadikan GBHN oleh siapa?", "id": "MPRS." },
  { "en": "Apa inti dari ajaran Resopim Soekarno?", "id": "Revolusi, Sosialisme Indonesia, Pimpinan Nasional." },
  { "en": "Pengerahan sukarelawan Trikora disebut Komando apa?", "id": "Komando Mandala." },
  { "en": "Pertempuran Laut Aru melibatkan pahlawan siapa?", "id": "Komodor Yos Sudarso." },
  { "en": "Kapal RI yang tenggelam di Laut Aru?", "id": "KRI Macan Tutul." },
  { "en": "UNTEA adalah badan PBB untuk apa di Irian Barat?", "id": "Pemerintahan sementara PBB." },
  { "en": "Apa kepanjangan UNTEA?", "id": "United Nations Temporary Executive Authority." },
  { "en": "Alasan utama Konfrontasi dengan Malaysia?", "id": "Pembentukan Federasi Malaysia dianggap neokolonialisme." },
  { "en": "Ganefo adalah pesta olahraga tandingan untuk apa?", "id": "Olimpiade." },
  { "en": "Apa kepanjangan Ganefo?", "id": "Games of the New Emerging Forces." },
  { "en": "Indonesia keluar dari PBB tahun berapa?", "id": "Tahun 1965." },
  { "en": "Alasan Indonesia keluar dari PBB?", "id": "Malaysia jadi anggota tidak tetap DK PBB." },
  { "en": "Siapa Ketua CC PKI pada masa G30S?", "id": "D.N. Aidit." },
  { "en": "Siapa Komandan Resimen Cakrabirawa saat G30S?", "id": "Brigjen Sabur." },
  { "en": "Siapa pemimpin gerakan G30S di lapangan?", "id": "Letkol Untung Syamsuri." },
  { "en": "Di mana para jenderal korban G30S ditemukan?", "id": "Lubang Buaya, Jakarta Timur." },
  { "en": "Tuntutan mahasiswa setelah G30S dikenal sebagai?", "id": "Tritura (Tri Tuntutan Rakyat)." },
  { "en": "Apa isi pertama Tritura?", "id": "Bubarkan PKI." },
  { "en": "Apa isi kedua Tritura?", "id": "Ritul kabinet Dwikora." },
  { "en": "Apa isi ketiga Tritura?", "id": "Turunkan harga." },
  { "en": "Aksi-aksi mahasiswa yang menuntut Tritura dikoordinir oleh?", "id": "KAMI, KAPPI." },
  { "en": "Supersemar sering dianggap sebagai tonggak lahirnya?", "id": "Orde Baru." },
  { "en": "Apa tindakan pertama Soeharto setelah menerima Supersemar?", "id": "Membubarkan PKI." },
  { "en": "Tap MPRS No. XXV/MPRS/1966 berisi tentang apa?", "id": "Pembubaran PKI dan larangan komunisme." },
  { "en": "Sidang Istimewa MPRS tahun 1967 memutuskan apa?", "id": "Mencabut kekuasaan Soekarno." },
  { "en": "Soeharto dijuluki \"Bapak Pembangunan\", mengapa?", "id": "Fokus pembangunan ekonomi." },
  { "en": "Selain Repelita, apa sebutan untuk program pangan Orde Baru?", "id": "Revolusi Hijau." },
  { "en": "Dampak Revolusi Hijau di bidang pertanian?", "id": "Peningkatan produksi beras (swasembada)." },
  { "en": "Waduk besar yang dibangun pada masa Orde Baru?", "id": "Waduk Jatiluhur, Saguling." },
  { "en": "Satelit komunikasi pertama Indonesia diluncurkan Orde Baru?", "id": "Satelit Palapa." },
  { "en": "Apa tujuan utama kebijakan fusi partai politik tahun 1973?", "id": "Menyederhanakan sistem kepartaian, stabilitas politik." },
  { "en": "PPP adalah fusi dari partai-partai berbasis apa?", "id": "Islam." },
  { "en": "PDI adalah fusi dari partai-partai berbasis apa?", "id": "Nasionalis dan non-Islam." },
  { "en": "Asas tunggal Pancasila diwajibkan bagi semua ormas berdasarkan UU apa?", "id": "UU Ormas 1985." },
  { "en": "Normalisasi Kehidupan Kampus/Badan Koordinasi Kemahasiswaan (NKK/BKK) untuk apa?", "id": "Meredam aktivisme mahasiswa." },
  { "en": "Apa penyebab utama Peristiwa Malari 1974?", "id": "Protes investasi asing (Jepang)." },
  { "en": "Siapa Perdana Menteri Jepang yang berkunjung saat Malari?", "id": "Kakuei Tanaka." },
  { "en": "Operasi khusus (Opsus) pada masa Orde Baru dipimpin oleh siapa?", "id": "Ali Moertopo." },
  { "en": "Petisi 50 berisi apa?", "id": "Kritik terhadap pidato Soeharto." },
  { "en": "Siapa saja tokoh terkenal dalam Petisi 50?", "id": "A.H. Nasution, Hoegeng, Ali Sadikin." },
  { "en": "Pembredelan pers sering terjadi, contoh majalah yang dibredel?", "id": "Tempo, Editor, DeTik." },
  { "en": "Kasus pembunuhan aktivis buruh Marsinah terjadi di provinsi mana?", "id": "Jawa Timur." },
  { "en": "Perang saudara di Timor Timur sebelum integrasi melibatkan Fretilin melawan siapa?", "id": "UDT, Apodeti." },
  { "en": "Apa kepanjangan Fretilin?", "id": "Frente Revolucionária de Timor-Leste Independente." },
  { "en": "Siapa pemimpin Fretilin yang memproklamasikan kemerdekaan Timor Timur?", "id": "Xanana Gusmão (kemudian)." },
  { "en": "Program transmigrasi Orde Baru bertujuan untuk apa?", "id": "Pemerataan penduduk, pembangunan daerah." },
  { "en": "Dampak negatif transmigrasi?", "id": "Konflik sosial, masalah lingkungan." },
  { "en": "Apa yang dimaksud dengan \"kelompok kepentingan\" pada masa Orde Baru?", "id": "Golongan yang diakomodasi rezim." },
  { "en": "Gebu Minang adalah contoh organisasi apa?", "id": "Organisasi kedaerahan (yang didukung)." },
  { "en": "Krisis moneter 1997/1998 dimulai dari negara mana di Asia?", "id": "Thailand." },
  { "en": "Apa dampak langsung krisis moneter bagi Indonesia?", "id": "Rupiah anjlok, PHK massal." },
  { "en": "Siapa Menteri Keuangan saat menandatangani LoI dengan IMF?", "id": "Mar'ie Muhammad." },
  { "en": "Kerusuhan Mei 1998 terjadi di kota-kota mana saja?", "id": "Jakarta, Solo, Medan, dll." },
  { "en": "Apa sasaran utama kerusuhan Mei 1998 selain mahasiswa?", "id": "Etnis Tionghoa." },
  { "en": "Gedung DPR/MPR diduduki mahasiswa pada tanggal berapa Mei 1998?", "id": "Sekitar 18-20 Mei 1998." },
  { "en": "Siapa Ketua DPR/MPR saat Soeharto lengser?", "id": "Harmoko." },
  { "en": "Pidato pengunduran diri Soeharto disiarkan dari mana?", "id": "Istana Merdeka." },
  { "en": "Apa landasan hukum Habibie naik menjadi Presiden?", "id": "Pasal 8 UUD 1945." },
  { "en": "Kabinet pertama B.J. Habibie bernama?", "id": "Kabinet Reformasi Pembangunan." },
  { "en": "Apa salah satu UU penting yang lahir di era Habibie?", "id": "UU Kemerdekaan Pers, UU Otonomi Daerah." },
  { "en": "Berapa banyak partai politik peserta Pemilu 1999?", "id": "48 partai." },
  { "en": "Mengapa Gus Dur dipilih MPR menjadi Presiden meski partainya bukan pemenang teratas?", "id": "Poros Tengah." },
  { "en": "Siapa tokoh utama Poros Tengah?", "id": "Amien Rais." },
  { "en": "Departemen apa yang dibubarkan Gus Dur dan menimbulkan kontroversi?", "id": "Departemen Sosial, Departemen Penerangan." },
  { "en": "Kasus apa yang sering dikaitkan dengan upaya impeachment Gus Dur?", "id": "Buloggate, Bruneigate." },
  { "en": "Sidang Istimewa MPR yang memberhentikan Gus Dur dipimpin oleh siapa?", "id": "Amien Rais." },
  { "en": "Megawati Soekarnoputri adalah presiden wanita pertama, dari partai apa?", "id": "Partai Demokrasi Indonesia Perjuangan (PDIP)." },
  { "en": "Privatisasi BUMN menjadi isu kontroversial pada masa siapa?", "id": "Megawati Soekarnoputri." },
  { "en": "Bom Bali I terjadi tahun berapa, pada masa pemerintahan siapa?", "id": "2002, Megawati." },
  { "en": "Siapa yang memenangkan Pilpres langsung pertama tahun 2004?", "id": "Susilo Bambang Yudhoyono (SBY)." },
  { "en": "Partai politik pengusung SBY pada Pemilu 2004?", "id": "Partai Demokrat." },
  { "en": "Pasangan SBY-JK dikenal dengan slogan apa?", "id": "Bersama Kita Bisa." },
  { "en": "Apa fokus utama pemerintahan SBY periode pertama?", "id": "Pemberantasan korupsi, perdamaian Aceh, ekonomi." },
  { "en": "Lembaga apa yang dibentuk SBY untuk menangani bencana?", "id": "BNPB (Badan Nasional Penanggulangan Bencana)." },
  { "en": "Siapa tokoh GAM yang menandatangani perjanjian Helsinki?", "id": "Malik Mahmud." },
  { "en": "Siapa wakil RI yang menandatangani perjanjian Helsinki?", "id": "Hamid Awaludin (Menteri Hukum dan HAM)." },
  { "en": "Kasus Bank Century mencuat pada masa pemerintahan siapa?", "id": "Susilo Bambang Yudhoyono (periode kedua)." },
  { "en": "Siapa Ketua KPK pertama?", "id": "Taufiequrachman Ruki." },
  { "en": "Amandemen UUD 1945 keempat (terakhir) dilakukan tahun berapa?", "id": "Tahun 2002." },
  { "en": "Salah satu perubahan mendasar dalam amandemen UUD 1945?", "id": "Pemilihan Presiden langsung." },
  { "en": "Mahkamah Konstitusi (MK) dibentuk sebagai hasil amandemen untuk apa?", "id": "Menguji UU terhadap UUD." },
  { "en": "Komisi Yudisial (KY) dibentuk untuk apa?", "id": "Mengawasi perilaku hakim." },
  { "en": "Undang-Undang Keterbukaan Informasi Publik disahkan tahun berapa?", "id": "Tahun 2008." },
  { "en": "Siapa lawan SBY-Boediono dalam Pilpres 2009?", "id": "Megawati-Prabowo, JK-Wiranto." },
  { "en": "Program PNPM Mandiri bertujuan untuk apa?", "id": "Pengentasan kemiskinan berbasis pemberdayaan masyarakat." },
  { "en": "Siapa lawan Jokowi-JK dalam Pilpres 2014?", "id": "Prabowo Subianto-Hatta Rajasa." },
  { "en": "Apa nama program unggulan Jokowi di bidang kesehatan?", "id": "Kartu Indonesia Sehat (KIS)." },
  { "en": "Pembangunan infrastruktur masif menjadi ciri pemerintahan siapa?", "id": "Joko Widodo." },
  { "en": "Ibu Kota Negara (IKN) baru diberi nama apa?", "id": "Nusantara." },
  { "en": "Undang-Undang Cipta Kerja (Omnibus Law) disahkan tahun berapa?", "id": "Tahun 2020." },
  { "en": "Pandemi COVID-19 mulai melanda Indonesia tahun berapa?", "id": "Tahun 2020." },
  { "en": "Siapa Wakil Presiden pada periode kedua Jokowi?", "id": "Ma'ruf Amin." },
  { "en": "Pemilihan Umum serentak (Pileg dan Pilpres) pertama kali diadakan tahun?", "id": "Tahun 2019." },
  { "en": "Relief Karmawibhangga di Borobudur menceritakan tentang apa?", "id": "Hukum sebab akibat." },
  { "en": "Candi Mendut dan Pawon terkait dengan candi apa?", "id": "Candi Borobudur (satu kesatuan)." },
  { "en": "Sastra Jawa Kuno berbentuk prosa disebut apa?", "id": "Parwa." },
  { "en": "Kakawin Smaradahana menceritakan kisah dewa siapa?", "id": "Dewa Kama dan Dewi Ratih." },
  { "en": "Siapa arsitek Belanda yang merancang Gedung Sate Bandung?", "id": "J. Gerber." },
  { "en": "Aliran seni lukis Mooi Indië menggambarkan apa?", "id": "Keindahan Hindia Belanda." },
  { "en": "Siapa pelopor seni patung modern Indonesia?", "id": "G. Sidharta Soegijo." },
  { "en": "Grup musik rock legendaris Indonesia tahun 1970-an?", "id": "God Bless, AKA." },
  { "en": "Siapa penulis novel \"Harimau! Harimau!\"?", "id": "Mochtar Lubis." },
  { "en": "Tetralogi Buru karya Pramoedya Ananta Toer terdiri dari buku apa saja?", "id": "Bumi Manusia, Anak Semua Bangsa, Jejak Langkah, Rumah Kaca." },
  { "en": "Upacara adat Seren Taun di Jawa Barat untuk apa?", "id": "Syukuran panen padi." },
  { "en": "Tradisi Pasola berasal dari daerah mana?", "id": "Sumba, Nusa Tenggara Timur." },
  { "en": "Kain tradisional Batak disebut apa?", "id": "Ulos." },
  { "en": "Kain tenun ikat ganda dari Desa Tenganan Bali?", "id": "Kain Gringsing." },
  { "en": "Sistem kepercayaan asli suku Dayak di Kalimantan?", "id": "Kaharingan." },
  { "en": "Tokoh penyebar Islam di Maluku yang terkenal?", "id": "Syekh Mansur (dari Ternate)." },
  { "en": "Sekolah THHK didirikan oleh komunitas apa?", "id": "Tionghoa." },
  { "en": "Apa kepanjangan THHK?", "id": "Tiong Hoa Hwee Koan." },
  { "en": "Siapa yang pertama kali mengemukakan konsep \"Wawasan Nusantara\"?", "id": "Tidak ada satu tokoh tunggal (proses)." },
  { "en": "Film \"Tiga Dara\" disutradarai oleh siapa?", "id": "Usmar Ismail." },
  { "en": "Stasiun TVRI mulai mengudara tahun berapa?", "id": "Tahun 1962." },
  { "en": "Hari Pers Nasional diperingati setiap tanggal berapa?", "id": "9 Februari." },
  { "en": "Siapa tokoh yang dikenal sebagai Bapak Koperasi Indonesia?", "id": "Mohammad Hatta." },
  { "en": "Apa makna \"Tut Wuri Handayani\" dalam semboyan pendidikan?", "id": "Di belakang memberi dorongan." },
  { "en": "Undang-Undang Pokok Agraria disahkan tahun berapa?", "id": "Tahun 1960." },
  { "en": "Apa tujuan utama Undang-Undang Pokok Agraria 1960?", "id": "Land reform, kepastian hukum tanah." },
  { "en": "Siapa tokoh perumus lambang Garuda Pancasila selain Sultan Hamid II?", "id": "Tim perumus (termasuk Yamin, Ki Hajar)." },
  { "en": "Istilah \"Bhinneka Tunggal Ika\" diambil dari kitab apa?", "id": "Sutasoma." },
  { "en": "Siapa yang mempopulerkan penggunaan kata \"Sarikat\" menjadi \"Serikat\"?", "id": "H. Agus Salim." },
  { "en": "Pemberontakan RMS (Republik Maluku Selatan) dipimpin oleh siapa?", "id": "Dr. Christiaan Soumokil." },
  { "en": "Kapan pemberontakan RMS diproklamasikan?", "id": "April 1950." },
  { "en": "Siapa pahlawan nasional yang gugur menumpas RMS?", "id": "Kolonel Slamet Rijadi." },
  { "en": "Penentuan Pendapat Rakyat (Pepera) 1969 di Irian Barat menggunakan sistem apa?", "id": "Musyawarah perwakilan." },
  { "en": "Konsep \"Poros Jakarta-Peking-Pyongyang\" ada pada masa siapa?", "id": "Soekarno (Demokrasi Terpimpin)." },
  { "en": "Siapa yang menggantikan D.N. Aidit sebagai pemimpin PKI setelah G30S?", "id": "Sudisman (sementara, lalu hancur)." },
  { "en": "Operasi Trisula untuk menumpas sisa PKI di daerah mana?", "id": "Blitar Selatan." },
  { "en": "Apa yang dimaksud \"floating mass\" (massa mengambang) Orde Baru?", "id": "Rakyat dilarang berpolitik praktis." },
  { "en": "Program ABRI Masuk Desa (AMD) bertujuan untuk apa?", "id": "Pendekatan sosial, pembangunan desa." },
  { "en": "Siapa Jenderal L.B. Moerdani yang berpengaruh di era Orde Baru?", "id": "Panglima ABRI, Menteri Pertahanan." },
  { "en": "Peristiwa 27 Juli 1996 (Kudatuli) adalah penyerbuan kantor partai apa?", "id": "Partai Demokrasi Indonesia (PDI)." },
  { "en": "Siapa yang menjadi Ketua Umum PDI versi pemerintah saat Kudatuli?", "id": "Soerjadi." },
  { "en": "Siapa Ketua Umum PDI yang digulingkan saat Kudatuli?", "id": "Megawati Soekarnoputri." },
  { "en": "Tim Gabungan Pencari Fakta (TGPF) Kerusuhan Mei 1998 dibentuk oleh siapa?", "id": "Presiden Habibie." },
  { "en": "Lembaga Ombudsman Nasional dibentuk pada masa pemerintahan siapa?", "id": "Abdurrahman Wahid." },
  { "en": "Siapa yang menandatangani UU Pemilu yang memungkinkan multipartai era Reformasi?", "id": "B.J. Habibie." },
  { "en": "Konflik komunal di Ambon dan Poso terjadi pada era apa?", "id": "Awal Reformasi." },
  { "en": "Siapa Ketua DPR periode 2004-2009 yang juga ketua Golkar?", "id": "Agung Laksono." },
  { "en": "Program konversi minyak tanah ke LPG digalakkan masa siapa?", "id": "Susilo Bambang Yudhoyono." },
  { "en": "Apa nama lain dari program Jaminan Kesehatan Nasional (JKN)?", "id": "BPJS Kesehatan." },
  { "en": "Siapa yang melantik Joko Widodo sebagai Presiden RI ke-7?", "id": "Ketua MPR (Zulkifli Hasan)." },
  { "en": "Asian Games 2018 diadakan di kota mana saja di Indonesia?", "id": "Jakarta dan Palembang." },
  { "en": "Vaksinasi COVID-19 pertama di Indonesia diberikan kepada siapa?", "id": "Presiden Joko Widodo." },
  { "en": "Undang-Undang Perlindungan Data Pribadi (UU PDP) disahkan tahun berapa?", "id": "Tahun 2022." },
  { "en": "Siapa Perdana Menteri RIS yang berasal dari Partai Masyumi?", "id": "Mohammad Natsir (Kabinet NKRI pertama)." },
  { "en": "Perang Padri berakhir dengan jatuhnya benteng terakhir kaum Padri, yaitu?", "id": "Bonjol." },
  { "en": "Siapa pahlawan wanita dari Aceh yang matanya buta di pengasingan?", "id": "Cut Nyak Dien." },
  { "en": "Di mana Cut Nyak Dien diasingkan hingga wafat?", "id": "Sumedang, Jawa Barat." },
  { "en": "Organisasi pergerakan keagamaan modernis selain Muhammadiyah?", "id": "Persatuan Islam (PERSIS)." },
  { "en": "Siapa tokoh utama Persis?", "id": "Ahmad Hassan (Hasan Bandung)." },
  { "en": "Siapa penulis \"Dari Penjara ke Penjara\"?", "id": "Tan Malaka." },
  { "en": "Tan Malaka dikenal dengan konsep politik apa?", "id": "Madilog (Materialisme, Dialektika, Logika)." },
  { "en": "Siapa yang mengomandani pendaratan Sekutu pertama di Indonesia pasca-Perang Dunia II?", "id": "Letjen Sir Philip Christison." },
  { "en": "Peraturan \"Poenale Sanctie\" dalam Koelie Ordonnantie mengatur tentang apa?", "id": "Sanksi pidana bagi kuli kontrak." },
  { "en": "Siapa arsitek Masjid Agung Demak menurut tradisi selain Sunan Kalijaga?", "id": "Raden Patah (prakarsa), para wali." },
  { "en": "Apa nama kapal dagang utama yang digunakan VOC untuk pelayaran antar benua?", "id": "Spiegelretourschip (kapal зеркало)." },
  { "en": "Kitab \"Bustanussalatin\" karya Nuruddin ar-Raniri berisi tentang apa?", "id": "Sejarah dan ajaran Islam Kesultanan Aceh." },
  { "en": "Siapa Ratu Kalinyamat dari Jepara yang terkenal menyerang Portugis di Malaka?", "id": "Putri Sultan Trenggana Demak." },
  { "en": "Perjanjian Saragosa (1529) membagi pengaruh Spanyol dan Portugis di mana terkait Maluku?", "id": "Garis batas timur Maluku." },
  { "en": "Apa \"Hongitochten\" yang dilakukan VOC di Maluku?", "id": "Pelayaran patroli pemusnah rempah ilegal." },
  { "en": "Siapa Tuanku Nan Renceh, tokoh ulama Padri yang radikal?", "id": "Salah satu Harimau Nan Salapan." },
  { "en": "Apa sebutan untuk pajak kepala yang diterapkan Raffles?", "id": "Hoofdgeld (bagian landrent)." },
  { "en": "Sekolah \"Burgeravondschool\" pada masa kolonial diperuntukkan bagi siapa?", "id": "Anak Eropa dan pribumi mampu." },
  { "en": "Apa nama organisasi perempuan pertama yang didirikan R.A. Kartini?", "id": "Bukan organisasi, tapi sekolah." },
  { "en": "Siapa Semaun yang memimpin pemogokan buruh besar tahun 1923?", "id": "Tokoh Sarekat Islam Merah/PKI." },
  { "en": "Lagu \"Di Timur Matahari\" diciptakan untuk menyemangati apa?", "id": "Perjuangan kemerdekaan (era Jepang)." },
  { "en": "Apa nama pengadilan adat pada masa kolonial Belanda?", "id": "Landraad (untuk pribumi, ada juga lainnya)." },
  { "en": "Siapa Mas Marco Kartodikromo, wartawan dan novelis pergerakan?", "id": "Penulis \"Student Hidjo\"." },
  { "en": "Organisasi \"Persatuan Bangsa Indonesia\" (PBI) didirikan oleh siapa?", "id": "Dr. Soetomo (setelah Budi Utomo)." },
  { "en": "Apa isi \"Sumpah Pemuda Kedua\" yang kurang dikenal (jika ada)?", "id": "Tidak ada Sumpah Pemuda Kedua resmi." },
  { "en": "Siapa K.H. Samanhudi, pendiri Sarekat Dagang Islam di Solo?", "id": "Pengusaha batik Laweyan." },
  { "en": "Apa nama surat kabar yang diterbitkan oleh Indische Partij?", "id": "Het Tijdschrift, De Expres." },
  { "en": "Siapa yang menjabat sebagai Gunseikan (kepala pemerintahan militer) Jepang di Jawa?", "id": "Bervariasi (misal Harada, Yamamoto)." },
  { "en": "Apa \"Seikerei\" yang diwajibkan Jepang setiap pagi?", "id": "Menghormat ke arah Tokyo." },
  { "en": "Siapa Laksamana Tadashi Maeda yang rumahnya menjadi tempat perumusan proklamasi?", "id": "Kepala perwakilan Angkatan Laut Jepang." },
  { "en": "Peristiwa \"Tiga Daerah\" (Brebes, Tegal, Pemalang) tahun 1945 merupakan gejolak apa?", "id": "Revolusi sosial anti-feodal/mapan." },
  { "en": "Siapa Tan Malaka yang mengusulkan konsep Republik Indonesia sebelum tokoh lain?", "id": "Penulis \"Naar de Republiek Indonesia\"." },
  { "en": "Badan Pekerja KNIP (BP-KNIP) diketuai oleh siapa?", "id": "Sutan Sjahrir (awalnya)." },
  { "en": "Siapa Jenderal Mallaby yang tewas dan memicu Pertempuran Surabaya?", "id": "Komandan Brigade 49 Inggris." },
  { "en": "Perundingan Hooge Veluwe (April 1946) gagal karena masalah apa?", "id": "Status kedaulatan RI." },
  { "en": "Siapa Lord Killearn, mediator Inggris dalam perundingan awal RI-Belanda?", "id": "Perwakilan khusus Inggris." },
  { "en": "Apa \"Garis Status Quo\" hasil Perjanjian Renville?", "id": "Garis demarkasi Van Mook." },
  { "en": "Siapa Teuku Muhammad Hasan, Gubernur Sumatra pertama era kemerdekaan?", "id": "Tokoh Aceh, anggota PPKI." },
  { "en": "Kabinet Amir Sjarifuddin II jatuh karena tidak mendapat dukungan dari partai mana?", "id": "Masyumi dan PNI." },
  { "en": "Siapa dr. Johannes Leimena, tokoh Kristen yang sering menjadi menteri?", "id": "Wakil Perdana Menteri, Menteri Kesehatan." },
  { "en": "Program Benteng (1950-an) bertujuan melindungi pengusaha apa?", "id": "Pengusaha pribumi dari persaingan asing." },
  { "en": "Siapa Sumitro Djojohadikusumo yang merancang Program Benteng?", "id": "Menteri Perdagangan dan Perindustrian." },
  { "en": "Pemilu 1955 menggunakan sistem apa untuk memilih anggota DPR?", "id": "Sistem perwakilan berimbang (proporsional)." },
  { "en": "Siapa Wilopo, Perdana Menteri yang kabinetnya dikenal sebagai Zaken Kabinet?", "id": "Tokoh PNI." },
  { "en": "Apa \"Deklarasi Ekonomi\" (Dekon) yang dikeluarkan Soekarno tahun 1963?", "id": "Strategi ekonomi berdikari sosialis." },
  { "en": "Siapa D.N. Aidit yang menjadi Ketua CC PKI sejak 1951?", "id": "Pemimpin utama PKI era Orla." },
  { "en": "Lembaga Kebudayaan Rakyat (Lekra) berafiliasi dengan organisasi apa?", "id": "Partai Komunis Indonesia (PKI)." },
  { "en": "Siapa Jenderal Soedirman yang memimpin perang gerilya meski sakit parah?", "id": "Panglima Besar TKR/TNI." },
  { "en": "Apa \"Nawaksara\", pidato pertanggungjawaban Soekarno yang ditolak MPRS?", "id": "Pidato Presiden di depan Sidang MPRS." },
  { "en": "Siapa Jenderal Maraden Panggabean, tokoh militer penting Orde Baru?", "id": "Menhankam/Pangab era Soeharto." },
  { "en": "Apa itu \"Asas Tunggal Pancasila\" yang diterapkan Orde Baru?", "id": "Semua parpol/ormas wajib berasas Pancasila." },
  { "en": "Siapa Prof. Dr. Soemitro Djojohadikoesoemo, arsitek ekonomi Orde Baru?", "id": "Menteri Riset, Menteri Perdagangan." },
  { "en": "Insiden Woyla adalah pembajakan pesawat Garuda oleh kelompok mana?", "id": "Kelompok ekstremis Islam (Komando Jihad)." },
  { "en": "Siapa Munir Said Thalib, aktivis HAM yang dibunuh tahun 2004?", "id": "Pendiri KontraS, Imparsial." },
  { "en": "Apa nama lengkap dari \"Tragedi Semanggi I dan II\"?", "id": "Penembakan mahasiswa di Jembatan Semanggi." },
  { "en": "Siapa Emil Salim, Menteri Lingkungan Hidup era Orde Baru yang dikenal?", "id": "Ekonom, ahli lingkungan." },
  { "en": "Program \"Dana Desa\" mulai digulirkan secara masif pada masa pemerintahan siapa?", "id": "Joko Widodo." },
  { "en": "Siapa Susi Pudjiastuti, Menteri Kelautan dan Perikanan yang terkenal dengan kebijakan penenggelaman kapal?", "id": "Pengusaha, menteri Kabinet Kerja." },
  { "en": "Apa nama lain Prasasti Yupa yang ditemukan di Kutai?", "id": "Prasasti Mulawarman." },
  { "en": "Siapa nama pendeta Brahmana yang memimpin upacara di Kerajaan Kutai?", "id": "Para Brahmana (umum)." },
  { "en": "Kerajaan Tarumanegara diperkirakan runtuh akibat serangan dari kerajaan mana?", "id": "Sriwijaya." },
  { "en": "Relief perahu bercadik di Candi Borobudur menggambarkan aktivitas apa?", "id": "Pelayaran dan perdagangan maritim." },
  { "en": "Prasasti Ligor (A & B) ditemukan di wilayah mana terkait Sriwijaya?", "id": "Semenanjung Malaya (Thailand Selatan)." },
  { "en": "Apa nama candi Buddha terkecil di kompleks Prambanan?", "id": "Candi Asu (bagian dari Lumbung)." },
  { "en": "Siapa Rakai Pikatan dari Dinasti Sanjaya yang menikahi Pramodawardhani?", "id": "Raja Mataram Kuno." },
  { "en": "Kitab \"Kakawin Hariwangsa\" digubah oleh Mpu Panuluh pada masa kerajaan apa?", "id": "Kerajaan Kediri." },
  { "en": "Siapa Mahisa Wongateleng, ayah dari Ken Arok menurut Pararaton?", "id": "Brahmana (ayah angkat/spiritual)." },
  { "en": "Arca \"Amoghapasa\" dari Padang Roco merupakan hadiah Kertanegara untuk kerajaan mana?", "id": "Kerajaan Dharmasraya (Melayu)." },
  { "en": "Siapa Dara Petak dan Dara Jingga yang dibawa dari Ekspedisi Pamalayu?", "id": "Putri Kerajaan Melayu." },
  { "en": "Apa nama pelabuhan utama Kerajaan Sunda menurut sumber Portugis?", "id": "Sunda Kelapa (Kalapa)." },
  { "en": "Siapa Nyi Ageng Serang, pejuang wanita yang membantu Pangeran Diponegoro?", "id": "Bangsawan, ahli strategi." },
  { "en": "Perang Padri tahap kedua (1821-1825) adalah perang antara siapa?", "id": "Kaum Padri melawan Kaum Adat." },
  { "en": "Apa \"Verplichte Leverantie\" yang diterapkan VOC selain tanam paksa?", "id": "Penyerahan wajib hasil bumi." },
  { "en": "Siapa Pieter Erberveld yang dihukum mati VOC karena dituduh memberontak?", "id": "Tokoh Indo-Jerman di Batavia." },
  { "en": "Benteng Fort Rotterdam di Makassar awalnya milik kerajaan apa?", "id": "Kerajaan Gowa (Benteng Ujung Pandang)." },
  { "en": "Siapa Herman Willem Daendels yang dijuluki \"Mas Galak\"?", "id": "Gubernur Jenderal Hindia Belanda." },
  { "en": "Apa \"Contingenten Stelsel\" yang diterapkan VOC?", "id": "Pajak wajib berupa hasil bumi." },
  { "en": "Siapa Thomas Dias, orang Portugis yang membantu Sultan Agung menyerang Batavia?", "id": "Ahli meriam (desersi VOC)." },
  { "en": "Cultuurprocenten dalam sistem Tanam Paksa diberikan kepada siapa?", "id": "Pejabat kolonial dan pribumi." },
  { "en": "Surat-surat Kartini kepada Stella Zeehandelaar diterbitkan dengan judul apa dalam bahasa Belanda?", "id": "Door Duisternis tot Licht." },
  { "en": "Sekolah HIS (Hollandsch-Inlandsche School) menggunakan bahasa pengantar apa?", "id": "Belanda (untuk pribumi)." },
  { "en": "Siapa Dr. Cipto Mangunkusumo yang dibuang karena tulisan kritisnya?", "id": "Tokoh Tiga Serangkai." },
  { "en": "Organisasi \"Roekoen Peladjar Indonesia\" (Roepi) bergerak di bidang apa?", "id": "Perkumpulan pelajar Indonesia." },
  { "en": "Siapa yang mengetuai Kongres Perempuan Indonesia pertama di Yogyakarta (1928)?", "id": "R.A. Soekonto." },
  { "en": "Apa isi utama \"Manifesto Politik 1925\" Perhimpunan Indonesia?", "id": "Penegasan cita-cita Indonesia Merdeka." },
  { "en": "Siapa Soekarni Kartodiwirjo, tokoh pemuda yang aktif dalam sekitar proklamasi?", "id": "Anggota golongan muda." },
  { "en": "Apa nama organisasi semi militer bentukan Jepang untuk pemudi?", "id": "Srikandi (bagian dari Fujinkai)." },
  { "en": "Siapa Kiai Haji Zainal Mustafa dari Tasikmalaya yang memberontak melawan Jepang?", "id": "Ulama pemimpin pesantren Sukamanah." },
  { "en": "Apa \"Romukyokai\" yang dibentuk Jepang?", "id": "Badan pengurus tenaga kerja (Romusha)." },
  { "en": "Siapa Wikana, tokoh pemuda yang ikut mendesak Soekarno-Hatta untuk proklamasi?", "id": "Anggota golongan muda." },
  { "en": "Peristiwa Hotel Yamato didahului oleh pengibaran bendera apa?", "id": "Bendera Belanda (Merah-Putih-Biru)." },
  { "en": "Siapa Residen Soedirman yang memimpin Pertempuran Lima Hari di Semarang?", "id": "Pimpinan KNI Semarang." },
  { "en": "Apa \"Palang Merah Indonesia\" (PMI) didirikan sebelum atau sesudah proklamasi?", "id": "Sesudah proklamasi (17 September 1945)." },
  { "en": "Siapa Mr. Tadjoedin Noor, tokoh Kalimantan Selatan di PPKI?", "id": "Wakil dari Kalimantan." },
  { "en": "Perjanjian Salatiga tahun 1757 membagi wilayah Mataram Islam menjadi apa saja?", "id": "Surakarta, Yogyakarta, Mangkunegaran." },
  { "en": "Siapa Raden Intan II, pahlawan dari Lampung yang melawan Belanda?", "id": "Bangsawan Lampung." },
  { "en": "Organisasi \"Nahdlatul Wathan\" di Lombok didirikan oleh siapa?", "id": "T.G.K.H. Muhammad Zainuddin Abdul Madjid." },
  { "en": "Siapa Sutan Syahrir yang menolak kebijakan diplomasi dengan Belanda via Linggarjati?", "id": "Tokoh sosialis, mantan perdana menteri." },
  { "en": "Agresi Militer Belanda II juga dikenal dengan nama sandi apa?", "id": "Operatie Kraai (Operasi Gagak)." },
  { "en": "Siapa Kolonel I Gusti Ngurah Rai yang memimpin Puputan Margarana?", "id": "Komandan Resimen Ciung Wanara." },
  { "en": "Apa isi \"Maklumat Politik No.1 Pemerintah RI\" tanggal 1 September 1945?", "id": "Pernyataan kedaulatan dan tujuan negara." },
  { "en": "Siapa Sumitro Djojohadikusumo yang pernah menjadi Dekan Fakultas Ekonomi UI?", "id": "Begawan ekonomi Indonesia." },
  { "en": "Kabinet Burhanuddin Harahap berhasil menyelenggarakan apa selain Pemilu 1955?", "id": "Pembubaran Uni Indonesia-Belanda." },
  { "en": "Siapa Arnold Mononutu, tokoh Nasionalis dari Sulawesi Utara?", "id": "Menteri Penerangan, Duta Besar." },
  { "en": "Apa itu \"Dewan Banteng\" yang dibentuk di Sumatra Tengah?", "id": "Cikal bakal PRRI." },
  { "en": "Siapa Ibnu Sutowo, tokoh yang lama memimpin Pertamina?", "id": "Dirut Pertamina era Orba." },
  { "en": "Apa \"Manipol\" yang menjadi acuan GBHN pada masa Demokrasi Terpimpin?", "id": "Manifesto Politik (pidato Soekarno)." },
  { "en": "Siapa Jenderal Ahmad Yani, korban G30S yang menjabat Menteri/Panglima Angkatan Darat?", "id": "Pahlawan Revolusi." },
  { "en": "Apa itu \"Tapol\" dan \"Napol\" pada masa Orde Baru?", "id": "Tahanan Politik dan Narapidana Politik." },
  { "en": "Siapa Ali Sadikin, Gubernur DKI Jakarta yang melakukan banyak gebrakan?", "id": "Tokoh militer, gubernur tegas." },
  { "en": "Program \"Panca Krida Kabinet Pembangunan\" dicanangkan oleh siapa?", "id": "Soeharto (awal Orde Baru)." },
  { "en": "Siapa Daoed Joesoef, Menteri Pendidikan yang menggagas NKK/BKK?", "id": "Menteri P&K era Orba." },
  { "en": "Apa itu \"Credietbank\" yang kemudian menjadi Bank Rakyat Indonesia (BRI)?", "id": "Bank kredit rakyat zaman Belanda." },
  { "en": "Siapa Dr. G.S.S.J. Ratulangi, pahlawan nasional dari Sulawesi Utara?", "id": "Gubernur Sulawesi pertama, tokoh BPUPKI." },
  { "en": "Taman Mini Indonesia Indah (TMII) digagas oleh siapa?", "id": "Ibu Tien Soeharto." },
  { "en": "Siapa Yap Thiam Hien, pengacara dan pejuang HAM terkenal?", "id": "Advokat berintegritas." },
  { "en": "Apa \"Piagam ASEAN\" (ASEAN Charter) yang diratifikasi Indonesia?", "id": "Konstitusi dasar ASEAN." },
  { "en": "Siapa Kuntowijoyo, sejarawan dan budayawan dari Yogyakarta?", "id": "Penulis, akademisi UGM." },
  { "en": "Apa itu \"Sertifikasi Guru\" yang bertujuan meningkatkan kualitas pendidik?", "id": "Proses pengakuan profesionalitas guru." },
  { "en": "Siapa Jimly Asshiddiqie, Ketua Mahkamah Konstitusi pertama?", "id": "Ahli hukum tata negara." },
  { "en": "Museum Fatahillah di Jakarta dulunya adalah gedung apa?", "id": "Stadhuis (Balai Kota Batavia)." },
  { "en": "Apa nama Prasasti Canggal yang menyebutkan pendirian lingga oleh Raja Sanjaya?", "id": "Prasasti dari Desa Canggal." },
  { "en": "Siapa Balaputradewa yang terusir dari Jawa dan menjadi raja Sriwijaya?", "id": "Pangeran Dinasti Syailendra." },
  { "en": "Kitab \"Arjunawijaya\" karya Mpu Tantular menceritakan tentang apa?", "id": "Kemenangan Arjuna Sasrabahu atas Rahwana." },
  { "en": "Apa sebutan untuk penguasa daerah bawahan Majapahit?", "id": "Bhre (misal Bhre Wirabhumi)." },
  { "en": "Siapa Syekh Siti Jenar, tokoh sufi kontroversial di Jawa?", "id": "Penyebar ajaran wahdatul wujud." },
  { "en": "\"Babad Dipanegara\" ditulis oleh Pangeran Diponegoro saat berada di mana?", "id": "Pengasingan (Manado/Makassar)." },
  { "en": "Siapa Stamford Raffles yang memerintahkan penyerbuan Keraton Yogyakarta tahun 1812?", "id": "Letnan Gubernur Inggris." },
  { "en": "\"Eindresume\" adalah laporan akhir penyelidikan apa pada masa kolonial?", "id": "Penyelidikan tentang hak tanah pribumi." },
  { "en": "Siapa Tirto Adhi Soerjo yang dianggap Bapak Pers Nasional?", "id": "Perintis jurnalisme modern Indonesia." },
  { "en": "Organisasi \"Persatuan Arab Indonesia\" (PAI) didirikan oleh siapa?", "id": "Abdurrahman Baswedan." },
  { "en": "Apa \"Petisi Thamrin\" yang diajukan di Volksraad?", "id": "Usulan penggunaan kata \"Indonesia\" resmi." },
  { "en": "Siapa Laksamana Muda Maeda yang menyediakan rumahnya untuk perumusan proklamasi?", "id": "Perwira AL Jepang simpatisan Indonesia." },
  { "en": "Peristiwa \"Pertempuran Bojongkokosan\" (Sukabumi) terjadi dalam konteks apa?", "id": "Perang kemerdekaan melawan Sekutu/NICA." },
  { "en": "Siapa Mr. Sartono, Ketua DPR pertama RIS dan Ketua PNI?", "id": "Ahli hukum, politisi." },
  { "en": "Kabinet Djuanda dikenal dengan program \"Panca Karya\", apa salah satunya?", "id": "Pembentukan Dewan Nasional." },
  { "en": "Siapa Frans Kaisiepo, pahlawan nasional dari Papua yang mengusulkan nama \"Irian\"?", "id": "Tokoh Papua pro-Republik." },
  { "en": "Manikebu (Manifesto Kebudayaan) ditentang oleh Lekra karena dianggap apa?", "id": "Humanisme universal, tidak revolusioner." },
  { "en": "Siapa Jenderal L.B. Moerdani, tokoh intelijen dan militer kuat Orde Baru?", "id": "Panglima ABRI, Menhankam." },
  { "en": "Program \"Kredit Usaha Tani\" (KUT) pada masa Orde Baru untuk apa?", "id": "Membantu modal petani." },
  { "en": "Siapa Wiji Thukul, penyair dan aktivis yang hilang era Orde Baru?", "id": "Simbol perlawanan seniman." },
  { "en": "Apa itu \"Komisi Kebenaran dan Rekonsiliasi\" (KKR) yang diusulkan pasca-Orba?", "id": "Upaya pengungkapan pelanggaran HAM masa lalu." },
  { "en": "Siapa H.S. Dillon, aktivis yang fokus pada isu HAM dan agraria?", "id": "Pejuang hak-hak petani." },
  { "en": "Prasasti Dinoyo (760 M) di Malang menyebutkan pendirian kuil untuk siapa?", "id": "Resi Agastya." },
  { "en": "Siapa Ranggalawe, adipati Tuban yang memberontak terhadap Raden Wijaya?", "id": "Salah satu pendukung awal Majapahit." },
  { "en": "Masjid Agung Banten dikenal dengan menaranya yang mirip apa?", "id": "Mercusuar." },
  { "en": "Siapa Kapitan Jonker, pemimpin masyarakat Mardijker di Batavia?", "id": "Perwira VOC keturunan Ambon." },
  { "en": "\"Buku Ende\" adalah kumpulan tulisan Soekarno tentang apa?", "id": "Pemikiran Islam dan kebangsaan." },
  { "en": "Siapa Mohammad Tabrani Soerjowitjitro, penggagas Kongres Pemuda pertama?", "id": "Jurnalis, tokoh pemuda." },
  { "en": "Apa nama sandi untuk pemberontakan PETA di Blitar?", "id": "Tidak ada nama sandi spesifik." },
  { "en": "Siapa Otto Iskandardinata (\"Si Jalak Harupat\"), pejuang kemerdekaan dari Jawa Barat?", "id": "Menteri Negara, tokoh Paguyuban Pasundan." },
  { "en": "Peristiwa Westerling di Makassar tahun 1947 adalah operasi apa?", "id": "Pembantaian oleh pasukan DST Belanda." },
  { "en": "Siapa Ida Anak Agung Gde Agung, Perdana Menteri Negara Indonesia Timur?", "id": "Tokoh federalis pro-RI." },
  { "en": "Sistem \"Demokrasi Desa\" pada masa Demokrasi Parlementer merujuk pada apa?", "id": "Pemilihan kepala desa langsung." },
  { "en": "Siapa Prof. Dr. Ir. Herman Johannes, Rektor UGM dan Menteri Pekerjaan Umum?", "id": "Ilmuwan, pejuang, negarawan." },
  { "en": "Gerakan \"Angkatan Pujangga Baru\" menekankan pada sastra yang bagaimana?", "id": "Modern, dinamis, berorientasi kebangsaan." },
  { "en": "Siapa Yap Kie Tiong (Haji Muhammad Ali), tokoh Tionghoa Muslim pendukung kemerdekaan?", "id": "Pengusaha, anggota BPUPKI." },
  { "en": "Program \"ABRI Masuk Desa\" (AMD) pada Orde Baru bertujuan apa?", "id": "Bakti sosial TNI, pembangunan desa." },
  { "en": "Siapa H.J.C. Princen (Poncke), pejuang HAM Belanda yang membela Indonesia?", "id": "Mantan tentara KNIL, aktivis LPHAM." },
  { "en": "Apa itu \"Gerakan Jumat Bersih\" yang populer di era Reformasi?", "id": "Aksi sosial kebersihan lingkungan." },
  { "en": "Candi Bajang Ratu di Trowulan berfungsi sebagai gerbang apa?", "id": "Gerbang masuk bangunan suci (Paduraksa)." },
  { "en": "Siapa Nala, panglima angkatan laut Majapahit yang terkenal?", "id": "Laksamana laut era Hayam Wuruk." },
  { "en": "Benteng Speelwijk di Banten dibangun oleh VOC untuk tujuan apa?", "id": "Mengawasi Kesultanan Banten." },
  { "en": "Siapa Pattiasina, tokoh dari Maluku yang menentang Perjanjian Renville?", "id": "Politisi, pejuang kemerdekaan." },
  { "en": "Apa \"Volkscredietwezen\" (Jawatan Kredit Rakyat) pada masa kolonial?", "id": "Lembaga pemberi kredit mikro." },
  { "en": "Siapa Douwes Dekker (Setiabudi) yang nama Belandanya berbeda dari Multatuli?", "id": "Ernest François Eugène Douwes Dekker." },
  { "en": "Organisasi \"Jong Islamieten Bond\" (JIB) diketuai oleh siapa?", "id": "Samsuridjal (salah satu ketua)." },
  { "en": "Lagu \"Sorak-Sorak Bergembira\" diciptakan oleh siapa?", "id": "Cornel Simanjuntak." },
  { "en": "Apa \"Heiho\", pasukan pembantu prajurit Jepang dari kalangan pribumi?", "id": "Tentara pembantu Jepang." },
  { "en": "Siapa Mr. Mohammad Roem, diplomat ulung Indonesia dalam banyak perundingan?", "id": "Delegasi RI di Roem-Royen, KMB." },
  { "en": "Peristiwa \"Bandung Lautan Api\" merupakan taktik bumi hangus terhadap siapa?", "id": "Pasukan Sekutu (Inggris)." },
  { "en": "Siapa Sri Paku Alam VIII, Wakil Gubernur DIY yang lama menjabat?", "id": "Bangsawan Pakualaman." },
  { "en": "Program \"Pembangunan Lima Tahun\" (Pelita) pertama dimulai tahun berapa?", "id": "Tahun 1969." },
  { "en": "Siapa Ali Alatas, Menteri Luar Negeri Indonesia yang lama menjabat?", "id": "Diplomat senior era Orba/Reformasi." },
  { "en": "Apa itu \"Komisi Ombudsman Nasional\" yang dibentuk era Gus Dur?", "id": "Lembaga pengawas pelayanan publik." },
  { "en": "Prasasti Tuk Mas di lereng Gunung Merbabu menggunakan aksara apa?", "id": "Pallawa." },
  { "en": "Siapa Kebo Iwa, patih Bali Aga yang melawan ekspansi Majapahit?", "id": "Tokoh legendaris Bali Kuno." },
  { "en": "Masjid Katangka di Gowa merupakan peninggalan Sultan siapa?", "id": "Sultan Alauddin (awal).." },
  { "en": "Siapa François Caron, Gubernur VOC di Formosa dan Ambon?", "id": "Pejabat tinggi VOC." },
  { "en": "Buku \"Student Hidjo\" karya Mas Marco Kartodikromo mengkritik apa?", "id": "Feodalisme Jawa, pendidikan kolonial." },
  { "en": "Siapa Abdul Muis, sastrawan dan tokoh Sarekat Islam?", "id": "Penulis \"Salah Asuhan\"." },
  { "en": "Apa \"Asrama Indonesia Merdeka\" di Menteng Raya 31?", "id": "Pusat kegiatan pemuda radikal." },
  { "en": "Siapa Sutan Takdir Alisjahbana, tokoh utama Pujangga Baru?", "id": "Sastrawan, budayawan, pemikir." },
  { "en": "Apa \"Piagam Persetujuan\" (Gentlemen's Agreement) antara Soekarno dan Hatta?", "id": "Pembagian peran kepemimpinan nasional." },
  { "en": "Siapa Kolonel Kawilarang yang memimpin penumpasan RMS di Maluku?", "id": "Komandan pasukan APRIS/TNI." },
  { "en": "Apa \"Deklarasi Bangkok\" tahun 1967 yang melahirkan ASEAN?", "id": "Piagam pendirian ASEAN." },
  { "en": "Siapa Mochtar Lubis, sastrawan pengkritik Orde Lama dan Orde Baru?", "id": "Penulis \"Jalan Tak Ada Ujung\"." },
  { "en": "Program \"Keluarga Berencana\" (KB) dicanangkan secara nasional tahun berapa?", "id": "Tahun 1970." },
  { "en": "Siapa Jenderal Benny Moerdani yang terkenal dengan operasi intelijennya?", "id": "Panglima ABRI, Kepala BAKIN." },
  { "en": "Apa itu \"Badan Pertimbangan Pendidikan Nasional\" (BPPN)?", "id": "Lembaga pemberi masukan kebijakan pendidikan." },
  { "en": "Prasasti Kalasan (778 M) menyebutkan pendirian candi untuk dewi siapa?", "id": "Dewi Tara." },
  { "en": "Siapa Tribhuwana Wijayatunggadewi, ratu Majapahit ibu Hayam Wuruk?", "id": "Penguasa wanita Majapahit." },
  { "en": "Benteng Victoria di Ambon dibangun oleh bangsa apa?", "id": "Portugis (Nossa Senhora da Anunciada)." },
  { "en": "Siapa Jan Coenraad Beynon, residen Yogyakarta yang dibunuh saat Geger Sepehi?", "id": "Residen Belanda di Yogyakarta." },
  { "en": "Apa itu \"Mindere Welvaart Commissie\" (Komisi Kemakmuran Rendah)?", "id": "Komisi penyelidik kemiskinan pribumi." },
  { "en": "Siapa Ki Sarmidi Mangunsarkoro, Menteri Pendidikan era Soekarno?", "id": "Tokoh Taman Siswa." },
  { "en": "Apa nama surat kabar pertama berbahasa Melayu di Hindia Belanda?", "id": "Selompret Melajoe." },
  { "en": "Siapa Mr. Kasman Singodimedjo, Jaksa Agung pertama RI?", "id": "Tokoh Muhammadiyah, Ketua KNIP." },
  { "en": "Apa \"Dokuritsu Junbi Inkai\" nama Jepang untuk PPKI?", "id": "Panitia Persiapan Kemerdekaan Indonesia." },
  { "en": "Siapa Letnan Komarudin yang berperan dalam Serangan Umum 1 Maret?", "id": "Komandan peleton TNI." },
  { "en": "Negara Indonesia Timur (NIT) beribukota di mana?", "id": "Makassar." },
  { "en": "Siapa Bahder Djohan, Menteri Pendidikan era Demokrasi Parlementer?", "id": "Tokoh Masyumi." },
  { "en": "Apa \"Doktrin Soekarno\" mengenai penyelesaian Irian Barat?", "id": "Irian Barat bagian mutlak RI." },
  { "en": "Siapa Letkol Untung Syamsuri, komandan Batalyon I Cakrabirawa?", "id": "Pemimpin lapangan G30S." },
  { "en": "Apa \"Pedoman Penghayatan dan Pengamalan Pancasila\" (P4)?", "id": "Program indoktrinasi Pancasila Orba." },
  { "en": "Siapa Prof. Widjojo Nitisastro, arsitek utama ekonomi Orde Baru?", "id": "Ketua Bappenas, Menko Ekuin." },
  { "en": "Peristiwa \"Priok Berdarah\" tahun 1984 dipicu oleh isu apa?", "id": "Penolakan asas tunggal Pancasila." },
  { "en": "Siapa Abdurrahman Wahid (Gus Dur) sebelum menjadi presiden?", "id": "Ketua Umum PBNU." },
  { "en": "Apa \"Candi Tikus\" di Trowulan diperkirakan berfungsi sebagai apa?", "id": "Petirtaan (pemandian suci)." },
  { "en": "Siapa Mpu Kanwa, pujangga yang menggubah Kakawin Arjunawiwaha?", "id": "Pujangga istana Raja Airlangga." },
  { "en": "Masjid Demak menggunakan tiang utama dari apa yang disebut \"saka tatal\"?", "id": "Potongan kayu (tatal)." },
  { "en": "Siapa Gustaaf Willem Baron van Imhoff, Gubernur Jenderal VOC pendiri Buitenzorg?", "id": "Pejabat tinggi VOC." },
  { "en": "Apa \"Batig Slot Stelsel\" (Sistem Saldo Untung) dalam keuangan kolonial?", "id": "Keuntungan bersih Hindia Belanda untuk Belanda." },
  { "en": "Siapa Dr. Ferdinand Lumbantobing, tokoh pejuang dari Tapanuli?", "id": "Dokter, politisi, menteri." },
  { "en": "Organisasi \"Sedyo Moeljo\" di Surakarta bergerak di bidang apa?", "id": "Pendidikan dan sosial wanita." },
  { "en": "Apa nama kantor berita Jepang pada masa pendudukan di Indonesia?", "id": "Domei." },
  { "en": "Siapa Latuharhary, tokoh Maluku yang memperjuangkan bentuk negara kesatuan?", "id": "Anggota BPUPKI/PPKI." },
  { "en": "Perundingan Kaliurang (1948) membahas implementasi perjanjian apa?", "id": "Perjanjian Renville." },
  { "en": "Siapa Sjafruddin Prawiranegara yang memimpin PDRI di Sumatra?", "id": "Menteri Kemakmuran, Gubernur BI." },
  { "en": "Apa \"Gerakan Assaat\" yang menuntut pembubaran RIS?", "id": "Dukungan untuk kembali ke NKRI." },
  { "en": "Siapa Roosseno Soerjohadikoesoemo, ahli konstruksi beton Indonesia?", "id": "Bapak Beton Indonesia." },
  { "en": "Apa itu \"Komando Jihad\" (Komji), kelompok Islam radikal era Orba?", "id": "Kelompok teroris." },
  { "en": "Siapa Jenderal Poniman, Panglima ABRI pada awal 1980-an?", "id": "Tokoh militer Orde Baru." },
  { "en": "Apa \"Undang-Undang Penanaman Modal Asing\" (UU PMA) tahun 1967?", "id": "Membuka investasi asing di Indonesia." },
  { "en": "Siapa Soedjatmoko, cendekiawan dan mantan Rektor Universitas PBB?", "id": "Pemikir sosial, diplomat." },
  { "en": "Apa \"Prasasti Gondosuli\" yang ditemukan di Temanggung, Jawa Tengah?", "id": "Prasasti beraksara Dewanagari." },
  { "en": "Siapa Dang Hyang Nirartha, pendeta Hindu yang menyebarkan agama di Bali dan Lombok?", "id": "Pedanda Sakti Wawu Rauh." },
  { "en": "Masjid Agung Palembang (Masjid Sultan Mahmud Badaruddin I) dibangun abad ke berapa?", "id": "Abad ke-18." },
  { "en": "Siapa Anthonie van Diemen, Gubernur Jenderal VOC yang mengirim ekspedisi ke Australia?", "id": "Pemimpin VOC." },
  { "en": "Apa \"Afdeeling B\" (Bagian B) dalam dinas intelijen kolonial?", "id": "Urusan politik dan keamanan dalam negeri." },
  { "en": "Siapa Dr. Johannes Latuihamallo, tokoh pendidikan dan pendeta dari Maluku?", "id": "Rektor UKIM, politisi Parkindo." },
  { "en": "Organisasi \"Persatuan Pemuda Kristen\" (PPK) didirikan tahun berapa?", "id": "Tahun 1945 (cikal bakal GMKI)." },
  { "en": "Apa nama surat kabar mingguan yang dipimpin Soekarno tahun 1930-an?", "id": "Fikiran Ra'jat." },
  { "en": "Siapa Yusuf Muda Dalam, Gubernur Bank Indonesia era Soekarno yang kontroversial?", "id": "Menteri Urusan Bank Sentral." },
  { "en": "Apa \"Operasi Jayawijaya\" dalam konteks Trikora?", "id": "Operasi militer perebutan Irian Barat." },
  { "en": "Siapa Subandrio, Menteri Luar Negeri era Demokrasi Terpimpin?", "id": "Wakil Perdana Menteri I." },
  { "en": "Apa \"Gerakan Mahasiswa Menggugat\" (GMM) yang muncul pasca Malari?", "id": "Kelompok mahasiswa kritis." },
  { "en": "Siapa Radius Prawiro, Menteri Keuangan yang lama menjabat era Orde Baru?", "id": "Ekonom, teknokrat." },
  { "en": "Apa \"Paket Kebijakan Oktober 1988\" (Pakto 88) di bidang perbankan?", "id": "Deregulasi sektor perbankan." },
  { "en": "Siapa Taufiq Kiemas, suami Megawati yang juga politisi senior PDIP?", "id": "Ketua MPR RI." },
  { "en": "Prasasti Siwagrha (856 M) menceritakan pembangunan candi apa?", "id": "Candi Prambanan (Siwagrha)." },
  { "en": "Siapa Arya Damar, adipati Palembang yang menaklukkan Bali atas perintah Majapahit?", "id": "Bangsawan Majapahit." },
  { "en": "Makam Sunan Giri di Gresik memiliki arsitektur gapura yang khas, disebut apa?", "id": "Gapura Bentar dan Paduraksa." },
  { "en": "Siapa Jacques Specx, Gubernur Jenderal VOC yang memindahkan pusat VOC ke Batavia?", "id": "Pejabat VOC (Coen yang memindahkan)." },
  { "en": "Apa \"Cultuurprocenten\" yang diterima pejabat pribumi dalam Tanam Paksa?", "id": "Persentase keuntungan dari hasil tanaman." },
  { "en": "Siapa H.J. van Mook, Letnan Gubernur Jenderal Hindia Belanda pasca-PD II?", "id": "Arsitek negara federal." },
  { "en": "Organisasi \"Gerakan Rakjat Indonesia\" (Gerindo) didirikan oleh siapa?", "id": "Amir Sjarifuddin, A.K. Gani." },
  { "en": "Apa isi \"Ikrar Pemuda\" yang diucapkan dalam Kongres Pemuda I?", "id": "Keinginan persatuan pemuda Indonesia." },
  { "en": "Siapa Jenderal Urip Sumohardjo, Kepala Staf Umum TKR pertama?", "id": "Tokoh militer senior." },
  { "en": "Apa nama operasi pendaratan pasukan Sekutu di Surabaya tahun 1945?", "id": "Operasi Mallaby (bagian dari AFNEI)." },
  { "en": "Siapa Dr. Sudarsono, Perdana Menteri RI pada masa Revolusi?", "id": "Tokoh PSI (Partai Sosialis Indonesia)." },
  { "en": "Apa \"Konferensi Denpasar\" tahun 1946 yang membentuk Negara Indonesia Timur?", "id": "Inisiatif Belanda membentuk negara federal." },
  { "en": "Siapa Natsir, tokoh Masyumi yang menyampaikan Mosi Integral?", "id": "Perdana Menteri, ulama." },
  { "en": "Apa \"Badan Pekerja Ajudan Jenderal\" (BPAJ) dalam struktur militer Orla?", "id": "Staf khusus Presiden Soekarno." },
  { "en": "Siapa Jenderal Soemitro, Pangkopkamtib yang menangani Peristiwa Malari?", "id": "Tokoh militer Orde Baru." },
  { "en": "Apa \"Repelita\" (Rencana Pembangunan Lima Tahun) VI fokus pada apa?", "id": "Pembangunan SDM dan teknologi." },
  { "en": "Siapa Goenawan Mohamad, pendiri majalah Tempo dan budayawan?", "id": "Sastrawan, esais." },
  { "en": "Apa \"UU Sistem Pendidikan Nasional\" No. 20 Tahun 2003 menggantikan UU mana?", "id": "UU No. 2 Tahun 1989." },
  { "en": "Arca Buddha Wisnu di Cibuaya, Karawang, menunjukkan pengaruh seni apa?", "id": "Seni Gupta dan Pallawa (India)." },
  { "en": "Siapa Menak Jingga, tokoh legendaris penguasa Blambangan yang melawan Majapahit?", "id": "Figur dalam cerita Damarwulan." },
  { "en": "Pintu gerbang makam Sunan Sendang Duwur di Lamongan memiliki hiasan apa?", "id": "Sayap mirip gapura candi." },
  { "en": "Siapa Laurens Reael, Gubernur Jenderal VOC yang juga seorang penyair?", "id": "Pejabat VOC berlatar humanis." },
  { "en": "Apa \"Landelijk Inkomsten Belasting Ordonnantie\" (LIBO) pada masa kolonial?", "id": "Ordonansi pajak pendapatan pedesaan." },
  { "en": "Siapa Sam Ratulangi yang terkenal dengan falsafah \"Si Tou Timou Tumou Tou\"?", "id": "Pahlawan Nasional dari Minahasa." },
  { "en": "Organisasi \"Katholieke Sociale Bond\" (KSB) bergerak di kalangan apa?", "id": "Buruh dan masyarakat Katolik." },
  { "en": "Apa isi \"Janji Koiso\" yang disampaikan Perdana Menteri Jepang?", "id": "Kemerdekaan Indonesia di kemudian hari." },
  { "en": "Siapa Buntaran Martoatmodjo, Menteri Kesehatan pertama RI?", "id": "Dokter, tokoh pergerakan." },
  { "en": "Apa \"Resolusi PBB 6 Januari 1949\" terkait Agresi Militer Belanda II?", "id": "Tuntutan pembebasan pemimpin RI." },
  { "en": "Siapa Dr. Halim, Perdana Menteri RIS dari kabinet apa?", "id": "Kabinet Peralihan (setelah Hatta)." },
  { "en": "Apa \"Program Ekonomi Gerakan Benteng\" yang dicanangkan Kabinet Natsir?", "id": "Melindungi pengusaha pribumi." },
  { "en": "Siapa Letjen Ahmad Yunus Mokoginta, tokoh militer dari Sulawesi Utara?", "id": "Panglima, duta besar." },
  { "en": "Apa \"Aksi Tritura\" yang dilakukan mahasiswa pada tahun 1966?", "id": "Demonstrasi menuntut tiga tuntutan rakyat." },
  { "en": "Siapa Emil Salim, Menteri Negara Pengawasan Pembangunan dan Lingkungan Hidup?", "id": "Ekonom, pelopor isu lingkungan." },
  { "en": "Program \"Jaring Pengaman Sosial\" (JPS) diluncurkan untuk mengatasi dampak apa?", "id": "Krisis moneter 1998." },
  { "en": "Siapa Kwik Kian Gie, Menteri Koordinator Ekonomi era Megawati?", "id": "Ekonom, politisi PDIP." },
  { "en": "Prasasti Mantyasih (Prasasti Balitung) menyebutkan daftar raja Mataram Kuno siapa saja?", "id": "Raja-raja Sanjaya sebelum Balitung." },
  { "en": "Siapa Gajah Enggon, patih Majapahit setelah Gajah Mada wafat?", "id": "Pengganti Gajah Mada." },
  { "en": "Masjid Agung Kasepuhan Cirebon memiliki \"Saka Guru\" dari kayu apa?", "id": "Kayu Jati." },
  { "en": "Siapa Hendrick Brouwer, Gubernur Jenderal VOC yang menetapkan Batavia sebagai pusat VOC?", "id": "Pejabat VOC (Coen lebih awal)." },
  { "en": "Apa \"Domein Verklaring\" dalam UU Agraria 1870?", "id": "Pernyataan tanah tak bertuan milik negara." },
  { "en": "Siapa Maria Ulfah Santoso, menteri wanita pertama Indonesia?", "id": "Menteri Sosial Kabinet Sjahrir II." },
  { "en": "Organisasi \"Muhammadiyah\" didirikan di kota mana?", "id": "Yogyakarta." },
  { "en": "Apa isi \"Sumpah Prajurit PETA\"?", "id": "Setia pada tanah air, bela negara." },
  { "en": "Siapa A.K. Pringgodigdo, Sekretaris Negara pertama RI?", "id": "Ahli hukum, birokrat." },
  { "en": "Apa nama lain Perjanjian Roem-Royen?", "id": "Roem-van Roijen Statement." },
  { "en": "Siapa Teuku Daud Beureu'eh yang memimpin DI/TII di Aceh?", "id": "Ulama, mantan Gubernur Militer Aceh." },
  { "en": "Apa \"Deklarasi Ekonomi\" (Dekon) tahun 1963 bertujuan mengatasi apa?", "id": "Inflasi dan kesulitan ekonomi." },
  { "en": "Siapa Syarif Thayeb, Menteri Pendidikan dan Kebudayaan era Orde Baru?", "id": "Tokoh militer, rektor UI." },
  { "en": "Program \"Pola Ilmiah Pokok\" (PIP) di perguruan tinggi dicanangkan kapan?", "id": "Akhir 1970-an/awal 1980-an." },
  { "en": "Siapa Jenderal Feisal Tanjung, Panglima ABRI pada masa akhir Orde Baru?", "id": "Tokoh militer." },
  { "en": "Apa \"Komisi Pemilihan Umum\" (KPU) independen pertama dibentuk tahun berapa?", "id": "Tahun 1999 (setelah Reformasi)." },
  { "en": "Prasasti Telaga Batu di Palembang berisi tentang apa?", "id": "Kutukan bagi pejabat tidak setia." },
  { "en": "Siapa Ken Dedes, permaisuri Ken Arok yang menjadi ibu raja-raja Jawa?", "id": "Putri Mpu Purwa." },
  { "en": "Masjid Agung Surakarta (Masjid Ageng Keraton Surakarta) dibangun oleh siapa?", "id": "Pakubuwana III." },
  { "en": "Siapa Rijklof van Goens, Gubernur Jenderal VOC yang menaklukkan Mataram?", "id": "Pejabat tinggi VOC." },
  { "en": "Apa \"Passerstelsel\" yang diterapkan untuk mengontrol pergerakan orang Tionghoa?", "id": "Sistem surat jalan (pas)." },
  { "en": "Siapa Soetan Daoem, tokoh pergerakan dari Minangkabau yang mendirikan sekolah INS Kayutanam?", "id": "Engku Mohammad Sjafei." },
  { "en": "Organisasi \"Sarekat Ambon\" didirikan oleh siapa?", "id": "Alexander Jacob Patty." },
  { "en": "Apa \"Gerakan Tiga A\" yang dipropagandakan Jepang?", "id": "Nippon Pemimpin, Pelindung, Cahaya Asia." },
  { "en": "Siapa Ki Bagus Hadikusumo, tokoh Muhammadiyah di BPUPKI?", "id": "Ulama, pejuang kemerdekaan." },
  { "en": "Apa \"Konferensi Pamuntjak\" (1949) yang membahas penyatuan sikap menghadapi Belanda?", "id": "Pertemuan tokoh-tokoh Sumatera." },
  { "en": "Siapa Sultan Syarif Kasim II dari Siak yang mendukung RI?", "id": "Menyumbang harta untuk perjuangan." },
  { "en": "Apa \"Dewan Revolusi\" yang diumumkan G30S?", "id": "Badan pengambil alih kekuasaan (gagal)." },
  { "en": "Siapa Ali Moertopo, tokoh intelijen Orde Baru yang memimpin Opsus?", "id": "Jenderal, politisi Golkar." },
  { "en": "Program \"Wajib Belajar 9 Tahun\" dicanangkan pada masa siapa?", "id": "Soeharto (implementasi bertahap)." },
  { "en": "Siapa Arief Budiman (Soe Hok Djin), sosiolog dan aktivis kritis?", "id": "Kakak Soe Hok Gie." },
  { "en": "Apa \"UU Anti-Terorisme\" pertama di Indonesia disahkan tahun berapa?", "id": "Tahun 2003 (Perppu 2002)." },
  { "en": "Prasasti Sojomerto dari Batang, Jawa Tengah, diduga terkait dinasti apa?", "id": "Dinasti Sailendra (awal)." },
  { "en": "Siapa Lembu Tal, adipati Madura yang membantu Raden Wijaya?", "id": "Tokoh dari Sumenep." },
  { "en": "Masjid Raya Medan (Masjid Al Mashun) dirancang oleh arsitek Belanda siapa?", "id": "Theodoor van Erp (J.A. Tingdeman)." },
  { "en": "Siapa Joan Maetsuycker, Gubernur Jenderal VOC dengan masa jabatan terlama?", "id": "Pemimpin VOC." },
  { "en": "Apa \"Wijkenstelsel\" yang diterapkan untuk pemukiman etnis Tionghoa?", "id": "Sistem kampung/distrik terpisah." },
  { "en": "Siapa Douwes Dekker (Multatuli) yang nama lengkapnya Eduard Douwes Dekker?", "id": "Penulis Max Havelaar." },
  { "en": "Organisasi \"Persatuan Wanita Republik Indonesia\" (Perwari) didirikan kapan?", "id": "Desember 1945." },
  { "en": "Apa \"Romusha\", kerja paksa pada masa pendudukan Jepang?", "id": "Tenaga kerja paksa." },
  { "en": "Siapa Adam Malik, salah satu pendiri kantor berita ANTARA?", "id": "Wakil Presiden, Menlu." },
  { "en": "Apa \"Konferensi Malino\" (1946) yang membentuk cikal bakal Negara Indonesia Timur?", "id": "Inisiatif Van Mook." },
  { "en": "Siapa Lambertus Nicodemus Palar, diplomat RI di PBB?", "id": "Pejuang diplomasi kemerdekaan." },
  { "en": "Apa \"Kabinet Karya\" yang dipimpin Djuanda Kartawidjaja?", "id": "Zaken kabinet non-partai." },
  { "en": "Siapa Jenderal Yoga Soegama, Kepala BAKIN yang lama menjabat?", "id": "Tokoh intelijen Orde Baru." },
  { "en": "Program \"Indonesia Sehat 2010\" dicanangkan pada masa siapa?", "id": "Megawati Soekarnoputri (visi)." },
  { "en": "Siapa Bondan Gunawan, aktivis dan politisi era Reformasi?", "id": "Mantan Sekneg era Gus Dur." },
  { "en": "Apa \"Prasasti Wurare\" yang merupakan perwujudan Kertanegara sebagai Buddha-Bhairawa?", "id": "Arca Joko Dolog." },
  { "en": "Siapa Mpu Prapanca yang menyamar saat menulis Nagarakretagama?", "id": "Dang Acarya Nadendra." },
  { "en": "Masjid Agung Yogyakarta (Masjid Gedhe Kauman) dibangun pada masa Sultan siapa?", "id": "Sri Sultan Hamengkubuwono I." },
  { "en": "Siapa Pieter Both, Gubernur Jenderal VOC pertama yang berkedudukan di Ambon?", "id": "Pejabat tinggi VOC." },
  { "en": "Apa \"Preangerstelsel\", sistem tanam paksa kopi di Priangan?", "id": "Wajib tanam kopi untuk VOC." },
  { "en": "Siapa Tjilik Riwut, pahlawan nasional dari Kalimantan Tengah?", "id": "Gubernur Kalteng, tokoh Dayak." },
  { "en": "Organisasi \"Partai Indonesia Raya\" (Parindra) merupakan fusi dari organisasi apa saja?", "id": "Budi Utomo, PBI, dll." },
  { "en": "Apa \"Jugun Ianfu\", wanita penghibur paksa tentara Jepang?", "id": "Korban kejahatan perang Jepang." },
  { "en": "Siapa Ahmad Soebardjo Djojoadisoerjo, Menteri Luar Negeri pertama RI?", "id": "Tokoh perumus proklamasi." },
  { "en": "Apa \"Komisi Jasa Baik\" (Good Offices Committee) nama lain KTN?", "id": "Komisi Tiga Negara." },
  { "en": "Siapa Dr. Ferdinand Lumban Tobing, Residen Tapanuli yang mendukung RI?", "id": "Tokoh Batak pejuang kemerdekaan." },
  { "en": "Apa \"Mosi Integral Natsir\" yang menyatukan kembali RIS ke NKRI?", "id": "Usulan Mohammad Natsir di DPR RIS." },
  { "en": "Siapa Mayjen Soeharto yang memimpin penumpasan G30S?", "id": "Pangkostrad saat itu." },
  { "en": "Apa \"Surat Keputusan Presiden Nomor 11 Tahun 1966\"?", "id": "Supersemar (dianggap sebagai dasar hukum)." },
  { "en": "Siapa Ali Wardhana, Menteri Keuangan penting era Orde Baru?", "id": "Anggota tim ekonomi Berkeley Mafia." },
  { "en": "Apa \"UU Pokok Pers No. 21 Tahun 1982\" yang mengontrol pers Orba?", "id": "Landasan hukum pembredelan pers." },
  { "en": "Siapa Arifin Panigoro, pengusaha dan politisi era Reformasi?", "id": "Pendiri Medco Group." },
  { "en": "Prasasti Kota Kapur (686 M) di Pulau Bangka berisi tentang apa?", "id": "Ancaman bagi pembangkang Sriwijaya." },
  { "en": "Siapa Mpu Bharada, pendeta yang membagi Kerajaan Kahuripan menjadi dua?", "id": "Brahmana sakti era Airlangga." },
  { "en": "Masjid Menara Kudus dibangun oleh siapa menurut tradisi?", "id": "Sunan Kudus (Ja'far Shadiq)." },
  { "en": "Siapa Antonio van Diemen, Gubernur Jenderal VOC yang mengirim Abel Tasman?", "id": "Pejabat VOC." },
  { "en": "Apa \"Plakaat Panjang\" (Lange Verklaring) yang mengikat raja-raja kecil?", "id": "Pernyataan tunduk pada Belanda." },
  { "en": "Siapa Dr. G. Moedjanto, sejarawan yang fokus pada sejarah Gereja Katolik?", "id": "Ahli sejarah gereja." },
  { "en": "Organisasi \"Indische Sociaal-Democratische Vereeniging\" (ISDV) didirikan oleh siapa?", "id": "Henk Sneevliet." },
  { "en": "Apa \"Sendenbu\", departemen propaganda pada masa pendudukan Jepang?", "id": "Badan propaganda Jepang." },
  { "en": "Siapa Sayuti Melik yang mengetik naskah Proklamasi Kemerdekaan?", "id": "Tokoh pergerakan, suami S.K. Trimurti." },
  { "en": "Apa \"Konferensi Meja Bundar\" (KMB) menghasilkan bentuk negara apa?", "id": "Republik Indonesia Serikat (RIS)." },
  { "en": "Siapa Mr. Wilopo, Perdana Menteri yang kabinetnya jatuh karena Peristiwa Tanjung Morawa?", "id": "Tokoh PNI." },
  { "en": "Apa \"Panca Azimat Revolusi\" yang sering diucapkan Soekarno?", "id": "Lima pegangan revolusi." },
  { "en": "Siapa Jenderal Abdul Haris Nasution, korban selamat G30S?", "id": "Menko Hankam/KSAB." },
  { "en": "Apa \"Dwi Fungsi ABRI\" yang menjadi doktrin Orde Baru?", "id": "Peran militer dalam hankam dan sospol." },
  { "en": "Siapa B.J. Habibie yang menggantikan Soeharto sebagai Presiden?", "id": "Wakil Presiden saat itu." },
  { "en": "Apa \"UU Otonomi Daerah\" No. 22 Tahun 1999 memberikan kewenangan apa?", "id": "Kewenangan luas ke daerah." },
  { "en": "Prasasti Kelurak (782 M) menyebutkan pendirian arca Manjusri oleh siapa?", "id": "Raja Indra (Dharanindra)." },
  { "en": "Siapa Raden Patah, pendiri Kesultanan Demak yang bergelar Senapati Jimbun?", "id": "Putra Brawijaya V Majapahit." },
  { "en": "Masjid Raya Baiturrahman di Aceh dibangun kembali oleh Belanda setelah apa?", "id": "Terbakar dalam Perang Aceh." },
  { "en": "Siapa Cornelis de Houtman, pemimpin ekspedisi Belanda pertama ke Nusantara?", "id": "Navigator Belanda." },
  { "en": "Apa \"Ethische Politiek\" (Politik Etis) yang bertujuan memajukan pribumi?", "id": "Kebijakan balas budi Belanda." },
  { "en": "Siapa Ernest Douwes Dekker (Danudirja Setiabudi), salah satu Tiga Serangkai?", "id": "Indo-Eropa pejuang kemerdekaan." },
  { "en": "Organisasi \"Pemuda Sosialis Indonesia\" (Pesindo) berafiliasi dengan partai apa?", "id": "Partai Sosialis Indonesia (PSI)." },
  { "en": "Apa \"Saudara\", surat kabar propaganda Jepang untuk pribumi?", "id": "Media propaganda Jepang." },
  { "en": "Siapa S.K. Trimurti, wartawan wanita dan Menteri Perburuhan pertama?", "id": "Pejuang wanita, istri Sayuti Melik." },
  { "en": "Apa \"Perjanjian New York\" (1962) yang menyerahkan Irian Barat ke UNTEA?", "id": "Kesepakatan RI-Belanda soal Irian Barat." },
  { "en": "Siapa Burhanuddin Harahap, Perdana Menteri yang menyelenggarakan Pemilu 1955?", "id": "Tokoh Masyumi." },
  { "en": "Apa \"Trikora\" (Tri Komando Rakyat) yang diumumkan Soekarno?", "id": "Komando pembebasan Irian Barat." },
  { "en": "Siapa Jenderal Soeharto yang menerima Supersemar dari Presiden Soekarno?", "id": "Pangkopkamtib saat itu." },
  { "en": "Apa \"Badan Perencanaan Pembangunan Nasional\" (Bappenas) bertugas apa?", "id": "Menyusun rencana pembangunan nasional." },
  { "en": "Siapa Emil Salim, ekonom yang dikenal sebagai Bapak Lingkungan Hidup Indonesia?", "id": "Menteri Lingkungan Hidup terlama." },
  { "en": "Apa \"Reformasi Birokrasi\" yang dicanangkan pasca Orde Baru?", "id": "Perbaikan tata kelola pemerintahan." },
  { "en": "Prasasti Ciaruteun di Bogor peninggalan kerajaan apa?", "id": "Kerajaan Tarumanegara." },
  { "en": "Siapa Gajah Mada yang mengucapkan Sumpah Palapa untuk menyatukan Nusantara?", "id": "Mahapatih Kerajaan Majapahit." },
  { "en": "Masjid Agung Demak memiliki atap berbentuk apa?", "id": "Tumpang tiga (limas bersusun)." },
  { "en": "Siapa Jan Pieterszoon Coen, Gubernur Jenderal VOC yang menghancurkan Jayakarta?", "id": "Pendiri Batavia." },
  { "en": "Apa \"Cultuurstelsel\" (Sistem Tanam Paksa) yang menyengsarakan rakyat?", "id": "Kewajiban menanam komoditas ekspor." },
  { "en": "Siapa Ki Hadjar Dewantara, pendiri Taman Siswa dan Bapak Pendidikan Nasional?", "id": "Suwardi Suryaningrat." },
  { "en": "Organisasi \"Nahdlatul Ulama\" (NU) didirikan oleh siapa?", "id": "K.H. Hasyim Asy'ari." },
  { "en": "Apa \"BPUPKI\" (Badan Penyelidik Usaha-usaha Persiapan Kemerdekaan Indonesia)?", "id": "Badan bentukan Jepang untuk kemerdekaan." },
  { "en": "Siapa Fatmawati, istri Soekarno yang menjahit Bendera Pusaka?", "id": "Ibu Negara pertama RI." },
  { "en": "Apa \"Agresi Militer Belanda I\" yang melanggar Perjanjian Linggarjati?", "id": "Serangan Belanda ke wilayah RI." },
  { "en": "Siapa Mohammad Hatta, Wakil Presiden pertama RI yang juga Bapak Koperasi?", "id": "Proklamator, negarawan." },
  { "en": "Apa \"Dekrit Presiden 5 Juli 1959\" yang membubarkan Konstituante?", "id": "Kembali ke UUD 1945." },
  { "en": "Siapa D.N. Aidit, pemimpin PKI pada masa Demokrasi Terpimpin?", "id": "Ketua Central Comite PKI." },
  { "en": "Apa \"Supersemar\" (Surat Perintah Sebelas Maret) yang kontroversial?", "id": "Surat perintah dari Soekarno ke Soeharto." },
  { "en": "Siapa Ali Sadikin, Gubernur DKI Jakarta yang membangun Taman Ismail Marzuki?", "id": "Gubernur legendaris Jakarta." },
  { "en": "Apa \"Krisis Moneter\" tahun 1998 yang memicu jatuhnya Orde Baru?", "id": "Krisis ekonomi Asia." },
  { "en": "Siapa B.J. Habibie, Presiden RI ketiga yang membawa transisi demokrasi?", "id": "Wakil Presiden Soeharto." },
  { "en": "Prasasti Kedukan Bukit (683 M) menceritakan perjalanan Dapunta Hyang menggunakan apa?", "id": "Perahu." },
  { "en": "Siapa Hayam Wuruk, raja Majapahit yang membawa kerajaan ke puncak kejayaan?", "id": "Rajasanagara." },
  { "en": "Masjid Raya Baiturrahman di Aceh menjadi simbol apa bagi rakyat Aceh?", "id": "Perjuangan dan identitas Islam." },
  { "en": "Siapa Cornelis de Houtman yang kapalnya mendarat di Banten tahun 1596?", "id": "Pemimpin ekspedisi Belanda pertama." },
  { "en": "Apa \"Hongi Tochten\", pelayaran patroli VOC untuk memusnahkan tanaman rempah ilegal?", "id": "Pelayaran pemusnah cengkih pala." },
  { "en": "Siapa Pangeran Diponegoro yang memimpin Perang Jawa melawan Belanda?", "id": "Pahlawan Nasional dari Yogyakarta." },
  { "en": "Organisasi \"Boedi Oetomo\" didirikan oleh mahasiswa sekolah apa?", "id": "STOVIA (Sekolah Dokter Jawa)." },
  { "en": "Apa \"Sumpah Pemuda\" yang diikrarkan pada Kongres Pemuda II?", "id": "Ikrar persatuan bangsa Indonesia." },
  { "en": "Siapa Soekarno, Presiden pertama RI yang juga Proklamator Kemerdekaan?", "id": "Putra Sang Fajar." },
  { "en": "Apa \"PPKI\" (Panitia Persiapan Kemerdekaan Indonesia) yang menggantikan BPUPKI?", "id": "Panitia pengesah UUD dan Presiden." },
  { "en": "Siapa Jenderal Soedirman, Panglima Besar TNI pertama yang memimpin gerilya?", "id": "Tokoh sentral perang kemerdekaan." },
  { "en": "Apa \"Konferensi Meja Bundar\" (KMB) yang menghasilkan pengakuan kedaulatan RI?", "id": "Perundingan RI-Belanda di Den Haag." },
  { "en": "Siapa Mohammad Natsir, Perdana Menteri pertama NKRI setelah RIS bubar?", "id": "Tokoh Masyumi, ulama." },
  { "en": "Apa \"Dwikora\" (Dwi Komando Rakyat) yang diumumkan Soekarno untuk konfrontasi Malaysia?", "id": "Komando ganyang Malaysia." },
  { "en": "Siapa Jenderal Soeharto, Presiden kedua RI yang memimpin Orde Baru?", "id": "The Smiling General." },
  { "en": "Apa \"Trilogi Pembangunan\" yang menjadi landasan kebijakan Orde Baru?", "id": "Stabilitas, pertumbuhan, pemerataan." },
  { "en": "Siapa Abdurrahman Wahid (Gus Dur), Presiden RI keempat dari kalangan ulama NU?", "id": "Bapak Pluralisme Indonesia." },
  { "en": "Prasasti Talang Tuo (684 M) berisi tentang pembuatan taman apa?", "id": "Taman Sriksetra untuk kemakmuran." },
  { "en": "Siapa Kertanegara, raja Singasari yang mengirim Ekspedisi Pamalayu?", "id": "Raja terakhir Singasari." },
  { "en": "Masjid Agung Sang Cipta Rasa di Cirebon dibangun oleh siapa?", "id": "Walisongo atas prakarsa Sunan Gunung Jati." },
  { "en": "Siapa Alfonso de Albuquerque, panglima Portugis yang menaklukkan Malaka?", "id": "Arsitek imperium Portugis di Asia." },
  { "en": "Apa \"Verplichte Leveranties\" (Penyerahan Wajib) yang diterapkan VOC?", "id": "Kewajiban menyerahkan hasil bumi ke VOC." },
  { "en": "Siapa Tuanku Imam Bonjol, pemimpin Kaum Padri dalam Perang Padri?", "id": "Pahlawan Nasional dari Sumatra Barat." },
  { "en": "Organisasi \"Sarekat Islam\" (SI) awalnya bernama apa?", "id": "Sarekat Dagang Islam (SDI)." },
  { "en": "Apa \"Indonesia Menggugat\", pidato pembelaan Soekarno di pengadilan Belanda?", "id": "Pledoi politik Soekarno." },
  { "en": "Siapa Mohammad Yamin, tokoh perumus dasar negara dan Sumpah Pemuda?", "id": "Sastrawan, sejarawan, politisi." },
  { "en": "Apa \"Peristiwa Rengasdengklok\" yang mendahului Proklamasi Kemerdekaan?", "id": "Penculikan Soekarno-Hatta oleh pemuda." },
  { "en": "Siapa Sutan Sjahrir, Perdana Menteri pertama RI yang memimpin diplomasi?", "id": "Tokoh sosialis, intelektual." },
  { "en": "Apa \"Perjanjian Renville\" yang merugikan wilayah kedaulatan RI?", "id": "Perundingan di atas kapal USS Renville." },
  { "en": "Siapa Sjafruddin Prawiranegara, Ketua PDRI dan Gubernur BI pertama?", "id": "Pemimpin Pemerintahan Darurat RI." },
  { "en": "Apa \"Pemilu 1955\", pemilihan umum pertama Indonesia yang demokratis?", "id": "Memilih DPR dan Konstituante." },
  { "en": "Siapa Letkol Untung, pemimpin Gerakan 30 September 1965?", "id": "Komandan Batalyon I Cakrabirawa." },
  { "en": "Apa \"Repelita\" (Rencana Pembangunan Lima Tahun) program ekonomi Orde Baru?", "id": "Tahapan pembangunan ekonomi nasional." },
  { "en": "Siapa Megawati Soekarnoputri, Presiden RI kelima dan putri Soekarno?", "id": "Presiden wanita pertama Indonesia." },
  { "en": "Prasasti Karang Berahi (686 M) di Jambi juga berisi tentang apa?", "id": "Kutukan bagi pembangkang Sriwijaya." },
  { "en": "Siapa Raden Wijaya, pendiri Kerajaan Majapahit setelah mengalahkan Jayakatwang?", "id": "Menantu Kertanegara." },
  { "en": "Masjid Kudus memiliki menara yang menyerupai bangunan apa?", "id": "Candi (Hindu-Jawa)." },
  { "en": "Siapa Francisco Serrão, orang Portugis pertama yang mencapai Kepulauan Banda?", "id": "Sahabat Magellan." },
  { "en": "Apa \"Batig Slot\" (Saldo Untung) yang diraup Belanda dari Tanam Paksa?", "id": "Keuntungan bersih untuk kas Belanda." },
  { "en": "Siapa Cut Nyak Meutia, pahlawan wanita Aceh yang bertempur bersama suaminya?", "id": "Pejuang wanita dari Aceh Utara." },
  { "en": "Organisasi \"Indische Partij\" didirikan oleh Tiga Serangkai, siapa saja mereka?", "id": "Douwes Dekker, Cipto, Suwardi." },
  { "en": "Apa \"Volksraad\" (Dewan Rakyat) yang dibentuk pemerintah kolonial Belanda?", "id": "Parlemen semu Hindia Belanda." },
  { "en": "Siapa Ki Sarmidi Mangunsarkoro, tokoh Taman Siswa yang menjadi Menteri Pendidikan?", "id": "Penerus ajaran Ki Hadjar Dewantara." },
  { "en": "Apa \"Janji Koiso\" yang diberikan Jepang mengenai kemerdekaan Indonesia?", "id": "Kemerdekaan di kemudian hari." },
  { "en": "Siapa Bung Tomo, orator ulung yang membakar semangat Pertempuran Surabaya?", "id": "Sutomo." },
  { "en": "Apa \"Serangan Umum 1 Maret 1949\" yang menunjukkan eksistensi TNI?", "id": "Serangan TNI ke Yogyakarta." },
  { "en": "Siapa Hamengkubuwono IX, Sultan Yogyakarta yang mendukung penuh RI?", "id": "Raja sekaligus negarawan." },
  { "en": "Apa \"NASAKOM\" (Nasionalis, Agama, Komunis) gagasan Soekarno?", "id": "Konsep penyatuan kekuatan politik." },
  { "en": "Siapa Jenderal Ahmad Yani, salah satu korban utama G30S?", "id": "Menteri/Panglima Angkatan Darat." },
  { "en": "Apa \"Dwifungsi ABRI\" yang melegitimasi peran militer dalam politik Orba?", "id": "Peran ganda ABRI (hankam & sospol)." },
  { "en": "Siapa Susilo Bambang Yudhoyono (SBY), Presiden RI pertama pilihan langsung?", "id": "Presiden keenam RI." },
  { "en": "Prasasti Nalanda di India menyebutkan pendirian wihara oleh raja Sriwijaya siapa?", "id": "Balaputradewa." },
  { "en": "Siapa Tribhuwana Tunggadewi, ratu yang memerintah Majapahit didampingi Gajah Mada?", "id": "Ibu Hayam Wuruk." },
  { "en": "Masjid Agung Demak memiliki \"Soko Guru\" (tiang utama) berjumlah berapa?", "id": "Empat." },
  { "en": "Siapa Cornelis Speelman, Gubernur Jenderal VOC yang terlibat Perang Trunojoyo?", "id": "Panglima VOC." },
  { "en": "Apa \"Edele Politiek\" (Politik Kesejahteraan) nama lain Politik Etis?", "id": "Politik balas budi." },
  { "en": "Siapa Teuku Umar, pahlawan Aceh yang terkenal dengan taktik perang pura-pura menyerah?", "id": "Suami Cut Nyak Dien." },
  { "en": "Organisasi \"Muhammadiyah\" didirikan oleh K.H. Ahmad Dahlan tahun berapa?", "id": "Tahun 1912." },
  { "en": "Apa \"Piagam Jakarta\" yang menjadi cikal bakal Pembukaan UUD 1945?", "id": "Rumusan dasar negara Panitia Sembilan." },
  { "en": "Siapa Latief Hendraningrat, salah satu pengibar bendera saat Proklamasi?", "id": "Komandan PETA Jakarta." },
  { "en": "Apa \"Bandung Lautan Api\", strategi bumi hangus di Bandung Selatan?", "id": "Pembakaran kota oleh pejuang RI." },
  { "en": "Siapa Assaat Datuk Mudo, pemangku jabatan Presiden RI saat RIS?", "id": "Ketua KNIP." },
  { "en": "Apa \"Deklarasi Djuanda\" yang menyatukan wilayah laut Indonesia?", "id": "Konsep Wawasan Nusantara maritim." },
  { "en": "Siapa Ali Sastroamidjojo, Perdana Menteri yang menyelenggarakan KAA Bandung?", "id": "Tokoh PNI." },
  { "en": "Apa \"Ganyang Malaysia\", slogan konfrontasi Indonesia terhadap Malaysia?", "id": "Bagian dari Dwikora." },
  { "en": "Siapa Adam Malik, Wakil Presiden ketiga RI dan tokoh diplomasi?", "id": "Jurnalis, diplomat, politisi." },
  { "en": "Apa \"Normalisasi Kehidupan Kampus/Badan Koordinasi Kemahasiswaan\" (NKK/BKK)?", "id": "Kebijakan Orba meredam mahasiswa." },
  { "en": "Siapa Joko Widodo (Jokowi), Presiden RI ketujuh?", "id": "Presiden Indonesia saat ini." },
  { "en": "Prasasti Gandasuli ditemukan di daerah mana?", "id": "Temanggung, Jawa Tengah." },
  { "en": "Siapa Mpu Sedah dan Mpu Panuluh yang menggubah Kakawin Bharatayuddha?", "id": "Pujangga Kerajaan Kediri." },
  { "en": "Masjid Agung Palembang dibangun pada masa pemerintahan sultan siapa?", "id": "Sultan Mahmud Badaruddin I Jayo Wikramo." },
  { "en": "Siapa Hendrik Zwaardecroon, Gubernur Jenderal VOC yang memulai perkebunan kopi?", "id": "Pejabat VOC." },
  { "en": "Apa \"Agrarische Wet\" (UU Agraria 1870) yang membuka Hindia bagi swasta asing?", "id": "UU liberalisasi tanah." },
  { "en": "Siapa Martha Christina Tiahahu, pahlawan wanita muda dari Maluku?", "id": "Pejuang wanita Pattimura." },
  { "en": "Organisasi \"Jong Sumatranen Bond\" bertujuan mempersatukan pemuda dari mana?", "id": "Sumatra." },
  { "en": "Apa \"Pledoi Indonesia Menggugat\" yang dibacakan Soekarno di Landraad Bandung?", "id": "Pembelaan atas tuduhan pemberontakan." },
  { "en": "Siapa Chaerul Saleh, tokoh pemuda yang terlibat dalam Peristiwa Rengasdengklok?", "id": "Pemimpin Menteng 31." },
  { "en": "Apa \"Maklumat X\" (Maklumat Wakil Presiden Nomor X) tanggal 16 Oktober 1945?", "id": "KNIP diberi kekuasaan legislatif." },
  { "en": "Siapa Amir Sjarifuddin, Perdana Menteri yang menandatangani Perjanjian Renville?", "id": "Tokoh sosialis." },
  { "en": "Apa \"Pemerintahan Darurat Republik Indonesia\" (PDRI) berpusat di kota mana?", "id": "Bukittinggi, Sumatra Barat." },
  { "en": "Siapa Djuanda Kartawidjaja, Perdana Menteri terakhir sebelum Demokrasi Terpimpin?", "id": "Penggagas Deklarasi Djuanda." },
  { "en": "Apa \"Tritura\" (Tri Tuntutan Rakyat) yang diserukan mahasiswa tahun 1966?", "id": "Tiga tuntutan kepada Soekarno." },
  { "en": "Siapa Jenderal L.B. Moerdani, Panglima ABRI yang dikenal tegas?", "id": "Tokoh intelijen dan militer." },
  { "en": "Apa \"Gerakan Reformasi 1998\" yang menuntut pengunduran diri Soeharto?", "id": "Gerakan mahasiswa dan rakyat." },
  { "en": "Siapa K.H. Abdurrahman Wahid (Gus Dur), tokoh NU dan Presiden RI ke-4?", "id": "Cucu K.H. Hasyim Asy'ari." }



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
            }, 7200);
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
