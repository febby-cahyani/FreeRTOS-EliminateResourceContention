# FreeRTOS-Demonstrate	a	simple	way	to	eliminate resource	contention	-	suspending	the	scheduler. 
Proyek ini mendemonstrasikan pengendalian akses terhadap sumber daya bersama menggunakan FreeRTOS Critical Section. Sistem ini dirancang untuk mengontrol sinkronisasi antara task-task yang beroperasi pada beberapa LED. Implementasi menggunakan STM32F401CCU6 sebagai mikrokontroler utama.

Diagram Task :

![Screenshot 2024-11-17 193544](https://github.com/user-attachments/assets/1d175f9c-7493-44de-aa46-037fd45d8765)

Hardware yang diperlukan :
1. STM32f401CCU6
2. LED

Software yang diperlukan :
1. STM32CubeIDE
2. FreeRTOS

Cara Kerja :
1. Inisialisasi Semaphore dan Task :
   - Proyek ini menggunakan dua task utama: GreenTask dan RedTask.
   - Keduanya bersaing untuk mengakses sumber daya bersama (fungsi AccessSharedData) yang dilindungi dengan Critical Section.
     
2. Deskripsi Task :
   
   a. 'RedTask' :
   - Menyalakan LED Merah.
   - Memasuki critical section untuk mengakses data bersama.
   - Mematikan LED Merah dan memberikan waktu delay 100ms sebelum mengulang.
   
   b. 'GreenTask' :
   - Menyalakan LED Hijau.
   - Memasuki critical section untuk mengakses data bersama.
   - Mematikan LED Hijau dan memberikan waktu delay 500ms sebelum mengulang.
   
3. Mekanisme Critical Section
   - Critical Section digunakan untuk mencegah task lain mengakses sumber daya bersama secara bersamaan.
   - Task yang sedang berada dalam critical section akan mengunci sumber daya, sehingga task lain harus menunggu sampai critical section dilepas.
     
4. Perilaku Task :
   
   a. 'GreenTask' dan 'RedTask' :
    - Kedua task mencoba memasuki critical section secara bergantian.
    - Jika ada konflik (misalnya saat sumber daya sudah digunakan), LED Kuning akan menyala untuk menunjukkan adanya kontensi.
        
   b. Pengendalian Critical Section :
      - Penggunaan fungsi taskENTER_CRITICAL() dan taskEXIT_CRITICAL() memastikan akses eksklusif ke sumber daya bersama.
        
5. Siklus Kerja LED :
   - LED Hijau dan Merah menyala bergantian dengan waktu delay masing-masing.
   - LED Kuning hanya menyala jika terjadi kontensi akses ke sumber daya.

Ringkasan Perilaku LED :

![Screenshot 2024-11-17 213337](https://github.com/user-attachments/assets/24bfec91-2245-4882-b4bd-7a61fb3da8fb)

Pinout Hardware :

![Pinout Hardware FreeRTOS](https://github.com/user-attachments/assets/8b814f64-fa02-4ee5-a5e5-d6a85f395bb2)

Hasil Hardware :
- Normal
  
  ![6-Normal-GIF](https://github.com/user-attachments/assets/5267ae37-9f61-47ca-b587-dcfbad881599)

- Konflik

  ![6-Konflik-GIF](https://github.com/user-attachments/assets/9d8d6017-2f4e-4560-9703-61e26119bb5d)


Hasil dari proyek ini sebagai berikut :

a. Normal :
- Task Hijau dan Merah bekerja bergantian tanpa konflik.
- LED Hijau dan Merah menyala sesuai urutan dengan waktu delay masing-masing.

b. Konflik : 

   Jika dua task mencoba mengakses sumber daya secara bersamaan, LED Kuning menyala untuk menunjukkan adanya konflik.

Project ini dikerjakan di Politeknik Elektronika Negeri Surabaya dengan dosen pengampu bapak Fernando Ardilla
