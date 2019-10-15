Nama	: Arvian Eka Saputra

NIM		: 175410041

Kelas	: TI-9
________________________________________
## Pertemuan 5

Configurasi CockroachDB

1. Download file terbaru cockroachdb dari website https://www.cockroachlabs.com

2. Ekstrak file archive yang sudah didownload dan buka melalui command prompt maupun PowerShell. Pastikan sudah masuk ke directory dari cockroach

    ```
    PS C:\Users\arvian> cd C:\cockroach-v19.2.0-beta.20190930.windows-6.2-amd64
    ```

3. Kemudian bisa kita cek versi dari cockroach ini untuk mengetahui juga bahwa cockroach siap digunakan.

    ```
    PS C:\cockroach-v19.2.0-beta.20190930.windows-6.2-amd64> .\cockroach version
    Build Tag:    v19.2.0-beta.20190930
    Build Time:   2019/10/02 15:03:35
    Distribution: CCL
    Platform:     windows amd64 (x86_64-w64-mingw32)
    Go Version:   go1.12.5
    C Compiler:   gcc 6.3.0
    Build SHA-1:  250f4c36de2b88eff443cf9be9cd5d2759312c88
    Build Type:   release
    ```

4. Langkah untuk melakukan instalasi cockroach menggunakan docker adalah sebagai berikut. Download docker terlebih dahulu di [docker](https://docs.docker.com/docker-for-windows/install/)

6. Kemudian install dan buka program docker untuk windows. Kemudian bisa kita cek versi dari docker.
    ```
    C:\Users\arvian>docker --version
    Docker version 19.03.2, build 6a30dfc
    ```

7. Selanjutnya untuk instalasi cockroach menggunakan docker.

	```
	C:\Users\arvian>docker pull cockroachdb/cockroach
	Using default tag: latest
	latest: Pulling from cockroachdb/cockroach
	27833a3ba0a5: Pull complete
	7eeeadc1cbce: Pull complete
	58d28f3214d5: Pull complete
	7c994667c40c: Pull complete
	Digest: sha256:44249e8133bd5c02165703854a86d84089fa741a018071cfe41b5ce4cda7ac39
	Status: Downloaded newer image for cockroachdb/cockroach:latest
	docker.io/cockroachdb/cockroach:latest
	```

7. Setelah itu silahkan buat Bridge Network pada docker.

	```
	C:\Users\arvian>docker network create -d bridge roachnet
	1f3ba672fe2ab44473730ce81de430782aadf9448fbca225a76991a19bb1e516
	```

8. Langkah selanjutnya silahkan jalankan service cockroachdb.

	```
	C:\Users\arvian>docker run -d --name=roach1 --hostname=roach1 --net=roachnet -p 26257:26257 -p 8080:8080 cockroachdb/cockroach start --insecure
	ab92733cc6459193137544d4a299bcbe4be8de0a98f9eace848fa8adad83a396
	```

9. Untuk memastikan sudah ready kita bisa cek container.

	```
	C:\Users\arvian>docker ps -a
	CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS                      PORTS                                              NAMES
	ab92733cc645        cockroachdb/cockroach   "/cockroach/cockroac…"   5 minutes ago       Up 5 minutes                0.0.0.0:8080->8080/tcp, 0.0.0.0:26257->26257/tcp   roach1
	02f6c477dedb        hello-world             "/hello"                 56 minutes ago      Exited (0) 56 minutes ago                                                      ecstatic_neumann
	```

10. Test Cluster.
    ```
    C:\Users\arvian>docker exec -it roach1 ./cockroach sql --insecure
    # Welcome to the cockroach SQL interface.
    # All statements must be terminated by a semicolon.
    # To exit: CTRL + D.
    #
    # Server version: CockroachDB CCL v19.1.5 (x86_64-unknown-linux-gnu, built 2019/09/23 14:12:16, go1.11.6) (same version as client)
    # Cluster ID: 637eb681-1428-4a71-b30e-3a84a9d0ebd4
    #
    # Enter \? for a brief introduction.
    #
    root@:26257/defaultdb>
    ```

11. Kemudian kita coba membuat database baru dengan nama arvian. Dan cek apakah database sudah berhasil dibuat.

	```
	root@:26257/defaultdb> create database arvian;
    CREATE DATABASE

    Time: 194.7431ms

    root@:26257/defaultdb> show databases;
    database_name
    +---------------+
    arvian
    defaultdb
    postgres
    system
    (4 rows)

    Time: 1.343ms
	```

12. Gunakan database yang sudah dibuat tadi.
    ```
    root@:26257/defaultdb> use arvian;
    SET

    Time: 420.5µs
    ```

13. Selanjutnya kita coba membuat tabel mhsisi serta menambahkan data ke dalam tabel ini. Dan kita cek sekalian.
    ```
    root@:26257/arvian> INSERT INTO arvian.mhsisi VALUES (1,175410041,'Arvian Eka Saputra');
    INSERT 1

    Time: 161.4963ms

    root@:26257/arvian> CREATE TABLE arvian.mhsisi (id INT PRIMARY KEY, nim INT, nama STRING);
    CREATE TABLE

    Time: 188.8227ms

    root@:26257/arvian> SELECT * FROM arvian.mhsisi;
    id |    nim    |        nama
    +----+-----------+--------------------+
    1 | 175410041 | Arvian Eka Saputra
    (1 row)

    Time: 1.1222ms
    ```
