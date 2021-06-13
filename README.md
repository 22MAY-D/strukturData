# strukturData
C++ Rumah Sakit

#include<iostream>
#include<stack>
#include<queue>
#include<list>
#include "util.hpp"
 
// Program ini menggunakan fitur C++11 untuk memudahkan pembuatan program
// Pastikan untuk meng-compile program dengan compiler C++11
 
int main()
{
    int menu;
    int tindakan;
    std::queue<fp::Pasien> antrian;
    std::stack<fp::Staff> staff;
    std::list<fp::Persediaan> supplies; 
 
    // Membuat data awal untuk tree
    fp::Tree_pengurus tree("dr. Budi Santoso", 45, "Direktur Rumah Sakit");
    tree.add_child("Reni Susianti", 35, "Kepala Bidang Kepegawaian");
    tree.add_child("Prakoso Tanjung", 25, "Kepala Bidang Tata Usaha");
    tree.children[0].add_child("Veronika Salsa", 27, "Dokter Umum");
    tree.children[0].add_child("Bilqis Bela", 27, "Apoteker");
    tree.children[1].add_child("Sri Lestari", 26, "Kepala Bagian Keuangan");
    tree.children[1].add_child("Rifqi Putra", 26, "Kepala Bagian Pelayanan");
    tree.children[1].children[0].add_child("Firda Natalia", 24, "Kepala Bagian Penyusun Anggaran");
    tree.children[1].children[0].add_child("Kevin Pramudita", 31, "Kepala Bagian Pembendaharaan Dana");
    tree.children[1].children[1].add_child("Bela Putri", 26, "Kepala Bagian Keperawatan");
    tree.children[1].children[1].add_child("Novian Dewi", 25, "Kepala Bagian Rekam Medik");
 
    // Membuat data awal untuk queue
    antrian.push(fp::Pasien("Daffa Rifqy", 14, "A"));
    antrian.push(fp::Pasien("Cintya Hasim", 21, "AB"));
    antrian.push(fp::Pasien("Mutya Sinta", 9, "A"));
    antrian.push(fp::Pasien("Brisia Bani", 39, "O"));
 
    // Membuat data awal untuk stack
    staff.push(fp::Staff("Mahardika", 22, "Petugas Kebersihan", "Senin"));
    staff.push(fp::Staff("Luftyana", 25, "Petugas Kebersihan", "Kamis"));
    staff.push(fp::Staff("Ilvan Putra", 26, "Petugas Keamanan", "Kamis"));
    staff.push(fp::Staff("Ivan Genuine", 27, "Petugas Lain-Lain", "Kamis"));
 
    // Membuat data awal untuk linked list
    supplies.push_front(fp::Persediaan("Obat", 70));
    supplies.push_front(fp::Persediaan("Set peralatan dokter", 1));
    supplies.push_front(fp::Persediaan("Obat", 90));
    supplies.push_front(fp::Persediaan("Masker medis", 100));
 
    while (menu != 9)
    {
        menu = 0;
        tindakan = 0;
        std::cout << "\nPilih menu" << std::endl;
        std::cout << "1. Riwayat pengiriman persediaan klinik (linked list)" << std::endl;
        std::cout << "2. Antrian pasien (queue)" << std::endl;
        std::cout << "3. Manajemen kepegawaian untuk staff kecil (stack)" << std::endl;
        std::cout << "4. Hierarki pengurus klinik (tree)" << std::endl;
        std::cout << "9. Keluar" << std::endl;
        std::cin >> menu;
 
        switch (menu)
        {
            case 1:
                while (tindakan != 9)
                {
                    std::string jenis;
                    int jumlah;
                    std::string id;
 
                    // Mencetak riwayat pengiriman terbaru
                    if(!supplies.empty())
                    {
                        std::cout << "\nPengiriman terbaru :" << std::endl;
                        supplies.front().print_data();
                    }
                    else std::cout << "\nTidak ada pengiriman terbaru" << std::endl;
 
                    std::cout << "\nPilih tindakan selanjutnya" << std::endl;
                    std::cout << "1. Tampilkan seluruh riwayat dari paling baru ke paling lama" << std::endl;
                    std::cout << "2. Tambahkan data pengiriman baru" << std::endl;
                    std::cout << "3. Edit data riwayat" << std::endl;
                    std::cout << "4. Hapus data riwayat" << std::endl;
                    std::cout << "9. Kembali" << std::endl;
                    std::cin >> tindakan;
 
                    switch (tindakan)
                    {
                        case 1:
                            if(!supplies.empty())
                            {
                                for(fp::Persediaan &supply : supplies)
                                {
                                    std::cout << "\n";
                                    supply.print_data();
                                }
                            }
                            break;
 
                        case 2:
                            std::cout << "Masukkan jenis pengiriman : ";
                            fflush(stdin);
                            std::getline(std::cin, jenis);
                            std::cout << "Masukkan jumlah : ";
                            std::cin >> jumlah;
                            supplies.push_front(fp::Persediaan(jenis, jumlah)); // Memanggil constructor untuk memasukkan data
                            break;
 
                        case 3:
                            if(!supplies.empty())
                            {
                                std::cout << "Masukkan ID yang ingin di edit : ";
                                std::cin >> id;
                                std::cout << "Masukkan jenis pengiriman : ";
                                fflush(stdin);
                                std::getline(std::cin, jenis);
                                std::cout << "Masukkan jumlah : ";
                                std::cin >> jumlah;
 
                                for(fp::Persediaan &supply : supplies)
                                {
                                    if(id == supply.id) // Pencarian data sesuai dengan ID
                                    {
                                        supply.jenis = jenis;
                                        supply.jumlah = jumlah;
                                    }
                                }
                            }
                            break;
 
                        case 4:
                            if(!supplies.empty())
                            {
                                std::cout << "Masukkan ID yang ingin dihapus : ";
                                std::cin >> id;
                                std::list<fp::Persediaan>::iterator it = supplies.begin(); // Memakai iterator untuk menghapus elemen dari list
 
                                for(fp::Persediaan &supply : supplies)
                                {
                                    if(supply.id == id) 
                                    {
                                        supplies.erase(it); // Di sini iterator digunakan sebagai argumen
                                        break;
                                    }
                                    else it++;
                                }
                            }
                            break;
 
                        default:
                            break;
                    }
                }                
                break;
 
            case 2:
                while(tindakan != 9)
                {
                    std::string nama_lengkap;
                    int usia;
                    std::string gol_darah;
 
                    // Mencetak data pasien yang ada di depan
                    if(!antrian.empty()) 
                    {
                        std::cout << "\nPanjang antrian : " << antrian.size() << std::endl;
                        std::cout << "\nPasien selanjutnya : " << std::endl;
                        antrian.front().print_data();
                    }
                    else std::cout << "\nAntrian kosong" << std::endl;
 
                    std::cout << "\nPilih tindakan selanjutnya : " << std::endl;
                    std::cout << "1. Tambah antrian" << std::endl;
                    std::cout << "2. Panggil pasien" << std::endl;
                    std::cout << "9. Kembali" << std::endl;
                    std::cin >> tindakan;
 
                    switch (tindakan)
                    {
                        case 1:
                            fflush(stdin);
                            std::cout << "Masukkan nama lengkap : ";
                            std::getline(std::cin, nama_lengkap);
                            fflush(stdin);
                            std::cout << "Masukkan usia : ";
                            std::cin >> usia;
                            fflush(stdin);
                            std::cout << "Masukkan golongan darah : ";
                            std::getline(std::cin, gol_darah);
 
                            antrian.push(fp::Pasien(nama_lengkap, usia, gol_darah)); // Memanggil constructor untuk memasukkan data
                            break;
 
                        case 2:
                            if(!antrian.empty())
                            {
                                std::cout << "Pasien " << antrian.front().nama_lengkap << " telah dipanggil." << std::endl;
                                antrian.pop();
                            }
                            break;
 
                        default:
                            break;
                    }
                }
                break;
 
            case 3:
                while(tindakan != 9)
                {
                    std::string nama_lengkap;
                    int usia;
                    std::string jabatan;
                    std::string hari_kerja;
 
                    // Mencetak data Staff yang baru-baru ini direkrut
                    if(!staff.empty()) 
                    {
                        std::cout << "\nJumlah staff : " << staff.size() << std::endl;
                        std::cout << "\nStaff terbaru : " << std::endl;
                        staff.top().print_data();
                    }
                    else std::cout << "\nTidak ada staff" << std::endl;
 
                    std::cout << "\nPilih tindakan selanjutnya : " << std::endl;
                    std::cout << "1. Rekrut staff" << std::endl;
                    std::cout << "2. Pecat staff" << std::endl;
                    std::cout << "9. Kembali" << std::endl;
                    std::cin >> tindakan;
 
                    switch (tindakan)
                    {
                        case 1:
                            fflush(stdin);
                            std::cout << "Masukkan nama lengkap : ";
                            std::getline(std::cin, nama_lengkap);
                            fflush(stdin);
                            std::cout << "Masukkan usia : ";
                            std::cin >> usia;
                            fflush(stdin);
                            std::cout << "Masukkan jabatan : ";
                            std::getline(std::cin, jabatan);
                            fflush(stdin);
                            std::cout << "Masukkan hari kerja : ";
                            std::getline(std::cin, hari_kerja);
 
                            staff.push(fp::Staff(nama_lengkap, usia, jabatan, hari_kerja));
                            break;
 
                        case 2:
                            if(!staff.empty())
                            {
                                std::cout << staff.top().jabatan << " " << staff.top().nama_lengkap << " telah dipecat." << std::endl;
                                staff.pop();
                            }
                            break;
 
                        default:
                            break;
                    }
                }                
                break;
 
            case 4:
                tree.traverse(); // Menu untuk tree terdapat pada file util.hpp
                break;
 
            default:
                break;
        }
    }
    return 0;
}
