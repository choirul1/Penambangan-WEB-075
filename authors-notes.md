# Clustering

## Pengertian 

Merupakan proses pengelompokan data menjadi sebuah beberapa kelompok baru, pengelompokan tersebut bisa berdasarkan beberapa ciri yang mirip antar data. Dalam kasus ini ciri yang mirip bisa kita ketahui dari kata yang menjadi ciri dari setiap data / dokumen.

## Metode

### K-mean Clustering

K-mean Clustering merupakan salah satu algoritma untuk melakukan clustering. Fungsi algoritma ini yaitu untuk membagi data menjadi beberapa kelompok berdasarkan class inputan yang diterima / jumlah kelompok (cluster) yang diinginkan. Algoritma ini akan mengelompokkan data atau objek ke dalam adalah data atau objek kedalam setiap class

#### Tahapan K-mean[¶](https://dianwib.github.io/WEB-MINING-1/Clustering/#tahapan-k-mean)

1. Menentukan jumlah cluster
2. Memilih K buah titik *centroid* secara random / acak sebanyak jumlah cluster yang diinginkan pada sebelumnya
3. Mengelompokkan data sehingga terbentuk K buah *cluster* dengan titik *centroid* dari setiap *cluster* merupakan titik *centroid* yang telah dipilih sebelumnya
4. Memerbaharui nilai titik *centroid*
5. Mengulangi langkah 3 dan 4 sampai nilai dari titik *centroid* tidak lagi berubah atau tidak ada perubahan objek pada setiap clas

```python
 # Clustering
kmeans = KMeans(n_clusters=5, random_state=0).fit(tfidf_matrix.todense())
write_csv("Kluster_label.csv", [kmeans.labels_])
for i in range(len(kmeans.labels_)):
    print("Doc %d =>> cluster %d" %(i+1, kmeans.labels_[i]))
```

*n_cluster menyatakan banyaknya cluster

### Silhoutte Coefisient[¶](https://dianwib.github.io/WEB-MINING-1/Clustering/#silhoutte-coefisient)

Silhoutte Coefisient merupakan salah satu metode yang dilakukan setelah melakukan clustering. Tujuan dari metode ini adalah untuk melihat seberapa cocok data dengan cluster

#### Tahapan Silhoutte[¶](https://dianwib.github.io/WEB-MINING-1/Clustering/#tahapan-silhoutte)

1. Mencari rata-rata jarak objek i dengan objek lainnya di dalam satu cluster **(ai)**.
2. Mencari rata-rata jarak objek i dengan objek lainnya di seluruh cluster lain, setelah menghitung semuanya, lalu pilih nilai terkecil dari seluruh jarak ke cluster lain **(bi)**.
3. Setelah itu hitung menggunakan rumus berikut :

![Material for MkDocs](https://dianwib.github.io/WEB-MINING-1/assets/images/WEecm.png)

```python
score = silhouette_score(df, preds, metric='euclidean')
```

## Implementasi

```python
banyak_cluster = list(range(2, 150))
for n_cluster in banyak_cluster:
    clusterer = KMeans(n_clusters=n_cluster)
    preds = clusterer.fit_predict((df))

    centers = clusterer.cluster_centers_
    score = silhouette_score(df, preds, metric='euclidean')
    temp.append(score)
    temp_pred.append(preds)

    # print ("Untuk kluster={},silhoute score :{} ".format(n_cluster, repr(score)))
    # print(preds)

print ("kluster terbaik")
print ("kluster ke > " + str(temp.index(max(temp)) + 2) + " >silhout> " + str(max(temp)))
```

### Clustering (K-mean) + Feature Selection (Random Forest)

```python
def RFE(cek, n_ranking):
    from sklearn.ensemble import RandomForestClassifier
    from sklearn.feature_selection import RFE
        rfe = RFE(RandomForestClassifier(n_estimators=50), n_features_to_select=1)
    rfe.fit(X, y)
    scores = []
    for i in range(num_features):
        scores.append((rfe.ranking_[i], X.columns[i]))

    print (sorted(scores, reverse=True))
    print_best_worst(scores, n_ranking)
```

### Clustering (K-mean) + Feature Selection (Chi Square).

```python
def UFS(cek, n_ranking):
    from sklearn.feature_selection import SelectKBest
    from sklearn.feature_selection import chi2, mutual_info_classif
        print ("UFS (Univariate Feature Selection) model chi^2 >>>>>>>")
        test = SelectKBest(score_func=chi2, k=2)
    test.fit(X, y)
    scores = []
    for i in range(num_features):
        score = test.scores_[i]
        scores.append((score, X.columns[i]))

    print (sorted(scores, reverse=True))
    print_best_worst(scores, n_ranking)
```

### Model Based Ranking model Random Forest

```python
def RandomForest():
    from sklearn.ensemble import RandomForestClassifier
    from sklearn.model_selection import cross_val_score
    clf = RandomForestClassifier(n_estimators=50, max_depth=4)
    scores = []
    num_features = len(X.columns)
    for i in range(num_features):
        col = X.columns[i]
        score = np.mean(cross_val_score(clf, X[col].values.reshape(-1, 1), y, cv=10))
        scores.append((int(score * 100), col))
        print (">>>>>>>>>>>>>>>>>>>>")
        print (i, ">", col, "score:", score)
```

#### Confustion matrix

Pada dasarnya, Confusion matrix hampir sama dengan akurasi, namun lebih detail. Jika Akurasi hanya melihat prediksi yang benar, maka confusion matrix melihat prediksi yang benar, dan prediksi yang salah.

Misalkan ada dua class "Merah" dan "Biru".

|           | Merah | Biru |
| --------- | ----- | ---- |
| **Merah** | 12    | 2    |
| **Biru**  | 4     | 15   |

Misalkan kolom adalah hasil prediksi dan baris merupakan label asli. Maka bisa kita baca seperti ini:

1. sebanyak 12 label "merah" diprediksi benar sebagai "merah"
2. sebanyak 2 label "merah" diprediksi salah sebagai "biru"
3. sebanyak 4 label "biru" diprediksi salah sebagai "merah"
4. sebanyak 15 label "biru" diprediksi benar sebagai "biru"

Code nya sebagai berikut:

```python
cm = confusion_matrix(y_test, predicted)
```

1. <https://www.researchgate.net/publication/323365687_Algoritme_Genetika_Untuk_Optimasi_K-Means_Clustering_Dalam_Pengelompokan_Data_Tsunami>