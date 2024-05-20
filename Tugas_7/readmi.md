# Tugas 7

## Getting Started

1. Masuk ke halaman ``https://labs.play-with-docker.com/`` 
2. Ketikkan perintahh pada terminal ``docker run -dp 80:80 docker/getting-started:pwd``, lalu tunggu hingga kontaider dimulai dan klik port 80
   ![alt text](img/gambar1.png)

## Our Application

1. Download zip yang telah disediakan
2. Lakukan unzip file dengan perintah ``unzip app.zip``
   ![alt text](img/gambar2.png)
3. Pindah ke direktori app/
4. lihat isi dari direktori app
   ![alt text](img/gambar3.png)
5. Untuk membangun aplikasi, buat file Dockerfile dengan perintah ``vi Dockerfile`` yang berisi:
    ```
    FROM node:10-alpine
    WORKDIR /app
    COPY . .
    RUN yarn install --production
    CMD ["node", "/app/src/index.js"]
    ```
    ![alt text](img/gambar4.png)
6. Lakukan build container dengan perintah ``docker build -t docker-101 . `` dan jalankan menggunakan perintah ``docker run -dp 3000:3000 docker-101``
   ![alt text](img/gambar5.png)
   ![alt text](img/gambar6.png)
7. Buka port 3000 
   ![alt text](img/gambar7.png)
8. Tambahkan beberapa tugas
   ![alt text](img/gambar8.png)

## Updating Our App
1. Ubah/update source coide di fil ``~/app/src/static/js/app.js`` pada line 56
   ![alt text](img/gambar9.png)
   ![alt text](img/gambar10.png)
2. Lakukan update image menggunakan perintah ``docker build -t docker-101 .`` dan jalankan container baru menggunakan perintah ``docker run -dp 3000:3000 docker-101``
   ![alt text](img/gambar11.png)
   ![alt text](img/gambar12.png)
Saat mencoba menjalankan akan terjadi masalah, karena port 3000 dari host sudah digunakan. Untuk memperbaikinya, perlu menghapus port 3000.
3. Mengganti container lama ke container baru, jalankan perintah ``docker ps`` untuk melihat id container, stop container dengan perintah ``docker stop <the-container-id>``, remove container menggunakan perintah ``docker rm <the-container-id>``, lalu jalankan lagi perintah ``docker run -dp 3000:3000 docker-101``
   ![alt text](img/gambar13.png)
   ![alt text](img/gambar14.png)
   ![alt text](img/gambar15.png)

## Sharing Our App

1. Buat repositori di Docker Hub 
   ![alt text](img/gambar16.png)
2. Lakukan pushing img dengan perintah ``docker push dockersamples/101-todo-app``, namun akan terjadi error karena tidak menemukan gambar dengan nama dockersamples/101-todo-app.
   ![alt text](img/gambar17.png)
3. Login ke docker hub dengan menggunakan perintah ``docker login -u YOUR-USER-NAME``
   ![alt text](img/gambar18.png)
4. Gunakan perintah tag docker untuk memberi nama baru pada gambar docker-101 menggunakan perintah ``docker tag docker-101 YOUR-USER-NAME/101-todo-app`` dan lakukan push lagi ``docker push YOUR-USER-NAME/101-todo-app``
   ![alt text](img/gambar19.png)
5. Jalankan image di new instance, namun sebelumnya wajib untuk menghapus instance lama dan menjalankan instance baru menggunakan perintah ``docker run -dp 3000:3000 YOUR-USER-NAME/101-todo-app``
   ![alt text](img/gambar20.png)

## Persisting Our DB
1. Mulai sebuah kontainer ubuntu yang akan membuat sebuah file bernama /data.txt dengan nomor acak antara 1 dan 10000 dengan perintah ``docker run -d ubuntu bash -c "shuf -i 1-10000 -n 1 -o /data.txt && tail -f /dev/null"``
   ![alt text](img/gambar21.png)
2. Melihat output dengan masuk ke dalam container, menggunakan perintah ``docker exec <container-id> cat /data.txt``
   ![alt text](img/gambar22.png)
3. Jalankan container ubuntu menggunakan perintah ``docker run -it ubuntu ls /``
   ![alt text](img/gambar23.png)
4. Buat volume menggunakan docker volume menggunakan perintah ``docker volume create todo-db``
   ![alt text](img/gambar24.png)
5. Jalankan todo container menggunakan perintah ``docker run -dp 3000:3000 -v todo-db:/etc/todos docker-101``
   ![alt text](img/gambar25.png)
6. Hapus container todo app menggunakan perintah ``docker rm -f <id>``
   ![alt text](img/gambar26.png)
7. Untuk melakukan pengecekkan penyimpanan data di Docker dapat menggunakan docker volume inspect ``docker volume inspect todo-db``
   ![alt text](img/gambar27.png)

## Using Bind Mounts
1. Pastikan bahwa tidak ada container docker-101 yang berjalan
2. Jalankan perintah ``docker run -dp 3000:3000 \ -w /app -v $PWD:/app \ node:10-alpine \ sh -c "yarn install && yarn run dev"``
    ![alt text](img/gambar28png)
3. Gunakan perintah ``docker logs -f <container-id>`` untuk melihat log
   ![alt text](img/gambar29png)
   ![alt text](img/gambar30png)
