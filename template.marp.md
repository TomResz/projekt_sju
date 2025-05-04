---
marp: true
math: mathjax
title: Raport z projektu
size: 16:9
paginate: true
mermaid: true
transition: fade
theme: gaia
backgroundImage: url("page-background.png")
footer: "**Raport SJU**"
style: |
  @import url('https://unpkg.com/tailwindcss@^2/dist/utilities.min.css');
---

<!-- Strona tytułowa -->

<div class="flex flex-col items-center justify-center h-full text-center">

<img src="https://upload.wikimedia.org/wikipedia/commons/3/34/Logo_PolSl.svg" class="w-60" />

#### Raport z projektu

###### *Imię i nazwisko:* Tomasz Respondek  
###### *Kierunek:* Teleinformatyka s.2  
###### *Semestr:* 1

</div>

---

#### Etap 1: Przygotowanie repozytorium projektu na GitHub 
- Wykonano *Fork* repozytorium na GitHub.  
- Repozytorium otwarto w środowisku **Codespaces**.
<div class="flex flex-row justify-center">

![width:600px](https://raw.githubusercontent.com/TomResz/projekt_sju/refs/heads/main/doc/pkt1/codespace_uruchomienie.png)
*Widok repozytorium w Codespaces*

</div>

---
#### Etap 1: Przygotowanie repozytorium projektu na GitHub - Docker część nr 1

<div class="mb-3">

- W kolejnym kroku został zbudowany i uruchomiony kontener Dockerowy.

</div>

<div class="flex flex-row justify-center align-center text-center">

![width:100%](https://raw.githubusercontent.com/TomResz/projekt_sju/refs/heads/main/doc/pkt1/pierwszy_build.png)
*Build kontenera Docker*

</div>

---
#### Etap 1: Przygotowanie repozytorium projektu na GitHub - Docker część nr 2

<div class="mb-3">

- W celu sprawdzenia poprawności poprawnego zbudowania kontenera zostało sprawdzone czy istnieją w kontenerze pliki projektu.

</div>

<div class="flex flex-row justify-center align-center text-center">

![width:100%](https://raw.githubusercontent.com/TomResz/projekt_sju/refs/heads/main/doc/pkt1/pliki_projektu.png)
*Pliki wewnątrz kontenera*
</div>


---

#### Etap 2: Modyfikacja Dockerfile

- Do pliku Dockerfile zostały dodane `Qiskit`, `Matplotlib`, `Pillow`, `Pycryptodomex`, `Cryptography`. Obraz został ponownie przebudowany.
```Docker
RUN pip3 install --disable-pip-version-check --no-cache-dir \
    ipykernel \
    jupyter \
    qiskit \
    matplotlib \
    Pillow \
    pycryptodomex \
    cryptography 
```

---
#### Etap 3: Kontener developerski
- Został stworzony plik konfiguracyjny do którego zostały dodane wymagane rozszerzenia.

```json
{
    "workspaceMount": "source=${localWorkspaceFolder},target=/home/vscode/workspace,type=bind,consistency=cached",
    "workspaceFolder": "/home/vscode/workspace",
    "name": "Projekt-SJU",
    "image": "sjuprojekt",
    "customizations": {
      "vscode": {
        "extensions": ["ms-python.python","ms-toolsaijupyter","yzhang.markdown-all-in-one","marp-team.marp-vscode",
          "github.vscode-github-actions"
        ]
      }
    },
    "postCreateCommand": "pip install --no-cache-dir -r requirements.txt && uname -a && python --version",
    "remoteUser": "vscode"
  }
```
---
#### Etap 4: Github Actions
- W tym etapie został stworzony plik [`.github/workflows/docker-publish.yml`](https://github.com/TomResz/projekt_sju/blob/main/.github/workflows/docker-publish.yml), w którym został napisany cały pipeline CI/DI. Poszczególne etapy zostały zaprezentowane na rysunku obok.


![bg right fit](https://raw.githubusercontent.com/TomResz/projekt_sju/195233304bc4791446169409fb22660f804608a3/doc/Editor%20_%20Mermaid%20Chart-2025-05-04-110749.svg)
![bg right auto width:80%](https://raw.githubusercontent.com/TomResz/projekt_sju/refs/heads/main/doc/pkt4/etapy.png)

---
#### Etap 4: Github Actions - działanie pipeline CI/CD
- W tym kroku zostało przeprowadzone sprawdzenie działania workflowu Github Actions

<div class="flex flex-colum justify-center align-center text-center gap-6">

<div>

![width:250px](https://raw.githubusercontent.com/TomResz/projekt_sju/refs/heads/main/doc/pkt4/udany_build.png)
<small>Poprawnie wykonany workflow </small>
</div>

<div>

![width:450px](https://raw.githubusercontent.com/TomResz/projekt_sju/refs/heads/main/doc/pkt4/udany_test.png)
<small>Poprawnie wykonany skrypt test.py</small>
</div>
</div>

---
#### Etap 4: Github Actions - zmiana image w kontenerze developerskim

- Po poprawnym wykonaniu workflow i otagowaniu nowej wersji obrazu w pliku JSON `.devcontainer/devcontainer.json` w sekcji image został zmieniony obraz na nowy wygenerowany na podstawie workflowa.

```json
...
"image": "ghcr.io/tomresz/projekt_sju:v1.0.0",
...
```

---
#### Etap 5: Praca z repozytorium w kontenerze developerskim
- W repozytorium został stworzony katalog `sample`, gdzie został dodany notatnik Jupyter'a.
- W celu poprawnego uruchomienia notatnika było trzebna uaktualnić plik `Dockerfile` brakującymi bibliotekami potrzebnymi do uruchomienia oraz dodać nowy tag w zdalnym repozytorium, który utworzy nową wersję obrazu.
- W następnym kroku trzeba było zmienić wersję obrazu w pliku `.devcontainer/devcontainer.json` na najnowszą. 
- Takie zmiany pozwoliły na bezproblemowe uruchomienie notatnika w kontenerze developerskim.

---
#### Adnotacje do projektu

Poniżej znajdują się przydatne linki związane z projektem:

- [Repozytorium GitHub](https://github.com/TomResz/projekt_sju) – główne repozytorium projektu.
- [Plik ustawień kontenera developerskiego (devcontainer.json)](https://github.com/TomResz/projekt_sju/blob/main/.devcontainer/devcontainer.json) – konfiguracja środowiska programistycznego.
- [Dockerfile](https://github.com/TomResz/projekt_sju/blob/main/Dockerfile) – definicja obrazu Dockera.
- [Workflow GitHub Actions](https://github.com/TomResz/projekt_sju/blob/main/.github/workflows/docker-publish.yml) – automatyzacja procesu CI/CD.

---
#### Wnioski z realizacji projektu

- **Najtrudniejszym etapem** było skonfigurowanie poprawnego działania workflow GitHub Actions.

- **Nauczyłem się:**
  - Pracy z kontenerami Docker w kontekście środowisk developerskich,
  - Tworzenia i konfigurowania plików `Dockerfile`,`devcontainer.json` oraz workflowów CI/CD,
  - Zarządzania wersjami obrazów Docker za pomocą GitHub Container Registry.
