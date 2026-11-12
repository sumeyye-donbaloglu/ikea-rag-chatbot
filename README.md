# IKEA Türkiye RAG Chatbot

IKEA Türkiye'nin resmi web sayfalarındaki bilgilere dayanarak kullanıcı sorularını yanıtlayan, **Retrieval-Augmented Generation (RAG)** mimarisiyle geliştirilmiş bir chatbot projesidir.


## Kullanılan Teknolojiler

- **n8n:** Workflow otomasyonu
- **Docker Compose:** Servislerin çalıştırılması
- **Qdrant:** Vektör veritabanı
- **Ollama / nomic-embed-text:** Metinlerin embedding'e dönüştürülmesi
- **Google Gemini:** Yanıt üretimi

## Nasıl Çalışır?

Proje iki ayrı n8n workflow'undan oluşur.

**1. Document Indexing**

IKEA Türkiye'nin resmi sayfalarından içerikler alınır, metin parçalarına ayrılır ve embedding'leri oluşturularak Qdrant veritabanına kaydedilir.

**2. RAG Chatbot**

Kullanıcının sorusu alınır. Qdrant üzerinden ilgili içerikler aranır ve bulunan bilgiler Google Gemini'ye bağlam olarak verilerek yanıt oluşturulur.

## RAG Yapılandırması

| Parametre | Değer |
|---|---|
| Chunk Size | 1000 |
| Chunk Overlap | 200 |
| Embedding Model | nomic-embed-text |
| Embedding Dimension | 768 |
| Vector Database | Qdrant |
| Collection | ikea-knowledge |
| Retrieval | Similarity Search |
| Top K | 10 |
| LLM | Google Gemini |

## Veri Kaynakları

- [IKEA Sipariş, Değişim ve İade](https://www.ikea.com.tr/internet-magazasi/siparis-degisim-iade)
- [IKEA Nakliye Hizmeti](https://www.ikea.com.tr/nakliye-hizmeti)

## Kurulum

1. Projeyi bilgisayarınıza klonlayın.
2. Docker Compose ile n8n, Qdrant ve Ollama servislerini başlatın:

   `docker compose up -d`

3. Ollama üzerinden `nomic-embed-text` modelini indirin:

   `docker compose exec ollama ollama pull nomic-embed-text`

4. `http://localhost:5678` adresinden n8n arayüzünü açın.
5. JSON workflow dosyalarını n8n'e içe aktarın.
6. Google Gemini API kimlik bilgilerini n8n üzerinden yapılandırın.
7. Önce indeksleme workflow'unu çalıştırın, ardından chatbot'u test edin.

## Örnek Sorular

**Soru:** IKEA'dan aldığım ürünü kaç gün içinde iade edebilirim?

**Yanıt:** Teslimat tarihinden itibaren 14 gün içinde iade edebilirsiniz.

**Soru:** IKEA siparişimin teslimatını nasıl takip edebilirim?

**Yanıt:** Siparişinize ait kargo takip bilgileri üzerinden teslimat durumunu kontrol edebilirsiniz.

**Soru:** IKEA'nın 2030 yılındaki CEO'su kim olacak?

**Yanıt:** Bu konuda elimde doğrulanmış bilgi bulunmuyor.

## Notlar

- Proje eğitim ve portföy amacıyla hazırlanmıştır.
- IKEA ile resmi bir bağlantısı bulunmamaktadır.
- API anahtarları ve kişisel kimlik bilgileri repository içerisinde paylaşılmamaktadır.
