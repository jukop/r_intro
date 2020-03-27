
Ehtorakenteet
=============

Viime kerralla tehdyt funktiot suorittavat aina samat komennot riippumatta syötteestä. Entä jos funktion toiminnassa pitäisi ottaa huomioon erilaisia tapauksia, eli suorittaa tiettyjä komentoja vain joissain tilanteissa? Tätä varten ohjelmointikielissä on ehtorakenteita, eli ns. if/else-rakenteita, jotka ohjaavat ohjelman toimintaa.

Tutustutaan ensin tarkemmin loogisiin operaattoreihin.

Loogiset operaattorit
---------------------

Tässä on lyhyt lista loogisista operaattoreista:

<table class="tg">
<tr>
    <th class="tg-0pky">Operaattori</th>
    <th class="tg-0pky">Kuvaus</th>

</tr>
<tr>
    <td class="tg-0pky">&lt;</td>
    <td class="tg-0pky">pienempi kuin</td>

</tr>
<tr>
    <td class="tg-0pky">&lt;=<br></td>
    <td class="tg-0pky">pienempi tai yhtä suuri kuin<br></td>

</tr>
<tr>
    <td class="tg-0pky">&gt;<br></td>
    <td class="tg-0pky">suurempi kuin</td>

</tr>
<tr>
    <td class="tg-0pky">&gt;=</td>
    <td class="tg-0pky">suurempi tai yhtä suuri kuin</td>

</tr>
<tr>
    <td class="tg-0pky">==</td>
    <td class="tg-0pky">yhtä kuin</td>

</tr>
<tr>
    <td class="tg-0pky">!=</td>
    <td class="tg-0pky">ei yhtä kuin</td>

</tr>
<tr>
    <td class="tg-0pky">!a</td>
    <td class="tg-0pky">ei a (negaation)</td>

</tr>
<tr>
    <td class="tg-0pky">a | b<br></td>
    <td class="tg-0pky">a TAI b alkioittain</td>

</tr>
<tr>
    <td class="tg-0pky">a || b<br></td>
    <td class="tg-0pky">a TAI b yksittäisille arvoille</td>

</tr>
<tr>
    <td class="tg-0pky">a &amp; b</td>
    <td class="tg-0pky">a JA b alkioittain</td>

</tr>
<tr>
    <td class="tg-0pky">a &amp;&amp; b</td>
    <td class="tg-0pky">a JA b yksittäisille arvoille</td>

</tr>
<tr>
    <td class="tg-0lax">a %in% b</td>
    <td class="tg-0lax">a kuuluu b:hen</td>

</tr>
</table>
Kaikki loogiset operaattorit palauttavat joko arvon TRUE tai FALSE. Vertailuoperaattorien käyttö on jo tullut tutuksi aikaisemmissa tehtävissä, mutta tutustutaan vähän tarkemmin viimeisten rivien operaattoreihin:

#### Negaatio

Looginen negaatio palauttaa loogisen lauseen vastakohdan, eli muuttaa arvon TRUE arvoksi FALSE ja arvon FALSE arvoksi TRUE.

``` r
10 > 12
```

    ## [1] FALSE

``` r
!(10 > 12)
```

    ## [1] TRUE

``` r
# Also works without parentheses
!10 > 12
```

    ## [1] TRUE

``` r
!is.na(NA)
```

    ## [1] FALSE

#### Looginen TAI (disjunktio)

Loogiselle TAI operaattorille annetaan kaksi loogista lausetta, ja TAI operaattori palauttaa TRUE, jos kummatkin tai jompikumpi lauseista on TRUE. R:ssä TAI merkitään pystyviivalla "|" tai kahdella pystyviivalla "||". "|" käy läpi vektoreita alkioittain, "||" operoi yksittäisiä arvoja, ja toista lausetta ei edes ajeta, jos ensimmäinen on TRUE (koska "||" palauttaa TRUE riippumatta toisen lauseen arvosta). Jos tämä tuntui monimutkaiselta, niin riittää muistaa, että ehtorakenteissa kannattaa käyttää muotoa "||".

``` r
10 > 12 || "a" < "b"
```

    ## [1] TRUE

``` r
2 > 1 || 4 > 2
```

    ## [1] TRUE

``` r
"a" > "c" || 1 > 10
```

    ## [1] FALSE

#### Looginen JA (konjunktio)

Loogiselle JA operaattorille annetaan kaksi lausetta. JA palauttaa TRUE, jos kummatkin lauseet ovat TRUE. R:ssä JA-operaattorit ovat "&" ja "&&", jotka käyttäytyvät kuten "|" ja "||".

``` r
10 > 12 && "a" < "b"
```

    ## [1] FALSE

``` r
2 > 1 && 4 > 2
```

    ## [1] TRUE

``` r
"a" > "c" && 1 > 10
```

    ## [1] FALSE

#### Osa kokonaisuutta

