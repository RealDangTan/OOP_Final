# OOP_Final
```cpp
#include<bits/stdc++.h>
using namespace std;

class GIANG_VIEN{
    protected:
        string ma_gv;
        string hoten;
        double gio_nghien_cuu;
        static int count_gv;
    public:
        GIANG_VIEN(string mgv, string hoTen): ma_gv(mgv), hoten(hoTen){
            count_gv++;
        }

        virtual void nhap(){
            cin.ignore(); cout<< "Nhap Ma Giang Vien: ";getline(cin, this->ma_gv);
            cout<<"Nhap ho va ten: ";getline(cin, this->hoten);
        }

        static int demgv(){
            return count_gv;
        }

        friend bool operator> (GIANG_VIEN& gv1, GIANG_VIEN& gv2){
            return gv1.gio_nghien_cuu > gv2.gio_nghien_cuu?false:true;
        }

        void in() {
            cout << ma_gv << "\t" << hoten << "\t" << gio_nghien_cuu << endl;
        }   

        double layGioNghienCuu(){
            return this->gio_nghien_cuu;
        }

        virtual void tinhGioNghienCuu() =0;

        virtual ~GIANG_VIEN() {
            count_gv--;
        };
};

class GIANG_VIEN_CO_HUU: public GIANG_VIEN{
    private:
        double so_bai_bao;
        double gio_bai_bao;
        double huong_dan_nghien_cuu;
        double he_so_nghien_cuu;
    public:
        GIANG_VIEN_CO_HUU(string mgv, string hoTen, double diem_btl, double diem_tt)
            :GIANG_VIEN(mgv, hoTen), so_bai_bao(diem_btl), gio_bai_bao(
                diem_tt){}
        void nhap(){
            GIANG_VIEN::nhap();
            cout<<"Nhap so bai bao: ";cin>>so_bai_bao;
            cout<<"Nhap gio bai bao: ";cin>>gio_bai_bao;
            cout<<"Nhap so lan huong dan nghien cuu: ";cin>>huong_dan_nghien_cuu;
            cout<<"Nhap he so nghien cuu: ";cin>>he_so_nghien_cuu;
        }
        void tinhGioNghienCuu(){
            gio_nghien_cuu = so_bai_bao*gio_bai_bao + huong_dan_nghien_cuu*he_so_nghien_cuu;
        }
};

class GIANG_VIEN_KIEM_GIANG: public GIANG_VIEN{
    private:
        double so_bai_bao;
        double gio_bai_bao;
        double gio_kiem_giang;
    public:
        GIANG_VIEN_KIEM_GIANG(string mgv, string hoTen, double so_bb, double gio_bb, double gio_kg)
                :GIANG_VIEN(mgv, hoTen), so_bai_bao(so_bb), gio_bai_bao(gio_bb), gio_kiem_giang(gio_kg){}
        void nhap(){
            GIANG_VIEN::nhap();
            cout<<"Nhap so bai bao: ";cin>>so_bai_bao; 
            cout<<"Nhap gio bai bao: ";cin>>gio_bai_bao; 
            cout<<"Nhap gio keim giang: ";cin>>gio_kiem_giang; 
        }
        void tinhGioNghienCuu(){
            gio_nghien_cuu = so_bai_bao*gio_bai_bao+gio_kiem_giang;
        }

};

int GIANG_VIEN::count_gv = 0;

struct Node {
    GIANG_VIEN* data;
    Node* next;
};


int main() {
    Node* head = NULL;
    Node* tail = NULL;
    int n; cout<< "Nhap so giang vien: ";cin>>n;
    for (int i = 0; i < n; i++) {
        int mode;
        cout << "Chon loai giang vien (1 - giang vien co huu, 2- giang vien kiem giang): "; cin >> mode;
        GIANG_VIEN* sv;
        if (mode == 1) {
            sv = new GIANG_VIEN_KIEM_GIANG("", "", 0, 0, 0);
        } else {
            sv = new GIANG_VIEN_CO_HUU("", "", 0, 0);
        }
        sv->nhap();
        sv->tinhGioNghienCuu();
        Node* tmp = new Node;
        tmp->data = sv;
        tmp->next = NULL;
        if (head == NULL) {
            head = tmp;
            tail = tmp;
        } else {
            tail->next = tmp;
            tail = tmp;
        }
    }
    for (Node* i = head; i != NULL; i = i->next) {
        for (Node* j = i->next; j != NULL; j = j->next) {
            if (*(i->data) > *(j->data)) {
                GIANG_VIEN* tmp = i->data;
                i->data = j->data;
                j->data = tmp;
            }
        }
    }

    Node* tmp = head;
    cout << "\nDanh sach giang vien da sap xep:\n";
    cout << "STT\tMa GV\tHo va ten\tGio nghien cuu\n";
    for (size_t i = 0; i < n; ++i) {
        cout << i + 1 << "\t";
        tmp->data->in();
        tmp = tmp->next;
    }

    cout << "\nTong so giang vien: " << GIANG_VIEN::demgv() << endl;
    for (Node* i = head; i != NULL; i = i->next) {
        delete i->data;
    }


    // for (int i = 0; i < n; i++) {
    //     int mode;
    //     cout << "Chon loai sinh vien (1 - hoc truc tiep, 2- hoc truc tuyen): "; cin >> mode;
    //     GIANG_VIEN* sv;
    //     if (mode == 1) {
    //         sv = new GIANG_VIEN_KIEM_GIANG("", "", 0, 0, 0);
    //     } else {
    //         sv = new GIANG_VIEN_CO_HUU("", "", 0, 0);
    //     }
    //     sv->nhap();
    //     sv->tinhGioNghienCuu();
    //     lophoc.push_back(sv);
    // }

    // for(int i=0;i<n-1;i++){
    //     for(int j=i+1;j<n;j++){
    //         if(lophoc[i]<lophoc[j]){
    //             GIANG_VIEN*tmp;
    //             tmp = lophoc[i];
    //             lophoc[i] = lophoc[j];
    //             lophoc[j] = tmp;
    //         }
    //     }
    // }

    // cout << "\nDanh sach sinh vien da sap xep:\n";
    // cout << "STT\tMa SV\tHo va ten\tDiem TB\n";
    // for (size_t i = 0; i < lophoc.size(); ++i) {
    //     cout << i + 1 << "\t";
    //     lophoc[i]->in();
    // }

    // cout << "\nTong so sinh vien: " << GIANG_VIEN::demgv() << endl;
    // for(int i=0;i<n;i++){
    //     delete lophoc[i];
    // }
}

```
