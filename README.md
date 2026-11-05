# oop_Nguyen-Quang-Loi
Bai tap thuc hanh OOP

#include <iostream>
#include <string>
#include <iomanip>
using namespace std;

class phuong_tien {
private:
    int ma_xe;
    string loai_xe;
    int gio_vao;
    int tg_do;

public:
    phuong_tien();
    phuong_tien(int ma, string loai, int gio, int tg);

    int lay_ma_xe() const;
    string lay_loai_xe() const;
    double tinh_phi() const;
    void hien_thi() const;
};

phuong_tien::phuong_tien() : ma_xe(0), loai_xe(""), gio_vao(0), tg_do(0) {}

phuong_tien::phuong_tien(int ma, string loai, int gio, int tg)
    : ma_xe(ma), loai_xe(loai), gio_vao(gio), tg_do(tg) {}

int phuong_tien::lay_ma_xe() const { return ma_xe; }
string phuong_tien::lay_loai_xe() const { return loai_xe; }

double phuong_tien::tinh_phi() const {
    double gia = (loai_xe == "Car") ? 6.0 : (loai_xe == "Motorcycle") ? 4.0 : 2.0;
    bool cao_diem = (gio_vao >= 6 && gio_vao <= 9) || (gio_vao >= 15 && gio_vao <= 18);
    double tong = 0;
    for (int h = 0; h < tg_do; ++h) {
        if (h < 2) tong += gia * (cao_diem ? 1.30 : 0.95);
        else       tong += gia * 0.90;
    }
    return tong;
}

void phuong_tien::hien_thi() const {
    cout << "  ID: " << ma_xe << " | " << loai_xe
         << " | " << gio_vao << "h | " << tg_do << "h"
         << " | Phi: $" << fixed << setprecision(2) << tinh_phi() << "\n";
}

class bai_xe {
private:
    int ma_bai;
    char kich_thuoc;
    phuong_tien pt1, pt2, pt3;   // sau nÃ y dÃ¹ng máº£ng
    bool co1, co2, co3;

public:
    bai_xe();
    bai_xe(int ma, char kt);

    int lay_ma() const;
    bool them(phuong_tien v);
    bool xoa(int ma_xe);
    bool chua(int ma_xe) const;
    void hien_thi() const;
    bool co_loai(string loai, int& dem, double& tong) const;
};

bai_xe::bai_xe() : ma_bai(0), kich_thuoc('S'), co1(false), co2(false), co3(false) {}

bai_xe::bai_xe(int ma, char kt) : ma_bai(ma), kich_thuoc(kt), co1(false), co2(false), co3(false) {}

int bai_xe::lay_ma() const { return ma_bai; }

bool bai_xe::them(phuong_tien v) {
    int cap = (kich_thuoc == 'S') ? 1 : (kich_thuoc == 'M') ? 2 : 3;
    if (cap >= 1 && !co1) { pt1 = v; co1 = true; return true; }
    if (cap >= 2 && !co2) { pt2 = v; co2 = true; return true; }
    if (cap >= 3 && !co3) { pt3 = v; co3 = true; return true; }
    return false;
}

bool bai_xe::xoa(int ma_xe) {
    if (co1 && pt1.lay_ma_xe() == ma_xe) {
        if (co2) {
            pt1 = pt2;
            if (co3) { pt2 = pt3; co3 = false; }
            else co2 = false;
        } else co1 = false;
        return true;
    }
    if (co2 && pt2.lay_ma_xe() == ma_xe) {
        if (co3) { pt2 = pt3; co3 = false; }
        else co2 = false;
        return true;
    }
    if (co3 && pt3.lay_ma_xe() == ma_xe) {
        co3 = false;
        return true;
    }
    return false;
}

bool bai_xe::chua(int ma_xe) const {
    if (co1 && pt1.lay_ma_xe() == ma_xe) return true;
    if (co2 && pt2.lay_ma_xe() == ma_xe) return true;
    if (co3 && pt3.lay_ma_xe() == ma_xe) return true;
    return false;
}

