# Discorso Iniziale — Presentazione della Tesi

Buongiorno, sono Laurent Ntibarikure e vi presento la mia tesi il cui principale argomento riguarda la riduzione della complessità nell'analisi numerica di array fasati.

## Array Fasati

Gli array fasati sono delle schiere d'antenne il cui scopo, oltre a fornire un maggior guadagno, è di consentire con opportuni sfasamenti nelle eccitazioni la variazione della direzione del lobo di radiazione principale, quindi una scansione del fascio.

In slide vi è un esempio di array planare di dipoli stampati ed il pattern di radiazione relativo ad un'alimentazione equifase.

Andando a variare le fasi di eccitazione dei dipoli allo stesso modo per i pannelli di dipoli che costituiscono l'array si ottiene una scansione azimutale. Variando invece le fasi tra i pannelli si ha una scansione nel piano di elevazione. Combinando entrambi gli sfasamenti si ottiene una scansione obliqua nel semispazio individuato dall'array.

Gli array fasati trovano impiego in molteplici applicazioni, sia in ambito radaristico e di telerilevamento che in quello delle telecomunicazioni. Per esempio, i radar di sorveglianza ed inseguimento odierni utilizzano array fasati per individuare bersagli ed inseguirli; avendo meno parti meccanicamente mobili permettono una scansione più rapida. Per aumentare la capacità dei sistemi di telecomunicazione, i nuovi standard prevedono l'impiego di array fasati per ottenere una diversità spaziale.

## Criteri di Analisi

Vediamo adesso i criteri scelti nell'analisi di array fasati. Il primo è quello di poter analizzare strutture radianti arbitrarie, sia nella forma degli elementi radianti e le loro distanze relative che nei materiali impiegati. Il secondo riguarda l'accuratezza e l'affidabilità dei risultati della simulazione.

Si è quindi scelta la tecnica degli elementi finiti come metodo di calcolo dei campi vicini generati dall'array d'antenne. Come vedremo nelle prossime slide, la tecnica degli elementi finiti ci consente di costruire un sistema matriciale, il cui numero di incognite, per il fatto che gli array sono strutture elettricamente grandi, risulta dell'ordine delle centinaia di migliaia a centinaia di milioni.

Inoltre il sistema deve essere risolto per ciascun angolo di scansione, portando a tempi computazionali elevati.

Vogliamo costruire un modello di radiazione che permetta di preservare, a seguito di una riduzione della complessità, le informazioni associate al pattern di radiazione. Questo è possibile, come vedremo, andando a parametrizzare il modello nelle direzioni di scansione e in quelle di osservazione che costituiscono il pattern.

## Tecnica degli Elementi Finiti

I campi vicini si ottengono dalla tecnica degli elementi finiti. La formulazione maggiormente utilizzata si basa sulla soluzione dell'equazione d'onda per il campo elettrico, derivata dalle equazioni di Maxwell nel dominio della frequenza e dalle relazioni costitutive del mezzo, assunto isotropo e tempo-invariante. *k₀* e *ζ₀* sono rispettivamente il numero d'onda e l'impedenza dello spazio libero.

L'equazione d'onda, equazione differenziale del 2° ordine, viene risolta nel dominio Ω con:
- condizioni al contorno di Dirichlet su Γ_E,
- di Neumann su Γ_H,
- condizioni di Robin su Γ_WG per la continuità del modo fondamentale nelle guide d'onda di alimentazione delle antenne,
- e su Γ_R per la radiazione.

Si procede quindi andando a discretizzare il dominio Ω in sotto-domini disgiunti (gli elementi), si introducono delle funzioni di base **w** per approssimare il campo elettrico, e infine con una proiezione di Galerkin dell'equazione d'onda nel campo elettrico approssimato si ottiene un sistema lineare **Ae** = **b** relativo alla *p*-esima porta di alimentazione.

La soluzione di ciascuno dei *P* sistemi lineari così ottenuti restituisce, col principio di sovrapposizione degli effetti, i coefficienti di espansione del campo elettrico generato dall'eccitazione equifase di tutte le porte. La legge di Faraday consente di ricavare il campo magnetico dal campo elettrico precedentemente calcolato.

## Campi Radiati e Pattern di Radiazione

Dobbiamo adesso calcolare i campi radiati dall'array, sfruttando il principio di Huygens: i campi vicini calcolati su Γ_R vengono usati come sorgenti del campo lontano. La relazione integrale restituisce la componente lentamente variante del campo elettrico radiato nella direzione di osservazione (θ, φ) in coordinate sferiche.

L'approssimazione per campi radiati consente di trascurare il calcolo del campo magnetico, essendo questo inversamente proporzionale al campo elettrico tramite l'impedenza dello spazio libero.

Infine il pattern di radiazione si ottiene dalle relazioni in slide. *P_in* è la potenza fornita all'array. Separando le componenti del campo lungo θ̂ e φ̂ è possibile ottenere informazioni sulla polarizzazione del campo radiato.

## Costruzione del Modello di Radiazione

