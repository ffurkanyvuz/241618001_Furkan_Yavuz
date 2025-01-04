import os
import getpass
import random

# Rastgele Türkçe isim ve soyisim listeleri
TURKCE_ISIMLER = [
    "Ahmet", "Mehmet", "Ayşe", "Fatma", "Emre", "Elif", "Hüseyin", "Hatice", "Mustafa", "Zeynep",
    "Ali", "Cemre", "Kemal", "Rabia", "Ebru", "Tamer", "Sibel", "Oğuz", "Büşra", "Deniz"
]

TURKCE_SOYISIMLER = [
    "Yılmaz", "Kaya", "Demir", "Şahin", "Çelik", "Aydın", "Erdem", "Öztürk", "Kurt", "Koç",
    "Arslan", "Doğan", "Aksoy", "Polat", "Uzun", "Keskin", "Yıldırım", "Güler", "Özkan", "Tuna"
]

SINIFLAR = ["9/A", "10/B", "11/C", "12/D"]

# Rastgele öğrenci oluşturma fonksiyonu
def rastgele_ogrenci_olustur(sinif_basina=25):
    ogrenciler = []
    ogrenci_no = 1  # Benzersiz öğrenci numarası
    for sinif in SINIFLAR:
        for _ in range(sinif_basina):
            isim = random.choice(TURKCE_ISIMLER)
            soyisim = random.choice(TURKCE_SOYISIMLER)
            dogum_yili = random.randint(2005, 2010)
            dogum_tarihi = f"{dogum_yili}-" + f"{random.randint(1, 12):02d}-" + f"{random.randint(1, 28):02d}"
            matematik = random.randint(0, 100)
            fizik = random.randint(0, 100)
            turkce = random.randint(0, 100)
            tarih = random.randint(0, 100)
            kimya = random.randint(0, 100)

            ogrenci = {
                "no": ogrenci_no,
                "isim": isim,
                "soyisim": soyisim,
                "dogum_tarihi": dogum_tarihi,
                "sinif": sinif,
                "matematik": matematik,
                "fizik": fizik,
                "turkce": turkce,
                "tarih": tarih,
                "kimya": kimya
            }
            ogrenciler.append(ogrenci)
            ogrenci_no += 1
    return ogrenciler


# Kullanıcı Yönetim Sistemi
class KullaniciYonetimSistemi:
    def __init__(self):
        self.kullanicilar = {"admin": {"sifre": "1234", "rol": "yonetici"}}
    
    def kullanici_dogrula(self, kullanici_adi, sifre):
        kullanici = self.kullanicilar.get(kullanici_adi)
        if kullanici and kullanici["sifre"] == sifre:
            return kullanici["rol"]
        return None


# Öğrenci Yönetim Sistemi
class OgrenciYonetimSistemi:
    def __init__(self):
        self.ogrenciler = rastgele_ogrenci_olustur()
        self.sonraki_no = len(self.ogrenciler) + 1

    def ogrenci_ekle(self, isim, soyisim, dogum_tarihi, sinif, matematik, fizik, turkce, tarih, kimya):
        ogrenci = {
            "no": self.sonraki_no,
            "isim": isim,
            "soyisim": soyisim,
            "dogum_tarihi": dogum_tarihi,
            "sinif": sinif,
            "matematik": matematik,
            "fizik": fizik,
            "turkce": turkce,
            "tarih": tarih,
            "kimya": kimya
        }
        self.ogrenciler.append(ogrenci)
        self.sonraki_no += 1
        print(f"Öğrenci başarıyla eklendi: {isim} {soyisim}")

    def ogrenci_listele(self, sinif=None):
        liste = self.ogrenciler if sinif is None else [o for o in self.ogrenciler if o["sinif"] == sinif]
        if not liste:
            print("Hiç öğrenci yok.")
            return

        print("{:<5} {:<15} {:<15} {:<12} {:<10} {:<10} {:<10} {:<10} {:<10} {:<10}".format(
            "No", "İsim", "Soyisim", "Doğum Tarihi", "Sınıf", "Matematik", "Fizik", "Türkçe", "Tarih", "Kimya"))
        print("-" * 100)
        for o in liste:
            print("{:<5} {:<15} {:<15} {:<12} {:<10} {:<10} {:<10} {:<10} {:<10} {:<10}".format(
                o["no"], o["isim"], o["soyisim"], o["dogum_tarihi"], o["sinif"],
                o["matematik"], o["fizik"], o["turkce"], o["tarih"], o["kimya"]))

    def not_analizi(self):
        if not self.ogrenciler:
            print("Hiç öğrenci yok.")
            return

        toplam_ogrenci = len(self.ogrenciler)
        toplam_puan = {
            "matematik": sum(o["matematik"] for o in self.ogrenciler),
            "fizik": sum(o["fizik"] for o in self.ogrenciler),
            "turkce": sum(o["turkce"] for o in self.ogrenciler),
            "tarih": sum(o["tarih"] for o in self.ogrenciler),
            "kimya": sum(o["kimya"] for o in self.ogrenciler)
        }
        print("\n=== Not Analizi ===")
        for ders, toplam in toplam_puan.items():
            print(f"{ders.capitalize()} Ortalama: {toplam / toplam_ogrenci:.2f}")

    def en_iyi_ogrenci(self):
        if not self.ogrenciler:
            print("Hiç öğrenci yok.")
            return
        en_iyi = max(self.ogrenciler, key=lambda o: (o["matematik"] + o["fizik"] + o["turkce"] + o["tarih"] + o["kimya"]) / 5)
        ortalama = (en_iyi["matematik"] + en_iyi["fizik"] + en_iyi["turkce"] + en_iyi["tarih"] + en_iyi["kimya"]) / 5
        print("\n=== En İyi Öğrenci ===")
        print(f"İsim: {en_iyi['isim']}")
        print(f"Soyisim: {en_iyi['soyisim']}")
        print(f"Sınıf: {en_iyi['sinif']}")
        print(f"Ortalama: {ortalama:.2f}")

    def ogrenci_sil(self, ogrenci_no):
        for ogrenci in self.ogrenciler:
            if ogrenci["no"] == ogrenci_no:
                self.ogrenciler.remove(ogrenci)
                print(f"Öğrenci {ogrenci['isim']} {ogrenci['soyisim']} silindi.")
                return
        print("Öğrenci bulunamadı.")

    def ogrenci_guncelle(self, ogrenci_no, **guncellemeler):
        for ogrenci in self.ogrenciler:
            if ogrenci["no"] == ogrenci_no:
                ogrenci.update(guncellemeler)
                print("Öğrenci bilgileri güncellendi.")
                return
        print("Öğrenci bulunamadı.")