void bai_xe::hien_thi() const {
    int cap = (kich_thuoc == 'S') ? 1 : (kich_thuoc == 'M') ? 2 : 3;
    int so_xe = co1 + co2 + co3;
    cout << "Bai " << ma_bai << " [" << kich_thuoc << "] "
         << so_xe << "/" << cap << " xe\n";
    if (co1) pt1.hien_thi();
    if (co2) pt2.hien_thi();
    if (co3) pt3.hien_thi();
}

bool bai_xe::co_loai(string loai, int& dem, double& tong) const {
    dem = 0; tong = 0;
    if (co1 && pt1.lay_loai_xe() == loai) { ++dem; tong += pt1.tinh_phi(); }
    if (co2 && pt2.lay_loai_xe() == loai) { ++dem; tong += pt2.tinh_phi(); }
    if (co3 && pt3.lay_loai_xe() == loai) { ++dem; tong += pt3.tinh_phi(); }
    return dem > 0;
}

class SystemInterface {
private:
    bai_xe bai1, bai2, bai3;   // sau nÃ y dÃ¹ng máº£ng
    bool co1, co2, co3;

public:
    SystemInterface();
    void runMenu();
};

SystemInterface::SystemInterface() : co1(false), co2(false), co3(false) {}

void SystemInterface::runMenu() {
    int chon;
    while (true) {
        cout << "\n===== QUAN LY BAI DO XE =====\n";
        cout << "1. Tao bai do\n";
        cout << "2. Them xe\n";
        cout << "3. Xoa bai do\n";
        cout << "4. Lay xe ra\n";
        cout << "5. Hien thi tat ca bai do\n";
        cout << "6. Hien thi xe theo bai\n";
        cout << "7. Liet ke xe theo loai\n";
        cout << "8. Bao cao phi trung binh\n";
        cout << "9. Thoat\n";
        cout << "Chon: ";
        cin >> chon;
//cÃ¡c e chuyá»ƒn sang lá»‡nh switch cho tháº§y
        if (chon == 1) {
            int ma; char kt;
            cout << "Ma bai: "; cin >> ma;
            cout << "Kich thuoc (S/M/L): "; cin >> kt;
            if (!co1)      { bai1 = bai_xe(ma, kt); co1 = true; }
            else if (!co2) { bai2 = bai_xe(ma, kt); co2 = true; }
            else           { bai3 = bai_xe(ma, kt); co3 = true; }
            cout << "Da tao bai " << ma << "\n";
        }
        else if (chon == 2) {
            int ma, gio, tg; string loai;
            cout << "Ma xe: "; cin >> ma;
            cout << "Loai (Car/Motorcycle/Bicycle): "; cin >> loai;
            cout << "Gio vao (0-23): "; cin >> gio;
            cout << "Thoi luong (1-23): "; cin >> tg;
            phuong_tien v(ma, loai, gio, tg);
            bool ok = false;
            if (co1 && !ok) ok = bai1.them(v);
            if (co2 && !ok) ok = bai2.them(v);
            if (co3 && !ok) ok = bai3.them(v);
            if (ok) cout << "Da them xe " << ma << " | Phi: $" << fixed << setprecision(2) << v.tinh_phi() << "\n";
            else    cout << "Tat ca bai da day!\n";
        }
        else if (chon == 3) {
            int ma;
            cout << "Ma bai can xoa: "; cin >> ma;
            if (co1 && bai1.lay_ma() == ma) {
                if (co2) { bai1 = bai2; if (co3) { bai2 = bai3; co3 = false; } else co2 = false; }
                else co1 = false;
                cout << "Da xoa bai " << ma << "\n";
            }
            else if (co2 && bai2.lay_ma() == ma) {
                if (co3) { bai2 = bai3; co3 = false; } else co2 = false;
                cout << "Da xoa bai " << ma << "\n";
            }
            else if (co3 && bai3.lay_ma() == ma) {
                co3 = false;
                cout << "Da xoa bai " << ma << "\n";
            }
        }
        else if (chon == 4) {
            int ma;
            cout << "Ma xe can lay ra: "; cin >> ma;
            bool ok = false;
            if (co1 && !ok) ok = bai1.xoa(ma);
            if (co2 && !ok) ok = bai2.xoa(ma);
            if (co3 && !ok) ok = bai3.xoa(ma);
            cout << (ok ? "Da lay xe ra\n" : "Khong tim thay xe\n");
        }
        else if (chon == 5) {
            if (co1) bai1.hien_thi();
            if (co2) bai2.hien_thi();
            if (co3) bai3.hien_thi();
        }
        else if (chon == 6) {
            int ma;
            cout << "Ma bai: "; cin >> ma;
            if (co1 && bai1.lay_ma() == ma) bai1.hien_thi();
            else if (co2 && bai2.lay_ma() == ma) bai2.hien_thi();
            else if (co3 && bai3.lay_ma() == ma) bai3.hien_thi();
        }
        else if (chon == 7) {
            string loai;
            cout << "Loai xe: "; cin >> loai;
            int dem; double tong;
            if (co1 && bai1.co_loai(loai, dem, tong)) bai1.hien_thi();
            if (co2 && bai2.co_loai(loai, dem, tong)) bai2.hien_thi();
            if (co3 && bai3.co_loai(loai, dem, tong)) bai3.hien_thi();
        }
        else if (chon == 8) {
            string loai_arr[3] = {"Car", "Motorcycle", "Bicycle"};
            for (int t = 0; t < 3; ++t) {
                int dem = 0; double tong = 0;
                int d; double s;
                if (co1) { bai1.co_loai(loai_arr[t], d, s); dem += d; tong += s; }
                if (co2) { bai2.co_loai(loai_arr[t], d, s); dem += d; tong += s; }
                if (co3) { bai3.co_loai(loai_arr[t], d, s); dem += d; tong += s; }
                cout << loai_arr[t] << ": ";
                if (dem == 0) cout << "khong co xe\n";
                else cout << dem << " xe | TB: $" << fixed << setprecision(2) << tong / dem << "\n";
            }
        }
        else if (chon == 9) break;
    }
}

