from dataclasses import dataclass
from unittest import removeResult
import mysql.connector
import os

# SQL TABLOLARI:
# login (id, username, password, unvan, studentnum)
# students (id, name, surname, studentnum)
# grades (id, studentnum, ders, vizenot, finalnot, ortalama, harfnot)
# lessons (id, ders_adi)

# Kullanıcı kaydı (öğrenci veya öğretmen ekleme)
def register(username, password, unvan, studentnum=None):
    try:
        connection = mysql.connector.connect(
            host="localhost",
            user="root",
            password="******",   # MySQL şifrenizi buraya yazın
            database="studentdb"
        )
        cursor = connection.cursor()
        sql = "INSERT INTO login (username, password, unvan, studentnum) VALUES (%s, %s, %s, %s)"
        cursor.execute(sql, (username, password, unvan, studentnum))
        connection.commit()
        print("Kullanici kaydi basarili.")
    except mysql.connector.Error as err:
        print("Hata:", err)
    finally:
        connection.close()

# Kullanıcı girişi (login)
def login(username, password):
    connection = mysql.connector.connect(
        host="localhost",
        user="root",
        password="******",
        database="studentdb"
    )
    cursor = connection.cursor()
    sql = "SELECT * FROM login WHERE username = %s AND password = %s"
    cursor.execute(sql, (username, password))
    user = cursor.fetchone()
    if user:
        print("Giris basarili.")
        return {"username": user[2], "unvan": user[4], "studentnum": user[1]}  
    else:
        print("Kullanici bulunamadi.")
        return None

# Öğrenci ekleme işlemi
def OgrenciEKle():
    def insertStudent(list):
        connection = mysql.connector.connect(
            host="localhost",
            user="root",
            password="******",
            database="studentdb"
        )
        cursor = connection.cursor()
        sql = "INSERT INTO Students(name,surname,studentnum) VALUES (%s,%s,%s)"
        values = list
        cursor.executemany(sql, values)

        try:
            connection.commit()         
        except mysql.connector.Error as err:
            print("hata", err)
        finally:
            connection.close()
            print("Database baglantisi kapandi")

    list = []
    while True:             
        name = input("Ogrencinin Adi:")
        surname = input("Ogrencinin Soyadi:")
        studentnum = int(input("Ogrenci Numarasi:"))
        list.append((name, surname, studentnum))
        result = input("Baska bir ogrenci eklemek istiyor musunuz? (e/h)")
        if result.lower() == "h":
            print("Kayitlar veritabanina aktariliyor...")
            insertStudent(list)
            break

# Öğrenci silme işlemi
def deleteStudent(studentnum):
    connection = mysql.connector.connect(
        host="localhost",
        user="root",
        password="******",
        database="studentdb"
    )
    cursor = connection.cursor()

    sql = "DELETE FROM students WHERE studentnum=%s"
    value = (studentnum,)
    cursor.execute(sql, value)
    try:
        connection.commit()
        print(f"{studentnum} numarali ogrenci silindi.")
    except mysql.connector.Error as err:
        print("hata", err)
    finally:
        connection.close()
        print("Database baglantisi kapandi")

# Vize notu girişi
def vizeNotGiris(puan, studentnum, ders):
    connection = mysql.connector.connect(
        host="localhost",
        user="root",
        password="******",
        database="studentdb"
    )
    cursor = connection.cursor()

    sql = "UPDATE grades SET vizenot=%s WHERE studentnum=%s AND ders=%s"
    value = (puan, studentnum, ders)
    cursor.execute(sql, value)

    try:
        connection.commit()
        print(f"{cursor.rowcount} adet vize notu basariyla girildi.")
    except mysql.connector.Error as err:
        print("hata", err)
    finally:
        connection.close()
        print("Database baglantisi kapandi")

