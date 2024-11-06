
# Képkockák - Kérdés-Válasz Formátumban

## 1. Mi a kód célja a képek betöltésénél és átméretezésénél?
**Válasz:**  
A kód két képet tölt be: egy előtér- és egy háttérképet, majd az előteret 4-szer kisebb méretűre átméretezi. Ezután a hátteren megjeleníti az előteret.

**Megjegyzés:**  
```cpp
Mat fg = imread("Kepek/pixelek/kurama.jpg", IMREAD_COLOR);  // Előtér betöltése
Mat bg = imread("Kepek/pixelek/background.jpg", IMREAD_COLOR); // Háttér betöltése
resize(fg, fg, Size(fg.cols / 4, fg.rows / 4));  // Előtér átméretezése
```

## 2. Hogyan működik a maszkok használata és a kép átfedése?
**Válasz:**  
A kód HSV színteret használ, hogy a megadott színeket maszkkal kiválogassa, majd csak a maszk szerinti részeket illeszti a háttérre.

**Megjegyzés:**  
```cpp
cvtColor(fg, hsv, COLOR_BGR2HSV);  // Színkonverzió HSV-re
inRange(hsv, Scalar(10,150,100), Scalar(100,255,255), mask);  // Maszk létrehozása
fg.copyTo(hatter, mask);  // Előtér átfedése a háttérre
```

## 3. Hogyan történik a kép színbeli módosítása, például a lila szín átalakítása?
**Válasz:**  
A kód a kép színeit HSV színtérben módosítja, így a lila színt (hue értékekkel azonosítva) egy megadott másik színre változtatja.

**Megjegyzés:**  
```cpp
cvtColor(fg, hsv, COLOR_BGR2HSV);
for (int i = 0; i < hsv.rows; i++) {
    for (int j = 0; j < hsv.cols; j++) {
        hue = hsv(i, j)[0];
        if (hue > 130 and hue < 160) {
            hsv(i, j)[0] = 15;  // Színárnyalat módosítása
        }
    }
}
cvtColor(hsv, hsv, COLOR_HSV2BGR);  // Visszakonvertálás BGR-re
```

## 4. Hogyan működik a minimális és maximális intenzitású képek szűrése?
**Válasz:**  
A kód egy képből kinyeri a minimális és maximális intenzitást, majd normalizálja az értékeket, hogy a kontrasztot javítsa.

**Megjegyzés:**  
```cpp
minMaxLoc(img, &min, &max);  // Min és max intenzitás meghatározása
Mat dest = (img - min)*(255 / (max - min));  // Normalizálás a kontraszt növelésére
imshow("dest", dest);  // Normalizált kép megjelenítése
```

---
