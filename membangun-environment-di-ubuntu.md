Sebelum membuat dan menjalankan automasi Robot Framework dengan appium di mesin Ubuntu, ada beberapa tools dan software yang diperlukan. List nya:

* [Java JDK](#install-java-jdk)
* [Android SDK](#install-android-sdk)
* [NodeJS (via NVM)](#install-node)
* [Appium (via npm)](#install-appium-server)
* [Python + pip](#install-python-dan-pip)
* [RobotFramework (via pip)](#install-robot-framework)
* [RobotFramework Appium Library (via pip)](#install-appium-library-untuk-robot-framework)
* [Kode editor](#kode-editor)

Bagi yang baru saja mulai implementasi, disarankan menjalankan semua step di halaman ini berurutan dari awal. Bagi yang sudah terinstall beberapa tools yang ada di list ini, bisa skip dan lanjut ke tools/software yang belum terpasang.

Tutorial ini dibuat dengan basis **Ubuntu versi 16.04**. Tidak menutup kemungkinan bisa dilakukan di versi atas maupun di bawahnya.

Perlu diingat dengan membangun environment di Ubuntu, maka kita hanya bisa menulis dan menjalankan automasi untuk device android saja. Untuk iOS, hanya bisa dijalankan melalui Mac OSX. Untuk yang ingin menjalankan automasi iOS, silahkan merujuk ke segmen tersendiri untuk iOS.

## Install Java JDK
Sebelum menginstall Java JDK, pertama-tama kita harus memastikan **OpenJDK** tidak terinstall di mesin ubuntu yang kita gunakan. Mengapa tidak menggunakan OpenJDK? Dari pengalaman penulis OpenJDK menyebabkan beberapa error pada appium. Walaupun dokumentasi appium sendiri menyebutkan support OpenJDK, namun penulis belum menyarankan (dan belum mencoba lagi) untuk menggunakannya. OpenJDK dan Java JDK juga rentan konflik jika dijalankan bersamaan, sehingga disarankan hanya punya salah satunya saja.

Untuk mengecek, coba jalankan perintah ini pada terminal:
```
$ java -version
```

Jika pada mesin ubuntu yang digunakan benar terdapat OpenJDK, maka perintah di atas akan menghasilkan output kira-kira seperti dibawah ini (versi mungkin agak berbeda):
```
java version "1.7.0_15"
OpenJDK Runtime Environment (IcedTea6 1.10pre) (7b15~pre1-0lucid1)
OpenJDK 64-Bit Server VM (build 19.0-b09, mixed mode)
```

Namun jika ternyata sudah terinstall Java JDK, outputnya akan seperti ini (versi mungkin akan berbeda):
```
java version "1.8.0_121"
Java(TM) SE Runtime Environment (build 1.8.0_121-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.121-b13, mixed mode)
```

Jika output mengatakan ada OpenJDK terinstall, maka jalankan perintah di bawah ini untuk menghapusnya:
```
$ sudo apt-get -y purge openjdk-\*
```

Selanjutnya, mari kita tambahkan repository Java JDK untuk ubuntu dengan menjalankan perintah di bawah ini dari terminal (ketik Y jika ditanyakan ingin lanjut atau tidak):
```
$ sudo add-apt-repository ppa:webupd8team/java && sudo apt-get update
```

Selanjutnya, jalankan perintah di bawah ini melalui terminal untuk memulai instalasi Java JDK:
```
$ sudo apt-get -y install oracle-java8-installer
```

Untuk memastikan instalasi sudah benar, jalankan kembali perintah ini melalui terminal:
```
$ java -version
```

Perintah di atas akan menghasilkan output yang kira-kira seperti ini (versi mungkin agak berbeda), yang menandakan instalasi telah berhasil:
```
java version "1.8.0_121"
Java(TM) SE Runtime Environment (build 1.8.0_121-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.121-b13, mixed mode)
```

## Install Android SDK
Android SDK adalah sekumpulan development tools untuk android. Appium memerlukan akses ke beberapa tools yang terdapat dalam SDK ini, seperti adb, uiautomator dll.

Untuk men-download Android SDK, jalankan perintah di bawah ini melalui terminal (file akan disimpan di direktori /home/username_folder_anda)
```
$ cd ~
$ wget https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip
```

Tunggu sampai proses download selesai.
Selanjutnya, jalankan perintah di bawah ini melalui terminal untuk mengextrak Android SDK yang telah kita download:
```
$ cd ~
$ mkdir android-sdk
$ cd android-sdk/
$ cp ~/sdk-tools-linux-3859397.zip ~/android-sdk/
$ unzip sdk-tools-linux-3859397.zip
$ rm -rf sdk-tools-linux-3859397.zip
```

Selanjutnya, kita perlu melakukan konfigurasi pada file ~/.bashrc yang merupakan salah satu syarat dari appium sendiri, juga supaya mempermudah kita menjalankan tools-tools dari Android SDK. Untuk mengedit file tersebut, kita bisa menggunakan editor seperti nano, dengan menjalankan perintah di bawah ini melalui terminal:
```
$ nano ~/.bashrc
```

Pindah ke bagian paling bawah dari file (dengan menekan tombol PGDN/PageDown di keyboard sampai menemukan akhir file), kemudian masukkan baris berikut:
```
export ANDROID_HOME=$HOME/android-sdk
export ANDROID_SDK=$ANDROID_HOME
PATH=$PATH:$ANDROID_HOME/build-tools
PATH=$PATH:$ANDROID_HOME/platform-tools
PATH=$PATH:$ANDROID_HOME/tools
PATH=$PATH:$ANDROID_HOME/tools/bin
export JAVA_HOME=$(readlink -f /usr/bin/javac | sed "s:/bin/javac::")
export PATH=$JAVA_HOME/bin:$PATH

export PATH
```

Simpan dengan menekan tombol CTRL+X dilanjutkan dengan Y -> enter.
Kemudian, jalankan perintah source di bawah melalui terminal untuk meng-apply ubahan, atau restart terminal (dengan menutup dan membuka kembali terminal)
```
source ~/.bashrc
```

Selanjutnya, jalankan perintah berikut di terminal untuk memulai instalasi tools-tools yang diperlukan dari Android SDK:
```
$ sdkmanager "build-tools;27.0.3" "platforms;android-27" "platform-tools"
```

## Install Node
Untuk install NodeJS ini, penulis menggunakan NVM (Node Version Manager) agar lebih mudah memanage beberapa versi Node yang berbeda, yang mungkin akan diperlukan ke depannya.

Untuk download dan install NVM, bisa dengan menjalankan perintah di bawah:
```
$ wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
```

Restart terminal setelah instalasi selesai (dengan menutup dan membuka kembali terminal). Setelah itu, jalankan perintah ini di terminal untuk menginstall NodeJS melalui NVM:
```
$ nvm install 6.14.1
$ nvm alias default 6.14.1
```

## Install Appium Server
Selanjutnya adalah menginstall appium server. Jalankan perintah berikut di terminal untuk menginstall appium:
```
$ npm install -g appium --verbose
```

Tunggu sampai proses instalasi selesai. Setelah selesai, untuk memastikan instalasi berhasil dengan baik, jalankan perintah berikut di terminal:
```
$ appium
```

Jika berhasil, maka terminal akan mengeluarkan output seperti ini (versi mungkin akan sedikit berbeda):
```
[Appium] Welcome to Appium v1.7.2
[Appium] Appium REST http interface listener started on 0.0.0.0:4723
```

Untuk sementara, kita bisa meng-close proses appium server dengan menekan tombol CTRL+C di terminal.

## Install Python Dan PIP
Python dan pip diperlukan untuk robotframework (karena Obviously, Robot Framework ditulis dengan bahasa python). Secara default, sistem operasi Ubuntu harusnya sudah terinstall python. Untuk memastikan, jalankan perintah berikut di terminal:
```
$ python -V
```

Tanda jika sudah terinstall, output dari command di atas kira-kira seperti ini (versi mungkin berbeda):
```
Python 2.7.12
```

Jika belum terinstall python, maka jalankan perintah berikut di terminal:
```
$ sudo apt-get install python python-tk
```

Selain python, kita butuh pip, yang merupakan paket manager nya python yang kita gunakan untuk menginstall robot framework nantinya. Untuk mengecek apakah pip sudah terinstall atau belum, jalankan perintah berikut di terminal:
```
$ pip -V
```

Jika sudah terinstall, perintah di atas akan menghasilkan output kira-kira seperti ini (versi mungkin berbeda):
```
pip 9.0.3 from /home/semutmerah/.pyenv/versions/2.7.14/lib/python2.7/site-packages (python 2.7)
```

Jika pip belum terinstall, maka bisa jalankan perintah ini di terminal untuk menginstall pip:
```
$ sudo apt-get -y install python-pip
```

## Install Robot Framework
Jalankan perintah berikut di terminal untuk menginstall robotframework mengggunakan pip:
```
$ pip install robotframework
```

## Install Appium Library Untuk Robot Framework
Robot Framework adalah Framework yang menentukan bahasa yang dipakai oleh script automasi. Namun robot framework jika berdiri sendiri tanpa bantuan library tambahan, dia tidak bisa berkomunikasi dengan appium server.

Supaya Robot Framework bisa berkomunikasi dengan appium, kita memerlukan wrapper atau library yang bertugas menterjemahkan perintah dari script robot framework ke appium server. Jika ada yang tertarik, repository appium library untuk robot framework bisa ditemukan [di alamat ini](https://github.com/serhatbolsu/robotframework-appiumlibrary)

Untuk menginstall appium library, jalankan perintah ini di terminal:
```
$ pip install robotframework-appiumlibrary
```

## Kode Editor
Khusus untuk kode editor, penulis tidak ingin mendikte pembaca untuk menggunakan editor tertentu. Namun catatan kecil, bagi pecinta RIDE, kabar buruknya anda tidak dapat menggunakan RIDE di ubuntu.

Namun jangan khawatir, ada beberapa alternatif editor lain seperti Sublime Text, Atom, dan Visual Studio Code. Atom bahkan memiliki plugin di repository plugin nya khusus untuk supporting kode Robot Framework.

Untuk cara instalasi, silahkan merujuk ke situs editor masing-masing ya :).
