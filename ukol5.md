# WS JavaScript 2 - Úkol 5

## Přidáme si hodiny

V rámci dnešního úkolu si do naší aplikace přidáme digitální hodiny, které budou zobrazovat aktuální čas.

Tyto hodiny ovšem nebudou v aplikaci zobrazené neustále, zobrazí se vždy jen na chvíli (ve formě "dialogového okna"), když uživatel při otevřené aplikaci zadá
na klávesnici tajné heslo, jako tomu bývá například u tzv. "cheatů" u některých počítačových her.

Jelikož se blížíme ke konci kurzu, není součástí zadání specifikace konkrétního způsobu implementace - možných řešení jsou spousty a je jen na Tobě, jakým
směrem se vydáš. Cílem je, aby sis ověřila, že na základě znalostí získaných v tomto a předchozích kurzech dokážeš samostatně na základě zadání funkcionality
navrhnout i vhodné řešení, což je ve skutečnosti činnost, kterou většina reálných programátorů tráví nejvíce času (analýza, návrh, algoritmizace) - napsat
samotný kód už zpravidla není nic složitého.

**Ale neboj, koučové jsou plně k dispozici a jsou připraveni Tě "popostrčit", kdykoliv se na něčem zasekneš.**

## Co si procvičíme

- analýza, návrh způsobu implementace
- odchytávání událostí
    - [EventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventListener)
    - metoda [EventTarget.addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- zobrazení "dialogového okna" (resp. jeho emulace)
- práce s řetězci nebo poli znaků
- [setTimeout(), setInterval()](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Timeouts_and_intervals)
- práce s objektem `Date`

## Zadání

- Pokud uživatel, který má aktuálně otevřenou naši aplikaci, zadá na klávesnici sekvenci znaků `t` `i` `m` `e`, dojde k zobrazení "dialogového okna" s aktuálním
  časem (který se bude minimálně jednou za sekundu aktualizovat, tzn. bude vypadat jako digitální hodiny).
    - Po pěti sekundách od zobrazení tohoto okna okno opět zmizí a dokud uživatel nezadá heslo znovu, nic zvláštního se neděje, aplikace se chová zcela
      standardně.
    - Nebudeme používat nativní dialogové okno prohlížeče (`alert`), ale komponentu vytvořenou pomocí HTML a CSS.

**CSS kódy můžeme použít ty, které jme použili na předchozích hodinách, na vzhled se u úkolu nebere ohled, v tomto kurzu se soustředíme především na
funkcionalitu a ovlivnění vzhledu pomocí JavaScriptu.**

## Implementační tipy

### Odchytávání kláves stisknutých na klávesnici

V minulých hodinách jsme několikrát používali na objektech představujících reference na HTML elementy metodu `addEventListener`. Jde o metodu, která nám zajistí
vykonání funkce, kterou jí předáme jako druhý argument, v momentě kdy dojde na objektu k události, jejíž název jí předáme jako první argument.

Následující kód mi zajistí, že pokaždé, když v rámci aktuálně vykresleného dokumentu dojde k události `keydown`
(tzn. dojde k "zamáčknutí" některé klávesy na klávesnici), dojde k vypsání znaku přiřazeného k dané klávese. Obdobně bych mohl odchytávat třeba událost `keyup`,
ke které dochází až v momentě, kdy je klávesa "puštěna"/"odzmáčknuta".

```javascript
document.addEventListener('keydown', (keyboardEvent) => {
  console.log(`Byla stisknuta klávesa '${keyboardEvent.key}'.`)
});
```

### Zobrazení aktuálního času

K získání aktuálního času využijeme vytvoření nové instance objektu třídy `Date`, stejně, jako jsme to dělali na předchozích hodinách.

Pokud máme v prohlížeči nastavenou naši aktuální lokalitu na Česko, následující kód bude mít výsledek např. `11:22:33`:

```javascript
console.log(
    (new Date()).toLocaleTimeString()
);
```

Do prohlížeče se nám ovšem vykreslí čas, který byl aktuální v momentě zavolání konstruktoru třídy `Date`, následně už se nám nebude čas v prohlížeči
aktualizovat a překreslovat.

Jak to vyřešit? Existuje více možností, ale pravděpodobně u všech budeme potřebovat použít některou z vestavěných funkci `setTimeout()` a `setInterval()`.

#### `setTimeout()`

`setTimeout()` nám zajistí, že se po uplynutí námi nastaveného (druhý argument) času (v milisekundách)
vykoná námi předaná funkce, tzn. následující kód mě pět vteřin po jeho vykonání pozdraví formou modálního prohlížečového dialogového okna.

```javascript
setTimeout(
    () => {
      alert('AHOJ!!!');
    },
    5000,
);
```

#### `setInterval()`

`setInterval()` se liší tím, že nevykoná předanou funkci jen jednou, po uplynutí zadaného času, ale bude vykonávat funkci opakovaně (předaný čas v milisekundách
použije jako periodu).

```javascript
setInterval(
    () => {
      alert('AHOJ!!!');
    },
    5000,
);
```

### Vraťme se k hodinám

Jedním z řešení, jak udržovat hodiny aktuální, může být vytváření nového objektu třídy `Date` každou sekundu.

Dalším z řešení může být uložení aktuálního času do proměnné a následná inkrementace o jednu sekundu pomocí opakování s intervalem `1000 ms`.

Výslednou hodnotu (minimálně) jednou za vteřinu použiji pro nastavení obsahu HTML elementu, který si před tím ve stránce připravím.

### Jak detekovat "heslo"

Pro detekci hesla existuje opět spousta způsobů.

Jedním z nich může být průběžné přidávání znaků do proměnné ve formě textového řetězce. Po každém stisku klávesy a přidáním příslušného symbolu do řetězce
ověřím, zda obsahuje (jako podřetězec) naše "heslo".

Pokud neobsahuje, nic se nestane, pokud heslo obsahuje, obsah proměnné "vynuluji"
(nastavím na prázdný řetězec, kvůli tomu, aby následně nedošlo k otevření hodin po stisknutí jakéhokoliv znaku)
a zavolám funkci, která mi zajistí zobrazení dialogového okna (příp. i jeho následné "zavření"/skrytí).

### Jak zobrazovat/skrývat hodiny

Existuje opět několik způsobů, minimálně některé z nich jsme zmínili (nebo i používali) na předchozích hodinách.

Můžu mít element s dialogovým oknem neustále součástí stránky a zobrazovat/skrývat jej pomocí změny CSS atributů.

Můžu element dynamicky do stránky (resp. do jejího DOM) přidávat a následně jej opět odebírat (tzn. když nemá být vidět, vůbec v DOM neexistuje).


