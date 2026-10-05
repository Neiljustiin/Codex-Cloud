# Catatan Lanjutan TA — Handover AI pada 5G Standalone

Tanggal pembaruan: 5 Oktober 2026

Dokumen ini merangkum konteks percakapan untuk melanjutkan perencanaan tugas akhir pada perangkat atau sesi lain. Ini adalah ringkasan kerja, bukan ekspor verbatim seluruh pesan chat.

## Konteks pengguna dan topologi

- Mahasiswa S1 Teknik Telekomunikasi; target pengerjaan sekitar enam bulan.
- Menginginkan topik yang layak dikerjakan, cukup menarik dari sisi pengguna, dan dapat dipresentasikan dengan jelas.
- Mode jaringan yang dipilih: 5G Standalone (SA).
- Kandidat software: Open5GS sebagai 5G Core dan srsRAN Project sebagai RAN; OAI sempat dipertimbangkan.
- Diagram awal memperlihatkan Internet/TELMAT Network Lab menuju switch, Core Mini PC, dua RAN Mini PC, dua USRP (perangkat yang direncanakan: satu B210 dan satu B205), GPSDO, lalu UE Oppo Reno 8 5G dan Motorola G35 5G.
- Pengguna belum pernah menguji handover dan belum pernah menguji setup dengan USRP tersebut.

## Usulan topik kerja

**Optimasi Keputusan Handover Berbasis Random Forest untuk Mengurangi Ping-Pong dan Gangguan Video Streaming pada Testbed 5G Standalone Dua Sel.**

Pertanyaan penelitian kerja: pada testbed 5G SA dua sel, apakah keputusan Random Forest yang menyarankan UE tetap di sel saat ini atau pindah ke sel tetangga dapat mengurangi ping-pong dan gangguan video dibandingkan baseline handover A3 yang telah dituning, tanpa meningkatkan handover gagal atau radio link failure?

Nilai pengguna yang ingin ditunjukkan: apakah video lebih jarang atau lebih sebentar buffering saat koneksi berpindah sel. Ping-pong handover adalah perpindahan sel A→B lalu kembali B→A dalam jendela waktu singkat; ini bukan ping jaringan dan bukan nama lain untuk video tersendat. Ukur ping-pong dan buffering secara terpisah.

## Kenapa Random Forest sebagai metode awal

Random Forest menggabungkan prediksi banyak pohon keputusan melalui voting. Ia praktis sebagai baseline ML untuk data pengukuran berbentuk tabel dan relatif ringan dibanding deep reinforcement learning. Contoh kandidat fitur: RSRP sel serving dan neighbor, perbedaan serta tren pengukuran; RSRQ/SINR hanya jika tersedia stabil. Keluaran awal dibatasi menjadi “tetap” atau “handover”.

Model perlu dilatih dari data yang memiliki label yang dirancang dengan benar. Jangan sekadar memberi label mengikuti keputusan A3 bila tujuan penelitian adalah menguji nilai tambah model. Kumpulkan data percobaan terkendali, pisahkan train/test berdasarkan sesi atau lintasan, bekukan model saat pengujian, dan evaluasi keputusan yang benar-benar dijalankan. Agar realistis, lakukan inferensi/rekomendasi offline terlebih dahulu; closed-loop hanya bila stack RAN menyediakan jalur kendali handover yang dapat diverifikasi.

## Risiko kelayakan yang perlu diuji pertama

Hambatan utama TA bukan algoritma, melainkan memastikan handover standar berulang berhasil pada arsitektur, RAN, SDR, dan UE yang tepat. Tetapkan lebih dulu apakah radio akan menjadi dua sel/DU di bawah satu CU atau dua gNB independen; jenis handover dan dukungan perangkat lunaknya berbeda.

Tutorial resmi srsRAN yang dirujuk menunjukkan contoh intra-gNB menggunakan dua sel di bawah satu CU-CP dan menyatakan tutorial tersebut menggunakan X310; tutorial itu menyebut perangkat B200-series tidak cocok untuk use case spesifiknya. Itu bukan bukti bahwa setiap kemungkinan konfigurasi B210+B205 pasti gagal atau berhasil. Uji konfigurasi aktual sejak bulan pertama dan jangan menjadikan kontrol AI closed-loop sebagai asumsi sebelum baseline handover berhasil.

Rencana waktu enam bulan: (1) kunci arsitektur, versi software dan attach 5G SA; (2) capai handover non-AI berulang dengan UE target dan log; (3) kumpulkan data serta tune baseline A3; (4) latih/validasi Random Forest; (5) uji pembanding dan layanan video; (6) analisis, dokumentasi, dan demonstrasi. Jika handover standar belum stabil di akhir bulan kedua, sederhanakan ruang lingkup atau revisi stack bersama pembimbing.

