# 🟢 UYGHUR_SOCIAL V3.5 

Python'ın `tkinter` kütüphanesi kullanılarak geliştirilmiş, **siber-neon** arayüzlü ve bot içermeyen yerel bir sosyal medya simülasyonu projesi.

---

## 🛠️ Proje Özellikleri
* **Şifresiz Kolay Giriş:** Sadece kullanıcı adınızı yazarak anında sisteme bağlanın.
* **Gerçek Zamanlı Akış:** `.json` tabanlı veritabanı sayesinde paylaşılan mesajları kalıcı olarak saklar ve listeler.
* **Siber Arayüz:** Hacker tarzı yeşil ve turkuaz tonlarında, gözü yormayan karanlık tema tasarımı.
* **%100 Güvenli ve Botsuz:** Tamamen sizin yazdığınız ve kaydettiğiniz verilerle çalışır.

---

## 💻 Nasıl Çalıştırılır?
1. Bilgisayarınıza **Python** veya **Thonny** kurun.
2. Bu depodaki kod dosyasını bilgisayarınıza indirin.
3. Kodu Thonny veya herhangi bir Python editöründe çalıştırın.
4. Kullanıcı adınızı girip `Giriş Yap` butonuna basın, ardından mesajınızı yazıp tüm dünyayla paylaşın!

---

## 🚀 Geliştirici Notu
Bu proje, **ibo_ucatli** tarafından geliştirilmiş bağımsız bir yazılım çalışmasıdır işte kodlar.
import tkinter as tk
from tkinter import messagebox
import json
import os

# Verilerin kaydedileceği dosya
VERI_DOSYASI = "uyghur_social_data.json"

# Dosya yoksa başlangıçta boş bir liste oluştur
if not os.path.exists(VERI_DOSYASI):
    with open(VERI_DOSYASI, "w") as f:
        json.dump([], f)

pencere = tk.Tk()
pencere.title("UyghurSocial Basit")
pencere.geometry("400x550")
pencere.configure(bg="#0a0a0a")

aktif_kullanici = None

# --- FONKSİYONLAR ---

def mesajlari_oku():
    with open(VERI_DOSYASI, "r") as f:
        return json.load(f)

def mesaj_kaydet(veriler):
    with open(VERI_DOSYASI, "w") as f:
        json.dump(veriler, f, indent=4)

def sisteme_baglan():
    global aktif_kullanici
    k_adi = giris_kullanici.get().strip().lower()
    
    if not k_adi:
        messagebox.showwarning("Uyarı", "Lütfen bir kullanıcı adı yazın!")
        return
        
    aktif_kullanici = k_adi
    lbl_durum.config(text=f"🟢 Bağlandı: @{aktif_kullanici}", fg="#00ff00")
    messagebox.showinfo("Başarılı", f"@{aktif_kullanici} olarak UyghurSocial'a giriş yapıldı!")
    akisi_yenile()

def mesaj_paylas():
    if not aktif_kullanici:
        messagebox.showwarning("Uyarı", "Önce yukarıdan giriş yapmalısınız!")
        return
        
    mesaj = txt_mesaj.get().strip()
    if not mesaj:
        return
        
    tum_mesajlar = mesajlari_oku()
    tum_mesajlar.append({"yazar": aktif_kullanici, "icerik": mesaj})
    mesaj_kaydet(tum_mesajlar)
    
    txt_mesaj.delete(0, tk.END)
    akisi_yenile()

def akisi_yenile():
    lst_akis.delete(0, tk.END)
    tum_mesajlar = mesajlari_oku()
    
    for msg in reversed(tum_mesajlar):
        lst_akis.insert(tk.END, f"👤 @{msg['yazar']}: {msg['icerik']}")
        lst_akis.insert(tk.END, "-" * 50)

# --- EN SADE ARAYÜZ TASARIMI ---

# Başlık
tk.Label(pencere, text="UYGHUR_SOCIAL", fg="#00ffcc", bg="#0a0a0a", font=("Courier New", 20, "bold")).pack(pady=10)
lbl_durum = tk.Label(pencere, text="🔴 Bağlantı Yok (Giriş Yapın)", fg="#ff0055", bg="#0a0a0a", font=("Arial", 10)).pack()

# Sadece Kullanıcı Adı Alanı ve Buton
frame_giris = tk.Frame(pencere, bg="#0a0a0a")
frame_giris.pack(pady=15)

giris_kullanici = tk.Entry(frame_giris, fg="#00ff00", bg="#1a1a1a", font=("Arial", 12), width=15, justify="center")
giris_kullanici.insert(0, "ibo_ucatli") # Kolaylık olsun diye varsayılan isim koyduk
giris_kullanici.grid(row=0, column=0, padx=5)

btn_baglan = tk.Button(frame_giris, text="Giriş Yap", command=sisteme_baglan, bg="#00ffcc", fg="#0a0a0a", font=("Arial", 10, "bold"))
btn_baglan.grid(row=0, column=1, padx=5)

# Canlı Akış Alanı
tk.Label(pencere, text="💬 CANLI AKIŞ", fg="#ffffff", bg="#0a0a0a", font=("Arial", 10, "bold")).pack(pady=(10, 2))

lst_akis = tk.Listbox(pencere, bg="#111111", fg="#ffffff", font=("Arial", 10), width=45, height=14)
lst_akis.pack(padx=20, pady=5)

btn_yenile = tk.Button(pencere, text="🔄 Yenile", command=akisi_yenile, bg="#222222", fg="#ffffff")
btn_yenile.pack(pady=5)

# Mesaj Gönderme Alanı
frame_mesaj = tk.Frame(pencere, bg="#0a0a0a")
frame_mesaj.pack(pady=15, fill="x", padx=25)

txt_mesaj = tk.Entry(frame_mesaj, fg="#00ff00", bg="#1a1a1a", font=("Arial", 11), width=28)
txt_mesaj.pack(side="left", ipady=4)

btn_gonder = tk.Button(frame_mesaj, text="Paylaş", command=mesaj_paylas, bg="#00ffcc", fg="#0a0a0a", font=("Arial", 10, "bold"))
btn_gonder.pack(side="right")

# İlk açılışta akışı yükle
akisi_yenile()

pencere.mainloop()
