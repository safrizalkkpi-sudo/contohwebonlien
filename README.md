<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Penyimpanan Arsip Daerah</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background-color: #1c1c1c; /* hitam lembut */
        color: white;
        margin: 0;
        padding: 20px;
    }
    /* Header */
    .header {
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: #2b2b2b;
        padding: 15px;
        border-radius: 10px;
        margin-bottom: 20px;
    }
    .header img {
        height: 60px;
        margin-right: 15px;
    }
    .header h1 {
        margin: 0;
        color: #4CAF50;
        font-size: 24px;
    }

    /* Form input */
    #formArsip {
        max-width: 600px;
        margin: 0 auto;
        text-align: left;
    }
    .form-group {
        display: flex;
        align-items: center;
        margin: 8px 0;
    }
    .form-group label {
        width: 150px;
    }
    .form-group input, .form-group select {
        flex: 1;
        padding: 6px;
        border-radius: 5px;
        border: none;
        background-color: #2b2b2b;
        color: white;
    }
    button {
        padding: 8px 15px;
        margin: 10px 5px 0 0;
        border-radius: 5px;
        border: none;
        background-color: #4CAF50;
        color: white;
        cursor: pointer;
    }

    /* Table */
    table {
        width: 100%;
        margin-top: 30px;
        border-collapse: collapse;
        background-color: #2b2b2b;
        color: white;
    }
    th, td {
        padding: 10px;
        border: 1px solid #444;
        text-align: left;
    }
    th {
        background-color: #3a3a3a;
    }
</style>
</head>
<body>

<!-- Header dengan logo dan nama kantor -->
<div class="header">
    <img src="logo.png" alt="Logo Kantor"> <!-- ganti logo.png dengan path logomu -->
    <h1>Dinas Kearsipan Kabupaten Deliserdang</h1>
</div>

<h2 style="text-align:center;">Penyimpanan Arsip Daerah</h2>

<!-- Form input arsip -->
<div id="formArsip">
    <div class="form-group">
        <label>Nomor Arsip:</label>
        <input type="text" id="nomor">
    </div>
    <div class="form-group">
        <label>Nama Daerah:</label>
        <input type="text" id="daerah">
    </div>
    <div class="form-group">
        <label>Jenis Arsip:</label>
        <select id="jenis">
            <option>Dokumen</option>
            <option>Foto</option>
            <option>Video</option>
            <option>Lainnya</option>
        </select>
    </div>
    <div class="form-group">
        <label>Tanggal:</label>
        <input type="date" id="tanggal">
    </div>
    <div class="form-group">
        <label>Foto/Icon (URL):</label>
        <input type="text" id="foto">
    </div>
    <button onclick="tambahData()">Tambah Data</button>
    <button onclick="downloadExcel()">Download Excel</button>
</div>

<!-- Tabel arsip -->
<table id="tabelArsip">
    <thead>
        <tr>
            <th>Nomor Arsip</th>
            <th>Nama Daerah</th>
            <th>Jenis Arsip</th>
            <th>Tanggal</th>
            <th>Foto/Icon</th>
        </tr>
    </thead>
    <tbody></tbody>
</table>

<script src="https://cdn.jsdelivr.net/npm/xlsx/dist/xlsx.full.min.js"></script>
<script>
let dataArsip = [];

function tambahData() {
    const nomor = document.getElementById("nomor").value;
    const daerah = document.getElementById("daerah").value;
    const jenis = document.getElementById("jenis").value;
    const tanggal = document.getElementById("tanggal").value;
    const foto = document.getElementById("foto").value;

    if(!nomor || !daerah || !jenis || !tanggal){
        alert("Nomor, Nama Daerah, Jenis, dan Tanggal wajib diisi!");
        return;
    }

    const data = {Nomor: nomor, Daerah: daerah, Jenis: jenis, Tanggal: tanggal, Foto: foto};
    dataArsip.push(data);
    updateTabel();
    clearForm();
}

function updateTabel() {
    const tbody = document.getElementById("tabelArsip").getElementsByTagName("tbody")[0];
    tbody.innerHTML = "";
    dataArsip.forEach(item => {
        const row = tbody.insertRow();
        row.insertCell(0).innerText = item.Nomor;
        row.insertCell(1).innerText = item.Daerah;
        row.insertCell(2).innerText = item.Jenis;
        row.insertCell(3).innerText = item.Tanggal;
        row.insertCell(4).innerText = item.Foto;
    });
}

function clearForm() {
    document.getElementById("nomor").value = "";
    document.getElementById("daerah").value = "";
    document.getElementById("jenis").selectedIndex = 0;
    document.getElementById("tanggal").value = "";
    document.getElementById("foto").value = "";
}

function downloadExcel() {
    const wb = XLSX.utils.book_new();
    const ws = XLSX.utils.json_to_sheet(dataArsip.map(item => ({
        "Nomor Arsip": item.Nomor,
        "Nama Daerah": item.Daerah,
        "Jenis Arsip": item.Jenis,
        "Tanggal": item.Tanggal,
        "Foto/Icon": item.Foto
    })));
    XLSX.utils.book_append_sheet(wb, ws, "Arsip Daerah");
    XLSX.writeFile(wb, "arsip_daerah.xlsx");
}
</script>

</body>
</html>
