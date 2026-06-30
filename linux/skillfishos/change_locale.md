# Zmiana języka z włoskiego (ITA) na angielski (ENG)

Poradnik opisuje sposób zmiany domyślnego języka SkillFishOS z włoskiego na angielski oraz naprawę pozostałości po włoskim locale.

System testowany na platformie AMD BC250.

Jeżeli po instalacji SkillFishOS część systemu, Heroic Games Launcher lub aplikacje SkillFish wyświetlają się po włosku, wykonaj poniższe kroki.

> **Uwaga:** przed rozpoczęciem warto wykonać kopię ważnych danych.

---

# Krok 1 – Zmień język systemu

Jeżeli system jest po włosku, otwórz:

**Impostazioni di sistema → Regione e lingua → Lingua**

lub w wyszukiwarce ustawień wpisz:

```text
lingua
```

Ustaw język na:

**English (United States)**

(Obecnie aplikacje SkillFishOS posiadają pełne tłumaczenie głównie na język angielski i włoski, dlatego English jest najbezpieczniejszym wyborem.)

Kliknij:

**Applica**

Następnie **uruchom komputer ponownie**.

---

# Krok 2 – Napraw locale systemowe

Otwórz terminal i wpisz:

```bash
sudo nano /etc/environment
```

Plik powinien zawierać wyłącznie:

```text
LANG=en_US.UTF-8
```

Usuń linie:

```text
LC_MESSAGES=it_IT.UTF-8
LC_TIME=it_IT.UTF-8
```

Zapisz plik (`Ctrl+O`, Enter) i zamknij edytor (`Ctrl+X`).

---

Następnie otwórz:

```bash
sudo nano /etc/default/locale
```

Usuń wpisy:

```text
LC_MESSAGES=it_IT.UTF-8
LC_TIME=it_IT.UTF-8
```

Pozostaw:

```text
LANG=en_US.UTF-8
```

Pozostałe wpisy `LC_ADDRESS`, `LC_NAME`, `LC_PAPER` itd. można pozostawić bez zmian.

---

# Krok 3 – Uruchom komputer ponownie

Restart jest konieczny.

Po ponownym uruchomieniu sprawdź w terminalu:

```bash
locale
```

Nie powinno już być wpisów:

```text
LC_MESSAGES=it_IT.UTF-8
LC_TIME=it_IT.UTF-8
```

---

# Krok 4 – Napraw katalogi użytkownika

Jeżeli nadal masz katalogi:

* Documenti
* Immagini
* Scaricati
* Musica
* Video

wykonaj:

```bash
rm ~/.config/user-dirs.dirs
rm ~/.config/user-dirs.locale
LANG=en_US.UTF-8 xdg-user-dirs-update --force
```

System utworzy nowe katalogi:

* Desktop
* Documents
* Downloads
* Music
* Pictures
* Public
* Templates
* Videos

---

# Krok 5 – Przenieś swoje dane

Przed usunięciem starych włoskich katalogów przenieś z nich wszystkie ważne pliki.

Przykład:

* Immagini → Pictures
* Documenti → Documents
* Scaricati → Downloads

Dopiero po upewnieniu się, że wszystkie dane zostały skopiowane, usuń stare katalogi.

---

# Krok 6 – Napraw zakładki w Dolphinie (jeżeli nadal prowadzą do włoskich katalogów)

Jeżeli kliknięcie np. **Pictures** nadal otwiera **Immagini**, otwórz:

```bash
nano ~/.local/share/user-places.xbel
```

Znajdź wpis:

```xml
<bookmark href="file:///home/NAZWA_UŻYTKOWNIKA/Immagini">
```

i zamień go na:

```xml
<bookmark href="file:///home/NAZWA_UŻYTKOWNIKA/Pictures">
```

Tak samo popraw pozostałe wpisy:

* Documenti → Documents
* Scaricati → Downloads
* Musica → Music
* Video → Videos
* Scrivania → Desktop

Następnie zamknij Dolphina:

```bash
killall dolphin
```

i uruchom go ponownie:

```bash
dolphin &
```

---

# Gotowe!

Po wykonaniu wszystkich kroków:

✅ System będzie działał z poprawnym angielskim locale.

✅ Aplikacje SkillFishOS będą uruchamiały się po angielsku.

✅ Heroic Games Launcher nie będzie przejmował włoskiego locale.

✅ Nowe katalogi użytkownika będą miały angielskie nazwy.

✅ Dolphin będzie otwierał poprawne katalogi.

---

**Poradnik został przygotowany na podstawie rzeczywistej diagnozy wykonanej na SkillFishOS 26.06.**

---

## Powiązane

- README.md — najczęściej spotykane problemy SkillFishOS.
- GitHub Issue #11 — zgłoszenie błędu dotyczącego locale.