4. Buat perubahan pada aplikasi di file src/static/js/app.js, yaitu merubah tombol "Add Item" menjadi hanya "add". Perubahan ini akan ada di baris 109. Lalu jalankan menggunakan perintah ``docker build -t docker-101 .``
   ![alt text](img/gambar31png)
   ![alt text](img/gambar32png)
   ![alt text](img/gambar33png)

## Multi Container App
1. Membuat jaringan yang akan melampirkan container MySql saat startup, pertama membuat network ``docker network create todo-app``, lalu jalankan menggunakan perintah
   ```
   docker run -d \
    --network todo-app --network-alias mysql \
    -v todo-mysql-data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=secret \
    -e MYSQL_DATABASE=todos \
    mysql:5.7
    ```
    ![alt text](img/gambar34.png)
    ![alt text](img/gambar35.png)
2. Untuk memastikan bahwa database kita sudah berjalan, hubungkan ke database dan verifikasi koneksi mengunakan perintah ``docker exec -it <mysql-container-id> mysql -p``
   ![alt text](img/gambar36.png)
3. Masukkan password "secret" , masukkan perintah ``SHOW DATABASES:`` untuk melihat isi database
   ![alt text](img/gambar37.png)
4. Melakukan koneksi ke MySQL, pertama tama Mulai kontainer baru menggunakan gambar nicolaka/netshoot menggunakan perintah ``docker run -it --network todo-app nicolaka/netshoot``. Pastikan untuk menghubungkannya ke jaringan yang sama.
   ![alt text](img/gambar38.png)
5. Gunakan perintah dig untuk mencari alamt ip untuk nama host mysql menggunakan perintah ``dig mysql``
   ![alt text](img/gambar39.png)
   ![alt text](img/gambar40.png)
6. Jalankan app yang terhubung dengan MySql, Kita akan menentukan setiap variabel lingkungan di atas, serta menghubungkan kontainer ke jaringan aplikasi kita menggunakan perintah
   ```
   docker run -dp 3000:3000 \
   -w /app -v $PWD:/app \
   --network todo-app \
   -e MYSQL_HOST=mysql \
   -e MYSQL_USER=root \
   -e MYSQL_PASSWORD=secret \
   -e MYSQL_DB=todos \
   node:10-alpine \
   sh -c "yarn install && yarn run dev"
   ```
   ![alt text](img/gambar41.png)
7. Menggunakan perintah ``docker logs <container-id>`` untuk melihat pesan yang menunjukkan bahwa itu menggunakan database MySql
   ![alt text](img/gambar42.png)
   ![alt text](img/gambar43.png)
8. Buka aplikasi todo app dan tambahkan beberapa todolist, lalu tampilkan table menggunakan perintah `` select * from todo_items;``
   ![alt text](img/gambar44.png)
   ![alt text](img/gambar45.png)

## Using Docker Compose

1. Install docker compose menggunakan perintah ``docker-compose version``
2. Membuat file compose dengan nama ``docker-compose.yml``, tambahkan untuk webservernya
   ```
       version: "3.7"

   services:
   app:
     image: node:10-alpine
     command: sh -c "yarn install && yarn run dev"
     ports:
     - 3000:3000
     working_dir: /app
     volumes:
     - ./:/app
     environment:
     MYSQL_HOST: mysql
     MYSQL_USER: root
     MYSQL_PASSWORD: secret
     MYSQL_DB: todos
   ```
3. Tambahkan service Database untuk mysql
   ```
      mysql:
      image: mysql:5.7
      volumes:
         - todo-mysql-data:/var/lib/mysql
      environment: 
         MYSQL_ROOT_PASSWORD: secret
         MYSQL_DATABASE: todos
   ```
4. Tambahkan volumes
   ```
   volumes:
     todo-mysql-data:
   ```
5. Lakukan compose up dengan menggunakan perintah `` docker compose up -d``
6. Cek logs

## Image Building Best Practice
1. Gunakan perintah ``docker image history`` untuk melihat lapisan-lapisan dalam gambar docker-101 yang telah dibuat sebelumnya
   ![alt text](img/gambar46.png)
2. Layer Caching, update Dockerfile di package .json, lalu build kembali menggunakan perintah ``docker build -t docker-101 .``
   ```
   FROM node:10-alpine
   WORKDIR /app
   COPY package.json yarn.lock ./
   RUN yarn install --production
   COPY . .
   CMD ["node", "/app/src/index.js"]
   ```
    ![alt text](img/gambar47.png)
3. Merubah file index.html ``src/static/index.html`` (like change the  to say "The Awesome Todo App").
   ![alt text](img/gambar48.png)
   ![alt text](img/gambar49.png)
5. Multi Stage Builds
   Maven/Tomcat Example
      ```
      FROM maven AS build
      WORKDIR /app
      COPY . .
      RUN mvn package

      FROM tomcat
      COPY --from=build /app/target/file.war /usr/local/tomcat/webapps 
      ```
   React
      ```
      FROM node:10 AS build
      WORKDIR /app
      COPY package* yarn.lock ./
      RUN yarn install
      COPY public ./public
      COPY src ./src
      RUN yarn run build

      FROM nginx:alpine
      COPY --from=build /app/build usr/share/nginx/html
      ```

