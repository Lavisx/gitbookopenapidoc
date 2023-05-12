# TestAPI
<!-- Yay, no errors, warnings, or alerts! -->

<h2 id="legenda">Legenda</h2>




* text podbarvený červeně je nedořešený, na pochybách nebo již neplatný. Pokud se chcete o něčem takovém dozvědět více, musíte kontaktovat mě 
    * [martin.kostal@netforms.cz](mailto:martin.kostal@netforms.cz)
* metody podbarvené žlutě ještě nejsou hotové
* metody podbarvené fialově se právě vyvíjejí
* metody podbarveny zeleně jsou dostupné na testovací verzi 
    * [ https://backoffice.testing.4net.tv](https://backoffice.testing.4net.tv)
* metody nepodbarvené jsou hotové a uvolněné na produkční verzi 
    * [https://backoffice.4network.tv/](http://backoffice.4network.tv/)
* nesouvisející - bezvýznamné značení pro účely samotného vývojáře

<h2 id="obecně-k-celému-api">Obecně k celému API</h2>




* API je kompletně v angličtině
* všechny metody jsou volané jako POST
* vstupem metod je validní JSON
* volání metody by mělo obsahovat hlavičku ‘Content-Type: application/json'
* všechny metody vrací atributy 
    * **success**: boolean** **
        * true / false
    * **message**: string 
        * chybová hláška
        * v případě, že se jedná o validační chyby vkládaných entit, pak atribut message obsahuje json s klíčem “validation_errors”, v němž je uloženo pole validačních chyb. V opačném případě obsahuje textovou hlášku o chybě.
        * atribut message chybí, pokud je success=true



<h2 id="http-stavové-chybové-kódy">HTTP Stavové/chybové kódy</h2>




* **200 - OK**
    * zpracování volání proběhlo úspěšně
    * výstup z metody obsahuje success = true \

* **400 - BAD REQUEST**
    * zpracování volání bylo neúspěšné
    * možné příčiny:
        * neplatný JSON na vstupu do metody (např. chybějící čárka, špatně escapované uvozovky…)
        * neplatně uvedená MAC Adresa (např. jako číslo)
        * request obsahuje data, která nemají požadovaný datový typ (např. jméno odběratele je zasláno jako číslo) \

* **403 - FORBIDDEN**
    * zpracování volání bylo neúspěšné
    * možné příčiny
        * neplatné přihlašovací údaje (login a password nebo přihlašovací token)
        * uživatelské jméno, heslo nebo přihlašovací token patří neaktivnímu Operátorovi, Odběrateli, nebo byl Uživatel Operátora zneaktivněn
        * byl zaslán neplatný autologin token, pro Odběratelovi metody
        * je přihlášen Uživatel Operátora, který nemá oprávnění ke spouštění volané metody \

* **404 - NOT FOUND**
    * zpracování volání bylo neúspěšné
    * možné příčiny:
        * Požadovaná entita k editaci dle zadaného ID nebyla nalezena.
        * Požadovaná entita ke smazání dle zadaného ID nebyla nalezena \

* **408 - REQUEST TIMEOUT**
    * zpracování volání bylo neúspěšné
    * možné příčiny:
        * vzdálený server neodpověděl v požadovaném čase (např. vzdálený server pro samoobsluhu) \

* **409 - CONFLICT**
    * zpracování volání bylo neúspěšné
    * možné příčiny:
        * data zaslané entity nejsou validní (obsahuje neexistující navazující entity - např. neexistující subentita kanál dle id_channel, parametry nejsou ve správném tvaru - např. nesprávná délka)
        * data zaslané entity jsou duplicitní (např. duplicitní kód, login, mac adresa)
        * snaha o volání metody, která je dostupná, ale nastavení BackOffice neumožňuje toto volání (např. požadavek na Samoobsluhu ve chvíli, kdy ji má Operátor zakázanou, nebo špatně nakonfigurovanou) \

* **500 - INTERNAL SERVER ERROR**
    * zpracování volání skončilo neočekávanou/neznámou chybou
    * pokud se tato chyba opakuje, kontaktujte prosím technickou podporu
    * možné příčiny:
        * zpracování metody se dostalo do neočekávaného stavu
        * nastala jiná serverová chyba