# Final notu girişi
def finalNotGiris(puan, studentnum, ders):
    connection = mysql.connector.connect(
        host="localhost",
        user="root",
        password="******",
        database="studentdb"
    )
    cursor = connection.cursor()

    sql = "UPDATE grades SET finalnot=%s WHERE studentnum=%s AND ders=%s"
    value = (puan, studentnum, ders)
    cursor.execute(sql, value)

    try:
        connection.commit()
        print(f"{cursor.rowcount} adet final notu basariyla girildi.")
    except mysql.connector.Error as err:
        print("hata", err)
    finally:
        connection.close()
        print("Database baglantisi kapandi")

# Harf notu girişi
def harfNotGiris(harfnotu, studentnum, ders):
    connection = mysql.connector.connect(
        host="localhost",
        user="root",
        password="******",
        database="studentdb"
    )
    cursor = connection.cursor()

    sql = "UPDATE grades SET harfnotu=%s WHERE studentnum=%s AND ders=%s"
    value = (harfnotu, studentnum, ders)
    cursor.execute(sql, value)

    try:
        connection.commit()
        print(f"{cursor.rowcount} adet harf notu basariyla girildi.")
    except mysql.connector.Error as err:
        print("hata", err)
    finally:
        connection.close()
        print("Database baglantisi kapandi")

# Öğrenci notlarını gösterme
def ogrenciNotlariniGoster(studentnum):
    try:
        connection = mysql.connector.connect(
            host="localhost",
            user="root",
            password="******",
            database="studentdb"
        )
        cursor = connection.cursor()
        sql = "SELECT * FROM grades INNER JOIN lessons ON lessons.id = grades.ders WHERE grades.studentnum = %s"
        cursor.execute(sql, (studentnum,))
        result = cursor.fetchall()
        for grade in result:
            vize = grade[2] if grade[2] is not None else "--"
            final = grade[3] if grade[3] is not None else "--"
            ortalama = grade[4] if grade[4] is not None else "-"
            harf = grade[5] if grade[5] is not None else "Sonuclandirilmadi."
            print(f"Ders Adı: {grade[7]} | Vize Notu: {vize} | Final Notu: {final} | Ortalama: {ortalama} | Harf Notu: {harf}")
    except mysql.connector.Error as err:
        print("Hata:", err)
    finally:
        connection.close()

# Öğrenci ortalamasını gösterme
def ogrenciOrtalamaGoster(studentnum):
    try:
        connection = mysql.connector.connect(
            host="localhost",
            user="root",
            password="******",
            database="studentdb"
        )
        cursor = connection.cursor()
        sql = "SELECT ortalama FROM grades WHERE studentnum = %s"
        cursor.execute(sql, (studentnum,))
        result = cursor.fetchone()
        if result:
            print(f"Ortalamaniz: {result[0]}")
        else:
            print("Ortalama bulunamadi.")
    except mysql.connector.Error as err:
        print("Hata:", err)
    finally:
        connection.close()

# Dersleri listeleme
def derslerListesi():
    try:
        connection = mysql.connector.connect(
            host="localhost",
            user="root",
            password="******",
            database="studentdb"
        )
        cursor = connection.cursor()
        cursor.execute("SELECT * FROM lessons")
        results = cursor.fetchall()
        for lesson in results:
            print(f"{lesson[0]} - {lesson[1]}")
    except mysql.connector.Error as err:
        print("Hata", err)
    finally:
        connection.close()

# Tüm öğrencileri listeleme
def ogrenciListesiTum():
    try:
        connection = mysql.connector.connect(
            host="localhost",
            user="root",
            password="******",
            database="studentdb"
        )
        cursor = connection.cursor()
        cursor.execute("SELECT * FROM students")
        results = cursor.fetchall()
        for student in results:
            print(f"Ogrenci Numarasi:{student[1]} Isim Soyisim: {student[2]}  {student[3]}")
    except mysql.connector.Error as err:
        print("Hata:", err)
    finally:
        connection.close()

