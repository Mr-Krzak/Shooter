# 🎮 Zasady Gry – Multiplayer Shooter (Phaser + Firebase)

---

# 1️⃣ Zasady Rozgrywki (Gameplay)

## 👤 Gracz

* Logowanie anonimowe przez Firebase
* Wybór nicku przed rozpoczęciem gry
* Każdy gracz posiada:

  * 5 HP
  * indywidualny kolor
  * licznik zabójstw (score)
* Po śmierci następuje respawn po 3 sekundach
* Respawn przywraca:

  * HP = 5
  * pozycję startową
  * status `alive = true`
* Kiedy od 6 sekund gracz nie otrzymał obrażeń ani nie strzelił to + 1HP co 3 sekundy (pierwsze po tych 6 sekundach)

---

## 🔫 Strzelanie

* Strzał wykonywany kliknięciem myszy
* Cooldown między strzałami: ~350 ms
* Pocisk:

  * porusza się w kierunku kursora
  * znika po 2 sekundach
  * znika po trafieniu ściany
  * znika po trafieniu gracza

---

## ❤️ System Obrażeń

* Trafienie odejmuje 1 HP
* Przy 0 HP:

  * gracz ginie
  * uruchamiana jest animacja śmierci
  * zabójca otrzymuje +1 punkt

---

## 🏆 Score / Ranking

* Zabicie przeciwnika = +1 punkt dla zabójcy
* Score przechowywany w Firebase
* Leaderboard sortowany malejąco po score
* Wynik wyświetlany na ekranie
* Każdy z graczy wyświetla swój wynik
* Aktualny leaderboard jest wyświetlany u każdego z graczy

---

# 2️⃣ Logika Multiplayer (Firebase)

## 📡 Synchronizacja Graczy

* Pozycja aktualizowana co ~100 ms
* Inni gracze wyświetlani jako prostokąty
* Po wyjściu gracza jego rekord usuwany (onDisconnect)

---

## 🔄 Synchronizacja Pocisków

* Pociski zapisywane w kolekcji `bullets`
* `onChildAdded` → tworzy pocisk u wszystkich graczy
* `onChildRemoved` → usuwa pocisk u wszystkich

---

## 💥 System Trafień

* Kolizja wykrywana przez physics overlap
* Trafienie zapisywane w `hits`
* Odbiorca trafienia zmniejsza swoje HP
* Przy śmierci:

  * zapis w `deaths`
  * aktualizacja score przez `runTransaction`

---

# 3️⃣ Środowisko Gry

## 🗺️ Mapa

* Rozmiar świata: 3000 x 3000 px
* Kamera podąża za lokalnym graczem
* Istnieją granice mapy

---

## 🧱 Ściany

* Obiekty typu static physics
* Blokują:

  * ruch gracza
  * pociski
* Pocisk po uderzeniu w ścianę zostaje usunięty

---

# 4️⃣ Grafika i Efekty

## 👤 Gracze

* Prostokąt 32x32 px
* Każdy ma losowy kolor
* Po śmierci:

  * fade out
  * powiększenie (scale 1.5)
  * reset przy respawnie

---

## 🔫 Pociski

* Okrągłe (circle body)
* Kolor: żółty (`0xffff00`)
* Rozmiar: ~12 px
* Brak grawitacji
* Prędkość: ~900 px/s

---

## 💥 Efekty Trafienia

* Czerwony okrąg
* Krótka animacja:

  * powiększenie
  * fade out
* Czas trwania: ~300 ms
* Widoczny dla wszystkich graczy

---

## 📊 UI

Widoczne elementy interfejsu:

* HP (lewy górny róg)
* Score (lewy górny róg)
* Leaderboard (prawy górny róg)

---

# 5️⃣ Ograniczenia i Uproszczenia

Obecna wersja gry:

* Brak interpolacji ruchu
* Logika obrażeń nie jest serwer‑autorytatywna
* Brak systemu granatów AOE
* Brak traila za pociskiem
* Brak systemu klas/broni

---

# 6️⃣ Model Architektury Gry

* Realtime arena multiplayer shooter 2D
* Każdy gracz jest równorzędnym klientem
* Firebase pełni rolę synchronizatora danych
* Brak serwera autorytatywnego

---

**Wersja dokumentu: Aktualna implementacja gry**