Metrik yang disarankan:
- Mobilitas: jumlah HO, ping-pong dalam jendela yang ditetapkan, HO success/failure, RLF, waktu keputusan dan waktu sampai trafik pulih.
- Pengalaman video/data: jumlah dan total durasi stall/buffering, packet loss, throughput aplikasi.
- Model: precision/recall/F1, false HO, keputusan terlambat, latensi inferensi.

Gunakan satu UE utama, server video lokal, dan konfigurasi player/buffer yang tetap. Jika video tidak buffering pada semua metode, laporkan hasil itu dengan jujur dan gunakan gangguan trafik/throughput sebagai metrik pendamping; jangan mengklaim peningkatan QoE video tanpa bukti.

## Publikasi yang dihimpun di analisis sebelumnya

1. M. Dzaferagic et al., “ML-Based Handover Prediction Over a Real O-RAN Deployment Using RAN Intelligent Controller,” *IEEE Transactions on Network and Service Management*, 22(1), 635–647, 2025. https://doi.org/10.1109/TNSM.2024.3468910
2. J. P. S. H. Lima et al., “User-Level Handover Decision Making Based on Machine Learning Approaches,” *Journal of Communication and Information Systems*, 37(1), 104–108, 2022. https://doi.org/10.14209/jcis.2022.11
3. A. Costa et al., “Skipping-Based Handover Algorithm for Video Distribution over Ultra-Dense VANET,” *Computer Networks*, 176, 107252, 2020. https://doi.org/10.1016/j.comnet.2020.107252
4. J. He et al., “A Reinforcement Learning Handover Parameter Adaptation Method Based on LSTM-Aided Digital Twin for UDN,” *Sensors*, 23(4), 2191, 2023. https://doi.org/10.3390/s23042191
5. M. Helmy et al., “Autoformer-Based Mobility and Handoff-Aware Prediction for QoE Enhancement in Adaptive Video Streaming in 4G/5G Networks,” *Journal of Network and Computer Applications*, 243, 104324, 2025. https://doi.org/10.1016/j.jnca.2025.104324
6. M. J. Sanjarani et al., “Handover Reduction in 5G Mobile Networks Using Ensemble Learning Method,” *Scientific Reports*, 2026. https://doi.org/10.1038/s41598-026-73269-1. Artikel tercatat terbit 26 September 2026 sebagai versi awal yang dapat diperbarui menuju Version of Record; verifikasi versi final sebelum mengutip hasil rinci.
7. M. S. Mollel et al., “A Survey of Machine Learning Applications to Handover Management in 5G and Beyond,” *IEEE Access*, 9, 45770–45802, 2021. https://doi.org/10.1109/ACCESS.2021.3067503

Sumber teknis yang dirujuk: dokumentasi Open5GS https://open5gs.org/open5gs/docs/ ; tutorial handover srsRAN https://docs.srsran.com/projects/project/en/latest/tutorials/source/handover/source/index.html ; rilis srsRAN https://github.com/srsran/srsRAN_Project/releases ; diskusi komunitas srsRAN tentang CU/DU https://github.com/srsran/srsRAN_Project/discussions/827 ; tutorial handover OAI https://github.com/OPENAIRINTERFACE/openairinterface5g/blob/develop/doc/handover-tutorial.md ; manual USRP B2x0 https://files.ettus.com/manual/page_usrp_b200.html .

Sumber untuk penjelasan istilah: dokumentasi RandomForestClassifier scikit-learn https://scikit-learn.org/1.8/modules/generated/sklearn.ensemble.RandomForestClassifier.html ; 3GPP TS 38.300 Release 17 melalui ETSI https://www.etsi.org/deliver/etsi_ts/138300_138399/138300/17.10.00_60/ts_138300v171000p.pdf ; S. Park et al., “ZEUS: Handover Algorithm for 5G to Achieve Zero Handover Failure,” *ETRI Journal*, 2022, https://onlinelibrary.wiley.com/doi/10.4218/etrij.2020-0356 .

## Berkas hasil sebelumnya

- Analisis PDF: `Analisis_Topik_TA_Handover_AI_5G_SA.pdf`
- Formulir DOCX: `LKS_Usulan_Topik_Capstone_Handover_5G_SA.docx`

Keduanya berada di folder `outputs` pada workspace sesi asal.

## Langkah berikutnya yang paling berguna

1. Konfirmasi model USRP kedua dengan tepat (B205mini-i atau model lain) serta konfigurasi clock/reference yang tersedia.
2. Putuskan satu arsitektur radio dan satu jenis handover yang benar-benar didukung.
3. Validasi attach 5G SA, neighbor measurements, handover standar, dan aliran IP/video sebelum mengembangkan model ML.
4. Tinjau kembali setiap publikasi dari halaman penerbit/DOI dan perluas pencarian sistematis sebelum menetapkan klaim novelty proposal.
