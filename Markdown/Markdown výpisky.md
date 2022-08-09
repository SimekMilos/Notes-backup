# Markdown
<img src="Markdown-logo.png" alt="Markdown Logo" width="600">


## Nástroje
- [dillinger.io](http://dillinger.io)
- [stackedit.io](http://stackedit.io)


## Základní Markdown
- **Odřádkování**:
    - *soft break* - 2 mezery na konci řádku
    - *hard break* - odděluje odstavce, dvojité odřádkování
    (prázdný řádek mezi odstavci)
- **HTML** - lze přímo vepisovat html kód (a tím pádem i JavaScript snippets)

### Formátování textu
- **Důraz** - Markdown definuje zdůraznění textu,
  které se běžně převádí na *italic*, *bold* a *italic-bold*.
  Toto chování lze ve stylech změnit a používat příkladně <u>underline</u>.

  ``` Markdown
  *italic*            _italic_
  **bold**            __bold__
  ***bold-italic***   ___bold-italic___
  
  ~~Přeškrtnuto (nestandardní)~~
  ==Zvýrazněno (nestandardní)==
  ```

- **Použití `*` a `_` jako normální znak** - je potřeba dát jak před tak za znak mezeru

### Seznamy
- **Neuspořádaný** - pomocí znaků `*`, `+` nebo `-`
- **Uspořádaný** - pomocí čísla a tečky  
  
  - Výsledek bude číslován vždy od jedničky  
  - Pokud spustíme seznam nechtěně (př. `2000. Začalo nové tisíciletí`),
    tak lze použít escape sekvenci: `2000\. Začalo nové tisíciletí`
- **Podseznam** - vytváří se odsazením o alespoň 1 mezeru (často 2 či 4)

  ``` Markdown
  * první bod      1. jedna
  + druhý bod      2. dva
  - třetí bod      3. tři
    * podseznam
  ```

- **Text se stejným odsazením** - pod bodem seznamu může být text se stejným
  odsazením. Musí být na samostatném řádku pod seznamem odsazený alespoň
  1 mezerou:

  ``` Markdown
  1. bod seznamu
     Text se stejným odsazením.
  ```

### Odkazy a obrázky
- **Odkaz**:

  ``` Markdown
  [text odkazu](http://www.example.com "Nepovinný titulek")
  [odkaz referencí][identifikátor]
  [lze použít i samotný text odkazu]
  
  [identifikátor]: http://www.example.com "Nepovinný titulek"
  [lze použít i samotný text odkazu]: http://www.example.com
  ```

  - <u>*Adresa*</u> - pokud jde o zdroj z webu, tak musí být včetně `http://`  
    Pokud jde o lokální zdroj, tak adresa může být relativní.  
    Adresa u verze s referencí lze uzavřít do `''`, `""` nebo `()`
    
  - <u>*Emailová adresa*</u> - bude maskována proti spambotům (není na 100%)

  - <u>*Identifikátor*</u> - **není** case sensitive

  - <u>*Automatický odkaz*</u> - uzavře se do `< >`:

    ``` Markdown
    <http://www.example.com>
    <email@email.com>
    ```

- **Obrázek** - funguje stejně jako odkaz, fungují pro něj stejná pravidla

  ``` Markdown
  ![alt text](http://www.sample.com/image.jpg "Nepovinný titulek")
  ![alt text][1]
  
  [1]: /path/to/img.jpg "Nepovinný titulek"
  ```

  - *Alt text* - text, který se zobrazí, pokud obrázek nelze načíst

### Nadpisy
- **Styl s podtržítky** - podtržítek může být jakýkoli počet (minimálně 1):

  ``` Markdown
  Nadpis 1
  ========

  Nadpis 2
  --------
  ```

- **Styl s hashtagy** - 6 úrovní:

  ``` Markdown
  # Nadpis 1
  ## Nadpis 2
  ### Nadpis 3
  ```

### Kód
- **Inline kód** - pokud je v kódu znak `` ` ``, tak lze použít více zpětných
  lomítek pro označení začátku a konce kódu

  ``` Markdown
  `var value = true`
  ``Zde může být znak ` ``
  ```

- **Blok kódu** - odsazuje se 4 mezerami (pokud má být blok součástí seznamu,
  tak musí být odsazen 8 mezerami)

  ``` Markdown
      if (value) {
          return true
      }
  ```

- **Code fencing** (nestandardní) - blok kódu bez potřeby odsazení  
  Umožňuje zvýraznění syntaxe dospecifikováním jazyka.

  ```` Markdown
  ``` JavaScript
  if (value) {
      return true
  }
  ```
  ````

### Ostatní
- **Horizontální čára** - pomocí 3 či více: `- - -`, `* * *` nebo `_ _ _`  
  Počet mezer mezi znaky nemá vliv (nemusí být žádná).

- **Citát**:
  ``` Markdown
  > Toto je citát ...
  >
  > o více řádcích s mezerou uvnitř.
  >> Je možné použít i zanořené citáty.
  ```

- **Escape sekvence** - pokud potřebujeme použít speciální znak,
  jako by byl normální

  ``` Markdown
  \*Tento text nebude italic\*
  ```

- **Poznámky pod čarou** (nestandardní):
  ``` Markdown
  Tento text má 2[^1] poznámky[^2] pod čarou.
  
  [^1]: Označením 2 je myšleno číslo 2.
  [^2]: Poznámky je množné číslo od slova poznámka.
  ```

- **Delší pomlčky** (nestandardní) - krátká pomlčka `-`, delší `--`
  a nejdelší `---`

- **Více odřádkování** - více odřádkování v Markdownu neznamená více
  odřádkování ve výsledném dokumentu. Na to lze použít HTML tag nedělitelné
  mezery `&nbsp;` následovaný **dvěma normálními mezerami**.

  ``` Markdown
  První odstavec.
  
  &nbsp;  
  &nbsp;  
  &nbsp;  
  
  Mezi tyto dva odstavce jsou vloženy 3 odřádkování navíc.
  ```

- **Komentáře** - Markdown přímo komentáře nemá, lze využít komentáře z HTML

  ``` Markdown
  <!--
  Toto je komentář.
  -->
  ```


## GitHub Flavored Markdown

### Seznam úkolů

- GitHub umí z TODO vytvořit progress bar. Lze také použít v pull requestech.

  ``` Markdown
  - [x] Toto je zaškrtnutý úkol
  - [ ] Toto je nezaškrtnutý úkol
  ```

### Tabulky
- Sloupce jsou oddělovány znakem `|` a hlavička znakem `-`

  ``` Markdown
  První sloupec hlavičky | druhý sloupec | třetí
  -----------------------|---------------|------
  První sloupec první řádek | Druhý sloupec první řádek
  Raz | Dva | Tři
  ```

### SHA reference
- Jakýkoli hash na komit bude automaticky zkonvertován na link k danému komitu
  na GitHubu

  ``` Markdown
  16c999e8c71134401a78d4d46435517b2271d6ac
  mojombo@16c999e8c71134401a78d4d46435517b2271d6ac
  mojombo/github-flavored-markdown@16c999e8c71134401a78d4d46435517b2271d6ac
  ```

### Issue reference
- Jakékoli číslo, které odkazuje na *Issue* nebo *Pull Request* bude
  automaticky změněno na link

  ``` Markdown
  #1
  mojombo#1
  mojombo/github-flavored-markdown#1
  ```

### Zmínění uživatele
- Napsáním `@uživatelské_jméno` upozorní daného uživatele, aby se podíval na
  daný komentář
- Lze zmínit i týmy

### Ostatní
- **Automatické odkazy** - z URL se automaticky stanou klikatelné odkazy na
  danou stránku
- **Přeškrtnutí** - podpora pro: `~~Přeškrtnuto~~`
- **Code fencing** - podpora code fencingu se zvýrazněním syntaxe
- **Smajlíci**: `:smile:` `:wink:`, seznam všech je [zde][smiley]

[smiley]: https://gist.github.com/rxaviers/7360908 "github.com"


## Jak psát Readme

- [Readme best practices][best practices]

- [How to write a good readme][good readme]

Části, které readme může obsahovat:  

- Představení/krátký popis
- Systémové a hardwarové požadavky
- Instrukce pro konfiguraci
- Instrukce pro instalaci
- Instrukce pro používání
- Známé chyby
- Řešení problémů (troubleshooting)
- Seznam autorů s jejich kontaktními údaji
- Poděkování
- Copyright a informace o licenci
- Seznam změn
   - pro programátory (changelog)
   - pro uživatele (news section)

[Gnits standard][G stand] dělí readme na více souborů:

Jméno souboru | Popis
--------------|------
README | Obecné informace
AUTHORS | Autoři a jejich zásluhy
THANKS | Poděkování
CHANGELOG | Detailní popis změn určený pro programátory
NEWS | Základní popis změn určený pro uživatele
INSTALL | Instrukce pro instalaci
COPYING/LICENSE | Copyright a licenční informace
BUGS | Známé chyby a instrukce, jak ohlašovat nové

Další soubory, které se často objevují, jsou FAQ a TODO.

[best practices]: https://github.com/jehna/readme-best-practices "GitHub"
[good readme]: https://bulldogjob.com/news/449-how-to-write-a-good-readme-for-your-github-project "bulldogjob.com"
[G stand]: https://en.wikipedia.org/wiki/Gnits_standards "Wikipedia"

