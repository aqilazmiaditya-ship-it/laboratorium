# ==========================================================
# SISTEM HYBRID PANEL SURYA & PIEZOELEKTRIK (FLIP MODE)
# SIMULASI IOT - PERANCANGAN (TANPA PERANGKAT KERAS)
# ==========================================================

import time
import random

# =========================
# PARAMETER AMBANG BATAS
# =========================
TEMP_THRESHOLD = 30.0      # °C (panel optimal di atas suhu ini)
VOLTAGE_MIN = 5.0          # Tegangan minimum sistem (Volt)

# =========================
# FUNGSI SENSOR (SIMULASI)
# =========================

def read_temperature():
    """
    Simulasi pembacaan sensor suhu
    """
    return round(random.uniform(25.0, 40.0), 2)

def read_rain_sensor():
    """
    Simulasi sensor hujan
    True  = hujan
    False = tidak hujan
    """
    return random.choice([True, False])

def read_solar_voltage():
    """
    Simulasi tegangan panel surya
    """
    return round(random.uniform(0.0, 18.0), 2)

def read_piezo_voltage():
    """
    Simulasi tegangan piezoelektrik
    """
    return round(random.uniform(0.0, 12.0), 2)

# =========================
# LOGIKA FLIP HYBRID
# =========================

def energy_flip_control(temperature, rain, v_solar, v_piezo):
    """
    Menentukan sumber energi aktif
    """
    if temperature >= TEMP_THRESHOLD and not rain and v_solar >= VOLTAGE_MIN:
        return "PANEL_SURYA"
    elif v_piezo >= VOLTAGE_MIN:
        return "PIEZOELEKTRIK"
    else:
        return "TIDAK_ADA_SUMBER"

# =========================
# SIMULASI PENGIRIMAN IOT
# =========================

def send_to_iot(data):
    """
    Simulasi pengiriman data ke sistem IoT
    """
    print("📡 DATA TERKIRIM KE IOT:")
    for key, value in data.items():
        print(f"   {key} : {value}")
    print("-" * 45)

# =========================
# PROGRAM UTAMA
# =========================

def main():
    print("=== SISTEM HYBRID PANEL SURYA & PIEZOELEKTRIK ===")

    try:
        while True:
            # Baca sensor
            temperature = read_temperature()
            rain_status = read_rain_sensor()
            solar_voltage = read_solar_voltage()
            piezo_voltage = read_piezo_voltage()

            # Tentukan sumber energi
            active_source = energy_flip_control(
                temperature,
                rain_status,
                solar_voltage,
                piezo_voltage
            )

            # Data IoT
            iot_data = {
                "Suhu (°C)": temperature,
                "Hujan": "YA" if rain_status else "TIDAK",
                "Tegangan Panel (V)": solar_voltage,
                "Tegangan Piezo (V)": piezo_voltage,
                "Sumber Energi Aktif": active_source
            }

            # Kirim ke IoT
            send_to_iot(iot_data)

            time.sleep(2)

    except KeyboardInterrupt:
        print("\n⛔ Sistem dihentikan oleh pengguna")

# =========================
# EKSEKUSI PROGRAM
# =========================

if __name__ == "__main__":
    main()
