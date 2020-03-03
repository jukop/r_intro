
# R intro

This repository contains materials used in an introductory level R course at university of Eastern Finland. For now, all the material here is written Finnish

## R-kurssi

Täältä löydät suomenkielisen materiaalin, joka tukee UEF:in R-kurssin suorittamista (R-kieli 3622223, 2 op). Jokaisen viikon materiaalit päivitetään tänne. Alla on ohjeet tarvittavien asioiden asentamiseen UEF:in koneille.


# RStudio

Erona Moodlessa löytyviin ohjeisiin, tuutoroinnissa käytetään R-ohjelmoinnin tukena RStudiota. ohjelmointiympäristö eli IDE (integrated development environment), joka tekee koodaamisesta huomattavasti mukavampaa.

## RStudion asennus

Näiden ohjeiden avulla saat asennettua RStudion ja sen mukana R:n UEF:in koneille. Omalle koneelle asennettaessa täytyy ensin asentaa R, ja sitten RStudio. Ohjeet löytyvät helposti Googlesta, esim [täältä](https://courses.edx.org/courses/UTAustinX/UT.7.01x/3T2014/56c5437b88fa43cf828bff5371c6a924/).

RStudion saa asennettua UEF:in koneilla Sofware Centerin kautta. Software Center löytyy Windowsin omalla haulla.

<img src="viikko_1/figures/software_center.jpg" alt="" width="300"/>

RStudio:n voi asentaa Software Centeristä, ja RStudion pitäisi sen jälkeen olla käytettävissä.

<img src="viikko_1/figures/RStudio_sc.jpg" alt="" width="300"/>


## RStudion käyttö

RStudion näkymässä on neljä osaa:

<img src="viikko_1/figures/RStudio_tutorial_edited.jpg" alt="" width="600"/>

#### 1. Editori. 

Editorilla kirjoitetaan R-koodia sisältäviä tiedostoja, eli R-skriptejä. Uuden skriptin saa auki painamalla File -> New File -> R Script (tai Ctrl + Shift + N). Skripteihin tutustutaan myöhemmin kurssilla, mutta ne ovat yksinkertaisuudessaan kokoelma R-komentoja, jotka yhdessä tekevät jotain, esimerkiksi analysoivat jonkin tutkimusprojektin datan tai piirtävät valmiista tuloksista kuvaajia.

Editoriin kirjoitettua koodia voi ajaa rivi kerrallaan painamalla rivin kohdalla Ctrl + Enter. Useamman rivin voi myös maalata ja suorittaa kerrallaan. Yläreunassa oleva "Source"-nappi ajaa kaiken nykyisen tiedoston koodin.

R-skriptejä voi tallentaa ihan kuin muitakin tiedostoja. R-skpriptien  tiedostopääte on .R

#### 2. Konsoli.

Konsolissa "ajetaan" eli suoritetaan R-komentoja. Jos editoriin kirjoitettua koodia ajetaan, RStudio ajaa komennot automaattisesti konsolissa. Konsolissa pelkkä Enter riittä koodirivin suorittamiseen. Voit kokeilla kirjoittaa konsoliin jonkun laskutoimituksen, kuten ```2 * 3``` ja painaa Enter, jolloin tuloksen pitäisi tulostua konsoliin. Voit myös kokeilla kirjoittaa laskuja editoriin, ja painaa Ctrl + Enter, jolloin pitäisi tapahtua sama asia.

Suurin ero konsolin ja editorin välillä on se, että **konsoliin kirjoitetut komennot eivät tallennu mihinkään tiedostoon**. Jos siis haluat säästää koodisi, se tulee kirjoittaa editoriin ja tallentaa .R-tiedostoon. Saman istunnon aikana tehtyjä komentoja voi konsolissa selata ylös- ja alas-nuolila.

Moodlen ohjeissa ja videoissa käytetään R:ää puhtaasta R-konsolista. Voit siis kuvitella, että kurssin videoissa näkyy vain RStudion tämä osa, ja muut osat ovat vain helpottamassa työtäsi.

#### 3. Työtila

Työtilassa näkyvät R-istunnon aikana luodut muuttujat. Näistä lisää ensimmäisen viikon materiaalissa.

#### 4. Kuvaajat / Paketit / Manuaali

Tässä osassa on monta käytännöllistä välilehteä:

- Plots: tänne ilmestyvät R:llä piirretyt kuvaajat
- Packages: Täältä voi hallita asennettuja paketteja (alla ohjeet tällä kurssilla tarvittavien pakettien asennukseen)
- Help: Täällä voi selata R:n manuaalia, jossa on ohjeet jokaiselle R-komennolla. Voit kokeilla ajaa editorissa tai konsolissa komennon ```?print```, joka avaa ```print```-funktion ohjesivun.


### Kurssin tehtävien tekeminen RStudiolla

Suurin osa kurssin tehtävistä on melko lyhyitä, joten ne voi tarvittaessa tehdä suoraan konsoliin. Suosittelen kuitenkin kirjoittamaan varsinkin pidemmät ja monimutkaisemmat tehtävät muistiin editoriin. Valitettavasti kurssin tehtävien aktivointi aiheuttaa RStudion toiminnassa hieman outouksia, koska kurssin tehtäviä ei ole suunniteltu RStudiolla tehtäviksi. Aina kun editorista ajaa komennon, editorin tilalle aukeaa "kysymys"-ikkuna. Tästä ei kuitenkaan tarvitse välittää, vaan ikkunan voi sulkea.

**HUOM:** kurssin osioiden tehtäviä ei voi tallentaa kesken osion, vaan jokainen osio on tehtävä kerralla kokonaan. Jos kuitenkin kirjoitat koodia editoriin ja tallennat tehtäviä .R-tiedostoon, voit tarvittaessa aloittaa osion toisena päivänä uudestaan ja ajaa edellisellä kerralla kirjoittamasi komennot helposti tiedostosta. Tehtäviä voi palauttaa vain UEFAD-verkon koneilla!.

Tehtävien tekemisen voi aloittaa komennolla ```Rkurssi::Rkurssi(123456)```, kun numeron 123456 korvaa omalla opiskelijanumerolla ja seuraamalla avautuvia ohjeita. Tätä varten tulee kuitenkin asentaa ```Rkurssi```-paketti. Tähän on ohjeet alla.

## Kurssilla tarvittavien R-pakettien asennus

R-ohjelmoinnissa asennetaan usein R-paketteja. Paketit ovat kokonaisuuksia, jotka lisäävät R:ään ominaisuuksia. Esimerkiksi tälle kurssille tarvittavat paketit arpovat opiskelijalle tehtäviä kurssin aihepiiristä ja lähettävät tiedon osioiden suorituksesta opettajalle.

Ensin asennetaan paketti ```sendmailR```. Valitaan RStudion oikean alakulman osasta Packages -> Install. Avautuvaan ikkunaan kirjoitetaan paketin nimeksi "sendmailR" ja asennetaan paketti. CRAN (Comprehensive R Archive Network) on paikka, johon iso osa R-paketeista on tallennettu, jotta ne on helppo asentaa.

<img src="viikko_1/figures/package_install.jpg" alt="" width="700"/>

 Itse tehtäviä arpova ```Rkurssi```-paketti ei ole CRAN:issa, vaan se pitää ladata kurssin Moodle-sivulta ```Rkurssi.zip```-tiedostona. Paketin asennusta varten valitaan Install-ikkunasta "Install from"-vaihtoehdoksi "Package Archive File", ja valitaan aukeavasta ikkunasta juuri ladattu  ```Rkurssi.zip```
 
 <img src="viikko_1/figures/install_rkurssi.jpg" alt="" width="300"/>