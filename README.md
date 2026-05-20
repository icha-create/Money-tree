<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Money Tree 🌳</title>
    <style>
        :root {
            --primary: #86efac;
            --dark: #1e3a1e;
            --bg: #f4f9f4;
            --white: #ffffff;
            --text: #2d3748;
            --muted: #64748b;
            --danger: #ef4444;
        }
        body {
            font-family: 'Inter', sans-serif;
            background-color: #e2e8f0;
            margin: 0;
            display: flex;
            justify-content: center;
        }
        .app-container {
            width: 100%;
            max-width: 420px;
            background: var(--bg);
            min-height: 100vh;
            position: relative;
            padding-bottom: 100px;
        }
        header {
            background: var(--primary);
            color: var(--dark);
            padding: 30px 20px;
            border-radius: 0 0 30px 30px;
            text-align: center;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
        }
        header h1 { margin: 0; font-size: 1.6rem; font-weight: 700; }
        header p { margin: 5px 0 0; opacity: 0.9; font-size: 0.8rem; font-weight: 500; }
        .screen { padding: 20px; animation: slideUp 0.4s ease; }
        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .input-card {
            background: var(--white);
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.03);
        }
        .input-field { margin-bottom: 15px; }
        .input-field label { display: block; font-size: 0.8rem; font-weight: 600; margin-bottom: 5px; color: var(--text); }
        input {
            width: 100%;
            padding: 12px;
            border: 1px solid #e2e8f0;
            border-radius: 10px;
            box-sizing: border-box;
            background: #f1f5f9;
        }
        .btn-primary {
            width: 100%;
            padding: 15px;
            background: var(--dark);
            color: white;
            border: none;
            border-radius: 12px;
            font-weight: 700;
            cursor: pointer;
            transition: 0.3s;
        }
        .bulan-group { margin-top: 25px; }
        .bulan-badge {
            background: var(--primary);
            color: var(--dark);
            padding: 5px 12px;
            border-radius: 20px;
            font-size: 0.75rem;
            font-weight: 700;
        }
        .item-card {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px;
            background: var(--white);
            margin: 10px 0;
            border-radius: 15px;
            border-left: 4px solid var(--primary);
            box-shadow: 0 2px 4px rgba(0,0,0,0.02);
        }
        .item-info span { font-weight: 600; display: block; color: var(--text); }
        .item-info small { color: var(--muted); font-size: 0.7rem; }
        .item-harga { font-weight: 800; color: var(--dark); }
        .bottom-nav {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            width: 85%;
            background: rgba(255,255,255,0.9);
            backdrop-filter: blur(10px);
            display: flex;
            padding: 10px;
            border-radius: 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.15);
        }
        .nav-item { flex: 1; text-align: center; cursor: pointer; color: var(--muted); }
        .icon { font-size: 1.2rem; display: block; }
        .nav-item p { margin: 0; font-size: 0.7rem; font-weight: 600; }
        .btn-reset { background: none; color: var(--danger); border: 1px solid #fee2e2; margin-top: 20px; padding: 10px; border-radius: 10px; width: 100%; cursor: pointer; }
        .hidden { display: none; }
    </style>
</head>
<body>

<div class="app-container">
    <header>
        <h1>Money Tree 🌳</h1>
        <p>Catatan Pengeluaran Cerdas</p>
    </header>

    <section id="layar-beranda" class="screen">
        <h2>Input Pengeluaran</h2>
        <div class="input-card">
            <div class="input-field">
                <label>Nama Pengeluaran</label>
                <input type="text" id="nama-item" placeholder="Misal: Jajan Boba">
            </div>
            <div class="input-field">
                <label>Harga (Rp)</label>
                <input type="number" id="harga-item" placeholder="50000">
            </div>
            <div class="input-field">
                <label>Tanggal</label>
                <input type="date" id="tanggal-item">
            </div>
            <button class="btn-primary" onclick="simpanData()">SIMPAN DATA</button>
        </div>
    </section>

    <section id="layar-arsip" class="screen hidden">
        <div class="arsip-header">
            <h2>Riwayat Arsip</h2>
            <p>Urut berdasarkan pengeluaran terbesar</p>
        </div>
        <div id="arsip-konten"></div>
        <button class="btn-reset" onclick="resetArsip()">Reset Semua Data</button>
    </section>

    <nav class="bottom-nav">
        <div class="nav-item" onclick="pindahLayar('beranda')">
            <span class="icon">🏠</span>
            <p>Beranda</p>
        </div>
        <div class="nav-item" onclick="pindahLayar('arsip')">
            <span class="icon">📁</span>
            <p>Arsip</p>
        </div>
    </nav>
</div>

<script>
let database = JSON.parse(localStorage.getItem('moneyTreeData')) || [];

function pindahLayar(namaLayar) {
    document.getElementById('layar-beranda').classList.add('hidden');
    document.getElementById('layar-arsip').classList.add('hidden');
    if (namaLayar === 'beranda') {
        document.getElementById('layar-beranda').classList.remove('hidden');
    } else {
        document.getElementById('layar-arsip').classList.remove('hidden');
        tampilkanArsip();
    }
}

function simpanData() {
    const nama = document.getElementById('nama-item').value;
    const harga = document.getElementById('harga-item').value;
    const tanggal = document.getElementById('tanggal-item').value;

    if (!nama || !harga || !tanggal) {
        alert("Ops! Isi semua data dulu ya biar rapi.");
        return;
    }

    const dataBaru = {
        id: Date.now(),
        nama: nama,
        harga: parseFloat(harga),
        tanggal: tanggal,
        bulan: new Date(tanggal).toLocaleString('id-ID', { month: 'long', year: 'numeric' })
    };

    database.push(dataBaru);
    localStorage.setItem('moneyTreeData', JSON.stringify(database));
    document.getElementById('nama-item').value = '';
    document.getElementById('harga-item').value = '';
    alert("Berhasil dicatat!");
    pindahLayar('arsip');
}

function tampilkanArsip() {
    const wadah = document.getElementById('arsip-konten');
    wadah.innerHTML = '';
    if (database.length === 0) {
        wadah.innerHTML = '<p style="text-align:center; color:#94a3b8; margin-top:20px;">Belum ada catatan.</p>';
        return;
    }
    const grupBulan = {};
    database.forEach(item => {
        if (!grupBulan[item.bulan]) grupBulan[item.bulan] = [];
        grupBulan[item.bulan].push(item);
    });
    for (let bulan in grupBulan) {
        let htmlBulan = `<div class="bulan-group"><span class="bulan-badge">${bulan}</span>`;
        grupBulan[bulan].sort((a, b) => b.harga - a.harga);
        grupBulan[bulan].forEach(item => {
            htmlBulan += `
                <div class="item-card">
                    <div class="item-info">
                        <span>${item.nama}</span>
                        <small>${item.tanggal}</small>
                    </div>
                    <div class="item-harga">Rp ${item.harga.toLocaleString('id-ID')}</div>
                </div>`;
        });
        htmlBulan += `</div>`;
        wadah.innerHTML += htmlBulan;
    }
}

function resetArsip() {
    if (confirm("Hapus semua memori pengeluaran?")) {
        database = [];
        localStorage.removeItem('moneyTreeData');
        tampilkanArsip();
    }
}
</script>
</body>
</html>
