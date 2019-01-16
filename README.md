# centos-offline-repo
  CentOS 7 ve 7.5 surumlerinde kaynaklarin kisitli oldugu durumlarda repository uzerinden kurulum yapmak gerektiginde eger online repository'den yararlanilamiyorsa offline olarak centos image dosyasindan faydalanilarak local repository olusturmak mumkundur. 
  
  1. Isletim sistemine ait veya ayni versiyona sahip image dosyasi indirildikten sonra centos isletim sistemi icerisinde bir dizine kopyalanir veya direk isletim sistemine USB Bellek baglanabiliyorsa asagidaki sekilde bir komut calistirarak image dosyasi isletim sistemine kopyalanir.
  
      `cp /media/USB_bellek/centos7.iso /opt/.`
 
  2. Ardindan /var dizini altinda bir dosya olusturulur ve image dosyasi bu dizin altina mount edilir.
  
      `mkdir -p /var/CentOSImage/Centos-7-x86_64` komutu ile dizin olusturulur.
      
      `mount -o loop,ro /opt/centos7.iso /var/CentOSImage/Centos-7-x86_64` komutu ile ilgili dizine mount edilir.
      
      (ro -> read-only)
      
  3. Sunucu yeniden baslatildiginda mount isleminin kaybolmamasi icin fstab a bir entry girilir.
    
      `/opt/centos7.iso /var/CentOSImage/Centos-7-x86_64 iso9660 loop,ro 0 0`
      
  4. /etc/yum.repos.d dizini altinda birden fazla repository dosyasi olmasi mumkun. Bu repository dosyalari icerisinde eger aktif olarak kullanilan repository varsa pasif hale getirilir. Bunun icin;
  
      `enabled=1` olanlar `enabled=0` olarak guncellenir. 
      
  5. Yeni bir repository dosyasi olusturulur;
  
      `vi CentOS-ISO.repo` (bu islemi `vi` harici bir editor ile de yapabilirsiniz.)
      
  6. Repository dosyasina ait bilgileri yaziyoruz;
  
      <pre>[LocalRepo]
      name=Centos-7-x86_64
      baseurl=file:///var/CentOSImage/Centos-7-x86_64
      gpgcheck=0
      enabled=1</pre>
      
      dosyayi bu sekilde kaydediyoruz.
  7. `yum clean all` komutu ile yum cache'ini temizliyoruz.
  
  8. `yum repolist` komutu ile baglanti saglanilan repolarin listesini gorebiliyoruz.
  
  
      [Kaynak ve Daha Fazla Bilgi Icin Tiklayiniz](https://docs.oracle.com/cd/E37670_01/E37355/html/ol_create_repo.html "Kaynak ve Daha Fazla Bilgi Icin Tiklayiniz") 
