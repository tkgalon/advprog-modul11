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