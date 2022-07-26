# KaliLinux-LivePersistence
A boot menu in Kali Linux “Live” allows users to enable persistent storage – retaining data on a USB drive on the boot up process – across reboots of the “Kali Live” OS. Persistent data are stored on a USB drive in an unexciting partition that can also have LUKS-encryption enabled.

# requirements
win32diskmanager
kali linux live OS

lakukan installasi win32diskmanager dan burn Kali Linux Live OS menggunakan win32diskmanager.

reboot komputer anda, lalu pilih opsi boot, disini pilih opsi boot menggunakan flashdisk, atau set manual boot priority diBIOS. seketika jika anda berhasil masuk ke boot kali linux, maka akan muncul beberapa opsi dimana pilih opsi "Live System (persistence, check kali.org/prst).
</br>
sebelumnya penting untuk anda ketahui, disini kita bisa menggunakan OS Kali Linux tanpa harus melakukan install package, firmware dan lain sebagainya, akan tetapi semua nya akan bersifat sementara/non-persistence/volatile, itu merupakan pengertian dari Live install kali linux. so, itu pastinya kurang nyaman bukan?, dimana file, tools, atau konfigurasi yang kita lakukan tidak tersimpan atau hilang ketika komputer mati/volatile. jadi disini kita memerlukan yang namanya disk persistence, dimana persistence akan dapat menyimpang perubahan yang kita lakukan pada OS.

# kali linux live persistence disk command set

sebelumnya disk directory pada linux terdiri dari root, home, swap dan lain sebagainya. pembagiannya pun cukup beragam dari directory name. mulai dari (sda/ sdb/ sdc dan seterusnya). untuk itu perlu kita ketahui dimana file system yang akan kita gunakan nantinya. cek file system name menggunakan ```cfdisk``` perintah. 
</br>
kita mulai melakukan checking menggunakan name file system <b>sda/</b>
```bash
sudo cfdisk /dev/sda
```
lakukan pengecekan pada disk size, dimana disk sda/ diatas memiliki size 298Gb. ini tentunya bukan flashdisk kita, jangan menyerah!. lakukan pengecekan lagi secara berkala, tambahan sdb/ ,sdc/ , sdd/ dan seterusnya sampai menemukan ukuran size disk yang sesuai diflashdisk anda!. setelah beberapa percobaan saya menemukan disk file system yang sesuai dengan size disk yang saya gunakan, yaitu 14Gb [/dev/sdb].
</br>
Ingat!, gunakan tombol panah pada keyboard, kursor anda tidak dapat berjalan disesi disini!. masuk ke Free Space dibagian paling bawah.
- pilih New [enter]
- masukkan size untuk persistence disk [enter]
- pilih primary [enter]
- pilih write [enter]
- typing 'yes' [enter]
disini setelah proses disk management diatas, disk akan menghasilkan system file /dev/sdb3
</br>
Nice, buat system type menggunakan perintah dibawah pada disk anda
```bash
sudo mkfs.ext4 -L persistence /dev/sdb3
```
```bash
sudo e2label /dev/sdb3 persistence
```
create folder lalu mount file system ke folder yang dibuat. kita juga akan umount disk untuk melihat perubahan
```bash
sudo mkdir -p /mnt/mydat
sudo mount /dev/sdb3 /mnt/mydat
cd /mnt/mydat
echo "/ union" > persistence.conf
sudo umount /dev/sdb3
```

# Testing
reboot komputer, lalu pilih lagi opsi 'Live System (persistence, check kali.org/prst)'. setelah masuk ke komputer lakukan uji testing dengan membuat simple file. buat file plain text. for example [tes.txt]
</br>
lakukan restart komputer.

### Note
jika komputer selesai restart, file yang kita buat tadi seharusnya masih ada/tidak hilang. jika berhasil, selamat anda telah membuat Live persistence, dan jika sebaliknya maka gagal
