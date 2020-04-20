
R-paketit
=========

Käytännön R-ohjelmointi nojaa tällä kurssilla esiteltyjen rakenteiden lisäksi hyvin pitkälti erilaisten pakettien käyttöön. Paketit ovat kooodikokoelmia, jotka sisältävät uusia funktioita, luokkia ja dataa eli ne laajentavat R:ää.

Pakettien asennus
-----------------

Useimmat R-paketit ovat CRANissa (Comprehensive R Archive Network). Ne voidaan asentaa komennolla `install.packages()`, tai RStudion asennusikkunan kautta, jota käytimmekin jo aikaisemmin ja joka käytännössä kutsuu `install.packages()`-funktiota . Käyttämällä funkiot asuoraan voi myös asentaa useamman paketin kerralla. Alla oleva komento asentaa dplyr- ja tidyr-paketit:

``` r
install.packages(c("dplyr", "tidyr"))
```

Pakettien käyttö
----------------

Jotta R-paketin sisältämät komennot saa käyttöön, paketti pitää ladata ja liittää R-työtilaan. Tämä tapahtuu komennolla `library()`:

``` r
library(tidyr)
```

Nyt kun tidyr on ladattu, voimme käyttää siinä olevia komentoja, joilla voi hallita data frame-muodossa olevaa dataa erittäin kätevästi. Otetaan esimerkiksi data tiedostosta drug\_data.csv

``` r
drug_data <- read.csv("drug_data.csv")
drug_data
```

    ##   Subject CRP_0 CRP_1 CRP_2 PCT_0 PCT_1 PCT_2
    ## 1       1   100    80    35   5.0   3.3   1.0
    ## 2       2    90    85    41   4.2   3.0   1.2
    ## 3       3   105   100    70   6.0   4.8   2.7
    ## 4       4    92    90    71   3.8   3.0   1.9
    ## 5       5   130   100    50   9.0   7.6   2.4

Tässä datassa on kuviteltu tutkimus, jossa tutkitaan antibiootin tehoa, eli sitä, kuinka hyvin lääke laskee tulehdukesn markkereita: CRP ja Prokalsitoniini (PCT). Kumpikin markkeri on mitattu alkutilanteessa (0) sekä yhden (1) ja kahden (2) päivän hoidon jälkeen. Kuvitellaan nyt, että tämä data haluttaisiin syöttää lineaariseen malliin, jolla ennustettaisiin CRP:n tasoa aikapisteen perusteella. Miten muotoilisit tällaisen mallin R:llä?

Aivan, mallin pitäisi näyttää tältä: `lm(CRP ~ Time, data = drug_data)`. Mutta datassa on tällä hetkellä CRP-arvoja kolmessa eri sarakkeessa, joita R luulee kolmeksi erilliseksi muuttujaksi! Dataa pitää siis muokata niin, että kaikki CRP- ja PCT-arvot ovat omissa sarakkeissaan, ja aikapistetiedot omassa sarakkeessaan. Tätä varten voisimme luoda uuden data framen, ja kehitellä jonkinlaisen silmukan, joka käy läpi alkuperäistä dataa ja poimii siitä arvoja oikeaan muotoon. Tämä on kuitenkin työlästä.

Onneksi asia on helppo korjata tidyr-paketin komennoilla. HUOM: tämän koodin toimintaa ei käsitellä tässä sen enempää, eikä sitä tarvitse ymmärtää. Tarkoitus on demonstroida sitä, kuinka monimutkaiselta tuntuva asia saadaan hoidettua kahdella koodirivillä, kun käytössä on oikea paketti.

``` r
drug_data %>%
  pivot_longer(-Subject, names_to = c("Var", "Time"), names_pattern = "(.*)_(.)", values_to = "Value") %>%
  pivot_wider(names_from = "Var", values_from = "Value")
```

    ## # A tibble: 15 x 4
    ##    Subject Time    CRP   PCT
    ##      <int> <chr> <dbl> <dbl>
    ##  1       1 0       100   5  
    ##  2       1 1        80   3.3
    ##  3       1 2        35   1  
    ##  4       2 0        90   4.2
    ##  5       2 1        85   3  
    ##  6       2 2        41   1.2
    ##  7       3 0       105   6  
    ##  8       3 1       100   4.8
    ##  9       3 2        70   2.7
    ## 10       4 0        92   3.8
    ## 11       4 1        90   3  
    ## 12       4 2        71   1.9
    ## 13       5 0       130   9  
    ## 14       5 1       100   7.6
    ## 15       5 2        50   2.4