# Öğrenci menüsü
def menu_ogrenci(studentnum):
    while True:
        print("OGRENCI MENU".center(50, '*'))
        secim = input("1-Notlari Gor\n2-Ortalamayi Gor\n3-Ders Listesini Gor\n4-Cikis\nSecim: ")
        if secim == '1':
            ogrenciNotlariniGoster(studentnum)
        elif secim == '2':
            ogrenciOrtalamaGoster(studentnum)
        elif secim == '3':
            derslerListesi()
        elif secim == '4':
            print("Cikis yapiliyor...")
            exit()
        else:
            print("Yanlis secim.")

# Öğretmen menüsü
def menu_ogretmen():
    while True:
        print("OGRETMEN MENU".center(50, '*'))
        secim = int(input("1-Ogrenci Listesi\n2-Not Giris Islemleri\n3-Ogrenci Ekleme-Silme Islemleri\n4-Cikis\nSecim:"))
        if secim == 1:
            ogrenciListesiTum()
        elif secim == 2:
            # Not giriş menüsü
            while True:
                secim2 = int(input("1-VIZE NOTU GIR \n2-FINAL NOTU GIR \n3-HARF NOTU GIR \n4-UST MENU \nSecim yapiniz:"))
                if secim2 == 1:
                    ogrenciListesiTum()
                    studentnum = int(input("Vize Notunu gireceginiz ogrencinin numarasi:"))
                    puan = float(input("Ogrencinin Vize Notu:"))
                    derslerListesi()
                    ders = int(input("Notu Girilecek dersin idsi:"))
                    vizeNotGiris(puan, studentnum, ders)
                elif secim2 == 2:
                    ogrenciListesiTum()
                    studentnum = int(input("Final Notunu gireceginiz ogrencinin numarasi:"))
                    puan = float(input("Ogrencinin Final Notu:"))
                    derslerListesi()
                    ders = int(input("Notu Girilecek dersin idsi:"))
                    finalNotGiris(puan, studentnum, ders)
                elif secim2 == 3:
                    ogrenciListesiTum()
                    studentnum = int(input("Harf Notunu gireceginiz ogrencinin numarasi:"))
                    harfnotu = input("Ogrencinin Harf Notu:")  # Harf notu (AA, BB, CC)
                    derslerListesi()
                    ders = int(input("Notu Girilecek dersin idsi:"))
                    harfNotGiris(harfnotu, studentnum, ders)
                elif secim2 == 4:
                    break
                else:
                    print('Yanlis secim.')
        elif secim == 3:
            # Öğrenci ekleme/silme menüsü
            while True:
                secim2 = int(input("1-Ogrenci Ekle \n2-Ogrenci Sil \n3-UST MENU \nSecim yapiniz:"))
                if secim2 == 1:
                    ogrenciListesiTum()
                    OgrenciEKle()
                elif secim2 == 2:
                    ogrenciListesiTum()
                    studentnum = int(input("Silinecek Ogrencinin numarasini giriniz:"))
                    deleteStudent(studentnum)
                elif secim2 == 3:
                    break
                else:
                    print("Hatali Giris Yaptiniz.")
        elif secim == 4:
            exit()
        else:
            print("Yanlis secim.")

# Ana menü
def anamenu():
    while True:
        print("ANA MENU".center(50, '*'))
        secim = input("1-Register\n2-Login\n3-Cikis\nSecim yapiniz:")
        if secim == '1':
            username = input("Kullanici adi:")
            password = input("Sifre:")
            unvan = input("Unvan (Ogrenci/Ogretmen):")
            if unvan.lower() == "ogrenci":
                studentnum = input("Ogrenci numarasi:")
            else:
                studentnum = None
            register(username, password, unvan, studentnum)
        elif secim == '2':
            username = input("Kullanici adi: ")
            password = input("Sifre: ")
            user = login(username, password)
            if user:
                if user.get("unvan", "").lower() == "ogrenci":
                    menu_ogrenci(user.get("studentnum"))
                elif user['unvan'].lower() == 'ogretmen':
                    menu_ogretmen()
        elif secim == '3':
            print("Program kapatildi.")
            break
        else:
            print("Yanlis secim.")

# Program başlat
anamenu()
