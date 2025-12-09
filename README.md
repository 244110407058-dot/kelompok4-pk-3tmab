






<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Kalkulator Jarak Waktu Kecepatan</title>

<script src="https://cdn.jsdelivr.net/npm/brython@3.11.0/brython.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/brython@3.11.0/brython_stdlib.js"></script>

<style>
    body { font-family: Arial; max-width: 700px; margin: 20px auto; padding: 20px; }
    .box { background: #f2f2f2; padding: 15px; border-radius: 8px; margin-bottom: 15px; }
    select, input { padding: 7px; margin: 5px 0; width: 200px; }
    button { padding: 10px 15px; margin-top: 10px; }
    #hasil { margin-top: 20px; padding: 15px; background: #fff3cd; border-radius: 8px; }
</style>
</head>

<body onload="brython()">

<h2>Kalkulator Jarak • Waktu • Kecepatan</h2>

<div class="box">
    <label>Pilih Perhitungan:</label><br>
    <select id="mode">
        <option value="speed">Hitung Kecepatan ( Jarak / Waktu)</option>
        <option value="distance">Hitung Jarak ( Kecepatan * Waktu)</option>
        <option value="time">Hitung Waktu ( Jarak / Kecepatan)</option>
    </select>
</div>

<!-- INPUT 1 -->
<div class="box">
    <h3>Input Nilai 1</h3>
    <input id="value1" type="number" step="any">

    <br>
    <label>Satuan Nilai 1:</label><br>

    <!-- Satuan Jarak -->
    <select id="u1_dist">
        <option value="">--Jarak--</option>
        <option value="km">Kilometer</option>
        <option value="m">Meter</option>
        <option value="cm">Centimeter</option>
    </select>

    <!-- Satuan Waktu -->
    <select id="u1_time">
        <option value="">--Waktu--</option>
        <option value="s">Detik</option>
        <option value="min">Menit</option>
        <option value="h">Jam</option>
    </select>

    <!-- Satuan Kecepatan -->
    <select id="u1_speed">
        <option value="">--Kecepatan--</option>
        <option value="ms">m/s</option>
        <option value="kmh">km/jam</option>
        <option value="cms">cm/s</option>
    </select>
</div>

<!-- INPUT 2 -->
<div class="box">
    <h3>Input Nilai 2</h3>
    <input id="value2" type="number" step="any">

    <br>
    <label>Satuan Nilai 2:</label><br>

    <!-- Satuan Jarak -->
    <select id="u2_dist">
        <option value="">--Jarak--</option>
        <option value="km">Kilometer</option>
        <option value="m">Meter</option>
        <option value="cm">Centimeter</option>
    </select>

    <!-- Satuan Waktu -->
    <select id="u2_time">
        <option value="">--Waktu--</option>
        <option value="s">Detik</option>
        <option value="min">Menit</option>
        <option value="h">Jam</option>
    </select>

    <!-- Kecepatan -->
    <select id="u2_speed">
        <option value="">--Kecepatan--</option>
        <option value="ms">m/s</option>
        <option value="kmh">km/jam</option>
        <option value="cms">cm/s</option>
    </select>
</div>

<button id="hitung">Hitung</button>

<div id="hasil">Hasil akan muncul di sini...</div>

<script type="text/python">

from browser import document

#------------------------------------------
# Fungsi Konversi Satuan
#------------------------------------------

def dist_to_m(v, u):
    return {"km": v*1000, "m": v, "cm": v/100}.get(u)

def time_to_s(v, u):
    return {"h": v*3600, "min": v*60, "s": v}.get(u)

def speed_to_mps(v, u):
    return {"ms": v, "kmh": v*1000/3600, "cms": v/100}.get(u)

def mps_to_text(v):
    return f"{v:.3f} m/s  |  {(v*3.6):.3f} km/jam"


#------------------------------------------
# Ambil satuan dari 3 dropdown terpisah
#------------------------------------------

def get_unit(group1, group2, group3):
    for g in (group1, group2, group3):
        if document[g].value != "":
            return document[g].value
    return None


#------------------------------------------
# Logika Perhitungan
#------------------------------------------

def hitung(ev):
    mode = document["mode"].value
    v1 = float(document["value1"].value)
    v2 = float(document["value2"].value)

    u1 = get_unit("u1_dist", "u1_time", "u1_speed")
    u2 = get_unit("u2_dist", "u2_time", "u2_speed")

    # -------- HITUNG KECEPATAN -------- #
    if mode == "speed":
        d = dist_to_m(v1, u1) or dist_to_m(v2, u2)
        t = time_to_s(v1, u1) or time_to_s(v2, u2)

        if d is None or t is None:
            document["hasil"].text = "Masukkan JARAK dan WAKTU yang benar."
            return

        speed = d / t
        document["hasil"].text = "Kecepatan = " + mps_to_text(speed)
        return

    # -------- HITUNG JARAK -------- #
    if mode == "distance":
        s = speed_to_mps(v1, u1) or speed_to_mps(v2, u2)
        t = time_to_s(v1, u1) or time_to_s(v2, u2)

        if s is None or t is None:
            document["hasil"].text = "Masukkan KECEPATAN dan WAKTU yang benar."
            return

        d = s * t
        document["hasil"].text = f"Jarak = {d:.3f} m  |  {d/1000:.3f} km"
        return

    # -------- HITUNG WAKTU -------- #
    if mode == "time":
        d = dist_to_m(v1, u1) or dist_to_m(v2, u2)
        s = speed_to_mps(v1, u1) or speed_to_mps(v2, u2)

        if d is None or s is None:
            document["hasil"].text = "Masukkan JARAK dan KECEPATAN yang benar."
            return

        t = d / s
        document["hasil"].text = f"Waktu = {t:.3f} detik | {t/60:.3f} menit | {t/3600:.3f} jam"
        return


document["hitung"].bind("click", hitung)

</script>

</body>
</html>