Jos koko pakettia ei halua ladata ja liittää, voi paketeista käyttää yksittäisiä komentoja käyttämällä muotoa `paketti::komento()`. Esimerkiksi tällä kurssilla käytetty komento `Rkurssi::Rkurssi()` ajaa paketista Rkurssi samannimisen funktion Rkurssi. Kahden kaksoispisteen notaatiota voi käyttää myös silloin, jos kahdessa paketissa on samanniminen funktio ja haluaa varmistaa, että käyttää varmasti oikeaa funktiota.

Bioconductor
------------

Bioinformaatikko törmää aikaisemmin tai myöhemmin Bioconductoriin. Bioconductorissa on paljon erilaisia työkaluja bionformaatikoille, mm. R-paketteja. Bioconductorin paketit asennetaan eri komennolla kuin muut R-paketit, mutta asennusohjeet on onneksi aina annettu paketin yhteydessä. Esimerkiksi [ggbio-paketin](http://bioconductor.org/packages/release/bioc/html/ggbio.html) asennus tehdään näin:

``` r
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install("ggbio")
```

Ensimmäinen osa koodista tarkistaa, onko BiocManager-paketti asennettu, ja jos ei, asentaa sen. BiocManager-pakettia käytetään Bioconductorin pakettien asennukseen. Toinen osa asentaa BiocManagerin avulla ggbio-paketin.

ggbio-paketilla voi visualisoida genomiikka-dataa. Ladataan ensin ggbio-paketti. Käydään sitten läpi muutama esimerkki ggbio-paketin toiminnallisuudesta (kopioitu paketin esimerkeistä).

``` r
library(ggbio)
```

Ideogrammi toisesta ihmisen kromosomista saadaan yhdellä koodirivillä:

``` r
p_ideo <- Ideogram(genome = "hg19", subchr = "chr2")
p_ideo
```

![](packages_files/figure-markdown_github/chromosome.jpg)

Alla on monimutkaisempi esimerkki, jossa visualisoidaan somaattisia mutaatioita ihmisen genomissa (tarkemmat tiedot löytyvät ggbio-paketin dokumentaatiosta). Tässä esimerkissä koodia on jo aikalailla, mutta tämä esimerkki löytyy suoraan paketin dokumentaatiosta, jolloin samankaltaisten kuvaajien piirtäminen omalla datalla on melko suoraviivaista.

``` r
data("CRC", package  = "biovizBase")
gr.crc1 <- crc.gr[crc.gr$individual == "CRC-1"]

p <- ggbio() +
  circle(gr.crc1, geom = "link", linked.to = "to.gr", aes(color = rearrangements)) +
  circle(mut.gr, geom = "rect", color = "steelblue") +
  circle(hg19sub, geom = "ideo", fill = "gray70") +
  circle(hg19sub, geom = "scale", size = 2) +
  circle(hg19sub, geom = "text", aes(label = seqnames), vjust = 0, size = 3)
p
```

![](packages_files/figure-markdown_github/circles.png)

Pakettien käyttöön tarvittavat taidot
-------------------------------------

Kuten aiemmin mainittiin, R-pakettien käyttö aloitetaan usein lukemalla paketin tekijöiden esimerkkikoodia, ja muokkaamalla sitä omiin tarpeisiin sopivaksi. Tätä varten pitää ymmärtää tällä kurssilla opittuja asioita:

-   Funktion käsite: mikä funktio on ja miten se toimii
-   Argumentit: miten funktiolle annetaan argumentteja? Tämä on erittäin tärkeää, sillä muuten on vaikea ymmärtää, mitä osaa esimerkkikoodista tulisi muokata, jotta se toimii omalla datalla.
-   Dokumentaation lukeminen: ?funktion\_nimi antaa syvempää tietoa funktiosta ja sen toiminnasta, ja dokumentaation ymmärtäminen on usein pakollista, jos haluaa käyttää funktiota omiin tarkoituksiinsa.
