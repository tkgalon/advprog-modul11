# advprog-modul11

Nama: Muhammad Zaid Ats Tsabit <br>
NPM: 2306224410 <br>
Kelas: Advprog-B
<hr>

1. Compare the application logs before and after you exposed it as a Service. Try to open the app several times while the proxy into the Service is running. What do you see in the logs? Does the number of logs increase each time you open the app?
![log1](/image/log1.jpeg)
![log2](/image/log2.jpeg)

Sebelum aplikasi diexpose sebagai Service, log hanya menunjukkan bahwa server HTTP dan UDP berhasil dijalankan. Namun setelah saya expose dan mengakses aplikasi melalui minikube service, log menunjukkan adanya banyak permintaan HTTP GET /. Ini membuktikan bahwa aplikasi berhasil menerima request dari luar cluster setelah Service dibuat. Setiap kali saya membuka URL dari minikube service, log mencatat request baru yang masuk. Jumlah baris log bertambah sesuai frekuensi saya mengakses aplikasi, membuktikan bahwa exposure berhasil.

2. Notice that there are two versions of `kubectl get` invocation during this tutorial section. The first does not have any option, while the latter has `-n` option with value set to
`kube-system`. What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?

Perintah kubectl get tanpa opsi -n menampilkan resource dari namespace default, yaitu tempat di mana membuat deployment dan servicenya. Sementara itu, perintah dengan opsi -n kube-system digunakan untuk melihat resource yang berjalan di namespace khusus sistem Kubernetes. Pod dan service buatan saya tidak muncul di output -n kube-system karena mereka tidak dibuat di namespace itu. Opsi -n sangat penting untuk menyaring resource berdasarkan scope namespace-nya. Ini memperjelas bahwa Kubernetes menggunakan isolasi namespace untuk mengelola resource secara terpisah dan terorganisir.


## Rolling Update & Kubernetes Manifest File

1. What is the difference between Rolling Update and Recreate deployment strategy?

Strategi Rolling Update mengganti versi aplikasi secara bertahap tanpa menghentikan layanan, sehingga pengguna tetap bisa mengakses aplikasi saat proses update berlangsung. Sedangkan, strategi Recreate akan menghentikan seluruh pod versi lama terlebih dahulu sebelum membuat versi baru, sehingga ada potensi downtime. Sehingga Rolling Update cocok untuk sistem yang butuh ketersediaan tinggi, sedangkan Recreate lebih simpel dan aman untuk kasus di mana downtime tidak masalah.

2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document
your attempt.

![step-recreate](/image/step-recreate.jpeg)
Mendelete dan mengapply dengan yaml baru, yaitu deployment-recreate.yaml

### Hasil Deployment ulang
![output](/image/output.jpeg)

3. Prepare different manifest files for executing Recreate deployment strategy.
Disini saya hanya mengganti strategi dari deployment yang sudah ada di tutorial. 

Dimana pada deployment.yaml menggunakan strategy RollingUpdate:
```
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
```

Sedangkan ddeployment-recreate.yaml menggunakan strategi Recreate:
```
  strategy:
    type: Recreate
```

4. What do you think are the benefits of using Kubernetes manifest files? Recall your experience in deploying the app manually and compare it to your experience when deploying the same app by applying the manifest files (i.e., invoking `kubectl apply -f` command) to the cluster.

Ketika deploy secara manual, saya harus mengetik banyak command seperti kubectl create deployment, kubectl expose, kubectl scale satu per satu yang cukup repetitive dan rawan error, sedangkan dengan manifest files cukup kubectl apply -f dan seluruh konfigurasi langsung terdeploy dengan konsisten. Dari situ saja sudah terlihat manfaat yang oke saat ingin melakukan deployment. Lalu, manifest files juga memberikan transparency yang lebih baik karena semua konfigurasi seperti jumlah replica, deployment strategy, port mapping, dan resource settings terlihat jelas dalam satu file YAML. Terakhir dengan adanya manifest file reproducibilitynya menjadi sangat mudah karena kita bisa deploy aplikasi yang sama persis di environments berbeda hanya dengan satu command aja.