%in%-operaattorilla voi tarkistaa, kuuluuko jokin arvo suurempaan joukkoon. Tämä voitaisiin toteuttaa myös usealla TAI-operaattorilla, mutta %in% on usein paljon kätevämpi.

``` r
dna_bases <- c("A", "C", "G", "T")
rna_bases <- c("A", "C", "G", "U")

"T" %in% dna_bases
```

    ## [1] TRUE

``` r
"T" %in% rna_bases
```

    ## [1] FALSE

``` r
# With negation
!"A" %in% dna_bases
```

    ## [1] FALSE

#### Monimutkaisemmat lauseet

Operaattoreita voidaan myös yhdistellä monimutkaisemmiksi lauseiksi. Tällöin lauseiden arviointijärjestys määritetään tarvittaessa suluilla.

``` r
dog <- list(breed = "golden retriever",
            height = 45,
            weight = 27)

dog$breed == "golden retriever" && dog$weight < 25 || dog$height < 50
```

    ## [1] TRUE

Ehtorakenteet
-------------

Aloitetaan esimerkistä: tehtävänä on kirjoittaa funktio, jolle annetaan syötteenä potilaan hemoglobiiniarvo. Funktion on tarkoitus hälyttää, jos hemoglobiini laskee alle viitearvojen alarajan 117. Kyseinen funktio voisi näyttää vaikka tältä:

``` r
hb_alert <- function(hb) {
  if (hb < 117) {
    return("Hemoglobin is low!")
  }
}
```

Funktiolla on siis yksi argumentti, hb eli hemoglobiiniarvo. Funktion sisällä on if-rakenne. If-rakenteessa on kaksi osaa: ehto, ja rakenteen sisäinen koodi. Rakenteen sisäinen koodi ajetaan vain, jos ehto täyttyy. Ehto merkitään if-komennon jälkeen sulkeisiin, ja rakenteen sisäinen koodi kirjoitetaan sulkeiden jälkeen hakasulkeiden sisään. (Jos hakasulkeiden sisään tulisi vain yksi rivi koodia, hakasulkeet voi jättää pois, mutta näissä esimerkeissä käytetään aina hakasulkeita).

Kokeillaan, miten funktio toimii eri hemoglobiiniarvoilla:

``` r
# Nothing happens
hb_alert(130)
# returns alert
hb_alert(110)
```

    ## [1] "Hemoglobin is low!"

Funktio siis toimii oletetusti, eli se hälyttää vain, jos hemoglobiinitaso on alle 117. Käyttäjän kannalta olisi kuitenkin kätevää saada jonkinlainen palaute myös silloin, kun hemoglobiinitaso on tarpeeksi korkea. Tätä varten voidaan käyttää else-komentoa:

``` r
hb_alert <- function(hb) {
  if (hb < 117) {
    return("Hemoglobin is low!")
  } else {
    return("Hemoglobin OK")
  }
}

hb_alert(130)
```

    ## [1] "Hemoglobin OK"

Else-komennon jälkeinen koodi siis ajetaan, jos ehto hb &lt; 117 ei täyty.

Tällä hetkellä funktiomme toimii vain naispotilaille, sillä miehillä hemoglobiiniarvojen alaraja on 134. Lisätään siis funktioomme argumentti sukupuolta varten ja muokataan sen toimintaa niin, että se osaa ottaa huomioon sukupuolen. Nyt if-rakenteen ehdosta tulee jo hieman monimutkaisempi:

``` r
hb_alert <- function(hb, sex) {
  if (sex == "female" && hb < 117 || sex == "male" && hb < 134) {
    return("Hemoglobin is low!")
  } else {
    return("Hemoglobin OK")
  }
}

hb_alert(hb = 120, sex = "female")
```

    ## [1] "Hemoglobin OK"

``` r
hb_alert(hb = 120, sex = "male")
```

    ## [1] "Hemoglobin is low!"

Entä jos haluaisimme tulostaa eri varoituksen mies- ja naispotilaille? Tähän tarvitaan "else if"-rakennetta:

``` r
hb_alert <- function(hb, sex) {
  if (sex == "female" && hb < 117) {
    return("Hemoglobin is low for a female!")
  } else if (sex == "male" && hb < 134) {
    return("Hemoglobin is low for a male!")
  } else {
    return("Hemoglobin OK")
  }
}

hb_alert(hb = 110, sex = "female")
```

    ## [1] "Hemoglobin is low for a female!"

``` r
hb_alert(hb = 120, sex = "male")
```

    ## [1] "Hemoglobin is low for a male!"

Nyt funktio tarkistaa ensin, onko potilas nainen ja onko hänen hemoglobiininsa alle 117. Jos ei, siirrytään eteenpäin ja tarkistetaan, onko potilas mies ja onko hänen hemoglobiininsa alle 130. Jos ei, siirrytään viimeiseen kohtaan, ja tulostetaan "Hemoglobin ok".

Else-if rakenteita voi olla rajoittamaton määrä ensimmäisen if-rakenteen jälkeen. Lisätään funktioon hälytys kriittisestä hemoglobiinin määrästä (hb &lt; 50) riippumatta sukupuolesta:

``` r
hb_alert <- function(hb, sex) {
  if (sex == "female" && hb < 117) {
    return("Hemoglobin is low for a female!")
  } else if (sex == "male" && hb < 134) {
    return("Hemoglobin is low for a male!")
  } else if (hb < 50) {
    return("Hemoglobin is critical")
  } else {
    return("Hemoglobin OK")
  }
}

hb_alert(hb = 32, sex = "female")
```

    ## [1] "Hemoglobin is low for a female!"

Kuten huomataan, yllä oleva koodi ei toimikaan, kuten piti. Näin alhaisella hemoglobiinilla pitäisi tulla varoitus kriittisestä tilasta. Koodi suoritus ei kuitenkaan ikinä etene kriittisen tilan varoitukseen asti, sillä ensimmäinen ehto täyttyy. Korjataan tilanne siirtämällä kriittisen tilan ehto ensimmäiseksi:

``` r
hb_alert <- function(hb, sex) {
  if (hb < 50) {
    return("Hemoglobin is critical")
  } else if (sex == "male" && hb < 134) {
    return("Hemoglobin is low for a male!")
  } else if (sex == "female" && hb < 117) {
    return("Hemoglobin is low for a female!")
  } else {
    return("Hemoglobin OK")
  }
}

hb_alert(hb = 32, sex = "female")
```

    ## [1] "Hemoglobin is critical"

``` r
hb_alert(hb = 120, sex = "female")
```

    ## [1] "Hemoglobin OK"

``` r
hb_alert(hb = 120, sex = "male")
```

    ## [1] "Hemoglobin is low for a male!"

Nyt funktio toimii haluamallamme tavalla!

Funktioissa voi myös olla useampi ehtorakenne. Ehtorakenteita käytetään usein tarkistamaan argumenttien arvoja. Lisätään ehtorakenteet argumenttien tarkistamiseksi:

``` r
hb_alert <- function(hb, sex) {
  # Hemoglobin should be numeric and positive
  if (!is.numeric(hb) || hb < 0) {
    return("Hemoglobin should be numeric and positive")
  }
  if (!sex %in% c("female", "male")) {
    return("This function can only deal with binary sex: female or male")
  }
  
  if (hb < 50) {
    return("Hemoglobin is critical")
  } else if (sex == "male" && hb < 134) {
    return("Hemoglobin is low for a male!")
  } else if (sex == "female" && hb < 117) {
    return("Hemoglobin is low for a female!")
  } else {
    return("Hemoglobin OK")
  }
}

hb_alert(hb = "120", sex = "female")
```

    ## [1] "Hemoglobin should be numeric and positive"

``` r
hb_alert(hb = 120, sex = "FEMALE")
```

    ## [1] "This function can only deal with binary sex: female or male"

Alkioiden poimiminen vektorista tietyh ehdon perusteella
--------------------------------------------------------

Tämän viikon tehtävissä tuli ainakin allekirjoittaneelle vastaan tilanne, jossa piti käydä läpi useita arvoja vektorista, ja säilyttää niistä ne, jotka täyttivät tietyn ehdon. Tätä aihetta käsitellään enemmän ensi viikolla, mutta tässä lyhyt vinkki tällaisten tehtävien ratkaisuun:

-   Luo apufunktio, joka ottaa syötteeksi yhden arvon, ja tarkistaa täyttyykö ehto. Tämän funktion tulee palauttaa TRUE, jos ehto täyttyy ja FALSE, jos ehto ei täyty.
-   Käytä funktiota `Vectorize`, jolla voit muuttaa funktiosi vektoroiduksi funktioksi. Kun vektoroidulle funktiolle annetaan vektori, jossa on monta alkiota, funktio ajetaan automaattisesti alkio kerrallaan, kuten monet R:n omat funktiot.
-   Käytä vektoroitua apufunktiota vektorin indeksointiin.

Tässä on esimerkki, jossa käydään läpi vektori DNA:n emäksiä, joista poimitaan vain sytosiinit ja guaniinit.

``` r
# Helper function
is_cg <- function(base) {
  if (base %in% c("C", "G")) {
    return(TRUE)
  } else {
    return(FALSE)
  }
}

# Vectorize
is_cg_vector <- Vectorize(is_cg)

# Main function
pick_cg <- function(bases) {
  only_cg <- bases[is_cg_vector(bases)]
  return(only_cg)
}

# NOTE: this only checks the first value of the vector
my_bases <- c("A", "C", "C", "T", "G", "T")
is_cg(my_bases)
```

    ## Warning in if (base %in% c("C", "G")) {: the condition has length > 1 and
    ## only the first element will be used

    ## [1] FALSE

``` r
# This works as expected
is_cg_vector(my_bases)
```

    ##     A     C     C     T     G     T 
    ## FALSE  TRUE  TRUE FALSE  TRUE FALSE

``` r
# Pick only C and G
pick_cg(my_bases)
```

    ## [1] "C" "C" "G"