Andiamo adesso a costruire il modello di radiazione: i *P* sistemi lineari ottenuti con la tecnica degli elementi finiti possono essere combinati andando a scegliere il fasore di eccitazione alla *p*-esima porta per un'interferenza costruttiva nella direzione di scansione. Si costruisce quindi una relazione ingresso-stato parametrizzata nella direzione di scansione.

La relazione che lega i campi vicini ai campi radiati nelle polarizzazioni considerate può essere vista come un operatore che agisce sui campi vicini. Per parametrizzare la relazione stato-uscita si sceglie di esprimere in serie di Fourier gli operatori calcolati per piani a φ costante.

Lo spettro degli operatori, essendo limitato in banda base, permette di approssimarli ritenendo i 2Q+1 principali termini della serie di Fourier. Per accoppiare il calcolo dei campi radiati alle soluzioni del sistema d'ingresso, si costruiscono degli operatori F_E e F_H che agendo sul vettore delle soluzioni restituiscono i campi elettrico e magnetico nei punti di campionamento su Γ_R.

In slide vi è lo schema del modello di radiazione, parametrizzato nella direzione di scansione con le funzioni ξ e nella direzione di osservazione su piano a φ costante con le funzioni η.

## Riduzione della Complessità

Procediamo quindi con la riduzione del numero di incognite con una tecnica di proiezione: si costruisce un'opportuna matrice rettangolare **V**, base del sottospazio di scansione nel quale andremo a proiettare l'intero modello di radiazione, per ottenere il modello ridotto schematizzato in slide.

**V** costituisce una base di espansione per le soluzioni del sistema ridotto per approssimare le soluzioni del sistema completo; deve quindi contenere le informazioni sul campo vicino in scansione. Si vanno quindi a raccogliere α vettori delle soluzioni del modello completo, rigorosamente linearmente indipendenti, per ottenere una base a seguito di un processo di ortonormalizzazione. L'errore introdotto nella riduzione della complessità si calcola nella norma L² come errore indotto al pattern rispetto a quello del modello completo.

## Esempio: Array 3×5 a Patch

Vediamo adesso un semplice esempio per illustrare le potenzialità della tecnica presentata. Si è progettato col CAD commerciale HFSS un array planare di 3×5 antenne a patch equispaziate di λ₀/2. Per costruire il modello di radiazione, i campi vicini verranno campionati in 1100 punti su Γ_R.

Si procede lanciando la simulazione in HFSS, impostando le funzioni di base al 1° ordine. Il dominio di analisi viene discretizzato con tetraedri, con dimensione massima degli spigoli di λ/3. Il mesh irregolare illustrato è il risultato di ulteriori riduzioni delle dimensioni dei tetraedri laddove vi sono forti gradienti del campo elettrico.

La memoria richiesta da HFSS è stata di 1.5 GB mentre il tempo computazionale di 38 min. Il pattern nei piani XZ e YZ viene calcolato in 35 s.

Estraendo le informazioni sul mesh e sui materiali di progetto da HFSS, abbiamo costruito il modello di radiazione completo, con circa 350 000 incognite. 21 sono i termini della serie di Fourier ritenuti per la relazione stato-uscita. Per un pattern 3D si sono calcolati gli operatori per 60 piani a φ costante con risoluzione azimutale di 3°.

La memoria richiesta dal modello completo è di circa 625 MB mentre il tempo computazionale complessivo è stato di circa 45 min. Per la parametrizzazione, i pattern nei piani XZ e YZ si ottengono in soli 150 ms.

## Risultati

Come possiamo vedere nel grafico dell'errore all'aumentare della dimensione di **V**, la riduzione del modello per una scansione nel piano XZ richiede solamente 3 vettori, ossia il numero di antenne lungo la direzione x. Si noti che, per la simmetria della struttura, le direzioni in φ = 0° e quelle in φ = 180° sono linearmente dipendenti.

La scansione nel piano YZ richiede soltanto 5 vettori per approssimare le soluzioni.

Per una scansione dell'intero semispazio nord, si sono scelte direzioni individuate da punti equispaziati sulla traiettoria spirale elicoidale illustrata. L'errore si annulla con 15 vettori, mentre con 17 si ha una condizione di costruzione di una base incompleta: si vanno a selezionare direzioni nei soli piani XZ e YZ, e queste non permettono di rappresentare il campo in scansione nell'intero semispazio nord.

Il modello ridotto viene quindi costruito con una base **V** di 15 vettori:
- il numero di incognite passa da ~350 000 a **15**,
- la memoria impegnata da 625 MB a **1.5 MB**,
- l'errore di riduzione rimane praticamente nullo.

Ogni frame di questo filmato richiederebbe con HFSS circa 7 min di computazione, il modello completo circa ½ secondo, mentre il modello ridotto impiega soltanto **63 ms**.

## Conclusioni e Sviluppi Futuri

La tecnica presentata risulta essere promettente nell'analisi di array fasati di grandi dimensioni.

Vi sarebbe inoltre la possibilità di parametrizzare il modello in frequenza e nelle caratteristiche dei materiali, permettendo di calcolare in tempi brevissimi informazioni sulla banda di lavoro e sulle tolleranze di fabbricazione.

Infine, questa tecnica permetterebbe di attuare un processo di ottimizzazione su pattern accurati.
