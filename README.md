# cmlabs-devops-freelance-test

Repository ini untuk menyelesaikan task yang ada yaitu "Buat File konfigurasi kubernetes dengan 2 node dan 3 pods per node, atur threshold memory di 128 Mb"

## Answer

Untuk menyelesaikan task ini saya tuliskan konfigurasi di file `3-pods-per-node.yaml`. Yang dimana didalamnya ada Object `Deployment` & `Service` Kubernetes. 

Dalam menjawab task ini saya membuat Kubernetes cluster di GCP. Mari kita asumsikan ada 2 node di Kubernetes cluster. Masing-masing node umumnya memiliki label standard yang bisa kita manfaatkan seperti `topology.kubernetes.io/zone` dan `kubernetes.io/hostname` seperti gambar dibawah ini

![Kubernetes node](/node.png)

Karena ekspetasi kita adalah ada 2 node dan ingin mendeploy 3 pods di masing-masing node, maka di Object Deployment saya mengkombinasikan dengan `NodeAffinity` dan `PodAntiAffinity`. 

Yang dimana di `NodeAffinity` saya menggunakan *soft rule* untuk memastikan pod di deploy di node dengan label `topology.kubernetes.io/zone` karena node di Kubernetes saya setting untuk berjalan multiple zone agar sistem kita *resilient* (High Avaibility)

Untuk `PodAntiAffinity` saya menggunakan *preffered rule* atau bisa juga disebut *hard rule*. Yang dimana saya memanfaatkan label `kubernetes.io/hostname` agar pod di di distribusikan ke semua hostname (node) yang ada tapi tetap sedikit patuh dengan `NodeAffinity` sebelumnya.

Maka jika lihat hasil akhirnya akan ada 3 pods di masing-masing node (2) seperti berikut

![Result](/result.png)