int main() {
    SystemInterface sys;
    sys.runMenu();
    return 0;
}
/---------------------------/
#include <iostream>
#include <string>
#include <iomanip>
#include <cstdlib>
#include <ctime>
#include <limits>
#include <cctype>
using namespace std;

class phuong_tien {
private:
    int ma_xe;
    string loai_xe;
    int gio_vao;
    int tg_do;

public:
    phuong_tien();
    phuong_tien(int ma, string loai, int gio, int tg);

    int lay_ma_xe() const;
    string lay_loai_xe() const;
    double tinh_phi() const;
    void hien_thi() const;
};

phuong_tien::phuong_tien() : ma_xe(0), loai_xe(""), gio_vao(0), tg_do(0) {}

phuong_tien::phuong_tien(int ma, string loai, int gio, int tg)
    : ma_xe(ma), loai_xe(loai), gio_vao(gio), tg_do(tg) {}

int phuong_tien::lay_ma_xe() const { return ma_xe; }
string phuong_tien::lay_loai_xe() const { return loai_xe; }

double phuong_tien::tinh_phi() const {
    double gia = (loai_xe == "Car") ? 6.0 : (loai_xe == "Motorcycle") ? 4.0 : 2.0;
    bool cao_diem = (gio_vao >= 6 && gio_vao <= 9) || (gio_vao >= 15 && gio_vao <= 18);
    double tong = 0;
    for (int h = 0; h < tg_do; ++h) {
        if (h < 2) tong += gia * (cao_diem ? 1.30 : 0.95);
        else       tong += gia * 0.90;
    }
    return tong;
}

void phuong_tien::hien_thi() const {
    cout << "  ID: " << ma_xe << " | " << loai_xe
         << " | " << gio_vao << "h | " << tg_do << "h"
         << " | Phi: $" << fixed << setprecision(2) << tinh_phi() << "\n";
}

