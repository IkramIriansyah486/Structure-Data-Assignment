# <h1 align="center"> Laporan Praktikum Modul 5_Hash Table</h1>
<p align="center"> Ikram Iriansyah <p>
<p align="center"> 2311102184 </p>

###  TUJUAN PRAKTIKUM
a.	Mahasiswa mampu menjelaskan definisi dan konsep dari Hash Code
b.	Mahasiswa mampu menerapkan Hash Code kedalam pemrograman

###  DASAR TEORI
a.	Pengertian Hash Table
Hash Table adalah struktur data yang mengorganisir data ke dalam pasangan kunci-nilai. Hash table biasanya terdiri dari dua komponen utama: array (atau vektor) dan fungsi hash. Hashing adalah teknik untuk mengubah rentang nilai kunci menjadi rentang indeks array.
Array menyimpan data dalam slot-slot yang disebut bucket. Setiap bucket dapat menampung satu atau beberapa item data. Fungsi hash digunakan untuk menghasilkan nilai unik dari setiap item data, yang digunakan sebagai indeks array. Dengan cara ini, hash table memungkinkan pencarian data dalam waktu yang konstan (O(1)) dalam kasus terbaik.
Sistem hash table bekerja dengan cara mengambil input kunci dan memetakkannya ke nilai indeks array menggunakan fungsi hash. Kemudian, data disimpan pada posisi indeks array yang dihasilkan oleh fungsi hash. Ketika data perlu dicari, input kunci dijadikan sebagai parameter untuk fungsi hash, dan posisi indeks array yang dihasilkan digunakan untuk mencari data. Dalam kasus hash collision, di mana dua atau lebih data memiliki nilai hash yang sama, hash table menyimpan data tersebut dalam slot yang sama dengan Teknik yang disebut chaining.
![Screenshot 2024-04-18 003005](https://github.com/IkramIriansyah486/Structure-Data-Assignment/assets/88477950/9e7cf109-d300-4f5a-a755-b60d41291f4b)

### Fungsi Hash Table
Fungsi hash membuat pemetaan antara kunci dan nilai, hal ini dilakukan melalui penggunaan rumus matematika yang dikenal sebagai fungsi hash. Hasil dari fungsi hash disebut sebagai nilai hash atau hash. Nilai hash adalah representasi dari string karakter asli tetapi biasanya lebih kecil dari aslinya.

### Operasi Hash Table
1.	Insertion:
Memasukkan data baru ke dalam hash table dengan memanggil fungsi hash untuk menentukan posisi bucket yang tepat, dan kemudian menambahkan data ke bucket tersebut.
2.	Deletion:
Menghapus data dari hash table dengan mencari data menggunakan fungsi hash, dan kemudian menghapusnya dari bucket yang sesuai.
3.	Searching:
Mencari data dalam hash table dengan memasukkan input kunci ke fungsi hash untuk menentukan posisi bucket, dan kemudian mencari data di dalam bucket yang sesuai.
4.	Update:
Memperbarui data dalam hash table dengan mencari data menggunakan fungsi hash, dan kemudian memperbarui data yang ditemukan.
5.	Traversal:
Melalui seluruh hash table untuk memproses semua data yang ada dalam tabel.

### Collision Resolution
Keterbatasan tabel hash adalah jika dua angka dimasukkan ke dalam fungsi hash menghasilkan nilai yang sama. Hal ini disebut dengan collision. Ada dua teknik untuk menyelesaikan masalah ini diantaranya :
![Screenshot 2024-04-18 003028](https://github.com/IkramIriansyah486/Structure-Data-Assignment/assets/88477950/52ba1339-642f-4867-87b2-8898b5901ea5)


### 1.	Open Hashing (Chaining)
Metode chaining mengatasi collision dengan cara menyimpan semua item data dengan nilai indeks yang sama ke dalam sebuah linked list. Setiap node pada linked list merepresentasikan satu item data. Ketika ada pencarian atau penambahan item data, pencarian atau penambahan dilakukan pada linked list yang sesuai dengan indeks yang telah dihitung dari kunci yang di hash. Ketika linked list memiliki banyak node, pencarian atau penambahan item data menjadi lambat, karena harus mencari di seluruh linked list. Namun, chaining dapat mengatasi jumlah item data yang besar dengan efektif, karena keterbatasan array dihindari.
### 2.	Closed Hashing
●	Linear Probing
Pada saat terjadi collision, maka akan mencari posisi yang kosong di bawah tempat terjadinya collision, jika masih penuh terus ke bawah, hingga ketemu tempat yang kosong. Jika tidak ada tempat yang kosong berarti HashTable sudah penuh.
●	Quadratic Probing
Penanganannya hampir sama dengan metode linear, hanya lompatannya tidak satu-satu, tetapi quadratic ( 12, 22, 32, 42, ... )
●	Double Hashing
Pada saat terjadi collision, terdapat fungsi hash yang kedua untuk menentukan posisinya kembali.

## Guided 

### 1.	Guided I

```C++
#include <iostream>
using namespace std;
const int MAX_SIZE = 10;
// Fungsi hash sederhana
int hash_func(int key)
{
return key % MAX_SIZE;
}
// Struktur data untuk setiap node
struct Node
{
int key;
int value;
Node *next;
Node(int key, int value) : key(key), value(value), next(nullptr) {}
};
// Class hash table
class HashTable
{
private:
Node **table;
public:
HashTable()
{
table = new Node *[MAX_SIZE]();
}
~HashTable()
{
for (int i = 0; i < MAX_SIZE; i++)
{
Node *current = table[i];
while (current != nullptr)
{
Node *temp = current;
current = current->next;
delete temp;
}
}
delete[] table;
}
// Insertion
void insert(int key, int value)
{
int index = hash_func(key);
Node *current = table[index];
while (current != nullptr)
{
if (current->key == key)
{
current->value = value;
return;
}
current = current->next;
}
Node *node = new Node(key, value);
node->next = table[index];
table[index] = node;
}
// Searching
int get(int key)
{
int index = hash_func(key);
Node *current = table[index];
while (current != nullptr)
{
if (current->key == key)
{
return current->value;
}
current = current->next;
}
return -1;
}
// Deletion
void remove(int key)
{
int index = hash_func(key);
Node *current = table[index];
Node *prev = nullptr;
while (current != nullptr)
{
if (current->key == key)
{
if (prev == nullptr)
{
table[index] = current->next;
}
else
{
prev->next = current->next;
}
delete current;
return;
}
prev = current;
current = current->next;
}
}
// Traversal
void traverse()
{
for (int i = 0; i < MAX_SIZE; i++)
{
Node *current = table[i];
while (current != nullptr)
{
cout << current->key << ": " << current->value << endl;
current = current->next;
}
}
}
};
int main()
{
HashTable ht;
// Insertion
ht.insert(1, 10);
ht.insert(2, 20);
ht.insert(3, 30);
// Searching
cout << "Get key 1: " << ht.get(1) << endl;
cout << "Get key 4: " << ht.get(4) << endl;
// Deletion
ht.remove(4);
// Traversal
ht.traverse();
return 0;
}
```
### Screenshot Progam
![Screenshot 2024-04-18 003812](https://github.com/IkramIriansyah486/Structure-Data-Assignment/assets/88477950/cf75c0d4-5b94-4c0f-a73c-c58ee016b823)

### Deskripsi Progam

Kode di atas digunakan untuk menjalankan hash table sederhana. Pada struct Node terdapat key & value bertipe data integer, Node* next, dan deklarasi Node. Ada juga class HashTable di mana ada private class dan public class. Dalam private class, dideklarasikan Node** table dan di dalam public class dideklarasikan HashTable(), ~HashTable(), dan fungsi lainnya yang dapat dipakai di int main(). Fungsi lainnya yang dipanggil di int main() adalah:
- void insert(int key, int value)</br>
    Digunakan untuk menambahkan key dan value ke dalam hash table
- int get(int key)</br>
    Digunakan untuk mencari key yang sudah ditambahkan user
- void remove(int key)</br>
    Digunakan untuk menghapus key dari hash table
- void traverse()</br> 
    Digunakan untuk menampilkan key dan value dalam hash table</br>
<p>Dalam int main(), HashTable dideklarasikan dengan objek ht. Program menambahkan tiga key dengan value 10, 20, dan 30 melalui ht.insert(). Lalu, dicari key 1 dan 4 dengan ht.get(). Selanjutnya, key 4 dihapus dengan ht.remove(). Pada akhirnya, program menampilkan key dan valuenya melalui ht.traverse() dengan outputnya adalah "1: 10, 2: 20, 3: 30".</p>

### 2.	Guided II

```C++
#include <iostream>
#include <string>
#include <vector>
using namespace std;
const int TABLE_SIZE = 11;
string name;
string phone_number;
class HashNode
{
public:
string name;
string phone_number;
HashNode(string name, string phone_number)
{
this->name = name;
this->phone_number = phone_number;
}
};
class HashMap
{
private:
vector<HashNode *> table[TABLE_SIZE];
public:
int hashFunc(string key)
{
int hash_val = 0;
for (char c : key)
{
hash_val += c;
}
return hash_val % TABLE_SIZE;
}
void insert(string name, string phone_number)
{
int hash_val = hashFunc(name);
for (auto node : table[hash_val])
{
if (node->name == name)
{
node->phone_number = phone_number;
return;
}
}
table[hash_val].push_back(new HashNode(name, phone_number));
}
void remove(string name)
{
int hash_val = hashFunc(name);
for (auto it = table[hash_val].begin(); it != table[hash_val].end();
it++)
{
if ((*it)->name == name)
{
table[hash_val].erase(it);
return;
}
}
}
string searchByName(string name)
{
int hash_val = hashFunc(name);
for (auto node : table[hash_val])
{
if (node->name == name)
{
return node->phone_number;
}
}
return "";
}
void print()
{
for (int i = 0; i < TABLE_SIZE; i++)
{
cout << i << ": ";
for (auto pair : table[i])
{
if (pair != nullptr)
{
cout << "[" << pair->name << ", " << pair->phone_number << "]";
}
}
cout << endl;
}
}
};
int main()
{
HashMap employee_map;
employee_map.insert("Mistah", "1234");
employee_map.insert("Pastah", "5678");
employee_map.insert("Ghana", "91011");
cout << "Nomer Hp Mistah : " << employee_map.searchByName("Mistah") << endl;
cout << "Phone Hp Pastah : " << employee_map.searchByName("Pastah") << endl;
employee_map.remove("Mistah");
cout << "Nomer Hp Mistah setelah dihapus : " << employee_map.searchByName("Mistah") << endl
<< endl;
cout << "Hash Table : " << endl;
employee_map.print();
return 0;
}
```
### Screenshot Progam
![Screenshot 2024-04-18 004105](https://github.com/IkramIriansyah486/Structure-Data-Assignment/assets/88477950/30b40d4a-a57c-44dd-b1e6-bbf0a184050c)

### Deskripsi Progam

Kode di atas digunakan untuk menjalankan hash table dengan node. Pada class HashNode, terdapat class public di mana class public ini memiliki string name dan phone_number. Lalu, di dalam class HashMap, terdapat class private dan public. Dalam class privatenya, dideklarasikan vector<HashNode*> table[TABLE_SIZE]. Dalam class publicnya, dideklarasikan int hashFunc(string key) beserta fungsi lainnya yang dapat dipakai di int main(). Fungsi lainnya yang dipanggil di int main() adalah:
- void insert(string name, string phone_number)</br> 
    Digunakan untuk menambahkan data name & phone_number
- void remove(string name)</br>
    Digunakan untuk menghapus data berdasarkan name
- string searchByName(string name)</br>
    Digunakan untuk mencari data berdasarkan name
- void print()</br> 
    Digunakan untuk menampilkan data name & phone_number</br>
<p>Dalam int main(), HashMap dideklarasikan dengan objek employee_map. Program menambahkan tiga data, yaitu name Mistah dan phone_number 1234, Pastah dan 5678, Ghana dan 91011 dengan employee_map.insert(). Lalu, program mencari data nomor hp milik Mistah dan Pastah melalui employee_map.searchByName(). Selanjutnya, program menghapus data milik Mistah melalui employee_map.remove(). Setelah dihapus, ditampilkan nomor hp Mistah yang telah dihapus melalui employee_map.searchByName(). Pada akhirnya, program menampilkan Hash Table dengan employee_map.print().</p>

## Unguided

1. Implementasikan hash table untuk menyimpan data mahasiswa. Setiap mahasiswa memiliki NIM dan nilai. Implementasikan fungsi untuk menambahkan data baru, menghapus data, mencari data berdasarkan NIM, dan mencari data berdasarkan nilai. Dengan ketentuan:</br> a. Setiap mahasiswa memiliki NIM dan nilai.</br> b. Program memiliki tampilan pilihan menu berisi poin C.</br> c. Implementasikan fungsi untuk menambahkan data baru, menghapus data, mencari data berdasarkan NIM, dan mencari data berdasarkan rentang nilai (80 – 90).

```C++
//IKRAM IRIANSYAH
//2311102184

#include <iostream>
#include <iomanip>
using namespace std;

const int MAX_SIZE = 10;

// Fungsi hash sederhana
int hash_func(long long key) {
    return key % MAX_SIZE;
}

// Struktur data untuk setiap node
struct Mahasiswa {
    long long NIM;
    string Nama; 
    int Nilai;
    Mahasiswa* next;
    Mahasiswa(long long NIM, string Nama, int Nilai) : NIM(NIM), Nama(Nama), Nilai(Nilai), next(nullptr) {} // Constructor
};

// Class hash table
class HashTable {
private:
    Mahasiswa** table;

public:
    HashTable() { // Constructor
        table = new Mahasiswa*[MAX_SIZE]();
    }

    ~HashTable() { // Destructor
        for (int i = 0; i < MAX_SIZE; i++) { // Menghapus semua node yang ada
            Mahasiswa* current = table[i]; // Menunjuk ke index yang sudah ditentukan
            while (current != nullptr) {
                Mahasiswa* temp = current;
                current = current->next;
                delete temp;
            }
        }
        delete[] table;
    }

    // Insertion
    void Insert(long long NIM, string Nama, int Nilai) {
        int index = hash_func(NIM); // Menggunakan fungsi hash untuk menentukan index
        Mahasiswa* current = table[index]; // Menunjuk ke index yang sudah ditentukan
        while (current != nullptr) {
            if (current->NIM == NIM) {
                current->Nilai = Nilai;
                return;
            }
            current = current->next;
        }
        Mahasiswa* mahasiswa = new Mahasiswa(NIM, Nama, Nilai);
        mahasiswa->next = table[index]; // Menunjuk ke node selanjutnya
        table[index] = mahasiswa; // Menunjuk ke node yang baru dibuat
    }

    Mahasiswa* SearchNIM(long long NIM) {
        int index = hash_func(NIM); // Menggunakan fungsi hash untuk menentukan index
        Mahasiswa* current = table[index]; // Menunjuk ke index yang sudah ditentukan
        while (current != nullptr) {
            if (current->NIM == NIM) {
                return current;
            }
            current = current->next;
        }
        return nullptr;
    }

    // Searching by value range (80 - 90)
    void SearchNilai(int StartScoreRange, int EndScoreRange) {
        cout << left << setw(15) << "-NAME-" << setw(20) << "-NIM-" << right << setw(2) << "-SCORE-" << endl;
        for (int i = 0; i < MAX_SIZE; i++) { // Untuk setiap index yang ada di tabel hash table ini, akan diiterasi satu per satu untuk mencari data yang sesuai dengan range yang diinginkan
            Mahasiswa* current = table[i];
            while (current != nullptr) {
                if (current->Nilai >= StartScoreRange && current->Nilai <= EndScoreRange) { // Jika nilai mahasiswa berada di dalam range yang diinginkan
                    cout << left << setw(15) << current->Nama << setw(20) << current->NIM << right << setw(6) << current->Nilai << endl; // Maka data mahasiswa tersebut akan ditampilkan
                }
                current = current->next;
            }
        }
    }

    // Deletion
    void Remove(long long NIM) {
    int index = hash_func(NIM); // Menggunakan fungsi hash untuk menentukan index
    Mahasiswa* current = table[index]; // Menunjuk ke index yang sudah ditentukan
    Mahasiswa* prev = nullptr;
    
    while (current != nullptr) { // Iterasi untuk mencari data yang akan dihapus
        if (current->NIM == NIM) { // Jika data ditemukan
            if (prev == nullptr) { // Jika data yang akan dihapus berada di awal index
                table[index] = current->next; // Maka index akan menunjuk ke node selanjutnya
            } else {
                prev->next = current->next; // Jika data yang akan dihapus berada di tengah atau akhir index, maka node sebelumnya akan menunjuk ke node setelahnya
            }
            delete current; // Menghapus node yang diinginkan
            cout << "\nCongrats! NIM " << NIM << " has been erased from existence!" << endl;
            return;
        }
        prev = current;
        current = current->next;
    }
    
    // Jika NIM tidak ditemukan
    cout << "\nHuh? NIM " << NIM << " is nowhere to be found." << endl;
}


    // Traversal
    void Traverse() {
        cout << left << setw(17) << "-NAME-" << setw(17) << "-NIM-" << setw(12) << "-SCORE-" << endl;
        for (int i = 0; i < MAX_SIZE; i++) { // Diiterasi untuk setiap index yang ada di tabel hash table ini
            Mahasiswa* current = table[i]; // Menunjuk ke index yang sudah ditentukan
            while (current != nullptr) { // Iterasi untuk menampilkan data mahasiswa
                cout << left << setw(17) << current->Nama << setw(19) << current->NIM << setw(12) << current->Nilai << endl;
                current = current->next;
        }
    }
}
};

int main() {
    HashTable ht; // Membuat objek hash table

    cout << "\n-=-=-=- PROGRAM HASH TABLE DATA MAHASISWA -=-=-=-" << endl;

    while (true) {
        cout << "\n-=- MENU -=-";
        cout << "\n1. Add Data Mahasiswa";
        cout << "\n2. Delete Data Mahasiswa";
        cout << "\n3. Search Data by NIM Mahasiswa";
        cout << "\n4. Search Score by Range";
        cout << "\n5. Show Data Mahasiswa";
        cout << "\n6. Exit";
        cout << "\nChoose your choice: ";
        int choice;
        cin >> choice;

        switch (choice) {
            case 1: {
                cout << "\n-=-= ADD DATA MAHASISWA =-=-" << endl;
                long long NIM;
                string Nama;
                int Nilai;
                cout << "Enter the student's name: ";
                cin.ignore(); // Untuk menghindari bug cin getline
                getline(cin, Nama); // Menggunakan getline untuk input string yang mengandung spasi
                cout << "Enter the NIM: ";
                cin >> NIM;
                cout << "Input the score: ";
                cin >> Nilai;
                ht.Insert(NIM, Nama, Nilai); // Memasukkan data mahasiswa ke dalam hash table
                cout << "\nSuccess! You have added the student data of " << Nama << " with the NIM " << NIM << " and the score " << Nilai << endl;
                break;
            }
            case 2: {
                cout << "\n-=-= DELETE DATA MAHASISWA =-=-" << endl;
                long long NIM;
                cout << "Input which NIM to delete: ";
                cin >> NIM;
                ht.Remove(NIM); // Menghapus data mahasiswa dari hash table
                break;
            }
            case 3: {
                cout << "\n-=-= SEARCH DATA BY NIM MAHASISWA =-=-" << endl;
                long long NIM;
                cout << "Input NIM to search for their data: ";
                cin >> NIM;
                Mahasiswa* mahasiswa = ht.SearchNIM(NIM); // Mencari data mahasiswa berdasarkan NIM
                if (mahasiswa != nullptr) // Jika data ditemukan
                    cout << "\nThe NIM " << NIM << " belongs to " << mahasiswa->Nama << " and has the score of " << mahasiswa->NIM << endl; // Maka data mahasiswa tersebut akan ditampilkan
                else
                    cout << "\nWhoops! Data not found!" << endl;
                break;
            }
            case 4: {
                cout << "\n-=-= SEARCH SCORE BY RANGE =-=-" << endl;
                int StartScoreRange, EndScoreRange;
                cout << "Range starts from Score: ";
                cin >> StartScoreRange;
                cout << "to Score: ";
                cin >> EndScoreRange;
                cout << "\nStudent(s) with the score between " << StartScoreRange << "-" << EndScoreRange << " are:\n";
                ht.SearchNilai(StartScoreRange, EndScoreRange); // Mencari data mahasiswa berdasarkan range nilai
                break;
            }
            case 5: {
                cout << "\n" << right << setw(8) << "" << "-=-= MAHASISWA DATA =-=-" << endl;
                ht.Traverse(); // Menampilkan data mahasiswa yang ada di hash table
                break;
            }
            case 6:
                cout << "\nThank you!";
                return 0;
            default:
                cout << "\nWhat did you just type..?" << endl;
        }
    }
}
```
### Screenshot Progam

### A.
![Screenshot 2024-04-18 004738](https://github.com/IkramIriansyah486/Structure-Data-Assignment/assets/88477950/36b99fe9-8afc-4447-8a70-d00f4e15e9d5)


### B dan C
![Screenshot 2024-04-18 005024](https://github.com/IkramIriansyah486/Structure-Data-Assignment/assets/88477950/30d53961-765b-4141-9a5c-a626ea7e8f0f)

![Screenshot 2024-04-18 005926](https://github.com/IkramIriansyah486/Structure-Data-Assignment/assets/88477950/2beeff38-1bf0-4732-9faa-266e7416bfa9)

![Screenshot 2024-04-18 010124](https://github.com/IkramIriansyah486/Structure-Data-Assignment/assets/88477950/929f9416-4ffe-4670-aeb4-eb75ff89148d)


### Deskripsi Progam

Kode di atas digunakan untuk menjalankan hash table. Pada struct Mahasiswa terdapat variable NIM dengan tipe data long long, Nama bertipe data string, Nilai bertipe data int, Mahasiswa* next, dan deklarasi Mahasiswa. Ada juga class HashTable di mana ada private class dan public class. Dalam private class, dideklarasikan Mahasiswa** table dan di dalam public class dideklarasikan HashTable(), ~HashTable(), dan fungsi lainnya yang dapat dipakai di int main(). Fungsi lainnya yang dipanggil di int main() adalah:
- void Insert(long long NIM, string Nama, int Nilai)</br>
    Digunakan untuk menambahkan NIM, nama, dan nilai
- Mahasiswa* SearchNIM(long long NIM)</br>
    Digunakan untuk mencari data mahasiswa berdasarkan NIM
- void SearchNilai(int StartScoreRange, int EndScoreRange)</br>
    Digunakan untuk mencari data mahasiswa berdasarkan range nilai
- void Remove(long long NIM)</br> 
    Digunakan untuk menghapus data mahasiswa berdasarkan NIM dalam hash table</br>
- void Traverse()
    Digunakan untuk menampilkan data Mahasiswa dalam hash table</br>
<p>Dalam int main(), HashTable dideklarasikan dengan objek ht. Program menampilkan menu di mana menu ini memiliki 6 opsi untuk dipilih user. 6 opsi ini diwakili oleh case dalam switch case. Case 1 menjalankan ht.Insert(NIM, Nama, Nilai) untuk menambahkan data mahasiswa, Case 2 menjalankan ht.Remove(NIM) untuk menghapus data mahasiswa, Case 3 menjalankan ht.SearchNIM(NIM) untuk mencari data mahasiswa, Case 4 menjalankan ht.SearchNilai(StartScoreRange, EndScoreRange) untuk mencari data mahasiswa dari range nilai, Case 5 menjalankan ht.Traverse() untuk menampilkan data mahasiswa, dan yang terakhir Case 6 digunakan untuk mengakhiri program tersebut. Sebagai demonstrasi.

### Refrensi

[1] GeeksforGeeks: https://www.geeksforgeeks.org/hash-table-data-structure/.<br/>
[2] Implementing a Hash Table in C++: https://www.digitalocean.com/community/tutorials/hash-table-in-c-plus-plus.





