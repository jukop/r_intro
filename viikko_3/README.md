
Uusia tietotyyppejä
===================

Esittely
--------

Tässä osassa tutustutaan neljään uuteen tietorakenteeseen:

-   [matrsiisi (matrix)](#matriisi)
-   [taulukko (array)](#taulukko)
-   [lista (list)](#lista)
-   [data frame](#data-frame)

Taulukko on juuri sitä miltä se kuulostaa: vektorintapainen tietorakenne, johon tallennetaan alkioita (elements), joilla on kaikilla sama luokka (class), eli esimerkiksi lukuja. Ero vektoriin on se, että taulukolla on useampi ulottuvuus. Matriisi on erikoistapaus taulukosta, sillä matriisi on kaksiulotteinen taulukko. Matriisi vastaa siis oikeastaan paremmin sitä mielikuvaa, joka monelle tulee mieleen suomen sanasta taulukko, ja matriisit ovatkin paljon yleisempiä kuin moniulotteiset taulukot.

Matriisi voi olla joillekin sanana tuttu myös tilastotieteen tai matematiikan kursseilta, ja R:n matriisi vastaakin matemaattista matriisia. Tästä syystä matriisi on hyvin yleinen tietorakenne, johon ei voi olla törmäämättä jos käyttää R:ää tutkimuksessa.

Lista on kokoelma alkioita, joilla voi olla eri luokkia. Data frame on matriisin kaltainen kaksiulotteinen tietorakenne, jonka sarakkeilla voi olla eri luokkia.

Aloitetaan matriiseista.

Matriisi
--------

### Matriisin luominen

Matriisin luominen on yksinkertaista, ja tapahtuu funktiolla `matrix`

``` r
matrix(1:9, nrow = 3, ncol = 3)
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7
    ## [2,]    2    5    8
    ## [3,]    3    6    9

Funktiolle matrix annetaan siis matriisiin tallennettavat luvut vektorina, sekä matriisin rivien ja sarakkeiden määrä (`ncol` ja `nrow`). Matriisi voi koostua myös kokonaan tietystä arvosta:

``` r
matrix(0, nrow = 2, ncol = 5)
```

    ##      [,1] [,2] [,3] [,4] [,5]
    ## [1,]    0    0    0    0    0
    ## [2,]    0    0    0    0    0

Useimmiten matriisien data luetaan R:ään jostain tiedostosta, joka on tuotettu Excelillä tai jollain muulla ohjelmalla (tutkimustulosten kirjaus suoraan R:ään olisi raskasta). Matriisien luonti käsin on kuitenkin hyvä osata, sillä pienillä matriiseilla on kätevää testata omaa koodia. Myös yllä olevan kaltaisia, esim. nollalla täytettyjä matriiseja on joskus kätevää käyttää "alustana", kun lasketaan omasta datasta tuloksia rivi tai sarake kerrallaan. Tämä johtuu siitä, että olemassa olevan matriisin rivin arvojen vaihtaminen on nopeampi operaatio kuin rivin lisääminen matriisiin.

### Matriisin koko

Joskus voi törmätä matriiseihin, joiden koko ei tiedä, tai ei halua olettaa. Tällöin tarvitaan funktioita, jotka kertovat matriisin koosta. Esimerkiksi, kun luetaan dataa R:ään tiedostoista, on hyvä tarkistaa, että kaikki rivit ja sarakkeet ovat mukana. `nrow` ja `ncol` palauttavat rivien ja sarakkeiden määrän, `dim` palauttaa matriisin rivien ja sarakkeiden määrän, rivit ensin.

``` r
X <- matrix(1:12, ncol = 4)
# Number of rows
nrow(X)
```

    ## [1] 3

``` r
# Number of columns
ncol(X)
```

    ## [1] 4

``` r
#Dimensions
dim(X)
```

    ## [1] 3 4

### Matriisin indeksointi

Matriisin indeksointi on hyvin samantapainen operaatio kuin vektorin indeksointi, eli matriisin perään laitetaan hakasulkeet ja niihin määritellään halutut arvot. Matriisin indeksoinnissa pitää kuitenkin antaa erikseen indeksit riveille ja sarakkeille, pilkulla erotettuna.

``` r
# Only nrow is enough, since the number of columns must be 3
X <- matrix(1:9, nrow = 3)
X
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7
    ## [2,]    2    5    8
    ## [3,]    3    6    9

``` r
# Element on second row, third column
X[2, 3]
```

    ## [1] 8

``` r
# The complete first row
X[1, ]
```

    ## [1] 1 4 7

``` r
# The second and third values of the second column
X[2:3, 3]
```

    ## [1] 8 9

``` r
# Get rows where the values of the first column is > 1
X[X[, 1] > 1, ]
```

    ##      [,1] [,2] [,3]
    ## [1,]    2    5    8
    ## [2,]    3    6    9

HUOM: jos matriisia indeksoidessa tuloksessa sarakkeiden tai rivien määrä on tasan yksi, kuten yllä olevissa esimerkeissä viimeistä lukuun ottamatta, tuloksena on vektori, ei matriisi. Jos haluaa tuloksen olevan matriisi, tulee hakasulkeisiin lisätä määre `drop = FALSE`

``` r
# The complete first row
X[1, ,drop = FALSE]
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7

``` r
# The second and third values of the second column
X[2:3, 3, drop = FALSE]
```

    ##      [,1]
    ## [1,]    8
    ## [2,]    9

Matriiseja voi myös muokata sijoittamalla haluttuihin paikkoihin uusia arvoja:

``` r
# Copy of X
X_new <- X
# Replace first row with new values
X_new[1, ] <- c(10, 13, 15)
X_new
```

    ##      [,1] [,2] [,3]
    ## [1,]   10   13   15
    ## [2,]    2    5    8
    ## [3,]    3    6    9

``` r
# Replacement can also be a single value, and will be recycled
X_new[2:3, 1] <- 0
X_new
```

    ##      [,1] [,2] [,3]
    ## [1,]   10   13   15
    ## [2,]    0    5    8
    ## [3,]    0    6    9

Rivejä tai sarakkeita voi myös poistaa. Tämä tapahtuu antamalla indeksi miinusmerkkisenä:

``` r
# Without first row
X[-1, ]
```

    ##      [,1] [,2] [,3]
    ## [1,]    2    5    8
    ## [2,]    3    6    9

``` r
# Without second column
X[, -2]
```

    ##      [,1] [,2]
    ## [1,]    1    7
    ## [2,]    2    8
    ## [3,]    3    9

#### Indeksimatriisi (index matrix)

Jos halutaan poimia useampi yksittäinen arvo matriisista, tulee käyttää indeksimatriisia (index matrix).

Esimerkiksi, jos haluttaisiin poimia äskeisestä X-matriisista arvot \[1, 2\], \[1, 3\] ja \[2,2\], tämä ei toimi:

``` r
X[c(1, 1, 2), c(2, 3, 2)]
```

    ##      [,1] [,2] [,3]
    ## [1,]    4    7    4
    ## [2,]    4    7    4
    ## [3,]    5    8    5

vaan tulee käyttää indeksimatriisia, jonka jokainen rivi antaa yhden halutun alkion indeksit, ensin rivi ja sitten sarake. Indeksimatriiseja tehdessä kannattaa asettaa lisämäärä `byrow = TRUE`, jolloin alkiot laitetaan matriisiin rivi kerrallaan, ei sarake kerrallaan niin kuin oletuksena.

``` r
i <- matrix(c(1, 2, 1, 3, 2, 2), nrow = 3, byrow = TRUE)
i
```

    ##      [,1] [,2]
    ## [1,]    1    2
    ## [2,]    1    3
    ## [3,]    2    2

``` r
X[i]
```

    ## [1] 4 7 5

### Matriisien rakentaminen vektoreista

Matriisi koostuu usein useammasta muuttujasta ja havainnoista. Yleensä jokainen rivi vastaa yhtä havaintoa, ja sarake muuttujaa. Tämän takia on hyvä tietää, miten yksittäisistä vektoreista saa koottua matriiseja. Alla olevassa esimerkissä on koottu yhteen matriisiin Star Wars-hahmojen pituuksia ja painoja. Tämä tapahtuu `cbind` funktiolla (column bind), joka nimensä mukaan yhdistää vektorit matriisin sarakkeiksi. `cbind` voi yhdistää myös valmiita matriiseja yhteen, niin että matriisit ovat "vierekkäin" eli yhdistetyssä matriisissa on kummankin matriisin sarakkeet (rivien määrän tulee olla sama). Vastaavasti `rbind` (row bind) yhdistää matriiseja "allekkain".

``` r
heights <- c(172, 167, 96, 202, 150, 178)
masses <- c(77, 75, 32, 136, 49, 120)

starwars <- cbind(heights, masses)
starwars
```

    ##      heights masses
    ## [1,]     172     77
    ## [2,]     167     75
    ## [3,]      96     32
    ## [4,]     202    136
    ## [5,]     150     49
    ## [6,]     178    120

### Rivien ja sarakkeiden nimeäminen

Matriisien rivit ja sarakkeet voi nimetä, ja usein tässä onkin järkeä. Yllä olevassa esimerkissä `starwars`-matriisin sarakkeet on nimetty alkuperäisten vektorin mukaan. Alla olevassa esimerkissä on lisää tapoja nimetä rivejä ja sarakkeita

``` r
# Set column names by naming arguments while building matrix from vectors
cbind(Height = heights, Mass = masses)
```

    ##      Height Mass
    ## [1,]    172   77
    ## [2,]    167   75
    ## [3,]     96   32
    ## [4,]    202  136
    ## [5,]    150   49
    ## [6,]    178  120

``` r
# Set column and row names explicitly
colnames(starwars) <- c("Height", "Mass")
rownames(starwars) <- c("Luke Skywalker", "C-3PO", "R2-D2", "Darth Vader", "Leia Organa", "Owen Lars")
starwars
```

    ##                Height Mass
    ## Luke Skywalker    172   77
    ## C-3PO             167   75
    ## R2-D2              96   32
    ## Darth Vader       202  136
    ## Leia Organa       150   49
    ## Owen Lars         178  120

Nimettyjä matriiseja voi indeksoida myös nimien perusteella:

``` r
starwars[c("Luke Skywalker", "R2-D2"), ]
```

    ##                Height Mass
    ## Luke Skywalker    172   77
    ## R2-D2              96   32

Matriisiin voi myös lisätä uusia sarakkeita `cbind` funktiolla. Alla lisätään matriisiin starwars uusi sarake, jossa on hahmojen BMI:

``` r
# Create a vector for BMI and add to matrix with cbind
bmi <- starwars[, "Mass"] / (starwars[, "Height"] / 100)^2
cbind(starwars, "BMI" = bmi)
```

    ##                Height Mass      BMI
    ## Luke Skywalker    172   77 26.02758
    ## C-3PO             167   75 26.89232
    ## R2-D2              96   32 34.72222
    ## Darth Vader       202  136 33.33007
    ## Leia Organa       150   49 21.77778
    ## Owen Lars         178  120 37.87401

### Matriiseilla laskeminen

Matriiseilla laskeminen on hyvin samankaltaista kuin vektoreilla laskeminen. Matriisin ja yksittäisen luvun välisessä operaatiossa matriisin alkiot käsitellään yksitellen. Samoin samankokoiset matriisit voi esim. lisätä yhteen, jolloin lisäys tapahtuu alkio kerrallaan.

``` r
X <- matrix(1:9, nrow = 3)
Y <- matrix(3:11, nrow = 3, ncol = 3)
# Element-wise multiplication
X * 2
```

    ##      [,1] [,2] [,3]
    ## [1,]    2    8   14
    ## [2,]    4   10   16
    ## [3,]    6   12   18

``` r
# Element-wise sum
X + Y
```

    ##      [,1] [,2] [,3]
    ## [1,]    4   10   16
    ## [2,]    6   12   18
    ## [3,]    8   14   20

Matriiseille on lisäksi määritelty paljon matriisien omia laskutoimituksia, niistä voi lukea lisää oppikirjasta. Matriisilaskentaa opiskelleille huomio: R:ssä oletuksena kertolasku tehdään alkioittain, matriisien kertolasku tapahtuu operaattorilla `%*%`.

Taulukko
--------

Kuten alussa todettiin, taulukot (array) ovat hyvin harvinaisia, joten niihin ei kannata tällä kurssilla keskittyä. Niitä kuitenkin tarvitaan joidenkin tehtävien tekemiseen, joten tässä on hyvin lyhyt oppimäärä taulukoista.

Taulukot ovat matriisien kaltaisia, mutta taulukossa voi olla yli kaksi ulottuvuutta. Oikeastaan matriisit ovat kaksiulotteisia taulukoita. Alla on esimerkki 3-ulotteisesta taulukosta, jota voi ajatella "peräkkäin" olevina matriiseina. Alla on kuva 1-ulotteisesta taulukosta eli vektorista, 2-ulotteisesta taulukosta eli matriisista ja 3-ulotteisesta taulukosta.

![](array.png)

Taulukkoja luodaan matriisien tapaan funktiolla `array`. Toisin kuin matriisien tapauksessa, `array`-funktiolle pitää kertoa rivien ja sarakkeiden määrän lisäksi ulottuvuuksien määrä. Alla oleva esimerkki luo 3-ulotteisen taulukon, jonka voi ajatella koostuvan kolmesta 4 x 2 matriisista.

``` r
my_array <- array(1:24, dim = c(4, 2, 3))
my_array
```

    ## , , 1
    ## 
    ##      [,1] [,2]
    ## [1,]    1    5
    ## [2,]    2    6
    ## [3,]    3    7
    ## [4,]    4    8
    ## 
    ## , , 2
    ## 
    ##      [,1] [,2]
    ## [1,]    9   13
    ## [2,]   10   14
    ## [3,]   11   15
    ## [4,]   12   16
    ## 
    ## , , 3
    ## 
    ##      [,1] [,2]
    ## [1,]   17   21
    ## [2,]   18   22
    ## [3,]   19   23
    ## [4,]   20   24

Taulukoita indeksoidaan aivan kuten matriiseja, mutta jokaiselle ulottuvuudelle on annettava oma indeksi:

``` r
# The first 2 rows of each "layer"
my_array[1:2, , ]
```

    ## , , 1
    ## 
    ##      [,1] [,2]
    ## [1,]    1    5
    ## [2,]    2    6
    ## 
    ## , , 2
    ## 
    ##      [,1] [,2]
    ## [1,]    9   13
    ## [2,]   10   14
    ## 
    ## , , 3
    ## 
    ##      [,1] [,2]
    ## [1,]   17   21
    ## [2,]   18   22

``` r
# Second column from last two layers
my_array[, 2, 2:3]
```

    ##      [,1] [,2]
    ## [1,]   13   21
    ## [2,]   14   22
    ## [3,]   15   23
    ## [4,]   16   24

Lista
-----

Lista (list) on vektorinkaltainen tietorakenne, jossa on järjestyksessä alkioita, jotka on mahdollisesti nimetty. Tärkeä ero vektoriin verrattuna on, että listan alkiot voivat olla erityyppisiä. Listoja luodaan `list`-funktiolla:

``` r
example_list <- list(c(1, 2, 3),
                     matrix(0, nrow = 3, ncol = 4),
                     "list can include anything")
example_list
```

    ## [[1]]
    ## [1] 1 2 3
    ## 
    ## [[2]]
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    0    0    0    0
    ## [2,]    0    0    0    0
    ## [3,]    0    0    0    0
    ## 
    ## [[3]]
    ## [1] "list can include anything"

``` r
subject_ids <- c("ANKL", "PEPA", "DIPR")
measurements <- matrix(c(1, 2.5, 3,
                         3.5, 5, 3,
                         2.3, 3, 1.6),
                       nrow = 3)
colnames(measurements) <- c("CRP", "HDL", "LDL")
rownames(measurements) <- subject_ids
# List names can be given with or without quotes
study <- list(Subject_ID = subject_ids,
              "Measurements" = measurements,
              Study_name = "Blood tests")
study
```

    ## $Subject_ID
    ## [1] "ANKL" "PEPA" "DIPR"
    ## 
    ## $Measurements
    ##      CRP HDL LDL
    ## ANKL 1.0 3.5 2.3
    ## PEPA 2.5 5.0 3.0
    ## DIPR 3.0 3.0 1.6
    ## 
    ## $Study_name
    ## [1] "Blood tests"

Listoja ja niiden kaltaisia olioita käytetään R:ssä paljon. Listoihin on kätevä kerätä erilaista tietoa, jotka halutaan säilyttää samassa paketissa. Esimerkiksi yksinkertaisetkin tilastolliset mallit tuottavat paljon erilaista tietoa, jotka pakataan listaan (tarkemmin listan kaltaiseen olioon, tästä lisää myöhemmin).

### Listojen alkioiden käsittely

Listan alkioihin pääsee käsiksi kahdella eri tavalla: kaksoishakasulkeilla `[[]]` tai, jos lista on nimetty, dollarimerkillä `$`:

``` r
# By position
study[[2]]
```

    ##      CRP HDL LDL
    ## ANKL 1.0 3.5 2.3
    ## PEPA 2.5 5.0 3.0
    ## DIPR 3.0 3.0 1.6

``` r
# By name
study[["Subject_ID"]]
```

    ## [1] "ANKL" "PEPA" "DIPR"

``` r
# Using dollar sign
study$Study_name
```

    ## [1] "Blood tests"

Listaa voi indeksoida myös yksinkertaisilla hakasulkeilla. Tällöin palautetaan aina lista, eikä yksittäistä alkiota kuten aiemmin. Tämän demonstroiminen vaatii tutustumista uuteen funktioon `class`, joka palauttaa argumenttinsa luokan (class). Vektorin luokka vaihtelee vektorin sisällön mukaan: numeric = lukuja, character = merkkijonoja, logical = loogisia arvoja, jne. Listojen luokka on luonnollisesti list. Lisätietoa: R:ssä kaikki muuttujiin tallennettavat tiedot ovat olioita (object). Ohjelmointikielten olioilla on aina luokka, joka määrittää sen ominaisuudet. Esimerkiksi `print` ja `plot`-komennot toimivat eri tavalla riippuen niiden argumentin luokasta.

Tarkastellaan alla, mikä ero yksinkertaisilla ja kaksinkertaisilla hakasulkeilla on listan indeksoinnissa:

``` r
# Returns a list of length one with the matrix as the only element
study[2]
```

    ## $Measurements
    ##      CRP HDL LDL
    ## ANKL 1.0 3.5 2.3
    ## PEPA 2.5 5.0 3.0
    ## DIPR 3.0 3.0 1.6

``` r
class(study[2])
```

    ## [1] "list"

``` r
# Returns the actual matrix
study[[2]]
```

    ##      CRP HDL LDL
    ## ANKL 1.0 3.5 2.3
    ## PEPA 2.5 5.0 3.0
    ## DIPR 3.0 3.0 1.6

``` r
class(study[[2]])
```

    ## [1] "matrix"

``` r
# Dollar sign also returns the matrix
class(study$Measurements)
```

    ## [1] "matrix"

``` r
# Single brackets works as subscripting just like with vectors
study[2:3]
```

    ## $Measurements
    ##      CRP HDL LDL
    ## ANKL 1.0 3.5 2.3
    ## PEPA 2.5 5.0 3.0
    ## DIPR 3.0 3.0 1.6
    ## 
    ## $Study_name
    ## [1] "Blood tests"

### Alkion lisäys listaan ja listojen yhdistäminen

Yksittäisen alkion voi lisätä listaan sijoittamalla listan johonkin indeksiin tai nimeen uusi arvo (indeksin pitää olla yhtä suurempi kuin listan pituus). HUOM! Listan alkio voi myös itse olla lista (sisäkkäinen lista = nested list).

``` r
# Add a character matrix as the fourth element of study
study[[4]] <- matrix(c("CPR", "HDL", "LDL",
                       "C-reactive protein", "High-density lipoprotein",
                       "Low-density lipoprotein"),
                     ncol = 2)
# An element of a list can also be a list
study[["professional"]] <- list(name = c("John H. Watson"),
                                position = "Medical doctor",
                                age = 45)
study
```

    ## $Subject_ID
    ## [1] "ANKL" "PEPA" "DIPR"
    ## 
    ## $Measurements
    ##      CRP HDL LDL
    ## ANKL 1.0 3.5 2.3
    ## PEPA 2.5 5.0 3.0
    ## DIPR 3.0 3.0 1.6
    ## 
    ## $Study_name
    ## [1] "Blood tests"
    ## 
    ## [[4]]
    ##      [,1]  [,2]                      
    ## [1,] "CPR" "C-reactive protein"      
    ## [2,] "HDL" "High-density lipoprotein"
    ## [3,] "LDL" "Low-density lipoprotein" 
    ## 
    ## $professional
    ## $professional$name
    ## [1] "John H. Watson"
    ## 
    ## $professional$position
    ## [1] "Medical doctor"
    ## 
    ## $professional$age
    ## [1] 45

``` r
# Note that the fourth element has no name
names(study)
```

    ## [1] "Subject_ID"   "Measurements" "Study_name"   ""            
    ## [5] "professional"

Listoja voi yhdistää vektorien tapaan `c()`-funktiolla:

``` r
# Concatenate two vectors
vector1 <- c(3, 6, 5)
vector2 <- c(1, 2, 3)
c(vector1, vector2)
```

    ## [1] 3 6 5 1 2 3

``` r
list1 <- list(vector = vector1,
              name = "list1")
list2 <- study[1:2]
# Concatenate three lists, names stay the same
c(list1, list2, list(first_element = "A", second = "B"))
```

    ## $vector
    ## [1] 3 6 5
    ## 
    ## $name
    ## [1] "list1"
    ## 
    ## $Subject_ID
    ## [1] "ANKL" "PEPA" "DIPR"
    ## 
    ## $Measurements
    ##      CRP HDL LDL
    ## ANKL 1.0 3.5 2.3
    ## PEPA 2.5 5.0 3.0
    ## DIPR 3.0 3.0 1.6
    ## 
    ## $first_element
    ## [1] "A"
    ## 
    ## $second
    ## [1] "B"

Data frame
----------

Data frame on erittäin yleinen tapa tallentaa tietoa R:ssä. Data frame on kaksiulotteinen tietorakenne, eli sillä on rivejä ja sarakkeita aivan kuten matriisilla. Data framen ja matriisin välillä on kuitenkin yksi tärkeä ero: data framen sarakkeet voivat olla eri luokan vektoreita.

Muutetaan esimerkiksi edellä nähdyn `study`-listan \`\`Subject\_ID`ja`Measurements\`\`\`-osat yhdeksi data frameksi:

``` r
study_data <- data.frame(Subject_ID = study$Subject_ID,
                         study$Measurements)
study_data
```

    ##      Subject_ID CRP HDL LDL
    ## ANKL       ANKL 1.0 3.5 2.3
    ## PEPA       PEPA 2.5 5.0 3.0
    ## DIPR       DIPR 3.0 3.0 1.6

`data.frame`-funktiolle voi antaa sekaisin yksittäisiä vektoreita, tai kokonaisia matriiseja tai valmiita data frameja, jotka kaikki yhdistetään yhdeksi data frameksi.

### Data framen käsittely

Vaikka data frame näyttää ulkoisesti matriisilta, data frame on itse asiassa lista, jonka kaikki alkiot ovat yhtä pitkiä vektoreita. Data framella on kuitenkin monta erityisominaisuutta, ja data frame käyttäytyy välillä kuin matriisi, välillä kuin lista. Tässä muutama esimerkki:

``` r
# Subscripting with brackets - as matrix
study_data[2:3, 1:3]
```

    ##      Subject_ID CRP HDL
    ## PEPA       PEPA 2.5   5
    ## DIPR       DIPR 3.0   3

``` r
# Rownames and colnames - as matrix
colnames(study_data)
```

    ## [1] "Subject_ID" "CRP"        "HDL"        "LDL"

``` r
# Individual columns can be accessed and added with dollar sign - as list
study_data$CRP
```

    ## [1] 1.0 2.5 3.0

``` r
study_data$height <- c(168, 185, 172)
study_data
```

    ##      Subject_ID CRP HDL LDL height
    ## ANKL       ANKL 1.0 3.5 2.3    168
    ## PEPA       PEPA 2.5 5.0 3.0    185
    ## DIPR       DIPR 3.0 3.0 1.6    172

``` r
# Filtering based on a variable can be done like this
study_data[study_data$HDL < 4, ]
```

    ##      Subject_ID CRP HDL LDL height
    ## ANKL       ANKL   1 3.5 2.3    168
    ## DIPR       DIPR   3 3.0 1.6    172

Data framet ovat erittäin käteviä, koska niihin voi helposti tallentaa sekä merkkijonoja, että numeerista dataa. Kannattaa kuitenkin muistaa, että matriisi on usein laskennan kannalta tehokkaampi tietorakenne, kuin data frame. Tästä ei tarvitse murehtia tällä kurssilla, mutta se on hyvä tietää jatkon kannalta, jos bioinformatiikkakursseilla tulee vastaan isompia datasettejä, joissa on osia, jotka voi tallentaa matriisina.

View()
------

Data frameja ja matriiseja tai niiden osia voi tulostaa R:n konsoliin kuten muitakin muuttujia. Tarkempaa tarkastelua varten kannattaa kuitenkin käyttää `View`-funktiota. `View` avaa ikkunan, jossa voi selata data framen tai matriisin rivejä ja sarakkeita, sekä järjestää arvoja halutun sarakkeen mukaan (tämä järjestys säilyy vain `View`-näkymässä, itse muuttujan rakenne ei muutu).