class bai_xe {
private:
    int ma_bai;
    char kich_thuoc;
    static const int MAX_VEHICLES = 3;
    phuong_tien vehicles[MAX_VEHICLES];  // sau nÃ y dÃ¹ng máº£ng
	int currentVehicleCount=0;  

public:
    bai_xe();
    bai_xe(int ma, char kt);

	bool isFull() const; //Ki?m tra bãi dã d?t s?c ch?a t?i da tuong ?ng v?i size chua.
    int lay_ma() const;
    int tim(int ma) const;
    bool them(phuong_tien v);
    bool xoa(int ma_xe);
    //bool chua(int ma_xe) const; 
    void hien_thi() const;
    bool co_loai(string loai, int& dem, double& tong) const;
    void hien_thi_loai(string loai) const;
};

bai_xe::bai_xe() : ma_bai(0), kich_thuoc('S'), currentVehicleCount(0) {}

bai_xe::bai_xe(int ma, char kt) : ma_bai(ma), kich_thuoc(kt), currentVehicleCount(0) {}

bool bai_xe::isFull() const { 
	int cap = (kich_thuoc == 'S') ? 1 : (kich_thuoc == 'M') ? 2 : 3;
	return currentVehicleCount>=cap;
}

int bai_xe::lay_ma() const { return ma_bai; }

bool bai_xe::them(phuong_tien v) {
    if(isFull()) return false;
    vehicles[currentVehicleCount]=v;
    currentVehicleCount++;
    return true;
}

bool bai_xe::xoa(int ma_xe) {
    int index=tim(ma_xe);
    if(index==-1) return false;
    for(int i=index;i<currentVehicleCount-1;i++){
    	vehicles[i]=vehicles[i+1];
	}
	currentVehicleCount--;
	return true;
}

/*bool bai_xe::chua(int ma_xe) const {
    for(int i=0;i<currentVehicleCount;i++){
    	if(vehicles[i].lay_ma_xe()==ma_xe);
    	return true;
	}
	return false;
}*/

int bai_xe::tim(int ma) const {
	for(int i=0;i<currentVehicleCount;i++){
		if(vehicles[i].lay_ma_xe()==ma) return i;
	}
	return -1;
}

void bai_xe::hien_thi() const {
    int cap = (kich_thuoc == 'S') ? 1 : (kich_thuoc == 'M') ? 2 : 3;
    cout << "Bai " << ma_bai << " [" << kich_thuoc << "] "
         << currentVehicleCount << "/" << cap << " xe\n";
    for(int i=0;i<currentVehicleCount;i++) vehicles[i].hien_thi();
}

bool bai_xe::co_loai(string loai, int& dem, double& tong) const {
    dem = 0; tong = 0;
    for(int i=0;i<currentVehicleCount;i++){
    	if(vehicles[i].lay_loai_xe()==loai){
    	dem++;
    	tong+=vehicles[i].tinh_phi();
    	}
	}
    return dem > 0;
}

void bai_xe::hien_thi_loai(string loai) const {
    for (int i = 0; i < currentVehicleCount; i++) {
        if (vehicles[i].lay_loai_xe() == loai) {
            vehicles[i].hien_thi();
        }
    }
}

class SystemInterface {
private:
	static const int MAX_LOTS = 10;
    bai_xe  parkingLots[MAX_LOTS];   // sau nÃ y dÃ¹ng máº£ng
    int currentLotCount=0;

public:
    SystemInterface();
    void runMenu();
    bool isParkIDExists(int pID) const;
    bool isVehicleIDExists(int vID) const;
    int generateRandomID();
    int generateRandomParkID();
    int timBai(int pID) const;
};


int SystemInterface::timBai(int pID) const {
    for (int i = 0; i < currentLotCount; i++) {
        if (parkingLots[i].lay_ma() == pID)
            return i;
    }
    return -1;
}

