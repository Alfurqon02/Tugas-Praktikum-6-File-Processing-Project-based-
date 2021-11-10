# Tugas-Praktikum-6-File-Processing-Project-based-
Oct 31, 2021


Buatlah sebuah program dengan bahasa C dengan ketentuan di bawah ini:

1. Memuat array berisi obyek bertipe struct (bisa diisi langsung melalui source code)
2. Memuat infinite loop yang memiliki setidaknya 4 opsi menu berikut:
 - write data ke binary file dari array tersebut
 - read data dari binary file ke array tersebut
 - update salah satu obyek dalam array tersebut (update secara langsung, tidak perlu melalui file dengan fseek)
 - menampilkan isi array tersebut [EDITED 31 Oct 20:19]



        /*  Nama    : Mohammad Al Furqon
            NIM     : M0521044
            Kelas   : Informatika B

            Universitas Sebelas Maret

            Tugas Praktikum 6: File Processing (Project-based)
        */



        #include <stdio.h>
        #include <sys/stat.h>
        #include <stdlib.h>
        #include <stdbool.h>
        #include <string.h>

        typedef struct biodata
        {
            char NIM[50];
            char nama[50];
            double IPK;
        }Biodata;

        Biodata bdt[3];
        int n = 3;

        void clearbuffer();
        int refresh();
        int tulisMahasiswa();
        int bacaMahasiswa();
        void printData(const struct biodata *bdt);
        int bacaData();
        int tulis1();
        int tulis2();
        int tulis3();
        int menu();

        void clearbuffer(){
            char ch;
            while ((ch = getchar())!='\n' && ch !=EOF);
        }

        int refresh(){
            int i;
            FILE *f = fopen("biodata.bin", "rb");
            if (!f)
            {
                printf ("EROR!");
            }
                for (i=0;i<3;i++)
                {
                    Biodata client = {"", "", 0.0};
                    int result = fread(&client, sizeof(Biodata), 1, f);

                    if (result != 0 && client.NIM != 0)
                    {

                        strcpy(bdt[i].NIM, client.NIM);
                        strcpy(bdt[i].nama, client.nama);
                        bdt[i].IPK = client.IPK;
                        n++;
                    }
                }

            fclose(f);
            return 0;
        }

        int tulisMahasiswa(){
            FILE *f;
            int i;
            f = fopen ("biodata.bin", "wb");

            if (!f) {
                printf ("EROR : File Tidak Dapat Dibuat!\n");
                exit(EXIT_FAILURE);
            }
                printf ("Masukkan Data 3 Mahasiswa : ");
                for(i=0;i<3; i++){
                printf("\nData ke-%d\n", i+1);
                printf ("NIM \t\t: ");
                scanf ("%s", bdt[i].NIM);
                clearbuffer();
                printf ("Nama \t\t: ");
                scanf ("%[^\n]", bdt[i].nama);
                clearbuffer();
                printf ("IPK \t\t: ");
                scanf ("%lf", &bdt[i].IPK);
                clearbuffer();


                fwrite(bdt[i].NIM, 50 * sizeof(char), 1, f);
                fwrite(bdt[i].nama, 50 * sizeof(char), 1, f);
                fwrite(&bdt[i].IPK, sizeof(double), 1, f);

                }
            fclose(f);
            printf("Sukses!\n");
            printf ("\nData Telah Disimpan Ke File biodata.bin");

         return 0;
        }

        int bacaMahasiswa(){
        FILE *f;
        int i;
        f = fopen("biodata.bin", "rb");
         if (!f) {
                printf ("EROR : File Tidak Dapat Dibaca!\n");
                exit(EXIT_FAILURE);
         }
            for (i=0;i<3;i++){
            printf("\nData Mahasiswa Ke-%d\n", i+1);
            fread(bdt[i].NIM, 50 * sizeof(char), 1, f);
            fread(bdt[i].nama, 50 * sizeof(char), 1, f);
            fread(&bdt[i].IPK, sizeof(double), 1, f);

            printf("NIM : %s\n", bdt[i].NIM);
            printf("Nama : %s\n", bdt[i].nama);
            printf("IPK : %.2f\n", bdt[i].IPK);
            }
        fclose(f);
        return 0;
        }

        void printData(const struct biodata *bdt){
            printf("NIM : %s", bdt->NIM);
            printf("\nNama : %s", bdt->nama);
            printf("\nIPK : %.2f\n", bdt->IPK);
        }

        int bacaData(){
            FILE *f;
            int i;

            f=fopen("biodata.bin", "rb");
            if(!f)
        {
            printf ("File Tidak Bisa Dibuka!");
            exit(1);
            fclose(f);
        }
            printf("%-10s%-50s%-10.s\n",
                   "NIM", "Nama","IPK");
            for ( i = 0; i < 3; i++)
            {
                fread(bdt[i].NIM, 50 * sizeof(char), 1, f);
                fread(bdt[i].nama, 50 * sizeof(char), 1, f);
                fread(&bdt[i].IPK, sizeof(double), 1, f);
                printf("%-10s%-50s%-10.2f\n", bdt[i].NIM, bdt[i].nama, bdt[i].IPK);
            }
            fclose(f);
            return 0;
        }

        int tulis1(){
        FILE *f;
            f = fopen ("biodata.bin", "wb");

            if (!f) {
                printf ("EROR : File Tidak Dapat Dibuat!\n");
                exit(EXIT_FAILURE);
            }
                fwrite(bdt[1].NIM, 50 * sizeof(char), 1, f);
                fwrite(bdt[1].nama, 50 * sizeof(char), 1, f);
                fwrite(&bdt[1].IPK, sizeof(double), 1, f);
                fwrite(bdt[2].NIM, 50 * sizeof(char), 1, f);
                fwrite(bdt[2].nama, 50 * sizeof(char), 1, f);
                fwrite(&bdt[2].IPK, sizeof(double), 1, f);
                fclose(f);
                return 0;

        }

        int tulis2(){
            FILE *f;
            f = fopen ("biodata.bin", "wb");

            if (!f) {
                printf ("EROR : File Tidak Dapat Dibuat!\n");
                exit(EXIT_FAILURE);
            }
                fwrite(bdt[0].NIM, 50 * sizeof(char), 1, f);
                fwrite(bdt[0].nama, 50 * sizeof(char), 1, f);
                fwrite(&bdt[0].IPK, sizeof(double), 1, f);
                fwrite(bdt[2].NIM, 50 * sizeof(char), 1, f);
                fwrite(bdt[2].nama, 50 * sizeof(char), 1, f);
                fwrite(&bdt[2].IPK, sizeof(double), 1, f);
                fclose(f);
                return 0;

        }

        int tulis3(){
        FILE *f;
            f = fopen ("biodata.bin", "wb");

            if (!f) {
                printf ("EROR : File Tidak Dapat Dibuat!\n");
                exit(EXIT_FAILURE);
            }
                fwrite(bdt[0].NIM, 50 * sizeof(char), 1, f);
                fwrite(bdt[0].nama, 50 * sizeof(char), 1, f);
                fwrite(&bdt[0].IPK, sizeof(double), 1, f);
                fwrite(bdt[1].NIM, 50 * sizeof(char), 1, f);
                fwrite(bdt[1].nama, 50 * sizeof(char), 1, f);
                fwrite(&bdt[1].IPK, sizeof(double), 1, f);
                fclose(f);
                return 0;
        }

        int updateMahasiswa(){
        FILE *f;
        bacaData();
            int baris;
            printf("\nPilih Baris Data Yang Ingin Diperbarui: ");
            scanf("%i", &baris);

            printf("Data Mahasiswa Baris %i Sebelum Dirubah : \n", baris);
            printData(&bdt[baris-1]);

            if (baris == 1){
                tulis1();
            }
            else if (baris == 2){
                tulis2();
            }
            else if (baris == 3){
                tulis3();
            }
            else {printf("Maaf Baris Tidak Ditemukan");
            updateMahasiswa();}

            system("pause");
            f = fopen("biodata.bin", "ab");
            if(!f)
        {
            printf ("File Tidak Bisa Dibuka!");
            exit(1);
            fclose(f);
        }
            printf("Masukkan Data Yang Baru : \n");
            printf("NIM : ");
            scanf ("%s", bdt[baris-1].NIM);
            printf("Nama : ");
            scanf("%s", bdt[baris-1].nama);
            printf("IPK : ");
            scanf("%lf", &bdt[baris-1].IPK);



            fwrite(bdt[baris-1].NIM, 50 * sizeof(char), 1, f);
            fwrite(bdt[baris-1].nama, 50 * sizeof(char), 1, f);
            fwrite(&bdt[baris-1].IPK, sizeof(double), 1, f);


            printf ("Data Berhasil Diubah!");
            fclose(f);

            return 0;
        }

        void tampilArray(){
            FILE *f;
            int i;
            f = fopen("biodata.bin", "rb");
            if (!f) {
                printf ("EROR : File Tidak Dapat Dibaca!\n");
                exit(EXIT_FAILURE);
         }
         for ( i = 0; i < 3; i++)
            {
                fread(bdt[i].NIM, 50 * sizeof(char), 1, f);
                fread(bdt[i].nama, 50 * sizeof(char), 1, f);
                fread(&bdt[i].IPK, sizeof(double), 1, f);

                 printf("%-20s%-20s%-20.2f\n", bdt[i].NIM, bdt[i].nama, bdt[i].IPK);
        }
                puts("=============================================");
                system("pause");
        }

        int menu(){

            unsigned int menuPilihan;
                refresh();
                puts ("Menu :");
                puts ("1. Tulis Data Mahasiswa");
                puts ("2. List Data Mahasiswa");
                puts ("3. Update Data");
                puts ("4. Tampilkan Array");
                printf ("\nPilihan : ");
                scanf ("%u", &menuPilihan);
                system("cls");
                  {
                      switch (menuPilihan){
                case 1 :
                    tulisMahasiswa();
                    break;
                case 2 :
                    bacaMahasiswa();
                    break;
                case 3 :
                    updateMahasiswa();
                    break;
                case 4 :
                    tampilArray();

                default :
                menu();
                    break;
        }
        }
        return 0;
        }

        int main(){
            menu();
            return 0;
        }
