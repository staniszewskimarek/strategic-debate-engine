# Strategic Debate Engine

Silnik n8n, w którym 9 frameworków strategicznych debatuje ze sobą nad zadanym problemem biznesowym, zamiast generować jedną, fałszywie pewną rekomendację.
<img width="777" height="626" alt="image" src="https://github.com/user-attachments/assets/af8ece11-10f5-4322-870c-c0d47bb08fac" />

## Problem

Większość strategicznych rekomendacji od AI ma jedną wadę: brzmią pewnie, bo nikt im nie zaprzeczył. Strategic Debate Engine zamiast jednego agenta z jedną odpowiedzią uruchamia 9 niezależnych ontologii, które celowo się ze sobą nie zgadzają, i wymusza między nimi krytykę zanim padnie jakakolwiek rekomendacja.

## Architektura

**Runda 1: Diagnoza.** Każdy z 9 ekspertów (Porter, Rumelt, Christensen, Blue Ocean, Effectuation, Jobs-to-be-Done, Lean Startup, Behavioural Strategy, Byron Sharp) dostaje ten sam problem, sformułowany przez Moderatora pod kątem jego własnej logiki.

**Runda 2: Cross-examination.** Każdy framework dostaje do krytyki dwie inne perspektywy (wg ustalonej macierzy par) i wskazuje, co tamten model pomija lub błędnie zakłada.

**Runda 3: Aktualizacja stanowiska.** Każdy ekspert widzi wszystkie krytyki ze strony pozostałych ośmiu, aktualizuje swoją pewność (X/10), wskazuje najsilniejszy argument przeciwny i mówi, co pozostaje niezmienne w jego stanowisku.

**Synthesis.** Mapuje strukturę debaty: największa zgodność, największy konflikt, najbardziej innowacyjny i najbardziej realistyczny kierunek, kluczowe niewiadome, rekomendacja syntetyczna.

**Meta Agent.** Analizuje samą debatę jako system: jakie ukryte założenia podzielali wszyscy eksperci nie kwestionując ich, jakich perspektyw zabrakło przy stole, czy dynamika krytyki wprowadziła bias.

Wynik trafia do pojedynczego pliku tekstowego z pełnym zapisem wszystkich trzech rund, syntezy i meta-analizy.

## Wymagania

- Instancja n8n (self-hosted lub cloud)
- Własny klucz API OpenAI podpięty jako credential typu "OpenAi account"
- Model: domyślnie skonfigurowany na `gpt-4.1` (można podmienić w każdym z węzłów `LLM_*`)

## Instalacja

1. Pobierz plik `strategic-debate-engine.json` z tego repozytorium.
2. W n8n: **Settings → Import from File** i wskaż pobrany plik.
3. Każdy węzeł credentiala OpenAI ma w pliku placeholder `REPLACE_WITH_YOUR_CREDENTIAL_ID`. Po imporcie n8n poprosi o przypisanie własnego credentiala do wszystkich węzłów typu `lmChatOpenAi` (jest ich 30, ale przypisanie jest jednorazowe i zbiorcze przez UI).
4. Aktywuj workflow i uruchom przez formularz startowy (Form Trigger), wpisując opis problemu strategicznego.

## Uwagi

- Każda pełna debata to ok. 30 wywołań modelu (3 rundy × 9 ekspertów + moderator + synteza + meta-analiza), licz się z kosztem API odpowiednim do długości promptów i wybranego modelu.
- Macierz par krytyki w Rundzie 2 jest ustalona na stałe w kodzie węzła `PrepareR2`, można ją swobodnie modyfikować.
- Workflow nie wydaje rekomendacji z fałszywą pewnością. Output to mapa niezgody, nie wyrocznia.

## Licencja

MIT, patrz [LICENSE](LICENSE).

## Autor

[Marek Staniszewski](https://heuristica.pl)
