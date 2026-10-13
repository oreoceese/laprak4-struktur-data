# <h1 align="center">Laporan Praktikum Modul 4 - SINGLY LINKED LIST (BAGIAN PERTAMA)</h1>
<p align="center">Zuhra Hanani Ramadhina - 109082500143</p>

## Dasar Teori
Linked list atau senarai berantai adalah salah satu bentuk struktur data yang berisi kumpulan data tersusun secara sekuensial, saling bersambungan, dinamis, dan terbatas[1]. Linked list terdiri dari sejumlah node yang dihubungkan secara linier dengan bantuan pointer. Berbeda dengan array yang ukurannya tetap, linked list dapat bertambah dan berkurang ukurannya saat program berjalan karena memori untuk tiap node dialokasikan secara dinamis[2]. Pada singly linked list, setiap node hanya memiliki satu pointer `next` yang menunjuk ke node berikutnya, dan node terakhir menunjuk ke `Nil` (NULL) sebagai penanda akhir list.

### A. Struktur Node dan Manajemen Memori<br/>
Setiap node pada singly linked list terdiri dari dua bagian, yaitu data (`isidata`) dan pointer `next`. Node dibuat dengan `new` (alokasi) dan dihapus dengan `delete` (dealokasi) agar memori yang tidak terpakai dikembalikan ke sistem. Penunjuk awal list disebut `first`, dan list dikatakan kosong jika `first` bernilai `Nil`.

#### 1. Node — terdiri dari data dan pointer `next` ke node berikutnya
#### 2. Alokasi (`new`) — membuat node baru di memori
#### 3. Dealokasi (`delete`) — mengembalikan memori node yang dihapus ke sistem

### B. Operasi pada Singly Linked List<br/>
Operasi dasar pada linked list meliputi penambahan node (insert), penghapusan node (delete), dan penelusuran list untuk menampilkan isi maupun menghitung jumlah node.

#### 1. Insert — insertFirst (di awal), insertAfter (setelah node tertentu), insertLast (di akhir)
#### 2. Delete — delFirst (hapus awal), delAfter (hapus setelah node tertentu), delLast (hapus akhir)
#### 3. Traversal — printList untuk menampilkan isi dan nbList untuk menghitung jumlah node
## Guided 

### 1. list.h

