# HOMEWORK 2 
## Analyzer scelti
### Contenuto
```java
Analyzer analyzer1 = CustomAnalyzer.builder()
				.withTokenizer(WhitespaceTokenizerFactory.class)
				.addTokenFilter(HyphenatedWordsFilterFactory.class)
				.addTokenFilter(WordDelimiterGraphFilterFactory.class)
				.addTokenFilter(LowerCaseFilterFactory.class)
				.addTokenFilter(SuggestStopFilterFactory.class )
				.build();
```
1- WhitespaceTokenizer: divide e scarta solo i caratteri di spazio bianco. quindi crea dei token ogni volta che ci sono degli spazi vuoti fra le parole. Include nei token anche la punteggiatura.
*In*: "To be, or what?"
*Out*: "To", "be,", "or", "what?"
2- HyphenatedWordsFilter: Se due parole sono separate da un "-" allora il token restituito e' la parola formata dalla concatenazione delle due parole scartando il "-".
*In*: "hyphen- ated"
*Out*:"hyphenated"
3- WordDelimiterGraphFilter: Questo filtro divide i token in corrispondenza dei delimitatori di parola.
Le regole per determinare i delimitatori sono determinate come segue:
"CamelCase" --> "Camel", "Case". 
"Gonzo5000"  --> "Gonzo", "5000". 
"hot-spot" -->"hot", "spot".
"O'Reilly's" --> "O", "Reilly"
"--hot-spot--" --> "hot", "spot"
4- LowerCaseFilter: rende caratteri maiuscoli e minuscoli equivalenti
*In*: "Down With CamelCase"
*Out*: "down", "with", "camelcase"
5-SuggestStopFilter: si tratta di uno stop filter che tokenizza le stopwords non seguite da un separatore
*In*: "The The"
*Out*: "the"(2)

### Titolo
```java
Analyzer analyzer2 = CustomAnalyzer.builder()        
                .addCharFilter(PatternReplaceCharFilterFactory.class, map)
                .withTokenizer(WhitespaceTokenizerFactory.class)
                .addTokenFilter(WordDelimiterGraphFilterFactory.class)
                .addTokenFilter(LowerCaseFilterFactory.class)
                .build();
```              
1- PatternReplaceCharFilter: utilizza le espressioni regolari per sostituire determinati pattern di caratteri. in questo caso e' stato adottato per sostituire nel nome del file il ".txt" con la stringa vuota.

## File indicizzati
Per i test degli indici sono stati utilizzati tre file di testo contenenti informazioni su personaggi politici. 
i tempi di indicizzazione dei tre documenti essendo di lunghezza simile sono circa di 0.43 secondi.

## Query utilizzate 
### Titolo
Churchill -->doc0 (WordDelimiter ha rimosso la "," dal token)
president--> doc0 e doc1 (pattern replace ha consentito di trovare i documenti cercando l'ultima parola del titolo)
usa president--> doc0 doc1 (Lowercase ha reso equivalenti USA ed usa)
### Contenuto
previously --> doc1
an--> non ha restituito alcun documento poiche' si tratta di una stopword
africanamerican--> doc1 (Hyphenated ha tokenizzato "African- American" come africanamerican)
states--> doc0 doc1 doc2 (nel doc2 wordDelimiter ha individuato i token "states" e "man" a partire da "statesMan" )
1874-->doc2 (wordDelimiter ha creato due token a partire dall'anno di nascita e di morte scritti nel testo separati dal "-")
lawyer--> doc0