bool SystemInterface::isParkIDExists(int pID) const {
	for(int i=0;i<currentLotCount;i++) {
		if(parkingLots[i].lay_ma()==pID) return true;
	}
	return false;
}

bool SystemInterface::isVehicleIDExists(int vID) const {
	for(int i=0;i<currentLotCount;i++){
		if(parkingLots[i].tim(vID)!=-1) return true;
	}
	return false;
}

int SystemInterface::generateRandomID(){
	int id;
	do{
		id=rand() % 9000+1000;
	} while(isVehicleIDExists(id));
	return id;
}

int SystemInterface::generateRandomParkID(){
	int id;
	do{
		id=rand() % 9000+1000;
	} while(isParkIDExists(id));
	return id;
}

SystemInterface::SystemInterface() : currentLotCount(0) {}

void SystemInterface::runMenu() {
    int chon;
    while (true) {
        cout << "\n===== QUAN LY BAI DO XE =====\n";
        cout << "1. Tao bai do\n";
        cout << "2. Them xe\n";
        cout << "3. Xoa bai do\n";
        cout << "4. Lay xe ra\n";
        cout << "5. Hien thi tat ca bai do\n";
        cout << "6. Hien thi xe theo bai\n";
        cout << "7. Liet ke xe theo loai\n";
        cout << "8. Bao cao phi trung binh\n";
        cout << "9. Thoat\n";
        cout << "Chon: ";
        cin >> chon;
//cÃ¡c e chuyá»ƒn sang lá»‡nh switch cho tháº§y
    switch(chon){
    	case 1: {
            if (currentLotCount >= MAX_LOTS) {
                cout << "Danh sach bai do da day!\n";
                break;
            }

            int ma;
            char kt;

            cout << "Ma bai: ";
            cin >> ma;

            if (isParkIDExists(ma)) {
                cout << "Ma bai da ton tai, tao ma ngau nhien!\n";
                ma = generateRandomParkID();
                cout << "Ma bai moi: " << ma << endl;
            }

            cout << "Kich thuoc (S/M/L): ";
            cin >> kt;

            kt = toupper(kt);

            if (kt != 'S' && kt != 'M' && kt != 'L') {
                cout << "Kich thuoc khong hop le!\n";
                break;
            }

            parkingLots[currentLotCount] = bai_xe(ma, kt);
            currentLotCount++;

            cout << "Da tao bai " << ma << "\n";
            break;
        }

        case 2: {
            int ma, gio, tg;
            string loai;

            cout << "Ma xe: ";
            cin >> ma;

            if (isVehicleIDExists(ma)) {
                cout << "Ma xe da ton tai!\n";
                break;
            }

            cout << "Loai (Car/Motorcycle/Bicycle): ";
            cin >> loai;

            if (loai != "Car" &&
                loai != "Motorcycle" &&
                loai != "Bicycle") {
                cout << "Loai xe khong hop le!\n";
                break;
            }

            cout << "Gio vao (0-23): ";
            cin >> gio;

            if (gio < 0 || gio > 23) {
                cout << "Gio vao khong hop le!\n";
                break;
            }

            cout << "Thoi luong (1-23): ";
            cin >> tg;

            if (tg < 1 || tg > 23) {
                cout << "Thoi luong khong hop le!\n";
                break;
            }

            phuong_tien v(ma, loai, gio, tg);

            bool ok = false;

            for (int i = 0; i < currentLotCount; i++) {
                if (parkingLots[i].them(v)) {
                    cout << "Da them xe " << ma
                         << " vao bai "
                         << parkingLots[i].lay_ma()
                         << " | Phi: $"
                         << fixed << setprecision(2)
                         << v.tinh_phi() << "\n";

                    ok = true;
                    break;
                }
            }

            if (!ok) {
                cout << "Tat ca bai da day hoac chua co bai nao!\n";
            }

            break;
        }

        case 3: {
            int ma;

            cout << "Ma bai can xoa: ";
            cin >> ma;

            int index = timBai(ma);

            if (index == -1) {
                cout << "Khong tim thay bai " << ma << "\n";
                break;
            }

            // Don mang sau khi xoa bai
            for (int i = index; i < currentLotCount - 1; i++) {
                parkingLots[i] = parkingLots[i + 1];
            }

            currentLotCount--;

            cout << "Da xoa bai " << ma << "\n";
            break;
        }

        case 4: {
            int ma;

            cout << "Ma xe can lay ra: ";
            cin >> ma;

            bool ok = false;

            for (int i = 0; i < currentLotCount; i++) {
                if (parkingLots[i].xoa(ma)) {
                    ok = true;
                    break;
                }
            }

            cout << (ok ? "Da lay xe ra\n" : "Khong tim thay xe\n");

            break;
        }

        case 5: {
            if (currentLotCount == 0) {
                cout << "Chua co bai do nao!\n";
                break;
            }

            for (int i = 0; i < currentLotCount; i++) {
                parkingLots[i].hien_thi();
            }

            break;
        }

        case 6: {
            int ma;

            cout << "Ma bai: ";
            cin >> ma;

            int index = timBai(ma);

            if (index != -1) {
                parkingLots[index].hien_thi();
            }
            else {
                cout << "Khong tim thay bai do!\n";
            }

            break;
        }

        case 7: {
    string loai;

    cout << "Loai xe: ";
    cin >> loai;

    int tongDem = 0;
    double tongPhi = 0;

    for (int i = 0; i < currentLotCount; i++) {
        int dem = 0;
        double tong = 0;

        if (parkingLots[i].co_loai(loai, dem, tong)) {
            cout << "\nBai " << parkingLots[i].lay_ma() << ":\n";

            parkingLots[i].hien_thi_loai(loai);

            tongDem += dem;
            tongPhi += tong;
        }
    }

    if (tongDem == 0) {
        cout << "Khong co xe thuoc loai nay!\n";
    }
    else {
        cout << "\nTong so xe: " << tongDem << "\n";
        cout << "Tong phi: $" << fixed
             << setprecision(2) << tongPhi << "\n";
    }

    break;
}

        
case 8: {
    string loai_arr[3] = {
        "Car", "Motorcycle", "Bicycle"
    };

    if (currentLotCount == 0) {
        cout << "Chua co bai do nao!\n";
        break;
    }

    cout << "\n===== BAO CAO THEO LOAI XE =====\n";

    // Bao cao theo tung loai xe
    for (int t = 0; t < 3; t++) {
        int dem = 0;
        double tong = 0;

        for (int i = 0; i < currentLotCount; i++) {
            int d = 0;
            double s = 0;

            parkingLots[i].co_loai(loai_arr[t], d, s);

            dem += d;
            tong += s;
        }

        cout << loai_arr[t] << ": ";

        if (dem == 0) {
            cout << "Khong co xe\n";
        }
        else {
            cout << dem << " xe"
                 << " | Tong phi: $" 
                 << fixed << setprecision(2) << tong
                 << " | Phi TB: $" << tong / dem << "\n";
        }
    }

    // Bao cao theo tung bai
    cout << "\n===== BAO CAO THEO TUNG BAI =====\n";

    for (int i = 0; i < currentLotCount; i++) {
        int dem = 0;
        double tong = 0;

        for (int t = 0; t < 3; t++) {
            int d = 0;
            double s = 0;

            parkingLots[i].co_loai(loai_arr[t], d, s);

            dem += d;
            tong += s;
        }

        cout << "\nBai " << parkingLots[i].lay_ma() << ": ";

        if (dem == 0) {
            cout << "Chua co xe\n";
        }
        else {
            cout << dem << " xe"
                 << " | Tong phi: $" << tong
                 << " | Phi trung binh: $" << tong / dem << "\n";
        }
    }

    break;
}


        case 9:
    		cout << "Thoat chuong trinh!\n";
    		return;
    	default:
            	cout << "Lua chon khong hop le!\n";
            }
        }
}

int main() {
	srand(time(0));
    SystemInterface sys;
    sys.runMenu();
    return 0;
}
