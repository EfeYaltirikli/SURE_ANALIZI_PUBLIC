SURE ANALIZI 1.0.0  (Qt / PySide6)   created by EYALTIRIKLI
==========================================================

Ne yapar: SAP cikti(lar)ini (.xls / .xlsx) okur; solda calisanlar, ustte
tezgahlar (Work ctr), hucrede o kisinin o tezgaha girdigi sure.
  - Tarih araligi secilir (tek gun ya da birden fazla gun).
  - Birden fazla gunde "Ayri sutun": her tezgah tarihlere bolunur, en
    basta GUNLUK TOPLAM grubu (her gunun toplami) durur.  "Toplam": aralik
    toplami tek sutunda.
  - Isim yaninda TOPLAM (birden fazla gunde Σ = aralik toplami).
  - Sure: Iscilik ya da Makina.
  - BOYAMA: "Altinda" / "Ustunde" esigi.  Ayri sutunda her GUNUN toplami,
    Toplam modunda ARALIK toplami boyanir.
  - Baslik tiklaninca siralar (Calisan, Yaka no, TOPLAM, gun toplamlari).
  - Excel'e aktar: ekrandaki tablo, ayni baslik duzeni ve boyamayla.
  - Son yuklenen dosyalar ve ayarlar acilista hatirlanir.

Okunan sutunlar (baslik metniyle dogrulanir; sutun kaymissa basligindan
bulunur, bulunamazsa okunmaz - "Yukleme raporu"nda yazar):
  C  Kyt tarihi    I  Work ctr    O  Makina suresi    P  Iscilik
  AB Yaka no       AC Calisan ismi
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
