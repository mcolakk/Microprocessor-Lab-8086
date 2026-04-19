# Microprocessor-Lab-8086
8086 Assembly dili ile yazılmış bellek analizi, aritmetik işlemler ve dizi kopyalama uygulamaları


# MICROPROCESSOR LAB

**Öğrenci:** Muhammet Muhlis COLAK  
**Öğrenci Numarası:** 2020555023  
**Tarih:** 19 Nisan 2026  

Bu proje, Mikroişlemciler Laboratuvarı kapsamında Assembly dili kullanılarak gerçekleştirilen bellek yönetimi, aritmetik işlemler ve veri kopyalama uygulamalarını içermektedir.

---

## Soru 1: Little Endian Bellek Yerleşimi

Verilen assembly kodları çalıştırıldığında, x86 mimarisinin "Little Endian" (Küçük Sonlu) özelliği gereği, verilerin en anlamsız baytı (LSB) düşük adrese, en anlamlı baytı (MSB) ise yüksek adrese yazılır. 

Buna göre `MOV WORD PTR [1000h], 1161h`, `MOV [1002h], 0103h` ve `MOV [1004h], 0167h` işlemleri sonucunda ilgili bellek adreslerinin onaltılık (hex) içerikleri aşağıdaki gibi olacaktır:

* **02000:** 61
* **02001:** 11
* **02002:** 03
* **02003:** 01
* **02004:** 67
* **02005:** 01

---

## Soru 2: Toplama İşlemi ve Belleğe Kaydetme

Bu bölümde 80h+80h işlemini gerçekleştiren ve sonucu `0300:3000h` adresine kaydeden Assembly programı yazılmış ve simülatör üzerinde test edilmiştir.

### Assembly Kodu

```assembly
ORG 100h
MOV AX, 0300h
MOV DS, AX
MOV AX, 80h
ADD AX, 80h
MOV [3000h], AX
HLT

Çalıştırma Sonucu ve Bellek Görüntüsü
Aşağıdaki sonuçlar, programın başarılı bir şekilde derlenip çalıştırıldığını ve bellek üzerindeki nihai çıktısını göstermektedir:

Output Console (Çıktı Konsolu):

[11:36:28] EasyCPU 8086 Simulator ready.

[11:40:36] Assembly successful: 6 instructions.

[11:40:41] HLT instruction

Memory Viewer (Bellek Görüntüleyici):

Yapılan toplama işlemi (80h + 80h = 0100h) sonucunda, "Little Endian" mimarisine uygun olarak bellek sonucu 00 01 şeklinde kaydedilmiştir.


Soru 3: Bellek Üzerinde Veri Kopyalama İşlemi
Bu program, 0200:1000h adresinden başlayan ve F4h değeri ile sonlanan bir byte dizisini, 0300:2000h hedef bellek adresine kopyalamaktadır.


MOV AX, 0200h
MOV DS, AX

; Kaynak verileri bellege yazma
MOV AL, 0AAh
MOV [1000h], AL

MOV AL, 0BBh
MOV [1001h], AL

MOV AL, 0F4h
MOV [1002h], AL

; Kopyalama islemine hazirlik ve Dongu
MOV SI, 1000h
MOV AX, 0300h
MOV ES, AX
MOV DI, 2000h

COPY_LOOP:
MOV AL, [SI]
MOV ES:[DI], AL
CMP AL, 0F4h
JZ FINISH
INC SI
INC DI
JMP COPY_LOOP

FINISH:
HLT


Çalıştırma Sonucu ve Bellek Görüntüsü
Program dizi kopyalama döngüsünü başarıyla tamamlamış ve kaynak verilerin bellek tablosu üzerinden doğruluğu teyit edilmiştir.

Output Console (Çıktı Konsolu):

[11:46:29] Assembly successful: 20 instructions.

[11:46:32] HLT instruction

Memory Viewer (Kopyalanan Kaynak Veriler):

1200 satırından itibaren bellek içeriklerinin AA, BB, F4 şeklinde sıralandığı Memory Viewer tablosundan doğrulanmıştır.
