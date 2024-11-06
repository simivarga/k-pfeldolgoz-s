
# Képkockák - Kérdés-Válasz Formátumban

## 1. Mi a kód célja a képek betöltésénél és átméretezésénél?
**Válasz:**  
A kód két képet tölt be: egy előtér- és egy háttérképet, majd az előteret 4-szer kisebb méretűre átméretezi. Ezután a háttérre helyezi az előteret, figyelve a színértékekre.

**Megjegyzés:**  
```cpp
Mat fg = imread("Kepek/pixelek/kurama.jpg", IMREAD_COLOR);  // Előtér betöltése
Mat bg = imread("Kepek/pixelek/background.jpg", IMREAD_COLOR); // Háttér betöltése
resize(fg, fg, Size(fg.cols / 4, fg.rows / 4));  // Előtér átméretezése
imshow("fg", fg);
Mat dest(fg.size(), fg.type(), Scalar(0, 0, 0));  // Üres kép a másoláshoz
```

## 2. Hogyan működik a maszk használata és kép átfedése?
**Válasz:**  
A kód HSV színtérre konvertálja az előteret, hogy egy maszkot készítsen az adott színtartomány alapján, majd az előtér elemeit a maszk alapján a háttérre illeszti.

**Megjegyzés:**  
```cpp
cvtColor(fg, hsv, COLOR_BGR2HSV);  // Színkonverzió HSV-re
inRange(hsv, Scalar(10,150,100), Scalar(100,255,255), mask);  // Maszk létrehozása
fg.copyTo(hatter, mask);  // Maszk segítségével átfedés
```

## 3. Hogyan történik egy adott színtartomány cseréje, pl. a lila szín átalakítása?
**Válasz:**  
A kód HSV színtérben módosítja az adott színcsatornát egy másik színértékre, például lilát sárgára.

**Megjegyzés:**  
```cpp
cvtColor(fg, hsv, COLOR_BGR2HSV);
for (int i = 0; i < hsv.rows; i++) {
    for (int j = 0; j < hsv.cols; j++) {
        hue = hsv(i, j)[0];
        if (hue > 130 and hue < 160) {
            hsv(i, j)[0] = 15;  // Szín módosítása
        }
    }
}
cvtColor(hsv, hsv, COLOR_HSV2BGR);  // Visszakonvertálás BGR-re
```

## 4. Hogyan történik a kontrasztjavítás intenzitás alapján?
**Válasz:**  
A minimális és maximális intenzitás alapján a képen kontrasztjavítást végez, normalizálja az értékeket a jobb láthatóság érdekében.

**Megjegyzés:**  
```cpp
minMaxLoc(img, &min, &max);  // Min és max intenzitás meghatározása
Mat dest = (img - min)*(255 / (max - min));  // Normalizálás kontrasztjavításra
imshow("dest", dest);
```

## 5. Hogyan működik a küszöbölés a kép binarizálására?
**Válasz:**  
A kód különböző küszöbértékeket használ a kép bináris átalakításához, például OTSU küszöböléssel és mediánszűréssel javítva a zajt.

**Megjegyzés:**  
```cpp
threshold(img, binary, 100, 255, THRESH_BINARY);  // Egyszerű küszöbölés
threshold(img, otsiu, 100, 255, THRESH_OTSU);  // OTSU automatikus küszöbölés
medianBlur(binary, binary, 5);  // Mediánszűrés zajcsökkentésre
```

## 6. Mi a célja a morfológiai műveleteknek?
**Válasz:**  
Morfológiai műveleteket, például TopHat, erózió és dilatáció használ a képelemek finomítására és háttérkiemelésre.

**Megjegyzés:**  
```cpp
morphologyEx(img, mask, MORPH_TOPHAT, getStructuringElement(MORPH_RECT, Size(5, 5)));  // TopHat
erode(mask, erod, s);  // Erózió
dilate(mask, dill, s);  // Dilatáció
```

## 7. Hogyan történik az élkeresés a Sobel operátorral?
**Válasz:**  
A Sobel operátor két irányú (x és y) gradiensét számítja a képen, amelyet külön ábrázol, majd kombinál az élek jobb kiemelése érdekében.

**Megjegyzés:**  
```cpp
Sobel(img, grad1, CV_32F, 1, 0);  // X irányú gradiens
Sobel(img, grad2, CV_32F, 0, 1);  // Y irányú gradiens
grad = cv::abs(grad1) + abs(grad2);  // Gradiensek összegzése
threshold(dest, dest2, 30, 255, THRESH_BINARY);  // Binarizált élek
```

## 8. Hogyan működik a kördetektálás a Hough transzformációval?
**Válasz:**  
A Hough transzformációval körök középpontját és sugárértékeit keresi a képen, amelyeket a gradiens alapú keresés után színesen jelöl.

**Megjegyzés:**  
```cpp
HoughCircles(img, korok, HOUGH_GRADIENT, 2, 20, 80, 100, 0, 25);  // Kördetektálás
for (auto c : korok) {
    if (img.at<uchar>(c[1], c[0]) > 128) {
        circle(dest, Point(c[0], c[1]), c[2], Scalar(0, 0, 255), 2);  // Világos kör
    } else {
        circle(dest, Point(c[0], c[1]), c[2], Scalar(255, 0, 0), 2);  // Sötét kör
    }
}
```

---
