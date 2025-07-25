# Axezi Restoran Chatbot RAG Workflow

Bu proje, Axezi restoranı için yapay zekâ destekli bir soru-cevap asistanı (chatbot) oluşturmak amacıyla iki ayrı n8n workflow içerir. Bu sistem, RAG (Retrieval-Augmented Generation) mimarisi kullanılarak geliştirilmiştir.

## 🔧 Kullanılan Teknolojiler

- 🧠 OpenAI Embeddings & GPT-4o-mini
- 🔍 Pinecone Vector Store
- 📂 Google Drive API (veri kaynağı olarak)
- 🔁 n8n (Workflow otomasyon aracı)

---

## 1. `Rag.json`: Veri Hazırlama Workflow'u

### Görevi:
Google Drive'daki belgeleri indirir, parçalara böler, embed vektörlerine dönüştürür ve Pinecone'a yükler.

### Akış:

1. Manuel başlatılır
2. Google Drive klasöründen dosyalar alınır
3. Her dosya:
   - İndirilir
   - OpenAI ile embed edilir
   - Küçük parçalara bölünür (chunking)
   - Pinecone'a namespace: `restoran` olarak yüklenir

---

## 2. `Rag2.json`: Chat Agent Workflow'u

### Görevi:
Kullanıcıdan gelen mesajları karşılayarak, sadece Vector Store'dan alınan bilgilerle yanıt verir.

### Özellikler:

- Cevaplar `gpt-4o-mini` modeli ile oluşturulur
- Samimi, kullanıcı dostu cevaplar verir
- Her cevabın sonunda emoji bulunur 😊
- Sadece `Vector Store` (Pinecone) datası kullanılır
- Önceki konuşmaları hatırlamak için kısa süreli bellek (`Simple Memory`) kullanılır

---

## 📌 Notlar

- Chatbot hiçbir zaman kendi arama yapmaz, yalnızca yüklenen veriyle çalışır.
- Daha fazla veri eklemek için `Rag.json` workflow'u yeniden çalıştırılmalıdır.
- Cevaplar kısa, net ve dostane olmalıdır.

---
 1. Rag.json – Veri Hazırlık ve Vektör Store'a Yükleme Workflow'u
Bu workflow'un amacı: Google Drive'dan belgeleri alıp OpenAI kullanarak embed (vektör) haline getirip Pinecone Vector Store'a yüklemektir. Bu, Retrieval-Augmented Generation (RAG) sisteminin bilgi deposunu hazırlamak için kullanılır.


Trigger: Manuel olarak çalıştırılır (manualTrigger).

Google Drive taraması: Belirli bir klasördeki dosyaları arar ve listeler.

Dosya indirme: Listelenen her dosyayı indirir.

İşleme döngüsü: Her dosya tek tek işlenmek üzere döngüye sokulur.

Embed oluşturma: Dosyadaki içerik, OpenAI Embeddings ile vektörleştirilir.

Veri parçalama: Metin içerikleri küçük parçalara bölünür (textSplitter) ve ardından Default Data Loader ile formatlanır.

Vector Store’a ekleme: Pinecone üzerinde belirtilen namespace'e (restoran) yüklenir.

🤖 2. Rag2.json – Sohbet Tabanlı AI Ajan Workflow'u
Bu workflow, kullanıcıdan gelen mesajlara yanıt veren bir Chat Agent içindir. AI, sadece Pinecone’daki verileri kullanarak (yani yukarıdaki sistemle yüklenmiş olanları) soruları yanıtlar.

Chat tetikleyici: Kullanıcı bir mesaj gönderdiğinde devreye girer.

AI Agent: Girdi mesajı alır, belirlenmiş davranış kurallarına göre işler.

Sistem mesajı: "Sadece Vector Store'u kullan, samimi ol ve her cevaba emoji ekle."

Dil modeli: gpt-4o-mini kullanılır.

Vektör Arama: Pinecone Vector Store’daki verileri kullanır (namespace: restoran).

Embedding: Yeni içerik gerekiyorsa embed oluşturmak için kullanılır.

Bellek: Önceki konuşmaları hatırlamak için Simple Memory kullanılır.




## 🚀 Kurulum

1. n8n kurulu değilse yükleyin.
2. Her iki `.json` dosyasını içeri aktarın.
3. Gerekli API bağlantı bilgilerini girin:
   - OpenAI
   - Pinecone
   - Google Drive
4. Önce `Rag.json` çalıştırılarak veri yüklenmelidir.
5. Daha sonra `Rag2.json` ile chatbot aktif hale gelir.

---

## 🧩 Katkıda Bulunmak

İyileştirme önerilerinizi ve yeni veri setlerini bizimle paylaşabilirsiniz!

