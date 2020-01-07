# KUBERNETES

# Membuat sebuah Deployment menggunakan Python+Flask
Pod dalam Kubernetes adalah kumpulan dari satu atau banyak Kontainer yang saling terhubung untuk kebutuhan administrasi dan jaringan. Deployment dalam Kubernetes selalu memeriksa kesehatan Pod dan melakukan restart saat Kontainer di dalam Pod tersebut mati. Deployment digunakan untuk membuat dan mereplikasi Pod.
1. Menggunakan perintah `kubectl create` untuk membuat Deployment. Pod menjalankan Kontainer berdasarkan image docker yang digunakan. Disini saya menggunakan image docker lindaagustina/python-flask:v1 (image ini saya buat pada pertemuan 8, yang telah saya push ke Docker Hub). 

  `kubectl create deployment python-flask --image=lindaagustina/python-flask:v1`

  Output :

  `deployment.apps/python-flask created`
