SURE ANALIZI 1.3.0  (Qt / PySide6)   created by EYALTIRIKLI
==========================================================

Ne yapar: SAP cikti(lar)ini (.xls / .xlsx) okur; solda calisanlar, ustte
tezgahlar (Work ctr), hucrede o kisinin o tezgaha girdigi sure.
  - Tarih araligi secilir (tek gun ya da birden fazla gun).
  - Birden fazla gunde "Ayri sutun": her tezgah tarihlere bolunur, en
    basta GUNLUK TOPLAM grubu (her gunun toplami) durur.  "Toplam": aralik
    toplami tek sutunda.
  - Isim yaninda TOPLAM (birden fazla gunde Σ = aralik toplami).
  - Sure: Iscilik ya da Makina.
  - "Gunluk toplam" kutusu: GUNLUK TOPLAM grubunu acar / kapatir.
  - BOYAMA: "Altinda" / "Ustunde" esigi.  Her TEZGAH GIRISI (tablodaki
    her dolu hucre) esige gore boyanir; toplamlar boyanmaz.  Esigin yaninda
    boyanan girislerin, tabloda gorunen tum girislere orani yazar:
    "%21,7 (13 / 60 giris) · sure %10,7" (sure = o girislerin suresinin
    gorunen toplam sureye orani).  Grup / kisi / tarih filtresine uyar.
  - VERI YUKLE: SAP ciktisi programin yanindaki VERITABANINA yazilir:
      sure_data.db  (exe ile ayni klasorde; derlenmis halde
      dist\SURE ANALIZI\sure_data.db).  WIP TRACKER gibi: yuklenen
    dosyadaki tarihler veritabaninda varsa once silinir, yenisi yazilir -
    ayni gun iki kez yuklense de iki kat sayilmaz.  Diger tarihler kalir.
    Yeri ve son yuklemeler: alttaki "Yukleme raporu".  Orada "Tum veriyi
    sil" de var.  Yeni surume gecerken sure_data.db dosyasini yeni exe'nin
    yanina kopyalayin, veriler gelir.
  - PARCA ILERLEMESI (kutu ile acilir/kapanir): tezgah basliginin altinda
    o gun tezgahta kesilen parca (E sutunu xxxxMYYGxx -> MYY; E bossa ayni
    seri no'lu (F) baska satirdan; hic yoksa seri no) ve bir cubuk:
      yesil = o tarihten ONCE o tezgahta o seri + operasyona girilen sure
      turuncu = O GUN (aralikta) girilen
      bos = standarda kalan  (standart: parca + operasyon (G sutunu))
    Sure: makina suresi (O); makina 0 olan giriste o girisin iscilik.
    O gun ikinci bir seri baglandiysa altinda "-> M18" ile o da gosterilir
    (saat sutunu Z'ye gore sirali).  3'ten fazla parca: "+N parca",
    hepsi ustune gelince cikan ipucunda.  Standardi asan cubuk kirmizi
    cerceveli.  Excel'e aktarmada 3. ve 4. satira yazilir.
  - STANDART SURE YUKLE: Excel - Parca no | Operasyon | Standart sure
    (saat).  Parca 2639M17G02 ya da M17 yazilabilir; bos parca hucresi
    ustteki parcayi tekrar eder.  Ornek: STANDART_SABLON.xlsx.  Yeni liste
    ayni parca + operasyonun eskisinin yerine gecer, digerleri kalir.

Okunan sutunlar (baslik metniyle dogrulanir; sutun kaymissa basligindan
bulunur, bulunamazsa okunmaz - "Yukleme raporu"nda yazar):
  C  Kyt tarihi    I  Work ctr    O  Makina suresi    P  Iscilik
  AB Yaka no       AC Calisan ismi
  E  Parca no      F  Seri no     G  Operasyon        Z  Saat (sira icin)
Ara bos satirlar ve tekrar eden baslik satirlari atlanir.  Okunamayan
her satir sebebiyle birlikte Yukleme raporunda sayilir.
Okunan dosya turleri: gercek .xls (97-2003), .xlsx, SAP'in HTML/MHTML,
XML ya da sekmeli metin olarak kaydettigi .xls - WIP TRACKER ile ayni okuyucu.

1) DOSYALARI .py YAPIN
  Kaynak dosyalar .txt uzantili gelir.  OKU.txt HARIC hepsini .py yapin.

  Windows - PowerShell (zipi actiginiz klasorde):
    Get-ChildItem *.txt |
      Where-Object { $_.Name -notin 'OKU.txt' } |
      Rename-Item -NewName { $_.Name -replace '\.txt$','.py' }

  Windows - Komut Istemi (cmd):
    ren *.txt *.py
    ren OKU.py OKU.txt

  Mac - Terminal:
    for f in *.txt; do
      [ "$f" = OKU.txt ] || mv "$f" "${f%.txt}.py"
    done

  4 adet .py olmali: sure_app.py, sure_core.py, sure_readers.py,
  sure_theme.py.  Calistirilan dosya: sure_app.py

2) KURULUM (bir kere, Windows cmd):
    py -m pip install PySide6 openpyxl xlrd pyinstaller
  Kontrol:
    py -c "import xlrd, sys; print(xlrd.__version__, sys.executable)"
  Denemek icin derlemeden calistirma:
    py sure_app.py

3) DERLEME (Windows, cmd):
  py -m PyInstaller --onedir --windowed --noupx --icon=sure_icon.ico ^
    --collect-all xlrd --name "SURE ANALIZI" sure_app.py

  Exe:  dist\SURE ANALIZI\SURE ANALIZI.exe
  (Not: komut "py -m PyInstaller" - buyuk P ve I ile.)
