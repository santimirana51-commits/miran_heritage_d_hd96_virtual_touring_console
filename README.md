# MIDAes HD96-24 Virtual Touring Console

Workstation mixing audio digital modular profesional berperforma tinggi berbasis **JUCE 8** dan standar **C++20**, mereplikasi arsitektur dan kapabilitas konsol panggung **MIDAS Heritage D HD96-24** dengan mesin DSP Graviton Mk 2 (144 Channel @ 96kHz).

---

## 🎛️ Fitur & Kapabilitas Sistem

- **DSP Mixing Core (144 Logical Channels @ 96kHz)**:
  - 144 Full Processing Input Channels (Preamp, 4-band Parametric EQ, Dynamic EQ, Gate, Compressor, All-Pass Filter, Delay).
  - 96 Mix/AUX Busses fleksibel, 24 Matrix Busses, Stereo Main L/R Master Bus.
  - 16-Channel Dugan Speech Automixer real-time terintegrasi.
  - 24 VCA Groups & 24 POPulation Groups dengan navigasi spill multi-channel.
- **Rak Efek Virtual (Graviton FX Rack)**:
  - DVR2 Digital Reverb, Plate Reverb, Convolution Reverb, TC SCF Chorus/Flanger, Delay Nebula, NonLin2 Stereo Reverb, dan Pitch Corrector FlayrTune.
- **Virtual Audio Transport & Jaringan VBAN**:
  - Transport audio user-mode real-time dengan proteksi loop feedback otomatis.
  - Streaming audio jaringan **VBAN UDP (Port 6980)** untuk integrasi nirkabel/kabel ke PC lain atau Voicemeeter tanpa driver pihak ketiga.
  - 4 pasang Virtual Cable Send/Return stereo dengan ASRC (Asynchronous Sample Rate Converter) dan metering dBFS.
- **OSC UDP Wireless Protocol Server (Chapter 27)**:
  - Daemon jaringan UDP berkecepatan tinggi pada **Port 53000** dengan zero-allocation packet codec.
  - Dukungan kontrol bidirectional real-time untuk tablet dan ponsel pintar Android.
- **Antarmuka Pengguna Konsol Asli (Touring Surface Layout)**:
  - GUI Midas Touring Console Surface interaktif, Strip Channel responsif, EBU R128 Loudness telemetry, dan web-based console interface.

---

## 📱 Wireless Android Remote Control

Aplikasi remote kontrol tablet & ponsel Android mandiri untuk mengendalikan konsol ini secara nirkabel melalui OSC UDP:
👉 **[Repositori Android Remote: santimirana51-commits/heritage_d_hd96_remote](https://github.com/santimirana51-commits/heritage_d_hd96_remote)**

*Tersedia file instalasi siap pasang **`HeritageRemote-debug.apk`** (~5.3 MB) di repositori remote tanpa perlu melakukan kompilasi manual.*

---

## ⚠️ Konfigurasi Jaringan & Windows Firewall

Agar perangkat Android Remote dan stream VBAN dapat terhubung ke PC mixer:

1. **Pastikan PC dan HP terhubung ke Wi-Fi / Router yang sama**.
2. **Cek Alamat IP PC**:
   Buka PowerShell dan jalankan:
   ```powershell
   (Get-NetIPAddress -AddressFamily IPv4 | Where-Object { $_.InterfaceAlias -notmatch 'Loopback|vEthernet' }).IPAddress
   ```
3. **Buka Port Windows Firewall (Wajib)**:
   Jalankan PowerShell sebagai Administrator:
   ```powershell
   Set-NetFirewallRule -DisplayName "openlive hd digital mixing system.exe" -Action Allow
   New-NetFirewallRule -DisplayName "OPENLIVE OSC UDP 53000" -Direction Inbound -Protocol UDP -LocalPort 53000 -Action Allow
   New-NetFirewallRule -DisplayName "OPENLIVE VBAN UDP 6980" -Direction Inbound -Protocol UDP -LocalPort 6980 -Action Allow
   ```

---

## 📁 Struktur Direktori

```
OPENLIVE_REFACTORED/
├── CMakeLists.txt             # Skrip konfigurasi build CMake modern (JUCE 8 + C++20)
├── README.md                  # Dokumentasi umum sistem
├── AGENTS.md                  # Panduan konvensi DSP & rekayasa audio
├── Launch_NOVA_HD.bat         # Skrip peluncuran cepat konsol mixer
├── docs/                      # Dokumentasi teknis, audit modul, dan catatan rilis
├── Resources/                 # Manual hardware HD96-24 dan aset grafis
├── Source/
│   ├── Application/           # Entry point aplikasi (Main.cpp, Application.h)
│   ├── Audio/                 # ASIO/WASAPI driver, OSC Server (53000), VBAN (6980)
│   ├── Mixer/                 # MixerEngine core, ChannelState, MasterBus, Dugan Automixer
│   ├── DSP/                   # Peaking EQ, Dynamic EQ, DN360, Channel AI, RTA, Spectrogram
│   ├── Effects/               # Virtual FX Rack (DVR2, Plate, NonLin2, Chorus, Delay)
│   ├── Routing/               # Patch Matrix, Bus Scalable Graph, Surround Panning
│   ├── GUI/                   # Midas Touring Surface, Channel Strip, Panel parameter
│   ├── MIDI/                  # MIDI Callback & Graviton MIDI Learn
│   ├── Recording/             # Multi-track WAV recorder non-blocking FIFO
│   └── Utilities/             # Cue List, Automation, Show Archive, Protocol parser
├── Tests/                     # Suite pengujian unit komprehensif (CTest)
└── scripts/                   # Skrip otomatisasi build dan benchmark
```

---

## 🛠️ Persyaratan Sistem & Kompilasi

- **Sistem Operasi**: Windows 10/11 x64 (atau macOS / Linux dengan toolchain JUCE)
- **Kompiler**: Visual Studio 2022 (MSVC 19.41+) dengan dukungan C++20
- **CMake**: Versi 3.22 atau lebih baru
- **JUCE Framework**: Versi 8.0.10 (terpasang lokal di `C:/Users/miran/JUCE` atau otomatis via `FetchContent`)

### Langkah Kompilasi (CMake):

```powershell
# 1. Konfigurasi build project
cmake -B build -G "Visual Studio 17 2022" -A x64 -DBUILD_TESTING=ON

# 2. Kompilasi Release binary
cmake --build build --config Release --parallel

# 3. Eksekusi unit test (Verifikasi 100% PASS)
ctest --test-dir build -C Release --output-on-failure
```

### Menjalankan Aplikasi Mixer:
Jalankan file biner hasil kompilasi:
`build\OPENLIVE_REFACTORED_artefacts\Release\OpenLive HD Digital Mixing System.exe`
atau klik dua kali `Launch_NOVA_HD.bat`.
