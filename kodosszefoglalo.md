

# Eredeti Kódrészletek és Magyarázatok

Az alábbiakban minden eredeti kódrészlet megtalálható, kiegészítve magyarázó megjegyzésekkel.

---

### 1. Képek Betöltése és Átméretezése

```cpp
Mat fg = imread("Kepek/pixelek/kurama.jpg", IMREAD_COLOR);  // Előtérkép betöltése
Mat bg = imread("Kepek/pixelek/background.jpg", IMREAD_COLOR); // Háttérkép betöltése
resize(fg, fg, Size(fg.cols / 4, fg.rows / 4));  // Az előtérkép méretének csökkentése
imshow("fg", fg);  // Az előtér megjelenítése
```

**Magyarázat:** Ez a kód az előtér- és háttérképet betölti és átméretezi, hogy a háttérre helyezhesse.

---

### 2. Maszk Létrehozása és Alkalmazása

```cpp
Mat fg = imread("Kepek/pixelek/narancs.jpg", IMREAD_COLOR);  
Mat hsv;
Mat hatter(fg.size(), fg.type(), Scalar(0, 0, 0));  
cvtColor(fg, hsv, COLOR_BGR2HSV);  // Kép átkonvertálása HSV színtérre
Mat mask;
inRange(hsv, Scalar(10,150,100), Scalar(100,255,255), mask);  // Maszk létrehozása színtartomány alapján
imshow("Hatter", hatter);
fg.copyTo(hatter, mask);  // Csak a maszk szerinti részek kerülnek átfedésre
imshow("eredmeny", hatter);
```

**Magyarázat:** A kód az előtér színtartományának megfelelő maszkot hoz létre, amelyet a háttérre illeszt, csak az előtér specifikus részeivel.

---

### 3. Színcsere a Képen

```cpp
Mat_<Vec3b> fg = imread("Kepek/pixelek/milka.jpg", IMREAD_COLOR);  
imshow("asdf", fg);
Mat_<Vec3b> hsv;  
cvtColor(fg, hsv, COLOR_BGR2HSV);  // Kép átalakítása HSV színtérre

int hue;
for (int i = 0; i < hsv.rows; i++) {
    for (int j = 0; j < hsv.cols; j++) {
        hue = hsv(i, j)[0];
        if (hue > 130 and hue < 160) {
            hsv(i, j)[0] = 15;  // A lilás árnyalatok sárgára váltása
        }
    }
}
cvtColor(hsv, hsv, COLOR_HSV2BGR);
imshow("lila", hsv);
```

**Magyarázat:** A lila színárnyalatok egy új színre történő cseréje HSV színtérben.

---

### 4. Intenzitás Alapú Kontrasztjavítás

```cpp
Mat img = imread("Kepek/javitas/deb_deep.png");  
double max, min;
minMaxLoc(img, &min, &max);  // Meghatározza a minimum és maximum intenzitást
Mat dest = (img - min)*(255 / (max - min));  // Normalizálja az értékeket
imshow("dest", dest);
```

**Magyarázat:** A kód normalizálja a kép intenzitását, javítva ezzel a kontrasztot.

---

### 5. Küszöbölés és Zajszűrés

```cpp
Mat img = imread("Kepek/kuszoboles/kutya_feher.jpg",IMREAD_GRAYSCALE);
Mat binary, otsiu, hasonlito;
threshold(img, binary, 100, 255, THRESH_BINARY);  // Bináris küszöbölés
threshold(img, otsiu, 100, 255, THRESH_OTSU);  // Automatikus OTSU küszöbölés
medianBlur(binary, binary, 5);  // Zaj csökkentése
imshow("binary", binary);
imshow("OTSU", otsiu);
imshow("szurt", binary);
```

**Magyarázat:** Ez a rész különböző küszöbértékeket alkalmaz binarizálásra és szűrést a zaj eltávolítására.

---

### 6. Morfológiai Műveletek

```cpp
Mat img = imread("Kepek/mofo/galaxy.jpg",IMREAD_GRAYSCALE);
Mat mask;
morphologyEx(img, mask, MORPH_TOPHAT, getStructuringElement(MORPH_RECT, Size(5, 5)));  // TopHat morfológia
imshow("maszk", mask);
```

**Magyarázat:** TopHat művelettel a kép világos részeit emeli ki a háttérből.

---

### 7. Élkeresés Sobel Operátorral

```cpp
Mat_<uchar> img = imread("Kepek/elkeres/go2.png", IMREAD_GRAYSCALE);
Mat_<uchar>G(img.size(), img.type());
for (int i = 0; i < img.rows - 1; i++) {
    for (int j = 0; j < img.cols - 1; j++) {
        G(i, j) = abs(img(i, j) - img(i + 1, j + 1) + img(i + 1, j) - img(i, j + 1));  // Gradiens számítása
    }
}
threshold(G, G, 45, 255, THRESH_BINARY);  // Küszöbölés bináris élekre
imshow("G", G);
```

**Magyarázat:** A Sobel operátorral a kép éleit számítja, amit bináris értékkel kiemel.

---

### 8. Kördetektálás Hough Transzformációval

```cpp
Mat_<uchar> img = imread("Kepek/elkeres/go2.png", IMREAD_GRAYSCALE);
vector<Vec3f> korok;
medianBlur(img, img, 3);  // Mediánszűrés
HoughCircles(img, korok, HOUGH_GRADIENT, 2, 20, 80, 100, 0, 25);  // Kördetektálás
for (auto c : korok) {
    if (img.at<uchar>(c[1], c[0]) > 128) {
        circle(dest, Point(c[0], c[1]), c[2], Scalar(0, 0, 255), 2);  // Világos kör piros jelöléssel
    } else {
        circle(dest, Point(c[0], c[1]), c[2], Scalar(255, 0, 0), 2);  // Sötét kör kék jelöléssel
    }
}
imshow("dest", dest);
```

**Magyarázat:** Hough transzformációval a körök észlelése és kiemelése a képen, különböző színekkel.

---

