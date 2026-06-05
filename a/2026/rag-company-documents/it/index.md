---
layout: article
title: "RAG sui documenti aziendali: quando serve davvero"
description: "Quando il RAG aiuta con i documenti aziendali, quando aggiunge complessità inutile e quali costi comporta in produzione."
date: 2026-06-03
lang: it
topics: AI applicata · RAG
reading_time: 6 min di lettura
language_current: Italiano
language_label: English
language_url: /a/2026/rag-company-documents/en/
language_alt_lang: en
language_default_url: /a/2026/rag-company-documents/en/
---

Costruire un sistema "AI" che risponde a domande sui documenti aziendali può sembrare un progetto lineare: indicizzi le fonti, colleghi un modello linguistico e restituisci una risposta con qualche riferimento ai documenti consultati.

Funziona in una demo, ma non quando entra nell'uso quotidiano, perché i documenti reali sono molto più variopinti di quanto si pensi. Ci sono versioni diverse dello stesso file, documenti che rimandano ad altri documenti, allegati, permessi, formati legacy, archivi zip, PDF di sole immagini, eccezioni, domande impreviste e utenti che non sanno ancora come formulare la domanda.

Il punto non è solo far rispondere il modello: è progettare tutto quello che viene prima della risposta. In pratica devi decidere come indicizzare i documenti, come cercarli, come usare metadati e versioni, come riordinare i risultati, quando rispondere e quando dichiarare che le fonti non bastano. Se queste scelte restano implicite, il risultato diventa imprevedibile.

## Quando ha senso usare un RAG

Il RAG ha senso quando la risposta deve poter essere collegata a una fonte. Se un utente chiede quali siano le regole per il rimborso di una trasferta, una risposta plausibile non basta. Il sistema deve sapere da quale policy arriva l'informazione, se è aggiornata e se vale per quel reparto. Lo stesso vale per contratti, procedure e documenti di compliance, dove una risposta basata sulla fonte sbagliata può creare problemi.

Il RAG è utile anche quando la domanda dell'utente non usa lo stesso linguaggio dei documenti. Una persona può chiedere se può farsi rimborsare il taxi, mentre la policy parla di spese per i mezzi pubblici. La sola ricerca per parole chiave può non bastare. Possono servire embedding, ricerca ibrida, metadati e spesso un reranker.

Un altro vantaggio di un sistema ben progettato è la possibilità di migliorare man mano che viene usato. Se registri le domande poste dagli utenti, i feedback sulle risposte e gli errori commessi dal sistema, puoi costruire un dataset utile a capire dove sbaglia.

A volte il problema sta nella pipeline di ricerca: documenti sbagliati o obsoleti, metadati incompleti, chunking, retrieval, reranking o contesto passato al modello.

In altri casi puoi usare quel dataset sia per fare fine-tuning, sia per migliorare retrieval, prompt e valutazione del sistema. A un certo livello, parte di questo processo può anche essere automatizzata: il sistema migliora da solo attraverso i feedback degli utenti, invece di restare fermo alla prima versione.

## Quando diventa una complicazione inutile

Ci sono casi in cui il RAG aggiunge più complessità di quanta ne risolva.

Se la base documentale è piccola, può bastare passare direttamente i documenti a un LLM con una finestra di contesto adeguata. Se l'obiettivo è classificare email, smistare ticket o estrarre pochi campi da documenti standard, un sistema RAG può essere una complicazione inutile.

C'è poi un problema di base documentale. Se l'azienda non sa quali documenti siano validi, la prima cosa da fare è lavorare su pulizia, versioni, ownership e archiviazione. Altrimenti rischi di costruire un sistema elegante sopra contenuti incoerenti.

Ancora prima dei documenti, c'è un problema più sottile: spesso non si riesce a definire il perimetro del sistema né a misurarne i risultati. Quali risposte deve dare? Su quali fonti? Quando deve rifiutarsi di rispondere? E non lo valuti chiedendo a tre colleghi se la risposta "sembra buona": ti servono domande reali, le risposte attese e le fonti giuste. All'inizio possono bastare pochi casi, ma devono essere espliciti.

A volte, poi, l'obiettivo non è rispondere meglio sui documenti, ma avviare azioni. Se il sistema deve consultare documenti, chiamare API, aggiornare stati, aprire ticket e applicare regole di business, allora il RAG è solo un pezzo. Bisogna progettare un workflow, non solo una ricerca documentale.

## I costi nascosti di un sistema RAG

Il costo del RAG non sta solo nella bolletta dei token a fine mese o nell'hardware da comprare se lo fai girare in casa. Sta nello sviluppo del sistema e in tutto quello che succede prima e dopo la chiamata al modello.

Devi estrarre testo da PDF, DOCX, pagine HTML, scansioni e allegati. A volte devi anche chiamare API, interrogare database o gestire un'altra lunga lista di integrazioni. Devi decidere come dividere i documenti, quali metadati conservare e come gestire le versioni. Poi devi indicizzare, cercare, filtrare, riordinare, comporre il contesto, generare la risposta, citare le fonti e registrare cosa è successo quando qualcosa va storto.

Lo sviluppo è una parte rilevante del progetto: integrazioni con archivi esistenti, gestione degli errori di parsing, tecniche dedicate per individuare ed estrarre metadati e dati strutturati, aggiornamento degli indici quando cambiano i documenti, valutazione delle risposte e strumenti per capire perché il sistema ha risposto in un certo modo.

Ci sono poi le parti che nella demo restano fuori: aggiornamenti incrementali, costi di ingestione e per domanda, tracciamento delle fonti, monitoraggio e regressioni quando cambi modello o strategia di chunking.

Questo non significa che il RAG vada evitato. Significa solo che va trattato come un sistema di produzione, non come una scorciatoia per far parlare un modello con i documenti.

## Come decidere

La domanda iniziale è se quello che cerchiamo deve poter essere ricondotto a una fonte aggiornata. Se la risposta è no, un RAG probabilmente non è il punto di partenza giusto. Se la risposta è sì, la domanda successiva è se la base documentale è abbastanza ordinata per costruirci sopra un RAG. Se non lo è, il primo lavoro è sui documenti: una base disordinata dà risposte sbagliate senza modo di accorgersene.

La scelta migliore è quasi sempre definire prima il sistema: cosa deve conoscere, come deve comportarsi, quali dati riceve, come si integra con il processo e con la gestione documentale.
Solo quando il perimetro è chiaro, ha senso iniziare a lavorare su un RAG.

Nel prossimo articolo entro nei dettagli tecnici: parsing, chunking, metadati, ricerca ibrida, reranking e generazione.