```C++
//hear guard digunakan untuk mencegah file header yg sama
//di-include lebih dari sekali dalam 1 program
#ifndef LIST_H
#define LIST_H
#define Nil NULL

#include<iostream>
using namespace std;

//deklarasi isi data struct mahasiswa
struct mahasiswa {
    string nama;
    string nim;
    int umur;
};

typedef mahasiswa dataMahasiswa; //memberikan nama alias dataMahasiswa untuk struct mahasiswa

typedef struct node *address; //mendefinisikan alias address sebagai pointer ke struct node

struct node { //node untuk isi dari linked listnya, isis setiap node adalah data & pointer next
    dataMahasiswa isidata;
    address next;
};

struct linkedList { //ini linked listnya
    address first;
};

//semua function & prosedure yang akan dipakai
bool isEmpty(linkedList list);
void createList(linkedList &list);
address alokasi(string nama, string nim, int umur);
void dealokasi(address &node);
void printList(linkedList list);
void insertFirst(linkedList &list, address nodeBaru);
void insertAfter(linkedList &list, address nodeBaru, address Prev);
void insertLast(linkedList &list, address nodeBaru);

void delFirst(linkedList &list);
void delLast(linkedList &list);
void delAfter(linkedList &list, address nodeHapus, address nodePrev);
int nbList(linkedList list);
void deleteList(linkedList &list);

#endif
```
File header ini mendefinisikan struct mahasiswa (nama, NIM, umur), lalu struct node yang berisi data mahasiswa dan pointer next, serta struct linkedList yang menyimpan pointer first sebagai penunjuk node pertama. File ini juga mendeklarasikan prototipe seluruh fungsi dan prosedur yang dipakai, dan typedef dipakai untuk membuat alias seperti address (pointer ke node). Include guard (#ifndef) mencegah header terbaca lebih dari sekali.

### 2. list.cpp

```C++
#include "list.h"
#include <iostream>
using namespace std;

//I.S = Initial State / kondisi awal
//F.S = Final State / kondisi akhir

//fungsi untuk cek apakah list kosong atau tidak
bool isEmpty(linkedList List) {
    if(List.first == Nil){
        return true;
    } else {
        return false;
    }
}

//pembuatan linked list kosong
void createList(linkedList &List) {
    /* I.S. sembarang
       F.S. terbentuk list kosong */
    List.first = Nil;
}

//pembuatan node baru dengan menerapkan manajemen memori
address alokasi(string nama, string nim, int umur) {
    /* I.S. sembarang
       F.S. mengembalikan alamat node baru dengan isidata = sesuai parameter dan next = Nil */
    address nodeBaru = new node;
    nodeBaru->isidata.nama = nama;
    nodeBaru->isidata.nim = nim;
    nodeBaru->isidata.umur = umur;
    nodeBaru->next = Nil;
    return nodeBaru;
}

//penghapusan node dengan menerapkan manajemen memori
void dealokasi(address &node) {
    /* I.S. P terdefinisi
       F.S. memori yang digunakan node dikembalikan ke sistem */
    node->next = Nil;
    delete node;
}

//prosedur-prosedur untuk insert / menambahkan node baru kedalam list
void insertFirst(linkedList &List, address nodeBaru) {
    /* I.S. sembarang, P sudah dialokasikan
       F.S. menempatkan elemen list (node) pada awal list */
    nodeBaru->next = List.first;
    List.first = nodeBaru;
}

void insertAfter(linkedList &List, address nodeBaru, address Prev) {
    /* I.S. sembarang, nodeBaru dan Prev alamat salah satu elemen list (node)
       F.S. menempatkan elemen (node) sesudah elemen node Prev */
    if (Prev != Nil) {
        nodeBaru->next = Prev->next;
        Prev->next = nodeBaru;
    } else {
        cout << "Node sebelumnya tidak valid!" << endl;
    }
}

void insertLast(linkedList &List, address nodeBaru) {
    /* I.S. sembarang, nodeBaru sudah dialokasikan
       F.S. menempatkan elemen nodeBaru pada akhir list */
    if (isEmpty(List)) {
        List.first = nodeBaru;
    } else {
        address nodeBantu = List.first;
        while (nodeBantu->next != Nil) {
            nodeBantu = nodeBantu->next;
        }
        nodeBantu->next = nodeBaru;
    }
}

//prosedur-prosedur untuk delete / menghapus node yang ada didalam list
void delFirst(linkedList &List){
    /* I.S. list tidak kosong
       F.S. node pertama di list terhapus*/
    address nodeHapus;
    if (isEmpty(List) == false) {
        nodeHapus = List.first;
        List.first = List.first->next;
        nodeHapus->next = Nil;
        dealokasi(nodeHapus);
    } else {
        cout << "List kosong!" << endl;
    }
}

void delLast(linkedList &List){
    /* I.S. list tidak kosong
       F.S. node terakhir di list terhapus */
    address nodeHapus, nodePrev;
    if(isEmpty(List) == false){
        nodeHapus = List.first;
        if(nodeHapus->next == Nil){
            List.first->next = Nil;
            dealokasi(nodeHapus);
        } else {
            while(nodeHapus->next != Nil){
                nodePrev = nodeHapus;
                nodeHapus = nodeHapus->next;
            }
            nodePrev->next = Nil;
            dealokasi(nodeHapus);
        }
    } else {
        cout << "list kosong" << endl;
    }
}

void delAfter(linkedList &List, address nodeHapus, address nodePrev){
    /* I.S. list tidak kosng, Prev alamat salah satu elemen list
       F.S. nodeBantu adalah alamat dari Prev->next, menghapus Prev->next dari list */
    if(isEmpty(List) == true){
        cout << "List kosong!" << endl;
    } else { //jika list tidak kosong
        if (nodePrev != Nil && nodePrev->next != Nil) {
            nodeHapus = nodePrev->next;
            nodePrev->next = nodeHapus->next;
            nodeHapus->next = Nil;
            dealokasi(nodeHapus);
        } else {
            cout << "Node sebelumnya (prev) tidak valid!" << endl;
        }
    }
}

//prosedur untuk menampilkan isi list
void printList(linkedList List) {
    /* I.S. list mungkin kosong
       F.S. jika list tidak kosong menampilkan semua info yang ada pada list */
    if (isEmpty(List)) {
        cout << "List kosong." << endl;
    } else {
        address nodeBantu = List.first;
        while (nodeBantu != Nil) {
            cout << "Nama : " << nodeBantu->isidata.nama << ", NIM : " << nodeBantu->isidata.nim << ", Usia : " << nodeBantu->isidata.umur << endl;
            nodeBantu = nodeBantu->next;
        }
    }
}

//function untuk menampilkan jumlah node didalam list
int nbList(linkedList List) {
    /* I.S. list sudah ada
       F.S. menampilkan jumlah node didalam list*/
    int count = 0;
    address nodeBantu = List.first;
    while (nodeBantu != Nil) {
        count++;
        nodeBantu = nodeBantu->next;
    }
    return count;
}

//prosedur untuk menghapus list (menghapus semua node didalam list)
void deleteList(linkedList &List){
    /* I.S. list sudah ada
       F.S. menghapus semua node didalam list*/
    address nodeBantu, nodeHapus;
    nodeBantu = List.first;
    while(nodeBantu != Nil){
        nodeHapus = nodeBantu;
        nodeBantu = nodeBantu->next;
        dealokasi(nodeHapus);
    }
    List.first = Nil;
    cout << "List sudah terhapus!" << endl;
}
```
File ini berisi implementasi semua operasi linked list. createList() membuat list kosong, alokasi() membuat node baru dengan new, dan dealokasi() menghapus node dengan delete. Prosedur insert (insertFirst, insertAfter, insertLast) menyambungkan node baru ke posisi yang sesuai dengan mengatur pointer next, sedangkan prosedur delete (delFirst, delAfter, delLast) melepas node dari rantai lalu mendealokasinya. Sisanya, printList() menelusuri list dari awal sampai Nil untuk menampilkan isinya, nbList() menghitung jumlah node, dan deleteList() menghapus semua node.

### 3. main.cpp

```C++
#include "list.h"

#include<iostream>
using namespace std;

int main() {
    linkedList List;
    address nodeA, nodeB, nodeC, nodeD, nodeE = Nil;
    createList(List);

    dataMahasiswa mhs;

    nodeA = alokasi("Dhimas", "2311102151", 20);
    nodeB = alokasi("Arvin", "2211110014", 21);
    nodeC = alokasi("Rizal", "2311110029", 20);
    nodeD = alokasi("Satrio", "2211102173", 21);
    nodeE = alokasi("Joshua", "2311102133", 21);

    insertFirst(List, nodeA);
    insertLast(List, nodeB);
    insertAfter(List, nodeC, nodeA);
    insertAfter(List, nodeD, nodeC);
    insertLast(List, nodeE);

    cout << "--- ISI LIST SETELAH DILAKUKAN INSERT ---" << endl;
    printList(List);
    cout << "jumlah node : " << nbList(List) << endl;
    cout << endl;

    delFirst(List);
    delLast(List);
    delAfter(List, nodeD, nodeC);

    cout << "--- ISI LIST SETELAH DILAKUKAN DELETE ---" << endl;
    printList(List);
    cout << "jumlah node : " << nbList(List) << endl;
    cout << endl;

    deleteList(List);
    cout << "--- ISI LIST SETELAH DILAKUKAN HAPUS LIST ---" << endl;
    printList(List);
    cout << "jumlah node : " << nbList(List) << endl;
    cout << endl;

    return 0;
}
```
Program utama membuat list kosong dan lima node mahasiswa (nodeA sampai nodeE), lalu menyusunnya dengan insert pertama, kedua, dan seterusnya sehingga urutan list menjadi A, C, D, B, E. Setelah isi list dan jumlah node ditampilkan, program menjalankan delFirst (hapus A), delLast (hapus E), dan delAfter (hapus D setelah C) sehingga tersisa C dan B. Terakhir deleteList() menghapus semua node dan program menampilkan list yang sudah kosong.

## Unguided 

### 1. Program ADT Singly Linked List Menggunakan insertFirst dan printInfo

#### Source Code singlylist.h
```C++
#ifndef SINGLYLIST_H
#define SINGLYLIST_H
#define Nil NULL

#include <iostream>
using namespace std;

typedef int infotype;
typedef struct ElmList *address;

struct ElmList {
    infotype info;
    address next;
};

struct List {
    address First;
};

void CreateList(List &L);
address alokasi(infotype x);
void dealokasi(address &P);
void printInfo(List L);
void insertFirst(List &L, address P);

void deleteFirst(List &L);
void deleteLast(List &L);
void deleteAfter(List &L, address Prev);
int nbList(List L);
void deleteList(List &L);

#endif
```
File header ini mendefinisikan ADT singly linked list. infotype adalah alias dari int, address adalah pointer ke struct ElmList, dan ElmList berisi data (info) serta pointer next ke node berikutnya. Struct List menyimpan pointer First sebagai penunjuk node pertama. File ini juga mendeklarasikan prototipe semua prosedur dan fungsi yang dipakai, dan #define Nil NULL dipakai sebagai penanda akhir list.

#### Source Code singlylist.cpp
```C++
#include "Singlylist.h"

void CreateList(List &L) {
    L.First = Nil;
}

address alokasi(infotype x) {
    address P = new ElmList;
    P->info = x;
    P->next = Nil;
    return P;
}

void dealokasi(address &P) {
    P->next = Nil;
    delete P;
}

void printInfo(List L) {
    address P = L.First;
    while (P != Nil) {
        cout << P->info << " ";
        P = P->next;
    }
    cout << endl;
}

void insertFirst(List &L, address P) {
    P->next = L.First;
    L.First = P;
}

void deleteFirst(List &L) {
    if (L.First != Nil) {
        address P = L.First;
        L.First = L.First->next;
        dealokasi(P);
    }
}

void deleteLast(List &L) {
    if (L.First != Nil) {
        if (L.First->next == Nil) {
            address P = L.First;
            L.First = Nil;
            dealokasi(P);
        } else {
            address Prev = L.First;
            while (Prev->next->next != Nil) {
                Prev = Prev->next;
            }
            address P = Prev->next;
            Prev->next = Nil;
            dealokasi(P);
        }
    }
}

void deleteAfter(List &L, address Prev) {
    if (Prev != Nil && Prev->next != Nil) {
        address P = Prev->next;
        Prev->next = P->next;
        dealokasi(P);
    }
}

int nbList(List L) {
    int count = 0;
    address P = L.First;
    while (P != Nil) {
        count++;
        P = P->next;
    }
    return count;
}

void deleteList(List &L) {
    address P;
    while (L.First != Nil) {
        P = L.First;
        L.First = L.First->next;
        dealokasi(P);
    }
    cout << "- List Berhasil Terhapus -" << endl;
}
```
File ini mengimplementasikan operasi dasar linked list. CreateList() membuat list kosong dengan mengisi First dengan Nil. alokasi() membuat node baru dengan new dan mengisi datanya, sedangkan dealokasi() menghapus node dengan delete. insertFirst() menyambungkan node baru ke depan list, dan printInfo() menelusuri list dari First sampai Nil untuk menampilkan isi tiap node.

#### Source Code main.cpp
```C++
#include "Singlylist.h"

int main() {
    List L;
    address P1, P2, P3, P4, P5 = Nil;
    CreateList(L);

    P1 = alokasi(2);
    insertFirst(L, P1);

    P2 = alokasi(0);
    insertFirst(L, P2);

    P3 = alokasi(8);
    insertFirst(L, P3);

    P4 = alokasi(12);
    insertFirst(L, P4);

    P5 = alokasi(9);
    insertFirst(L, P5);

    printInfo(L);
    return 0;
}
```
Program utama membuat list kosong, lalu mengalokasikan lima node dengan nilai 2, 0, 8, 12, dan 9. Setiap node dimasukkan dengan insertFirst(), sehingga node yang dimasukkan terakhir menjadi node pertama. Karena itu urutan list menjadi 9, 12, 8, 0, 2, sesuai output yang diminta soal.

### Output Unguided 1 :

##### Output 
![Screenshot Output Unguided 1](https://github.com/oreoceese/laprak4-struktur-data/blob/6e8e67ed9adcd69dad101bf69c047c56cf7fcacb/4-soal1.png)


### 2. Dari soal Latihan pertama, lakukan penghapusan node 9 menggunakan deleteFirst(), node 2 menggunakan deleteLast(), dan node 8 menggunakan deleteAfter(). Kemudian tampilkan jumlah node yang tersimpan menggunakan nbList() dan lakukan penghapusan seluruh node menggunakan deleteList(). 


#### Source Code main2.cpp
```C++
#include "Singlylist.h"

int main() {
    List L;
    address P1, P2, P3, P4, P5 = Nil;
    CreateList(L);

    P1 = alokasi(2);
    insertFirst(L, P1);
    P2 = alokasi(0);
    insertFirst(L, P2);
    P3 = alokasi(8);
    insertFirst(L, P3);
    P4 = alokasi(12);
    insertFirst(L, P4);
    P5 = alokasi(9);
    insertFirst(L, P5);

    deleteFirst(L);        // hapus node 9
    deleteLast(L);         // hapus node 2
    deleteAfter(L, P4);    // hapus node 8 (node setelah 12)

    printInfo(L);
    cout << "Jumlah node : " << nbList(L) << endl << endl;

    deleteList(L);
    cout << "Jumlah node : " << nbList(L) << endl;

    return 0;
}
```
Program membentuk list yang sama seperti soal 1 (9, 12, 8, 0, 2), lalu menghapus node 9 dengan deleteFirst(), node 2 dengan deleteLast(), dan node 8 dengan deleteAfter() setelah node 12. Sisa list adalah 12 dan 0, dengan jumlah node 2 dari nbList(). Terakhir deleteList() menghapus seluruh node, sehingga jumlah node menjadi 0.

### Output Unguided 2 :

#### Output
![Screenshot Output Unguided 2](https://github.com/oreoceese/laprak4-struktur-data/blob/6e8e67ed9adcd69dad101bf69c047c56cf7fcacb/4-soal2.png)


## Kesimpulan
Berdasarkan praktikum yang telah dilakukan, dapat disimpulkan bahwa singly linked list merupakan struktur data dinamis yang tersusun dari node-node yang saling terhubung melalui pointer `next`, sehingga ukurannya dapat bertambah dan berkurang saat program berjalan tanpa harus ditentukan di awal seperti array. Operasi dasar pada linked list, yaitu insert, delete, dan penelusuran (traversal), dilakukan dengan mengatur pointer antar node dan harus dilakukan dengan hati-hati agar rantai node tidak terputus. Penerapan manajemen memori menggunakan `new` dan `delete` juga penting agar memori node yang tidak terpakai dikembalikan ke sistem. Melalui latihan guided dan unguided, mahasiswa dilatih membangun ADT singly linked list dalam program multi-file serta menerapkan operasi penambahan, penghapusan, dan penghitungan node.

## Referensi
[1] Triase. (2020). Diktat Edisi Revisi : STRUKTUR DATA. Medan: UNIVERSTAS ISLAM NEGERI SUMATERA UTARA MEDAN.
<br>[2] Indahyanti, Uce., Rahmawati, Yunianita. (2020). *Buku Ajar Algoritma dan Pemrograman dalam Bahasa C++*. Sidoarjo: UMSIDA Press. Diakses melalui https://doi.org/10.21070/2020/978-623-6833-67-4.
