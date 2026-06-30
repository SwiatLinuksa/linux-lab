# SkillFishOS

SkillFishOS to dystrybucja oparta na Debianie SID (Experimental), testowana na platformie AMD BC250.

Ten dział zawiera najczęściej spotykane problemy napotkane podczas testów oraz sprawdzone rozwiązania.

---

# Środowisko testowe

**Platforma:** AMD BC250

**System:** SkillFishOS

---

# Znane problemy

## 1. Btrfs / GRUB Rescue

### Objawy

Instalacja z domyślnym systemem plików **Btrfs** kończyła się uruchomieniem systemu w trybie **GRUB Rescue**.

Na tym samym sprzęcie instalacja z **ext4** przebiegała poprawnie.

### Tymczasowe rozwiązanie

Podczas instalacji wybrać system plików **ext4** zamiast domyślnego Btrfs.

> Status: zgłoszono jako błąd w repozytorium SkillFishOS.

---

## 2. Docker

### Objawy

AI Panel nie uruchamiał kontenerów Docker.

Przyczyną był brak dodania aktywnego użytkownika do grupy `docker`.

### Rozwiązanie

```bash
sudo usermod -aG docker $USER
```

Następnie wylogować się i zalogować ponownie.

> Status: zgłoszono jako błąd w oryginalnym repozytorium SkillFishOS.

---

## 3. AI (Ollama + AMD iGPU)

### Objawy

Modele Ollama były uruchamiane na procesorze zamiast na zintegrowanym układzie graficznym AMD.

### Rozwiązanie

W pliku `compose.yaml` należy dodać zmienną środowiskową:

```yaml
environment:
  - OLLAMA_IGPU_ENABLE=1
```

Następnie ponownie uruchomić kontenery:

```bash
docker compose down
docker compose up -d
```

Sprawdzenie wykorzystania GPU:

```bash
docker exec skillfish-ollama ollama ps
```

Prawidłowy wynik:

```text
PROCESSOR    100% GPU
```

> Status: zgłoszono jako błąd w oryginalnym repozytorium SkillFishOS.

---

# Dodatkowe poradniki

- [locale.md](locale.md) — zmiana języka z włoskiego (ITA) na angielski (ENG).

---

# Powiązane zgłoszenia

- Btrfs / GRUB Rescue
- Docker (brak grupy `docker`)
- AMD iGPU / Ollama (`OLLAMA_IGPU_ENABLE=1`)