def ekran_temizle():
    os.system('cls' if os.name == 'nt' else 'clear')


def ana_program():
    kullanici_sistemi = KullaniciYonetimSistemi()
    ogrenci_sistemi = OgrenciYonetimSistemi()

    print("\n=== Öğrenci Bilgi Sistemine Giriş ===")
    kullanici_adi = input("Kullanıcı Adı: ").strip()
    sifre = getpass.getpass("Şifre: ").strip()

    rol = kullanici_sistemi.kullanici_dogrula(kullanici_adi, sifre)
    if not rol:
        print("Geçersiz kullanıcı adı veya şifre!")
        return

    print(f"Hoşgeldiniz, {kullanici_adi}!")

    while True:
        ekran_temizle()
        print("\n--- Öğrenci Bilgi Sistemi ---")
        print("1. Öğrenci Ekle")
        print("2. Öğrencileri Listele")
        print("3. Not Analizi")
        print("4. En İyi Öğrenci")
        print("5. Öğrenci Güncelle")
        print("6. Öğrenci Sil")
        print("7. Çıkış")

        secim = input("Seçiminizi yapın (1-7): ").strip()
        ekran_temizle()

        if secim == '1':
            try:
                isim = input("Öğrencinin adı: ").strip()
                soyisim = input("Öğrencinin soyadı: ").strip()
                dogum_tarihi = input("Doğum tarihi (YYYY-AA-GG): ").strip()
                sinif = input("Sınıf: ").strip()
                matematik = float(input("Matematik notu (0-100): "))
                fizik = float(input("Fizik notu (0-100): "))
                turkce = float(input("Türkçe notu (0-100): "))
                tarih = float(input("Tarih notu (0-100): "))
                kimya = float(input("Kimya notu (0-100): "))
                ogrenci_sistemi.ogrenci_ekle(isim, soyisim, dogum_tarihi, sinif, matematik, fizik, turkce, tarih, kimya)
            except ValueError:
                print("Hatalı giriş! Notlar sayısal olmalıdır.")
        elif secim == '2':
            sinif = input("Sınıf filtresi (boş bırakabilirsiniz): ").strip()
            ogrenci_sistemi.ogrenci_listele(sinif if sinif else None)
            input("\nDevam etmek için Enter'a basın...")
        elif secim == '3':
            ogrenci_sistemi.not_analizi()
            input("\nDevam etmek için Enter'a basın...")
        elif secim == '4':
            ogrenci_sistemi.en_iyi_ogrenci()
            input("\nDevam etmek için Enter'a basın...")
        elif secim == '5':
            try:
                ogrenci_no = int(input("Güncellenecek öğrenci numarasını girin: ").strip())
                isim = input("Yeni isim (boş bırakabilirsiniz): ").strip()
                soyisim = input("Yeni soyisim (boş bırakabilirsiniz): ").strip()
                dogum_tarihi = input("Yeni doğum tarihi (YYYY-AA-GG, boş bırakabilirsiniz): ").strip()
                sinif = input("Yeni sınıf (boş bırakabilirsiniz): ").strip()
                matematik = input("Yeni Matematik notu (0-100, boş bırakabilirsiniz): ").strip()
                fizik = input("Yeni Fizik notu (0-100, boş bırakabilirsiniz): ").strip()
                turkce = input("Yeni Türkçe notu (0-100, boş bırakabilirsiniz): ").strip()
                tarih = input("Yeni Tarih notu (0-100, boş bırakabilirsiniz): ").strip()
                kimya = input("Yeni Kimya notu (0-100, boş bırakabilirsiniz): ").strip()

                guncellemeler = {}
                if isim: guncellemeler["isim"] = isim
                if soyisim: guncellemeler["soyisim"] = soyisim
                if dogum_tarihi: guncellemeler["dogum_tarihi"] = dogum_tarihi
                if sinif: guncellemeler["sinif"] = sinif
                if matematik: guncellemeler["matematik"] = float(matematik)
                if fizik: guncellemeler["fizik"] = float(fizik)
                if turkce: guncellemeler["turkce"] = float(turkce)
                if tarih: guncellemeler["tarih"] = float(tarih)
                if kimya: guncellemeler["kimya"] = float(kimya)

                ogrenci_sistemi.ogrenci_guncelle(ogrenci_no, **guncellemeler)
            except ValueError:
                print("Geçersiz giriş!")
        elif secim == '6':
            try:
                ogrenci_no = int(input("Silinecek öğrenci numarasını girin: ").strip())
                ogrenci_sistemi.ogrenci_sil(ogrenci_no)
            except ValueError:
                print("Geçersiz giriş!")
        elif secim == '7':
            print("İyi günler!")
            break
        else:
            print("Geçersiz seçim!")
            input("\nDevam etmek için Enter'a basın...")


if __name__ == "__main__":
    ana_program()
