# WS JavaScript 2 - Úkol 6

## Výběr roku a měsíce v kalendáři

## Co si procvičíme

- Tvorbu vlastních HTMLElementů
- Odchytávání událostí
    - [EventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventListener)
    - metoda [EventTarget.addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- Konverze čísla v řetězci na číslo
    - [parseInt()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) 
- Práce s objektem `Date`

## Zadání

- Nad čtverečky kalendáře umístit 2 `<select>`y pro rok a měsíc
    - Selecty i jejich options jsou vytvořeny programově pomocí třídy rozšiřující HTMLElement
    - Select roku by měl zobrazovat aktuální rok a 80 let zpět do minulosti a 20 let do budoucna.
    - Select měsíce by měl zobrazovat jméno měsíce
    - Vždy po zobrazení předvolen aktuální rok a měsíc
- Při změně roku anebo měsíce se musí změni čtverečky vypisující data na zvolený rok a měsíc
    - hodnota v proměnné `currentDate` není konstantní
- Pokud je zvolen aktuální rok a měsíc, bude čtvereček aktuálního dne zvýrazněn*

\* _CSS lze zvolit libovolné, musí jít ale vidět, že je čtvereček nějak zvýrazněn_

## Tipy

### Získání aktuální vybrané hodnoty v selectu

Při naslouchání na eventy je v prvním parametru listeneru dostupná proměnná (`1`), která v sobě nese informace o právě vykonávaném eventu (proměnná je typu object). V tomto objektu můžeme najít vlastnost `target` a na ní následně `value`. V tomto `event.target.value` (`2`) bude vždy hodnota právě zvoleného/kliknutého option.

```javascript
document.querySelector('select').addEventListener('change', (event) /*1*/ => {
  console.log(event.target.value /*2*/);
});
```

### Typ aktuálně vybrané hodnoty

Když vytváříme nový element `option`, můžeme mu nastavit vlastnost `.value`. Při nastavování hodnoty lze do vlastnosti přiřadit jakákoliv hodnota (`3`) – řetězec, číslo, boolean atd. V HTML se však tento datový typ zachovat nedá a hodnota je vždy překonvertována na řetězec tzn. z čísla se stává číslo v řetězci – 3 => "3", u booleanu – true => "true".

```javascript
const option = document.createElement('option');
option.value = "ahoj"; /*3*/
console.log(option.value) // "ahoj"
option.value = 1; /*3*/
console.log(option.value) // "1"
option.value = 1.1; /*3*/
console.log(option.value) // "1.1"
option.value = true; /*3*/
console.log(option.value) // "true"
```

Při naslouchání na změny v selectech tedy věnujte pozornost tomu, že `event.target.value` (získávám řetězec) a hodnota rok/měsíc, kterou posíláme do `new Date()` (vyžaduji číslo) nejsou kompatibilní. 

### Práce s měsíci

V předchozí hodině jsme si říkali, jak JavaScript pracuje s daty a jejich měsíci. Měsíce jsou interně uloženy v poli a tedy indexovány od 0. 1. ledna 2021 je tedy `new Date(year: 2021, month: 0, day: 1)`
