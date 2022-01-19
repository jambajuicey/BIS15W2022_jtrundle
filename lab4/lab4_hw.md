---
title: "Lab 4 Homework"
author: "Julie Trundle"
date: "2022-01-19"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Data
For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

**1. Load the data into a new object called `homerange`.**


```r
homerange <- readr::read_csv("data/Tamburelloetal_HomeRangeDatabase.csv")
```

```
## Rows: 569 Columns: 24
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (16): taxon, common.name, class, order, family, genus, species, primarym...
## dbl  (8): mean.mass.g, log10.mass, mean.hra.m2, log10.hra, dimension, preyma...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  


```r
view(homerange)
```


```r
dim(homerange)
```

```
## [1] 569  24
```


```r
colnames(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```


```r
glimpse(homerange)
```

```
## Rows: 569
## Columns: 24
## $ taxon                      <chr> "lake fishes", "river fishes", "river fishe…
## $ common.name                <chr> "american eel", "blacktail redhorse", "cent…
## $ class                      <chr> "actinopterygii", "actinopterygii", "actino…
## $ order                      <chr> "anguilliformes", "cypriniformes", "cyprini…
## $ family                     <chr> "anguillidae", "catostomidae", "cyprinidae"…
## $ genus                      <chr> "anguilla", "moxostoma", "campostoma", "cli…
## $ species                    <chr> "rostrata", "poecilura", "anomalum", "fundu…
## $ primarymethod              <chr> "telemetry", "mark-recapture", "mark-recapt…
## $ N                          <chr> "16", NA, "20", "26", "17", "5", "2", "2", …
## $ mean.mass.g                <dbl> 887.00, 562.00, 34.00, 4.00, 4.00, 3525.00,…
## $ log10.mass                 <dbl> 2.9479236, 2.7497363, 1.5314789, 0.6020600,…
## $ alternative.mass.reference <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
## $ mean.hra.m2                <dbl> 282750.00, 282.10, 116.11, 125.50, 87.10, 3…
## $ log10.hra                  <dbl> 5.4514026, 2.4504031, 2.0648696, 2.0986437,…
## $ hra.reference              <chr> "Minns, C. K. 1995. Allometry of home range…
## $ realm                      <chr> "aquatic", "aquatic", "aquatic", "aquatic",…
## $ thermoregulation           <chr> "ectotherm", "ectotherm", "ectotherm", "ect…
## $ locomotion                 <chr> "swimming", "swimming", "swimming", "swimmi…
## $ trophic.guild              <chr> "carnivore", "carnivore", "carnivore", "car…
## $ dimension                  <dbl> 3, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3…
## $ preymass                   <dbl> NA, NA, NA, NA, NA, NA, 1.39, NA, NA, NA, N…
## $ log10.preymass             <dbl> NA, NA, NA, NA, NA, NA, 0.1430148, NA, NA, …
## $ PPMR                       <dbl> NA, NA, NA, NA, NA, NA, 530, NA, NA, NA, NA…
## $ prey.size.reference        <chr> NA, NA, NA, NA, NA, NA, "Brose U, et al. 20…
```


```r
spec(homerange)
```

```
## cols(
##   taxon = col_character(),
##   common.name = col_character(),
##   class = col_character(),
##   order = col_character(),
##   family = col_character(),
##   genus = col_character(),
##   species = col_character(),
##   primarymethod = col_character(),
##   N = col_character(),
##   mean.mass.g = col_double(),
##   log10.mass = col_double(),
##   alternative.mass.reference = col_character(),
##   mean.hra.m2 = col_double(),
##   log10.hra = col_double(),
##   hra.reference = col_character(),
##   realm = col_character(),
##   thermoregulation = col_character(),
##   locomotion = col_character(),
##   trophic.guild = col_character(),
##   dimension = col_double(),
##   preymass = col_double(),
##   log10.preymass = col_double(),
##   PPMR = col_double(),
##   prey.size.reference = col_character()
## )
```

**3. Change the class of the variables `taxon` and `order` to factors and display their levels.** 


```r
homerange$taxon <- as.factor(homerange$taxon)
homerange$order <- as.factor(homerange$order)
glimpse(homerange)
```

```
## Rows: 569
## Columns: 24
```

```
## Warning in grepl(",", levels(x), fixed = TRUE): input string 50 is invalid UTF-8
```

```
## $ taxon                      <fct> lake fishes, river fishes, river fishes, ri…
## $ common.name                <chr> "american eel", "blacktail redhorse", "cent…
## $ class                      <chr> "actinopterygii", "actinopterygii", "actino…
## $ order                      <fct> anguilliformes, cypriniformes, cypriniforme…
## $ family                     <chr> "anguillidae", "catostomidae", "cyprinidae"…
## $ genus                      <chr> "anguilla", "moxostoma", "campostoma", "cli…
## $ species                    <chr> "rostrata", "poecilura", "anomalum", "fundu…
## $ primarymethod              <chr> "telemetry", "mark-recapture", "mark-recapt…
## $ N                          <chr> "16", NA, "20", "26", "17", "5", "2", "2", …
## $ mean.mass.g                <dbl> 887.00, 562.00, 34.00, 4.00, 4.00, 3525.00,…
## $ log10.mass                 <dbl> 2.9479236, 2.7497363, 1.5314789, 0.6020600,…
## $ alternative.mass.reference <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
## $ mean.hra.m2                <dbl> 282750.00, 282.10, 116.11, 125.50, 87.10, 3…
## $ log10.hra                  <dbl> 5.4514026, 2.4504031, 2.0648696, 2.0986437,…
## $ hra.reference              <chr> "Minns, C. K. 1995. Allometry of home range…
## $ realm                      <chr> "aquatic", "aquatic", "aquatic", "aquatic",…
## $ thermoregulation           <chr> "ectotherm", "ectotherm", "ectotherm", "ect…
## $ locomotion                 <chr> "swimming", "swimming", "swimming", "swimmi…
## $ trophic.guild              <chr> "carnivore", "carnivore", "carnivore", "car…
## $ dimension                  <dbl> 3, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3…
## $ preymass                   <dbl> NA, NA, NA, NA, NA, NA, 1.39, NA, NA, NA, N…
## $ log10.preymass             <dbl> NA, NA, NA, NA, NA, NA, 0.1430148, NA, NA, …
## $ PPMR                       <dbl> NA, NA, NA, NA, NA, NA, 530, NA, NA, NA, NA…
## $ prey.size.reference        <chr> NA, NA, NA, NA, NA, NA, "Brose U, et al. 20…
```

**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**


```r
taxa <- data.frame(select(homerange, taxon, common.name, class, order, family, genus, species))
taxa
```

```
##             taxon                      common.name          class
## 1     lake fishes                     american eel actinopterygii
## 2    river fishes               blacktail redhorse actinopterygii
## 3    river fishes              central stoneroller actinopterygii
## 4    river fishes                    rosyside dace actinopterygii
## 5    river fishes                    longnose dace actinopterygii
## 6    river fishes                      muskellunge actinopterygii
## 7   marine fishes                          pollack actinopterygii
## 8   marine fishes                           saithe actinopterygii
## 9   marine fishes                lined surgeonfish actinopterygii
## 10  marine fishes          orangespine unicornfish actinopterygii
## 11  marine fishes            bluespine unicornfish actinopterygii
## 12  marine fishes                    redlip blenny actinopterygii
## 13  marine fishes                   giant trevally actinopterygii
## 14    lake fishes                        rock bass actinopterygii
## 15    lake fishes                      pumpkinseed actinopterygii
## 16    lake fishes                         bluegill actinopterygii
## 17   river fishes                  longear sunfish actinopterygii
## 18   river fishes                  smallmouth bass actinopterygii
## 19    lake fishes                  largemouth bass actinopterygii
## 20    lake fishes                    white crappie actinopterygii
## 21  marine fishes eastern triangular butterflyfish actinopterygii
## 22  marine fishes          Tahititan butterflyfish actinopterygii
## 23  marine fishes            chevron butterflyfish actinopterygii
## 24  marine fishes              melon butterflyfish actinopterygii
## 25  marine fishes           teardrop butterflyfish actinopterygii
## 26  marine fishes                         red moki actinopterygii
## 27  marine fishes              redspotted hawkfish actinopterygii
## 28  marine fishes                   dwarf hawkfish actinopterygii
## 29  marine fishes                          cabezon actinopterygii
## 30  marine fishes              japanese shrimpgoby actinopterygii
## 31  marine fishes                  bluebanded goby actinopterygii
## 32  marine fishes                       rusty goby actinopterygii
## 33  marine fishes                    blackeye goby actinopterygii
## 34  marine fishes                  longfinned goby actinopterygii
## 35  marine fishes                     bermuda chub actinopterygii
## 36  marine fishes                  spanish hogfish actinopterygii
## 37  marine fishes                  humphead wrasse actinopterygii
## 38  marine fishes     mediterranean rainbow wrasse actinopterygii
## 39  marine fishes                    slippery dick actinopterygii
## 40  marine fishes                yellowhead wrasse actinopterygii
## 41  marine fishes                     clown wrasse actinopterygii
## 42  marine fishes                  blackear wrasse actinopterygii
## 43  marine fishes        bluestreak cleaner wrasse actinopterygii
## 44  marine fishes                    ballan wrasse actinopterygii
## 45  marine fishes                     maori wrasse actinopterygii
## 46  marine fishes            california sheepshead actinopterygii
## 47  marine fishes                           cunner actinopterygii
## 48  marine fishes                  bluehead wrasse actinopterygii
## 49  marine fishes                      moon wrasse actinopterygii
## 50  marine fishes               thumbprint emperor actinopterygii
## 51  marine fishes                   mutton snapper actinopterygii
## 52  marine fishes             schoolmaster snapper actinopterygii
## 53  marine fishes                checkered snapper actinopterygii
## 54  marine fishes                     gray snapper actinopterygii
## 55  marine fishes               yellowtail snapper actinopterygii
## 56  marine fishes                  ocean whitefish actinopterygii
## 57  marine fishes                 european seabass actinopterygii
## 58  marine fishes                   white goatfish actinopterygii
## 59  marine fishes             whitesaddle goatfish actinopterygii
## 60    lake fishes                     yellow perch actinopterygii
## 61  marine fishes                    canary damsel actinopterygii
## 62  marine fishes                       cherubfish actinopterygii
## 63  marine fishes                       damselfish actinopterygii
## 64  marine fishes              twinspot damselfish actinopterygii
## 65  marine fishes              whitetail dascyllus actinopterygii
## 66  marine fishes                     wards damsel actinopterygii
## 67  marine fishes               australian gregory actinopterygii
## 68  marine fishes               bicolor damselfish actinopterygii
## 69  marine fishes                 cocoa damselfish actinopterygii
## 70  marine fishes             steephead parrotfish actinopterygii
## 71  marine fishes               striped parrotfish actinopterygii
## 72  marine fishes             rivulated parrotfish actinopterygii
## 73  marine fishes               redband parrotfish actinopterygii
## 74  marine fishes               redtail parrotfish actinopterygii
## 75  marine fishes                redfin parrotfish actinopterygii
## 76  marine fishes             stoplight parrotfish actinopterygii
## 77  marine fishes                     peacock hind actinopterygii
## 78  marine fishes                          graysby actinopterygii
## 79  marine fishes                   yellowfin hind actinopterygii
## 80  marine fishes                       coral hind actinopterygii
## 81  marine fishes                         red hind actinopterygii
## 82  marine fishes                    dusky grouper actinopterygii
## 83  marine fishes                      red grouper actinopterygii
## 84  marine fishes                   nassau grouper actinopterygii
## 85  marine fishes                   greasy grouper actinopterygii
## 86  marine fishes                  redbanded perch actinopterygii
## 87  marine fishes             half-banded seaperch actinopterygii
## 88  marine fishes                    black grouper actinopterygii
## 89  marine fishes                        kelp bass actinopterygii
## 90  marine fishes                 barred sand bass actinopterygii
## 91  marine fishes                    coral grouper actinopterygii
## 92  marine fishes                      coral trout actinopterygii
## 93  marine fishes                           comber actinopterygii
## 94  marine fishes                   painted comber actinopterygii
## 95  marine fishes                           salema actinopterygii
## 96  marine fishes                gilthead seabream actinopterygii
## 97   river fishes                  cutthroat trout actinopterygii
## 98   river fishes                       gila trout actinopterygii
## 99   river fishes                    rainbow trout actinopterygii
## 100  river fishes                  atlantic salmon actinopterygii
## 101   lake fishes                      brown trout actinopterygii
## 102  river fishes                  mottled sculpin actinopterygii
## 103  river fishes                   banded sculpin actinopterygii
## 104  river fishes                          sculpin actinopterygii
## 105 marine fishes                  copper rockfish actinopterygii
## 106 marine fishes          japanese black rockfish actinopterygii
## 107 marine fishes               quillback rockfish actinopterygii
## 108 marine fishes                   black rockfish actinopterygii
## 109 marine fishes                    blue rockfish actinopterygii
## 110   lake fishes                  yellow bullhead actinopterygii
## 111 marine fishes            long-snouted seahorse actinopterygii
## 112 marine fishes                    worm pipefish actinopterygii
## 113 marine fishes        atlantic sharpnose puffer actinopterygii
## 114         birds                     golden eagle           aves
## 115         birds                   common buzzard           aves
## 116         birds           short-toed snake eagle           aves
## 117         birds                  Bonelli's eagle           aves
## 118         birds                     booted eagle           aves
## 119         birds                 Egyptian vulture           aves
## 120         birds                          gadwall           aves
## 121         birds              northern brown kiwi           aves
## 122         birds                European nightjar           aves
## 123         birds                    oystercatcher           aves
## 124         birds                        inca dove           aves
## 125         birds               common wood pigeon           aves
## 126         birds             European turtle dove           aves
## 127         birds                  European roller           aves
## 128         birds                           hoopoe           aves
## 129         birds             great spotted cuckoo           aves
## 130         birds                    common cuckoo           aves
## 131         birds               greater roadrunner           aves
## 132         birds             banded ground-cuckoo           aves
## 133         birds                    Cooper's hawk           aves
## 134         birds                 Northern goshawk           aves
## 135         birds             Eurasian sparrowhawk           aves
## 136         birds               sharp-shinned hawk           aves
## 137         birds                  red-tailed hawk           aves
## 138         birds              red-shouldered hawk           aves
## 139         birds                  Swainson's hawk           aves
## 140         birds                      hen harrier           aves
## 141         birds                Montagu's harrier           aves
## 142         birds                         red kite           aves
## 143         birds                         caracara           aves
## 144         birds            red-throated caracara           aves
## 145         birds                    lanner falcon           aves
## 146         birds                   prairie falcon           aves
## 147         birds                 peregrine falcon           aves
## 148         birds                 American kestrel           aves
## 149         birds                 European kestrel           aves
## 150         birds                     hazel grouse           aves
## 151         birds                      sage grouse           aves
## 152         birds                     dusky grouse           aves
## 153         birds                 willow ptarmigan           aves
## 154         birds                   grey partridge           aves
## 155         birds                     black grouse           aves
## 156         birds             western capercaillie           aves
## 157         birds          greater prairie-chicken           aves
## 158         birds                  brown wood rail           aves
## 159         birds                        corncrake           aves
## 160         birds                        king rail           aves
## 161         birds                melodious warbler           aves
## 162         birds                  long-tailed tit           aves
## 163         birds                         woodlark           aves
## 164         birds         red-throated ant tanager           aves
## 165         birds          red-crowned ant tanager           aves
## 166         birds             Eurasian treecreeper           aves
## 167         birds         streaked fantail warbler           aves
## 168         birds                     common raven           aves
## 169         birds               spotted nutcracker           aves
## 170         birds             Peruvian plantcutter           aves
## 171         birds              grasshopper sparrow           aves
## 172         birds                   indigo bunting           aves
## 173         birds                   Abert's towhee           aves
## 174         birds                    canyon towhee           aves
## 175         birds            American tree sparrow           aves
## 176         birds                 chipping sparrow           aves
## 177         birds                    common linnet           aves
## 178         birds                 common chaffinch           aves
## 179         birds                   European serin           aves
## 180         birds               eastern meadowlark           aves
## 181         birds               western meadowlard           aves
## 182         birds             yellow-breasted chat           aves
## 183         birds                red-backed shrike           aves
## 184         birds                loggerhead shrike           aves
## 185         birds               lesser grey shrike           aves
## 186         birds                  woodchat shrike           aves
## 187         birds             northern mockingbird           aves
## 188         birds                    white wagtail           aves
## 189         birds           western yellow wagtail           aves
## 190         birds               spotted flycatcher           aves
## 191         birds                northern wheatear           aves
## 192         birds                  common redstart           aves
## 193         birds                         whinchat           aves
## 194         birds           black-capped chickadee           aves
## 195         birds               Carolina chickadee           aves
## 196         birds                     Oak titmouse           aves
## 197         birds                        marsh tit           aves
## 198         birds                 mourning warbler           aves
## 199         birds              common yellowthroat           aves
## 200         birds             prothonotary warbler           aves
## 201         birds                         ovenbird           aves
## 202         birds             Blackburnian warbler           aves
## 203         birds               Kirtland's warbler           aves
## 204         birds                 magnolia warbler           aves
## 205         birds           chestnut-sided warbler           aves
## 206         birds          American yellow warbler           aves
## 207         birds                American redstart           aves
## 208         birds     black-throated green warbler           aves
## 209         birds                   Canada warbler           aves
## 210         birds        Western Bonelli's warbler           aves
## 211         birds           tooth-billed bowerbird           aves
## 212         birds                 common firecrest           aves
## 213         birds                        goldcrest           aves
## 214         birds                European nuthatch           aves
## 215         birds                          wrentit           aves
## 216         birds                Marmora's warbler           aves
## 217         birds                 Dartford warbler           aves
## 218         birds                   Berwick's wren           aves
## 219         birds                    Carolina wren           aves
## 220         birds                       house wren           aves
## 221         birds                    Eurasian wren           aves
## 222         birds                 eastern bluebird           aves
## 223         birds               eastern wood pewee           aves
## 224         birds                 least flycatcher           aves
## 225         birds         American gray flycatcher           aves
## 226         birds                 eastern kingbird           aves
## 227         birds               black-capped vireo           aves
## 228         birds                     Bell's vireo           aves
## 229         birds                 white-eyed vireo           aves
## 230         birds                   red-eyed vireo           aves
## 231         birds                    great bittern           aves
## 232         birds                    least bittern           aves
## 233         birds                 black woodpecker           aves
## 234         birds                 Eurasian wryneck           aves
## 235         birds          white-backed woodpecker           aves
## 236         birds       middle spotted woodpeckers           aves
## 237         birds   Eurasian three-toed woodpecker           aves
## 238         birds           grey-headed woodpecker           aves
## 239         birds        European green woodpecker           aves
## 240         birds                           kakapo           aves
## 241         birds                     greater rhea           aves
## 242         birds                      lesser rhea           aves
## 243         birds                       boreal owl           aves
## 244         birds                   long-eared owl           aves
## 245         birds                       little owl           aves
## 246         birds               Eurasian eagle-owl           aves
## 247         birds                 great horned owl           aves
## 248         birds               Eurasian pygmy owl           aves
## 249         birds                        snowy owl           aves
## 250         birds                        tawny owl           aves
## 251         birds                         barn owl           aves
## 252         birds                          ostrich           aves
## 253         birds                   ornate tinamou           aves
## 254       mammals                giant golden mole       mammalia
## 255       mammals              Grant's golden mole       mammalia
## 256       mammals                        pronghorn       mammalia
## 257       mammals                           impala       mammalia
## 258       mammals                       hartebeest       mammalia
## 259       mammals                    barbary sheep       mammalia
## 260       mammals                   American bison       mammalia
## 261       mammals                   European bison       mammalia
## 262       mammals                             goat       mammalia
## 263       mammals                     Spanish ibex       mammalia
## 264       mammals                   Peter's dukier       mammalia
## 265       mammals                       bay dikier       mammalia
## 266       mammals                 mountain gazelle       mammalia
## 267       mammals             G\xfcnther's dik-dik       mammalia
## 268       mammals                    mountain goat       mammalia
## 269       mammals                           argali       mammalia
## 270       mammals                    bighorn sheep       mammalia
## 271       mammals                         steenbok       mammalia
## 272       mammals                          chamois       mammalia
## 273       mammals                     common eland       mammalia
## 274       mammals                         bushbuck       mammalia
## 275       mammals                     greater kudu       mammalia
## 276       mammals                            moose       mammalia
## 277       mammals                           chital       mammalia
## 278       mammals                         roe deer       mammalia
## 279       mammals                         red deer       mammalia
## 280       mammals                        sika deer       mammalia
## 281       mammals                      fallow deer       mammalia
## 282       mammals                 Reeves's muntjac       mammalia
## 283       mammals                        mule deer       mammalia
## 284       mammals                white-tailed deer       mammalia
## 285       mammals                      pampas deer       mammalia
## 286       mammals                             pudu       mammalia
## 287       mammals                         reindeer       mammalia
## 288       mammals                          giraffe       mammalia
## 289       mammals                            okapi       mammalia
## 290       mammals                   desert warthog       mammalia
## 291       mammals                  Chacoan peccary       mammalia
## 292       mammals                 collared peccary       mammalia
## 293       mammals             white-lipped peccary       mammalia
## 294       mammals                 water chevrotain       mammalia
## 295       mammals                        red panda       mammalia
## 296       mammals                       arctic fox       mammalia
## 297       mammals                   Ethiopian wolf       mammalia
## 298       mammals                           culpeo       mammalia
## 299       mammals          South American gray fox       mammalia
## 300       mammals                          kit fox       mammalia
## 301       mammals                     Ruppel's fox       mammalia
## 302       mammals                        swift fox       mammalia
## 303       mammals   thick-tailed three-toed jerboa       mammalia
## 304       mammals                            fossa       mammalia
## 305       mammals                          cheetah       mammalia
## 306       mammals                          caracal       mammalia
## 307       mammals                              cat       mammalia
## 308       mammals                          wildcat       mammalia
## 309       mammals                       jaguarundi       mammalia
## 310       mammals                           ocelot       mammalia
## 311       mammals                           margay       mammalia
## 312       mammals                           serval       mammalia
## 313       mammals                      Canada lynx       mammalia
## 314       mammals                    Eurasian lynx       mammalia
## 315       mammals                     Iberian lynx       mammalia
## 316       mammals                           bobcat       mammalia
## 317       mammals                   Geoffroy's cat       mammalia
## 318       mammals                           jaguar       mammalia
## 319       mammals                          leopard       mammalia
## 320       mammals                            tiger       mammalia
## 321       mammals                      leopard cat       mammalia
## 322       mammals                           cougar       mammalia
## 323       mammals                     snow leopard       mammalia
## 324       mammals                   marsh mongoose       mammalia
## 325       mammals                  yellow mongoose       mammalia
## 326       mammals            common dwarf mongoose       mammalia
## 327       mammals                Egyptian mongoose       mammalia
## 328       mammals            white-tailed mongoose       mammalia
## 329       mammals                         aardwolf       mammalia
## 330       mammals                            tayra       mammalia
## 331       mammals                   greater grison       mammalia
## 332       mammals                        wolverine       mammalia
## 333       mammals                  American marten       mammalia
## 334       mammals                     beech marten       mammalia
## 335       mammals             European pine marten       mammalia
## 336       mammals                           fisher       mammalia
## 337       mammals                            stoat       mammalia
## 338       mammals               long-tailed weasel       mammalia
## 339       mammals                           ferret       mammalia
## 340       mammals                    European mink       mammalia
## 341       mammals              black-footed ferret       mammalia
## 342       mammals                     least weasel       mammalia
## 343       mammals                  Siberian weasel       mammalia
## 344       mammals                  American badger       mammalia
## 345       mammals                         kinkajou       mammalia
## 346       mammals                      giant panda       mammalia
## 347       mammals                       sloth bear       mammalia
## 348       mammals                     common genet       mammalia
## 349       mammals                       cape genet       mammalia
## 350       mammals               large indian civet       mammalia
## 351       mammals                    Western quoll       mammalia
## 352       mammals                      tiger quoll       mammalia
## 353       mammals             white-footed dunnart       mammalia
## 354       mammals                 brown antechinus       mammalia
## 355       mammals   Northern three-striped opossum       mammalia
## 356       mammals       elegant fat-tailed opossum       mammalia
## 357       mammals         Lumholtz's tree-kangaroo       mammalia
## 358       mammals              antilopine kangaroo       mammalia
## 359       mammals            black-striped wallaby       mammalia
## 360       mammals            Western grey kangaroo       mammalia
## 361       mammals            Eastern grey kangaroo       mammalia
## 362       mammals                  common wallaroo       mammalia
## 363       mammals               red-necked wallaby       mammalia
## 364       mammals                     red kangaroo       mammalia
## 365       mammals              allied rock-wallaby       mammalia
## 366       mammals                  eastern bettong       mammalia
## 367       mammals              long-footed potoroo       mammalia
## 368       mammals                   greater glider       mammalia
## 369       mammals        bridled nail-tail wallaby       mammalia
## 370       mammals             red-legged pademelon       mammalia
## 371       mammals             red-necked pademelon       mammalia
## 372       mammals                    swamp wallaby       mammalia
## 373       mammals          common brushtail possum       mammalia
## 374       mammals      northern hairy-nosed wombat       mammalia
## 375       mammals                    common wombat       mammalia
## 376       mammals                European hedgehog       mammalia
## 377       mammals              long-eared hedgehog       mammalia
## 378       mammals                     pygmy rabbit       mammalia
## 379       mammals                    snowshoe hare       mammalia
## 380       mammals                      Arctic hare       mammalia
## 381       mammals          black-tailed jackrabbit       mammalia
## 382       mammals                        cape hare       mammalia
## 383       mammals                    European hare       mammalia
## 384       mammals                      Indian hare       mammalia
## 385       mammals                    mountain hare       mammalia
## 386       mammals                  European rabbit       mammalia
## 387       mammals                     swamp rabbit       mammalia
## 388       mammals               eastern cottontail       mammalia
## 389       mammals                     marsh rabbit       mammalia
## 390       mammals                     plateau pika       mammalia
## 391       mammals                    American pika       mammalia
## 392       mammals            rufous elephant shrew       mammalia
## 393       mammals         four-toed elephant shrew       mammalia
## 394       mammals     golden-rumped elephant shrew       mammalia
## 395       mammals            east African mole rat       mammalia
## 396       mammals                 golden bandicoot       mammalia
## 397       mammals         Southern brown bandicoot       mammalia
## 398       mammals                            horse       mammalia
## 399       mammals                 white rhinoceros       mammalia
## 400       mammals                 black rhinoceros       mammalia
## 401       mammals                      maned sloth       mammalia
## 402       mammals                   Asian elephant       mammalia
## 403       mammals            African bush elephant       mammalia
## 404       mammals       southern grasshopper mouse       mammalia
## 405       mammals                  mountain beaver       mammalia
## 406       mammals               cape dune mole rat       mammalia
## 407       mammals              Damaraland mole rat       mammalia
## 408       mammals                  common mole rat       mammalia
## 409       mammals                    cape mole rat       mammalia
## 410       mammals                 silvery mole rat       mammalia
## 411       mammals                   naked mole rat       mammalia
## 412       mammals                  Patagonian mara       mammalia
## 413       mammals                  plains viscacha       mammalia
## 414       mammals          western red-backed vole       mammalia
## 415       mammals            large-headed rice rat       mammalia
## 416       mammals           Siberian brown lemming       mammalia
## 417       mammals                       field vole       mammalia
## 418       mammals                  California vole       mammalia
## 419       mammals                     montane vole       mammalia
## 420       mammals                     prairie vole       mammalia
## 421       mammals                      meadow vole       mammalia
## 422       mammals                    woodland vole       mammalia
## 423       mammals                       water vole       mammalia
## 424       mammals                     wood lemming       mammalia
## 425       mammals             bushy-tailed woodrat       mammalia
## 426       mammals             dusky-footed woodrat       mammalia
## 427       mammals                   desert woodrat       mammalia
## 428       mammals          Southern plains woodrat       mammalia
## 429       mammals                          muskrat       mammalia
## 430       mammals                     cotton mouse       mammalia
## 431       mammals         salt marsh harvest mouse       mammalia
## 432       mammals             southern bog lemming       mammalia
## 433       mammals          dwarf fat-tailed jerboa       mammalia
## 434       mammals               Cuvier's spiny rat       mammalia
## 435       mammals                 Tome's spiny rat       mammalia
## 436       mammals              Brazilian porcupine       mammalia
## 437       mammals         North American porcupine       mammalia
## 438       mammals            Botta's pocket gopher       mammalia
## 439       mammals              spectacled dormouse       mammalia
## 440       mammals                   hazel dormouse       mammalia
## 441       mammals               giant kangaroo rat       mammalia
## 442       mammals           Merriam's kangaroo rat       mammalia
## 443       mammals               Ord's kangaroo rat       mammalia
## 444       mammals       banner-tailed kangaroo rat       mammalia
## 445       mammals           Stephen's kangaroo rat       mammalia
## 446       mammals                   cape porcupine       mammalia
## 447       mammals         Indian crested porcupine       mammalia
## 448       mammals   African brush-tailed porcupine       mammalia
## 449       mammals              yellow-necked mouse       mammalia
## 450       mammals                       wood mouse       mammalia
## 451       mammals               Indian desert jird       mammalia
## 452       mammals              broad-toothed mouse       mammalia
## 453       mammals               Malagasy giant rat       mammalia
## 454       mammals         White-bellied\xa0nesomys       mammalia
## 455       mammals                     island mouse       mammalia
## 456       mammals                           coruro       mammalia
## 457       mammals                Siberian chipmunk       mammalia
## 458       mammals           Northern parl squirrel       mammalia
## 459       mammals         Northern flying squirrel       mammalia
## 460       mammals         Southern flying squirrel       mammalia
## 461       mammals            yellow-bellied marmot       mammalia
## 462       mammals                        groundhog       mammalia
## 463       mammals                red bush squirrel       mammalia
## 464       mammals                 Abert's squirrel       mammalia
## 465       mammals            eastern gray squirrel       mammalia
## 466       mammals                Japanese squirrel       mammalia
## 467       mammals                     fox squirrel       mammalia
## 468       mammals                     red squirrel       mammalia
## 469       mammals       California ground squirrel       mammalia
## 470       mammals        Columbian ground squirrel       mammalia
## 471       mammals       Franklin's ground squirrel       mammalia
## 472       mammals           arctic ground squirrel       mammalia
## 473       mammals          spotted ground squirrel       mammalia
## 474       mammals   thirteen-lined ground squirrel       mammalia
## 475       mammals                    rock squirrel       mammalia
## 476       mammals             yellow-pine chipmunk       mammalia
## 477       mammals                   least chipmunk       mammalia
## 478       mammals                Colorado chipmunk       mammalia
## 479       mammals                   Uinta chipmunk       mammalia
## 480       mammals            American red squirrel       mammalia
## 481       mammals          striped ground squirrel       mammalia
## 482       mammals       greater white-footed shrew       mammalia
## 483       mammals                     arctic shrew       mammalia
## 484       mammals                   cinereus shrew       mammalia
## 485       mammals                    crowned shrew       mammalia
## 486       mammals                    slender shrew       mammalia
## 487       mammals                long-clawed shrew       mammalia
## 488       mammals                  star-nosed mole       mammalia
## 489       mammals                     eastern mole       mammalia
## 490       mammals                    European mole       mammalia
## 491       mammals                       Roman mole       mammalia
## 492       lizards                spiny tail lizard       reptilia
## 493        snakes               western worm snake       reptilia
## 494        snakes               eastern worm snake       reptilia
## 495        snakes                            racer       reptilia
## 496        snakes             yellow bellied racer       reptilia
## 497        snakes                   ringneck snake       reptilia
## 498        snakes             eastern indigo snake       reptilia
## 499        snakes            great plains ratsnake       reptilia
## 500        snakes                 western ratsnake       reptilia
## 501        snakes                    hognose snake       reptilia
## 502        snakes               European whipsnake       reptilia
## 503        snakes                Eastern kingsnake       reptilia
## 504        snakes                        milksnake       reptilia
## 505        snakes                        coachwhip       reptilia
## 506        snakes                      grass snake       reptilia
## 507        snakes           copperbelly watersnake       reptilia
## 508        snakes              Northern watersnake       reptilia
## 509        snakes               redbacked ratsnake       reptilia
## 510        snakes                     gopher snake       reptilia
## 511        snakes                       pine snake       reptilia
## 512        snakes             butlers garter snake       reptilia
## 513        snakes              giant garter snakes       reptilia
## 514        snakes                Aesculapian snake       reptilia
## 515        snakes                broadheaded snake       reptilia
## 516        snakes                      tiger snake       reptilia
## 517        snakes                       blacksnake       reptilia
## 518       lizards            Galapagos land iguana       reptilia
## 519       lizards           Bahamian Andros iguana       reptilia
## 520       lizards                      blue iguana       reptilia
## 521       lizards            Anegada ground iguana       reptilia
## 522       lizards          Angel island chuckwalla       reptilia
## 523       lizards                common chuckwalla       reptilia
## 524       lizards                    desert iguana       reptilia
## 525       lizards                  Tenerife lizard       reptilia
## 526       lizards             High Mountain Lizard       reptilia
## 527        snakes       southwestern carpet python       reptilia
## 528       lizards                      land mullet       reptilia
## 529        snakes                       copperhead       reptilia
## 530        snakes                      cottonmouth       reptilia
## 531        snakes              namaqua dwarf adder       reptilia
## 532        snakes                     fer-de-lance       reptilia
## 533        snakes              western diamondback       reptilia
## 534        snakes                       sidewinder       reptilia
## 535        snakes               timber rattlesnake       reptilia
## 536        snakes          blacktailed rattlesnake       reptilia
## 537        snakes         midget faded rattlesnake       reptilia
## 538        snakes         twin-spotted rattlesnake       reptilia
## 539        snakes               Mojave rattlesnake       reptilia
## 540        snakes                tiger rattlesnake       reptilia
## 541        snakes                chinese pit viper       reptilia
## 542        snakes                   Armenian viper       reptilia
## 543        snakes                  snubnosed viper       reptilia
## 544       turtles       Eastern long-necked turtle       reptilia
## 545       turtles      Dalh's toad-headed tortoise       reptilia
## 546       turtles           common snapping turtle       reptilia
## 547       turtles           midland painted turtle       reptilia
## 548       turtles                   chicken turtle       reptilia
## 549       turtles                Blanding's turtle       reptilia
## 550       turtles             European pond turtle       reptilia
## 551       turtles       yellow-blotched map turtle       reptilia
## 552       turtles                ornate box turtle       reptilia
## 553       turtles              Spanish pond turtle       reptilia
## 554       turtles               Eastern mud turtle       reptilia
## 555       turtles        stripe-necked musk turtle       reptilia
## 556       turtles                  stinkpot turtle       reptilia
## 557     tortoises              red-footed tortoise       reptilia
## 558     tortoises                  desert tortoise       reptilia
## 559     tortoises                  gopher tortoise       reptilia
## 560     tortoises              travancore tortoise       reptilia
## 561     tortoises    Speke's hinge-backed tortoise       reptilia
## 562     tortoises               impressed tortoise       reptilia
## 563     tortoises        bushmanland tent tortoise       reptilia
## 564     tortoises                 leopard tortoise       reptilia
## 565     tortoises            spur-thighed tortoise       reptilia
## 566     tortoises           mediterranean tortoise       reptilia
## 567     tortoises          Russian steppe tortoise       reptilia
## 568     tortoises                Egyptian tortoise       reptilia
## 569       turtles   Eastern spiny softshell turtle       reptilia
##                     order            family           genus
## 1          anguilliformes       anguillidae        anguilla
## 2           cypriniformes      catostomidae       moxostoma
## 3           cypriniformes        cyprinidae      campostoma
## 4           cypriniformes        cyprinidae     clinostomus
## 5           cypriniformes        cyprinidae     rhinichthys
## 6             esociformes          esocidae            esox
## 7              gadiformes           gadidae      pollachius
## 8              gadiformes           gadidae      pollachius
## 9             perciformes      acanthuridae      acanthurus
## 10            perciformes      acanthuridae            naso
## 11            perciformes      acanthuridae            naso
## 12            perciformes         blennidae   ophioblennius
## 13            perciformes        carangidae          caranx
## 14            perciformes     centrarchidae     ambloplites
## 15            perciformes     centrarchidae         lepomis
## 16            perciformes     centrarchidae         lepomis
## 17            perciformes     centrarchidae         lepomis
## 18            perciformes     centrarchidae     micropterus
## 19            perciformes     centrarchidae     micropterus
## 20            perciformes     centrarchidae         pomoxis
## 21            perciformes    chaetodontidae       chaetodon
## 22            perciformes    chaetodontidae       chaetodon
## 23            perciformes    chaetodontidae       chaetodon
## 24            perciformes    chaetodontidae       chaetodon
## 25            perciformes    chaetodontidae       chaetodon
## 26            perciformes  cheilodactylidae  cheilodactylus
## 27            perciformes       cirrhitidae  amblycirrhitus
## 28            perciformes       cirrhitidae  cirrhitichthys
## 29            perciformes          cottidae scorpaenichthys
## 30            perciformes          gobiidae   amblyeleotris
## 31            perciformes          gobiidae      lythrypnus
## 32            perciformes          gobiidae       priolepis
## 33            perciformes          gobiidae    rhinogobiops
## 34            perciformes          gobiidae    valenciennea
## 35            perciformes        kyphosidae        kyphosus
## 36            perciformes          labridae        bodianus
## 37            perciformes          labridae        chelinus
## 38            perciformes          labridae           coris
## 39            perciformes          labridae     halichoeres
## 40            perciformes          labridae     halichoeres
## 41            perciformes          labridae     halichoeres
## 42            perciformes          labridae     halichoeres
## 43            perciformes          labridae       labroides
## 44            perciformes          labridae          labrus
## 45            perciformes          labridae   opthalmolepis
## 46            perciformes          labridae   semicossyphus
## 47            perciformes          labridae   tautogolabrus
## 48            perciformes          labridae      thalassoma
## 49            perciformes          labridae      thalassoma
## 50            perciformes       lethrinidae       lethrinus
## 51            perciformes        lutjanidae        lutjanus
## 52            perciformes        lutjanidae        lutjanus
## 53            perciformes        lutjanidae        lutjanus
## 54            perciformes        lutjanidae        lutjanus
## 55            perciformes        lutjanidae         ocyurus
## 56            perciformes     malacanthidae    caulolatilus
## 57            perciformes         moronidae   dicentrarchus
## 58            perciformes          mullidae  mulloidichthys
## 59            perciformes          mullidae      parupeneus
## 60            perciformes          percidae           perca
## 61            perciformes     pomacanthidae       abudefduf
## 62            perciformes     pomacanthidae      centropyge
## 63            perciformes     pomacentridae         chromis
## 64            perciformes     pomacentridae     chrysiptera
## 65            perciformes     pomacentridae       dascyllus
## 66            perciformes     pomacentridae     pomacentrus
## 67            perciformes     pomacentridae       stegastes
## 68            perciformes     pomacentridae       stegastes
## 69            perciformes     pomacentridae       stegastes
## 70            perciformes          scaridae       chlorurus
## 71            perciformes          scaridae          scarus
## 72            perciformes          scaridae          scarus
## 73            perciformes          scaridae       sparisoma
## 74            perciformes          scaridae       sparisoma
## 75            perciformes          scaridae       sparisoma
## 76            perciformes          scaridae       sparisoma
## 77            perciformes        serranidae   cephalopholis
## 78            perciformes        serranidae   cephalopholis
## 79            perciformes        serranidae   cephalopholis
## 80            perciformes        serranidae   cephalopholis
## 81            perciformes        serranidae     epinephelus
## 82            perciformes        serranidae     epinephelus
## 83            perciformes        serranidae     epinephelus
## 84            perciformes        serranidae     epinephelus
## 85            perciformes        serranidae     epinephelus
## 86            perciformes        serranidae  hypoplectrodes
## 87            perciformes        serranidae  hypoplectrodes
## 88            perciformes        serranidae    mycteroperca
## 89            perciformes        serranidae      paralabrax
## 90            perciformes        serranidae      paralabrax
## 91            perciformes        serranidae    plectropomus
## 92            perciformes        serranidae    plectropomus
## 93            perciformes        serranidae        serranus
## 94            perciformes        serranidae        serranus
## 95            perciformes          sparidae           sarpa
## 96            perciformes          sparidae          sparus
## 97          salmoniformes        salmonidae    oncorhynchus
## 98          salmoniformes        salmonidae    oncorhynchus
## 99          salmoniformes        salmonidae    oncorhynchus
## 100         salmoniformes        salmonidae           salmo
## 101         salmoniformes        salmonidae           salmo
## 102       scorpaeniformes          cottidae          cottus
## 103       scorpaeniformes          cottidae          cottus
## 104       scorpaeniformes          cottidae          cottus
## 105       scorpaeniformes        sebastidae        sebastes
## 106       scorpaeniformes        sebastidae        sebastes
## 107       scorpaeniformes        sebastidae        sebastes
## 108       scorpaeniformes        sebastidae        sebastes
## 109       scorpaeniformes        sebastidae        sebastes
## 110          siluriformes       ictaluridae       ictalurus
## 111       syngnathiformes      syngnathidae     hippocampus
## 112       syngnathiformes      syngnathidae        nerophis
## 113 tetraodontiformes\xa0    tetraodontidae    canthigaster
## 114       accipitriformes      accipitridae          aquila
## 115       accipitriformes      accipitridae           buteo
## 116       accipitriformes      accipitridae       circaetus
## 117       accipitriformes      accipitridae      hieraaetus
## 118       accipitriformes      accipitridae      hieraaetus
## 119       accipitriformes      accipitridae        neophron
## 120          anseriformes          anatidae            anas
## 121        apterygiformes       apterygidae         apteryx
## 122      caprimulgiformes     caprimulgidae     caprimulgus
## 123       charadriiformes    haematopodidae      haematopus
## 124         columbidormes        columbidae     scardafella
## 125         columbiformes        columbidae         columba
## 126         columbiformes        columbidae    streptopelia
## 127         coraciiformes        coraciidae        coracias
## 128         coraciiformes          upupidae           upupa
## 129          cuculiformes         cuculidae        clamator
## 130          cuculiformes         cuculidae         cuculus
## 131          cuculiformes         cuculidae       geococcyx
## 132          cuculiformes         cuculidae     neopmorphus
## 133         falconiformes      accipitridae       accipiter
## 134         falconiformes      accipitridae       accipiter
## 135         falconiformes      accipitridae       accipiter
## 136         falconiformes      accipitridae       accipiter
## 137         falconiformes      accipitridae           buteo
## 138         falconiformes      accipitridae           buteo
## 139         falconiformes      accipitridae           buteo
## 140         falconiformes      accipitridae          circus
## 141         falconiformes      accipitridae          circus
## 142         falconiformes      accipitridae          milvus
## 143         falconiformes        falconidae        caracara
## 144         falconiformes        falconidae        daptrius
## 145         falconiformes        falconidae           falco
## 146         falconiformes        falconidae           falco
## 147         falconiformes        falconidae           falco
## 148         falconiformes        falconidae           falco
## 149         falconiformes        falconidae           falco
## 150           galliformes       phasianidae          bonasa
## 151           galliformes       phasianidae    centrocercus
## 152           galliformes       phasianidae     dendragapus
## 153           galliformes       phasianidae         lagopus
## 154           galliformes       phasianidae          perdix
## 155           galliformes       phasianidae          tetrao
## 156           galliformes       phasianidae          tetrao
## 157           galliformes       phasianidae     typmanuchus
## 158            gruiformes          rallidae        aramides
## 159            gruiformes          rallidae            crex
## 160            gruiformes          rallidae          rallus
## 161         passeriformes    acrocephalisae       hippolais
## 162         passeriformes      aegithalidae      aegithalos
## 163         passeriformes         alaudidae         lululla
## 164         passeriformes      cardinalidae           habia
## 165         passeriformes      cardinalidae           habia
## 166         passeriformes         certhidae         certhia
## 167         passeriformes      cisticolidae       cisticola
## 168         passeriformes          corvidae          corvus
## 169         passeriformes          corvidae       nucifraga
## 170         passeriformes        cotingidae       phytotoma
## 171         passeriformes       emberizidae      ammodramus
## 172         passeriformes       emberizidae       passerina
## 173         passeriformes       emberizidae          pipilo
## 174         passeriformes       emberizidae          pipilo
## 175         passeriformes       emberizidae        spizella
## 176         passeriformes       emberizidae        spizella
## 177         passeriformes      fringillidae       carduelis
## 178         passeriformes      fringillidae       fringilla
## 179         passeriformes      fringillidae         serinus
## 180         passeriformes         icteridae       sturnella
## 181         passeriformes         icteridae       sturnella
## 182         passeriformes          incertae         icteria
## 183         passeriformes          laniidae         laniuis
## 184         passeriformes          laniidae         laniuis
## 185         passeriformes          laniidae          lanius
## 186         passeriformes          laniidae          lanius
## 187         passeriformes           mimidae           mimus
## 188         passeriformes      motacillidae       motacilla
## 189         passeriformes      motacillidae       motacilla
## 190         passeriformes      muscicapidae       muscicapa
## 191         passeriformes      muscicapidae        oenanthe
## 192         passeriformes      muscicapidae     phoenicurus
## 193         passeriformes      muscicapidae        saxicola
## 194         passeriformes           paridae           parus
## 195         passeriformes           paridae           parus
## 196         passeriformes           paridae           parus
## 197         passeriformes           paridae           parus
## 198         passeriformes         parulidae  geothlypis\xa0
## 199         passeriformes         parulidae      geothylpis
## 200         passeriformes         parulidae    protonotaria
## 201         passeriformes         parulidae         seiurus
## 202         passeriformes         parulidae       setophaga
## 203         passeriformes         parulidae       setophaga
## 204         passeriformes         parulidae       setophaga
## 205         passeriformes         parulidae       setophaga
## 206         passeriformes         parulidae       setophaga
## 207         passeriformes         parulidae       setophaga
## 208         passeriformes         parulidae       setophaga
## 209         passeriformes         parulidae        wilsonia
## 210         passeriformes    phylloscopidae    phylloscopus
## 211         passeriformes ptilonorhynchidae    scenopoeetes
## 212         passeriformes         regulidae         regulus
## 213         passeriformes         regulidae         regulus
## 214         passeriformes         stittidae           sitta
## 215         passeriformes          sylvidae         chamaea
## 216         passeriformes         sylviidae          sylvia
## 217         passeriformes         sylviidae          sylvia
## 218         passeriformes     troglodytidae      thryomanes
## 219         passeriformes     troglodytidae     thryothorus
## 220         passeriformes     troglodytidae     troglodytes
## 221         passeriformes     troglodytidae     troglodytes
## 222         passeriformes          turdidae          sialia
## 223         passeriformes        tyrannidae        contopus
## 224         passeriformes        tyrannidae       empidonax
## 225         passeriformes        tyrannidae       empidonax
## 226         passeriformes        tyrannidae        tyrannus
## 227         passeriformes        vireonidae           vireo
## 228         passeriformes        vireonidae           vireo
## 229         passeriformes        vireonidae           vireo
## 230         passeriformes        vireonidae           vireo
## 231        pelecaniformes          ardeidae        botaurus
## 232        pelecaniformes          ardeidae      ixobrychus
## 233            piciformes           picidae       dryocopus
## 234            piciformes           picidae            jynx
## 235            piciformes           picidae        picoides
## 236            piciformes           picidae        picoides
## 237            piciformes           picidae        picoides
## 238            piciformes           picidae           picus
## 239            piciformes           picidae           picus
## 240        psittaciformes       strigopidae        strigops
## 241            rheiformes           rheidae            rhea
## 242            rheiformes           rheidae            rhea
## 243          strigiformes         strigidae        aegolius
## 244          strigiformes         strigidae            asio
## 245          strigiformes         strigidae          athene
## 246          strigiformes         strigidae            bubo
## 247          strigiformes         strigidae            bubo
## 248          strigiformes         strigidae      glaucidium
## 249          strigiformes         strigidae          nyctea
## 250          strigiformes         strigidae           strix
## 251          strigiformes         tytonidae            tyto
## 252      struthioniformes     struthionidae        struthio
## 253          tinamiformes         tinamidae     nothoprocta
## 254          afrosoricida   chrysochloridae    chrysospalax
## 255          afrosoricida   chrysochloridae      eremitalpa
## 256          artiodactyla    antilocapridae     antilocapra
## 257          artiodactyla           bovidae       aepyceros
## 258          artiodactyla           bovidae      alcelaphus
## 259          artiodactyla           bovidae      ammotragus
## 260          artiodactyla           bovidae           bison
## 261          artiodactyla           bovidae           bison
## 262          artiodactyla           bovidae           capra
## 263          artiodactyla           bovidae           capra
## 264          artiodactyla           bovidae     cephalophus
## 265          artiodactyla           bovidae     cephalophus
## 266          artiodactyla           bovidae         gazella
## 267          artiodactyla           bovidae         madoqua
## 268          artiodactyla           bovidae        oreamnos
## 269          artiodactyla           bovidae            ovis
## 270          artiodactyla           bovidae            ovis
## 271          artiodactyla           bovidae      raphicerus
## 272          artiodactyla           bovidae       rupicapra
## 273          artiodactyla           bovidae     taurotragus
## 274          artiodactyla           bovidae     tragelaphus
## 275          artiodactyla           bovidae     tragelaphus
## 276          artiodactyla          cervidae           alces
## 277          artiodactyla          cervidae            axis
## 278          artiodactyla          cervidae       capreolus
## 279          artiodactyla          cervidae          cervus
## 280          artiodactyla          cervidae          cervus
## 281          artiodactyla          cervidae            dama
## 282          artiodactyla          cervidae       muntiacus
## 283          artiodactyla          cervidae      odocoileus
## 284          artiodactyla          cervidae      odocoileus
## 285          artiodactyla          cervidae      ozotoceros
## 286          artiodactyla          cervidae            pudu
## 287          artiodactyla          cervidae        rangifer
## 288          artiodactyla        giraffidae         giraffa
## 289          artiodactyla        giraffidae          okapia
## 290          artiodactyla            suidae    phacochoerus
## 291          artiodactyla       tayassuidae       catagonus
## 292          artiodactyla       tayassuidae          pecari
## 293          artiodactyla       tayassuidae         tayassu
## 294          artiodactyla        tragulidae      hyemoschus
## 295             carnivora         ailuridae         ailurus
## 296             carnivora           canidae          alopex
## 297             carnivora           canidae           canis
## 298             carnivora           canidae     pseudalopex
## 299             carnivora           canidae     pseudalopex
## 300             carnivora           canidae          vulpes
## 301             carnivora           canidae          vulpes
## 302             carnivora           canidae          vulpes
## 303             carnivora         dipodidae      stylodipus
## 304             carnivora        eupleridae    cryptoprocta
## 305             carnivora           felidae        acinonyx
## 306             carnivora           felidae         caracal
## 307             carnivora           felidae           felis
## 308             carnivora           felidae           felis
## 309             carnivora           felidae     herpailurus
## 310             carnivora           felidae       leopardus
## 311             carnivora           felidae       leopardus
## 312             carnivora           felidae     leptailurus
## 313             carnivora           felidae            lynx
## 314             carnivora           felidae            lynx
## 315             carnivora           felidae            lynx
## 316             carnivora           felidae            lynx
## 317             carnivora           felidae       oncifelis
## 318             carnivora           felidae        panthera
## 319             carnivora           felidae        panthera
## 320             carnivora           felidae        panthera
## 321             carnivora           felidae    prionailurus
## 322             carnivora           felidae            puma
## 323             carnivora           felidae           uncia
## 324             carnivora       herpestidae          atilax
## 325             carnivora       herpestidae        cynictis
## 326             carnivora       herpestidae        helogale
## 327             carnivora       herpestidae       herpestes
## 328             carnivora       herpestidae       ichneumia
## 329             carnivora          hyanidae        proteles
## 330             carnivora        mustelidae            eira
## 331             carnivora        mustelidae        galictis
## 332             carnivora        mustelidae            gulo
## 333             carnivora        mustelidae          martes
## 334             carnivora        mustelidae          martes
## 335             carnivora        mustelidae          martes
## 336             carnivora        mustelidae          martes
## 337             carnivora        mustelidae         mustela
## 338             carnivora        mustelidae         mustela
## 339             carnivora        mustelidae         mustela
## 340             carnivora        mustelidae         mustela
## 341             carnivora        mustelidae         mustela
## 342             carnivora        mustelidae         mustela
## 343             carnivora        mustelidae         mustela
## 344             carnivora        mustelidae         taxidea
## 345             carnivora       procyonidae           potos
## 346             carnivora           ursidae      ailuropoda
## 347             carnivora           ursidae        melursus
## 348             carnivora        viverridae         genetta
## 349             carnivora        viverridae         genetta
## 350             carnivora        viverridae         viverra
## 351         dasyuromorpha        dasyuridae        dasyurus
## 352         dasyuromorpha        dasyuridae        dasyurus
## 353         dasyuromorpha        dasyuridae     sminthopsis
## 354         dasyuromorpia        dasyuridae      antechinus
## 355       didelphimorphia       didelphidae     monodelphis
## 356       didelphimorphia       didelphidae        thylamys
## 357           diprodontia      macropodidae     dendrolagus
## 358           diprodontia      macropodidae        macropus
## 359           diprodontia      macropodidae        macropus
## 360           diprodontia      macropodidae        macropus
## 361           diprodontia      macropodidae        macropus
## 362           diprodontia      macropodidae        macropus
## 363           diprodontia      macropodidae        macropus
## 364           diprodontia      macropodidae        macropus
## 365           diprodontia      macropodidae       petrogale
## 366           diprodontia        potoroidae       bettongia
## 367           diprodontia        potoroidae        potorous
## 368           diprodontia   pseudocheiridae     petauroides
## 369         diprotodontia      macropodidae     onychogalea
## 370         diprotodontia      macropodidae       thylogale
## 371         diprotodontia      macropodidae       thylogale
## 372         diprotodontia      macropodidae        wallabia
## 373         diprotodontia     phalangeridae     trichosurus
## 374         diprotodontia        vombatidae     lasiorhinus
## 375         diprotodontia        vombatidae        vombatus
## 376        erinaceomorpha       erinaceidae       erinaceus
## 377        erinaceomorpha       erinaceidae     hemiechinus
## 378            lagomorpha         leporidae     brachylagus
## 379            lagomorpha         leporidae           lepus
## 380            lagomorpha         leporidae           lepus
## 381            lagomorpha         leporidae           lepus
## 382            lagomorpha         leporidae           lepus
## 383            lagomorpha         leporidae           lepus
## 384            lagomorpha         leporidae           lepus
## 385            lagomorpha         leporidae           lepus
## 386            lagomorpha         leporidae     oryctolagus
## 387            lagomorpha         leporidae      sylvilagus
## 388            lagomorpha         leporidae      sylvilagus
## 389            lagomorpha         leporidae      sylvilagus
## 390            lagomorpha       ochotonidae        ochotona
## 391            lagomorpha       ochotonidae        ochotona
## 392         macroscelidea   macroscelididae    elephantulus
## 393         macroscelidea   macroscelididae     petrodromus
## 394         macroscelidea   macroscelididae     rhynchocyon
## 395          monotrematae    tachyglossidae    tachyoryctes
## 396       peramelemorphia       peramelidae         isoodon
## 397       peramelemorphia       peramelidae         isoodon
## 398        perissodactyla           equidae           equus
## 399        perissodactyla    rhinocerotidae   ceratotherium
## 400        perissodactyla    rhinocerotidae         diceros
## 401                pilosa      bradypodidae        bradypus
## 402           proboscidea      elephantidae         elephas
## 403           proboscidea      elephantidae       loxodonta
## 404                 roden        cricetidae       onychomys
## 405              rodentia     aplodontiidae      aplodontia
## 406              rodentia      bathyergidae      bathyergus
## 407              rodentia      bathyergidae       cryptomys
## 408              rodentia      bathyergidae       cryptomys
## 409              rodentia      bathyergidae       georychus
## 410              rodentia      bathyergidae    heliophobius
## 411              rodentia      bathyergidae  heterocephalus
## 412              rodentia          caviidae      dolichotus
## 413              rodentia     chinchillidae      lagostomus
## 414              rodentia        cricetidae   clethrionomys
## 415              rodentia        cricetidae       hylaeamys
## 416              rodentia        cricetidae          lemmus
## 417              rodentia        cricetidae        microtus
## 418              rodentia        cricetidae        microtus
## 419              rodentia        cricetidae        microtus
## 420              rodentia        cricetidae        microtus
## 421              rodentia        cricetidae        microtus
## 422              rodentia        cricetidae        microtus
## 423              rodentia        cricetidae        microtus
## 424              rodentia        cricetidae          myopus
## 425              rodentia        cricetidae         neotoma
## 426              rodentia        cricetidae         neotoma
## 427              rodentia        cricetidae         neotoma
## 428              rodentia        cricetidae         neotoma
## 429              rodentia        cricetidae         ondatra
## 430              rodentia        cricetidae      peromyscus
## 431              rodentia        cricetidae reithrodontomys
## 432              rodentia        cricetidae      synaptomys
## 433              rodentia         dipodidae      pygeretmus
## 434              rodentia        echimyidae      proechimys
## 435              rodentia        echimyidae      proechimys
## 436              rodentia    erethizontidae         coendou
## 437              rodentia    erethizontidae       erethizon
## 438              rodentia         geomyidae        thomomys
## 439              rodentia          gliridae      graphiurus
## 440              rodentia          gliridae     muscardinus
## 441              rodentia      heteromyidae       dipodomys
## 442              rodentia      heteromyidae       dipodomys
## 443              rodentia      heteromyidae       dipodomys
## 444              rodentia      heteromyidae       dipodomys
## 445              rodentia      heteromyidae       dipodomys
## 446              rodentia       hystricidae         hystrix
## 447              rodentia       hystricidae         hystrix
## 448              rodentia       hystridicae       atherurus
## 449              rodentia           muridae        apodemus
## 450              rodentia           muridae        apodemus
## 451              rodentia           muridae        meriones
## 452              rodentia           muridae       pseudomys
## 453              rodentia        nesomyidae      hypogeomys
## 454              rodentia        nesomyidae         nesomys
## 455              rodentia        nesomyidae         nesomys
## 456              rodentia      octodontidae      spalacopus
## 457              rodentia         sciuridae        eutamias
## 458              rodentia         sciuridae      funambulus
## 459              rodentia         sciuridae       glaucomys
## 460              rodentia         sciuridae       glaucomys
## 461              rodentia         sciuridae         marmota
## 462              rodentia         sciuridae         marmota
## 463              rodentia         sciuridae       paraxerus
## 464              rodentia         sciuridae         sciurus
## 465              rodentia         sciuridae         sciurus
## 466              rodentia         sciuridae         sciurus
## 467              rodentia         sciuridae         sciurus
## 468              rodentia         sciuridae         sciurus
## 469              rodentia         sciuridae    spermophilus
## 470              rodentia         sciuridae    spermophilus
## 471              rodentia         sciuridae    spermophilus
## 472              rodentia         sciuridae    spermophilus
## 473              rodentia         sciuridae    spermophilus
## 474              rodentia         sciuridae    spermophilus
## 475              rodentia         sciuridae    spermophilus
## 476              rodentia         sciuridae          tamias
## 477              rodentia         sciuridae          tamias
## 478              rodentia         sciuridae          tamias
## 479              rodentia         sciuridae          tamias
## 480              rodentia         sciuridae    tamiasciurus
## 481              rodentia         sciuridae           xerus
## 482          soricomorpha         soricidae       crocidura
## 483          soricomorpha         soricidae           sorex
## 484          soricomorpha         soricidae           sorex
## 485          soricomorpha         soricidae           sorex
## 486          soricomorpha         soricidae           sorex
## 487          soricomorpha         soricidae           sorex
## 488          soricomorpha          talpidae       condylura
## 489          soricomorpha          talpidae        scalopus
## 490          soricomorpha          talpidae           talpa
## 491          soricomorpha          talpidae           talpa
## 492              squamata          agamidae       uromastyx
## 493              squamata        colubridae       carphopis
## 494              squamata        colubridae       carphopis
## 495              squamata        colubridae         coluber
## 496              squamata        colubridae         coluber
## 497              squamata        colubridae       diadophis
## 498              squamata        colubridae      drymarchon
## 499              squamata        colubridae          elaphe
## 500              squamata        colubridae          elaphe
## 501              squamata        colubridae       heterodon
## 502              squamata        colubridae       hierophis
## 503              squamata        colubridae    lampropeltis
## 504              squamata        colubridae    lampropeltis
## 505              squamata        colubridae     masticophis
## 506              squamata        colubridae          natrix
## 507              squamata        colubridae         nerodia
## 508              squamata        colubridae         nerodia
## 509              squamata        colubridae      oocatochus
## 510              squamata        colubridae       pituophis
## 511              squamata        colubridae       pituophis
## 512              squamata        colubridae      thamnophis
## 513              squamata        colubridae      thamnophis
## 514              squamata        colubridae         zamenis
## 515              squamata          elapidae   hoplocephalus
## 516              squamata          elapidae        notechis
## 517              squamata          elapidae      pseudechis
## 518              squamata         iguanidae      conolophus
## 519              squamata         iguanidae         cyclura
## 520              squamata         iguanidae         cyclura
## 521              squamata         iguanidae         cyclura
## 522              squamata         iguanidae      sauromalus
## 523              squamata         iguanidae      sauromalus
## 524              squamata        lacertilia     dipsosaurus
## 525              squamata        lacertilia        gallotia
## 526              squamata       liolaemidae      phymaturus
## 527              squamata        pythonidae         morelia
## 528              squamata         scincidae         egernia
## 529              squamata         viperidae     agkistrodon
## 530              squamata         viperidae     agkistrodon
## 531              squamata         viperidae           bitis
## 532              squamata         viperidae        bothrops
## 533              squamata         viperidae        crotalus
## 534              squamata         viperidae        crotalus
## 535              squamata         viperidae        crotalus
## 536              squamata         viperidae        crotalus
## 537              squamata         viperidae        crotalus
## 538              squamata         viperidae        crotalus
## 539              squamata         viperidae        crotalus
## 540              squamata         viperidae        crotalus
## 541              squamata         viperidae        gloydius
## 542              squamata         viperidae     montivipera
## 543              squamata         viperidae          vipera
## 544            testudines          chelidae       chelodina
## 545            testudines          chelidae     mesoclemmys
## 546            testudines       chelydridae        chelydra
## 547            testudines          emydidae       chrysemys
## 548            testudines          emydidae     deirochelys
## 549            testudines          emydidae       emydoidea
## 550            testudines          emydidae            emys
## 551            testudines          emydidae       graptemys
## 552            testudines          emydidae       terrapene
## 553            testudines       geoemydidae        mauremys
## 554            testudines     kinosternidae     kinosternon
## 555            testudines     kinosternidae    sternotherus
## 556            testudines     kinosternidae    sternotherus
## 557            testudines      testudinidae      geochelone
## 558            testudines      testudinidae        gopherus
## 559            testudines      testudinidae        gopherus
## 560            testudines      testudinidae     indotestudo
## 561            testudines      testudinidae         kinixys
## 562            testudines      testudinidae        manouria
## 563            testudines      testudinidae     psammobates
## 564            testudines      testudinidae    stigmochelys
## 565            testudines      testudinidae         testudo
## 566            testudines      testudinidae         testudo
## 567            testudines      testudinidae         testudo
## 568            testudines      testudinidae         testudo
## 569            testudines      trionychidae         apalone
##                      species
## 1                   rostrata
## 2                  poecilura
## 3                   anomalum
## 4                funduloides
## 5                 cataractae
## 6                masquinongy
## 7                 pollachius
## 8                     virens
## 9                   lineatus
## 10                 lituratus
## 11                 unicornis
## 12                atlanticus
## 13                 ignobilis
## 14                 rupestris
## 15                  gibbosus
## 16               macrochirus
## 17                 megalotis
## 18                  dolomieu
## 19                 salmoides
## 20                 annularis
## 21                 baronessa
## 22                 trichrous
## 23              trifascialis
## 24              trifasciatus
## 25              unimaculatus
## 26              spectrabilis
## 27                     pinos
## 28                     falco
## 29                marmoratus
## 30                  japonica
## 31                     dalli
## 32                  hipoliti
## 33                 nicholsii
## 34               longipinnis
## 35                 sectatrix
## 36                     rufus
## 37                 undulatus
## 38                     julis
## 39                bivittatus
## 40                   garnoti
## 41               maculipinna
## 42                     poeyi
## 43                dimidiatus
## 44                  bergylta
## 45                lineolatus
## 46                   pulcher
## 47                 adspersus
## 48               bifasciatum
## 49                    lunare
## 50                     harak
## 51                    analis
## 52                    apodus
## 53                decussatus
## 54                   griseus
## 55                 chrysurus
## 56                  princeps
## 57                    labrax
## 58             flavolineatus
## 59                porphyreus
## 60                flavescens
## 61                   luridus
## 62                      argi
## 63                   chromis
## 64                biocellata
## 65                   aruanus
## 66                     wardi
## 67                  apicalis
## 68                  partitus
## 69                variabilis
## 70               microrhinos
## 71                     iseri
## 72                 rivulatus
## 73              aurofrenatum
## 74              chrysopterum
## 75                rubripinne
## 76                    viride
## 77                     argus
## 78                 cruentata
## 79               hemistiktos
## 80                   miniata
## 81                  guttatus
## 82                marginatus
## 83                     morio
## 84                  striatus
## 85                   tauvina
## 86                    huntii
## 87               maccullochi
## 88                    bonaci
## 89                clathratus
## 90                 nebulifer
## 91                 areolatus
## 92                 leopardus
## 93                  cabrilla
## 94                    scriba
## 95                     salpa
## 96                   auratus
## 97                    clarki
## 98                     gilae
## 99                    mykiss
## 100                    salar
## 101                   trutta
## 102                   bairdi
## 103                carolinae
## 104                    gobio
## 105                 caurinus
## 106                  inermis
## 107                  maliger
## 108                 melanops
## 109                 mustinus
## 110                  natalis
## 111               guttulatus
## 112           lumbriciformis
## 113                 rostrata
## 114               chrysaetos
## 115                    buteo
## 116                 gallicus
## 117                fasciatus
## 118                 pennatus
## 119             percnopterus
## 120                 strepera
## 121                australis
## 122                europaeus
## 123               ostralegus
## 124                     inca
## 125                 palumbus
## 126                   turtur
## 127                 garrulus
## 128                    epops
## 129               glandarius
## 130                  canorus
## 131            californianus
## 132               radiolosus
## 133                 cooperii
## 134                 gentilis
## 135                    nisus
## 136                 striatus
## 137              jamaicensis
## 138                 lineatus
## 139                swainsoni
## 140                  cyaneus
## 141                 pygargus
## 142                   milvus
## 143                 cheriway
## 144               americanus
## 145                biarmicus
## 146                mexicanus
## 147               peregrinus
## 148               sparverius
## 149              tinnunculus
## 150                  bonasia
## 151             urophasianus
## 152                 obscurus
## 153                  lagopus
## 154                   perdix
## 155                   tetrix
## 156                urogallus
## 157          cupido pinnatus
## 158                    wolfi
## 159                     crex
## 160                  elegans
## 161               polyglotta
## 162                 caudatus
## 163                  arborea
## 164               fuscicauda
## 165                   rubica
## 166               familiaris
## 167                 juncidis
## 168                    corax
## 169            caryocatactes
## 170                raimondii
## 171               savannarum
## 172                   cyanea
## 173                   aberti
## 174                   fuscus
## 175                  arborea
## 176                passerina
## 177                cannabina
## 178                  coelebs
## 179                  serinus
## 180                    magna
## 181                 neglecta
## 182                   virens
## 183                 collurio
## 184             ludovicianus
## 185                    minor
## 186                  senator
## 187              polyglottos
## 188                     alba
## 189                    flava
## 190                  striata
## 191                 oenanthe
## 192              phoenicurus
## 193                  rubetra
## 194             atricapillus
## 195             carolinensis
## 196                inornatus
## 197                palustris
## 198             philadelphia
## 199                  trichas
## 200                   citrea
## 201             aurocapillus
## 202                    fusca
## 203                kirtlandi
## 204                 magnolia
## 205             pensylvanica
## 206                 petechia
## 207                ruticilla
## 208                   virens
## 209               canadensis
## 210                  bonelli
## 211             dentirostris
## 212             ignicapillus
## 213                  regulus
## 214                 europaea
## 215                 fasciata
## 216                    sarda
## 217                   undata
## 218                 bewickiI
## 219             ludovicianus
## 220                    aedon
## 221              troglodytes
## 222                   sialis
## 223                   virens
## 224                  minimus
## 225                 wrightii
## 226                 tyrannus
## 227             atricapillus
## 228                    belli
## 229                  griseus
## 230                olivaceus
## 231                stellaris
## 232                   exilis
## 233                  martius
## 234                torquilla
## 235                 leucotos
## 236                   medius
## 237              tridactylus
## 238                    canus
## 239                  viridis
## 240              habroptilus
## 241                americana
## 242                  pennata
## 243                 funereus
## 244                     otus
## 245                   noctua
## 246                     bubo
## 247              virginianus
## 248               passerinum
## 249                scandiaca
## 250                    aluco
## 251                     alba
## 252                  camelus
## 253                   ornata
## 254               trevelyani
## 255                   granti
## 256                americana
## 257                 melampus
## 258               buselaphus
## 259                   lervia
## 260                    bison
## 261                  bonasus
## 262                   hircus
## 263                pyrenaica
## 264               callipygus
## 265                 dorsalis
## 266                  gazella
## 267                guentheri
## 268               americanus
## 269                    ammon
## 270               canadensis
## 271               campestris
## 272                rupicapra
## 273                     oryx
## 274                 scriptus
## 275             strepsiceros
## 276                    alces
## 277                     axis
## 278                capreolus
## 279                  elaphus
## 280                   nippon
## 281                     dama
## 282                  reevesi
## 283                 hemionus
## 284              virginianus
## 285              bezoarticus
## 286                     puda
## 287                 tarandus
## 288           camelopardalis
## 289                johnstoni
## 290              aethiopicus
## 291                  wagneri
## 292                   tajacu
## 293                   pecari
## 294                aquaticus
## 295                  fulgens
## 296                  lagopus
## 297                 simensis
## 298                 culpaeus
## 299                  griseus
## 300                  macroti
## 301                 ruppelli
## 302                    velox
## 303                    telum
## 304                    ferox
## 305                  jubatus
## 306                  caracal
## 307                    catus
## 308               sylvestris
## 309             yagouaroundi
## 310                 pardalis
## 311                   wiedii
## 312                   serval
## 313               canadensis
## 314                     lynx
## 315                 pardinus
## 316                    rufus
## 317                geoffroyi
## 318                     onca
## 319                   pardus
## 320                   tigris
## 321              bengalensis
## 322                 concolor
## 323                    uncia
## 324              paludinosus
## 325              penicillata
## 326                  parvula
## 327               inchneumon
## 328                albicauda
## 329                cristatus
## 330                  barbata
## 331                  vittata
## 332                     gulo
## 333                americana
## 334                    foina
## 335                   martes
## 336                 pennanti
## 337                  erminea
## 338                  frenata
## 339                     furo
## 340                 lutreola
## 341                 nigripes
## 342                  nivalis
## 343                 sibirica
## 344                    taxus
## 345                   flavus
## 346              melanoleuca
## 347                  ursinus
## 348                  genetta
## 349                  tigrina
## 350                  zibetha
## 351                geoffroii
## 352                maculatus
## 353                 leucopus
## 354                 stuartii
## 355                americana
## 356                  elegans
## 357                lumholtzi
## 358              antilopinus
## 359                 dorsalis
## 360              fuliginosus
## 361                giganteus
## 362                 robustus
## 363              rufogriseus
## 364                    rufus
## 365                assimilis
## 366                 gaimardi
## 367                 longipes
## 368                   volans
## 369                 fraenata
## 370               stigmatica
## 371                   thetis
## 372                  bicolor
## 373                vulpecula
## 374                 krefftii
## 375                  ursinus
## 376                europaeus
## 377                  auritus
## 378               idahoensis
## 379               americanus
## 380                 arcticus
## 381             californicus
## 382                 capensis
## 383                europaeus
## 384              nigricollis
## 385                  timidus
## 386                cuniculus
## 387                aquaticus
## 388               floridanus
## 389                palustris
## 390                curzoniae
## 391                 princeps
## 392                rufescens
## 393            tetradactylus
## 394              chrysopygus
## 395                splendens
## 396                  auratus
## 397                 obesulus
## 398                 caballus
## 399                    simum
## 400                 bicornis
## 401                torquatus
## 402                  maximus
## 403                 africana
## 404                 torridus
## 405                     rufa
## 406                  suillus
## 407               damarensis
## 408              hottentotus
## 409                 capensis
## 410         argenteocinereus
## 411                   glaber
## 412                patagonus
## 413                  maximus
## 414             californicus
## 415             megacephalus
## 416                sibiricus
## 417                 agrestis
## 418             californicus
## 419                 montanus
## 420              ochrogaster
## 421           pennsylvanicus
## 422                pinetorum
## 423              richardsoni
## 424             schisticolor
## 425                  cinerea
## 426                 fuscipes
## 427                   lepida
## 428                 micropus
## 429                zibethica
## 430               gossypinus
## 431              raviventris
## 432                  cooperi
## 433                  pumilio
## 434                  cuvieri
## 435             semispinosus
## 436              prehensilis
## 437                 dorsatum
## 438                   bottae
## 439                 ocularis
## 440             avellanarius
## 441                   ingens
## 442                 merriami
## 443                    ordii
## 444              spectabilis
## 445                stephensi
## 446         africaeaustralis
## 447                   indica
## 448                africanus
## 449              flavicollis
## 450               sylvaticus
## 451                hurrianae
## 452                   fuscus
## 453                 antimena
## 454                audeberti
## 455                    rufus
## 456                   cyanus
## 457                sibiricus
## 458                 pennanti
## 459                 sabrinus
## 460                   volans
## 461             flaviventris
## 462                    monax
## 463                palliatus
## 464                   aberti
## 465             carolinensis
## 466                      lis
## 467                    niger
## 468                 vulgaris
## 469                 beecheyi
## 470              columbianus
## 471               franklinii
## 472                  parryii
## 473                spilosoma
## 474         tridecemlineatus
## 475               variegatus
## 476                  amoenus
## 477                  minimus
## 478           quadrivittatus
## 479                 umbrinus
## 480               hudsonicus
## 481               erythropus
## 482                  russula
## 483                 arcticus
## 484                 cinereus
## 485                coronatus
## 486              gracillimus
## 487             unguiculatus
## 488                 cristata
## 489                aquaticus
## 490                 europaea
## 491                   romana
## 492                aegyptius
## 493                   vermis
## 494                  viridis
## 495              constrictor
## 496 constrictor flaviventris
## 497                punctatus
## 498                  couperi
## 499           guttata emoryi
## 500                 obsoleta
## 501              platirhinos
## 502             viridiflavus
## 503            getula getula
## 504               triangulum
## 505                flagellum
## 506                   natrix
## 507            erythrogaster
## 508                 sipeodon
## 509             rufodorsatus
## 510                catenifer
## 511             melanoleucus
## 512                  butleri
## 513                    gigal
## 514              longissimus
## 515              bungaroides
## 516                 scutatus
## 517             porphyriacus
## 518                 pallidus
## 519                  cyclura
## 520                   lewisi
## 521                  pinguis
## 522                 hispidua
## 523                   obesus
## 524                 dorsalis
## 525                  galloti
## 526              flagellifer
## 527        spilota imbricata
## 528                    major
## 529               contortrix
## 530              piscivorous
## 531               schneideri
## 532                    asper
## 533                    atrox
## 534                 cerastes
## 535                 horridus
## 536                 molossus
## 537        oreganus concolor
## 538                   pricei
## 539               scutulatus
## 540                   tigris
## 541              shedaoensis
## 542                   raddei
## 543                 latastei
## 544              longicollis
## 545                    dahli
## 546               serpentina
## 547          picta marginata
## 548              reticularia
## 549               blandingii
## 550              orbicularis
## 551            flavimaculata
## 552                   ornata
## 553                  leprosa
## 554                rubrubrum
## 555           minor peltifer
## 556                 odoratus
## 557               carbonaria
## 558                agassizii
## 559               polyphemus
## 560             travancorica
## 561                   spekii
## 562                 impressa
## 563                tentorius
## 564                 pardalis
## 565                   graeca
## 566                 hermanii
## 567               horsfieldi
## 568               kleinmanni
## 569                spinifera
```

**5. The variable `taxon` identifies the large, common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**  


```r
table(homerange$taxon)
```

```
## 
##         birds   lake fishes       lizards       mammals marine fishes 
##           140             9            11           238            90 
##  river fishes        snakes     tortoises       turtles 
##            14            41            12            14
```

**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**


```r
table(homerange$trophic.guild)
```

```
## 
## carnivore herbivore 
##       342       227
```

**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  


```r
carni <- filter(homerange, trophic.guild == "carnivore")
carni
```

```
## # A tibble: 342 × 24
##    taxon   common.name   class   order  family genus species primarymethod N    
##    <fct>   <chr>         <chr>   <fct>  <chr>  <chr> <chr>   <chr>         <chr>
##  1 lake f… american eel  actino… angui… angui… angu… rostra… telemetry     16   
##  2 river … blacktail re… actino… cypri… catos… moxo… poecil… mark-recaptu… <NA> 
##  3 river … central ston… actino… cypri… cypri… camp… anomal… mark-recaptu… 20   
##  4 river … rosyside dace actino… cypri… cypri… clin… fundul… mark-recaptu… 26   
##  5 river … longnose dace actino… cypri… cypri… rhin… catara… mark-recaptu… 17   
##  6 river … muskellunge   actino… esoci… esoci… esox  masqui… telemetry     5    
##  7 marine… pollack       actino… gadif… gadid… poll… pollac… telemetry     2    
##  8 marine… saithe        actino… gadif… gadid… poll… virens  telemetry     2    
##  9 marine… giant treval… actino… perci… caran… cara… ignobi… telemetry     4    
## 10 lake f… rock bass     actino… perci… centr… ambl… rupest… mark-recaptu… 16   
## # … with 332 more rows, and 15 more variables: mean.mass.g <dbl>,
## #   log10.mass <dbl>, alternative.mass.reference <chr>, mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, hra.reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic.guild <chr>, dimension <dbl>, preymass <dbl>,
## #   log10.preymass <dbl>, PPMR <dbl>, prey.size.reference <chr>
```

```r
herbi <- filter(homerange, trophic.guild == "herbivore")
herbi
```

```
## # A tibble: 227 × 24
##    taxon  common.name   class   order  family  genus species primarymethod N    
##    <fct>  <chr>         <chr>   <fct>  <chr>   <chr> <chr>   <chr>         <chr>
##  1 marin… lined surgeo… actino… perci… acanth… acan… lineat… direct obser… <NA> 
##  2 marin… orangespine … actino… perci… acanth… naso  litura… telemetry     8    
##  3 marin… bluespine un… actino… perci… acanth… naso  unicor… telemetry     7    
##  4 marin… redlip blenny actino… perci… blenni… ophi… atlant… direct obser… 20   
##  5 marin… bermuda chub  actino… perci… kyphos… kyph… sectat… telemetry     11   
##  6 marin… cherubfish    actino… perci… pomaca… cent… argi    direct obser… <NA> 
##  7 marin… damselfish    actino… perci… pomace… chro… chromis direct obser… <NA> 
##  8 marin… twinspot dam… actino… perci… pomace… chry… biocel… direct obser… 18   
##  9 marin… wards damsel  actino… perci… pomace… poma… wardi   direct obser… <NA> 
## 10 marin… australian g… actino… perci… pomace… steg… apical… direct obser… <NA> 
## # … with 217 more rows, and 15 more variables: mean.mass.g <dbl>,
## #   log10.mass <dbl>, alternative.mass.reference <chr>, mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, hra.reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic.guild <chr>, dimension <dbl>, preymass <dbl>,
## #   log10.preymass <dbl>, PPMR <dbl>, prey.size.reference <chr>
```

**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**  


```r
mean(carni$mean.hra.m2)
```

```
## [1] 13039918
```


```r
mean(herbi$mean.hra.m2)
```

```
## [1] 34137012
```


```r
carni_mass_vector <- c(carni$mean.hra.m2)
carni_mass_vector
```

```
##   [1]    282750.00       282.10       116.11       125.50        87.10
##   [6]     39343.50      9056.41     44516.15     52773.00     10406.90
##  [11]      4950.00      4420.00        97.50      1936.50     34403.33
##  [16]    158000.00        63.24        59.50        64.91       220.93
##  [21]       274.94      2046.67         2.54        46.20      3303.11
##  [26]        28.27         0.50         0.03         0.32        61.50
##  [31]       517.80     50000.00       227.60       142.50       128.50
##  [36]       158.40       163.70        33.16      4028.59       856.00
##  [41]     15134.00      1368.11       103.30        11.87      1988.10
##  [46]   7640000.00      6660.00      1340.60     35384.00   4172000.00
##  [51]     35474.00     17712.35      5400.00     19200.00     11400.00
##  [56]        27.00         1.10      1300.00      2120.00        12.00
##  [61]       217.00      1243.40      5312.00   2087500.00     18305.00
##  [66]    299000.00         2.19         6.83   1435000.00      3760.25
##  [71]     10002.71    168000.00     18796.00    761666.67   1107500.00
##  [76]     29754.81       101.25       343.70       174.00        45.00
##  [81]     15288.40        83.90        47.00        48.13      2157.75
##  [86]       815.40      1571.00    250000.00     34692.40      6145.74
##  [91]        19.90        12.60       151.50  27550000.00  50240000.00
##  [96]  78500000.00  19620000.00 117300000.00  63570000.00    463900.00
## [101]    785000.00   2460000.00   1000000.00  12560000.00  12560000.00
## [106]  38460000.00    550000.00    499000.00   2254095.45  40000000.00
## [111]   7100000.00    995525.10   4249192.50    639402.30   2464531.65
## [116]   2521187.55 200980000.00  19625000.00 241000000.00    666000.00
## [121]  50000000.00  25778434.50 153860000.00   1416397.50   3000000.00
## [126]     90000.00     44000.00     30000.00     42000.00     82960.43
## [131]     60702.75     48562.20     47000.00     14400.00  28000000.00
## [136]     10926.50      1052.18     16996.77     30756.06     42000.00
## [141]     10000.00     30351.38     30351.38      1335.46     15782.72
## [146]     75676.10    635800.00     80000.00      4046.85    785000.00
## [151]     10117.13     10000.00     15378.03      4500.00      7300.00
## [156]     14568.66     14973.35     24281.10     22662.36      7689.02
## [161]      5260.91     14973.35     10117.13      5260.91     33993.54
## [166]      7284.33      6070.28      1699.68      1942.49      6474.96
## [171]     10117.13     35000.00     16500.00     19900.00     21000.00
## [176]      3237.48      3300.00      2800.00      4856.22      1214.06
## [181]      4046.85     10117.13     10117.13     44029.73      1780.61
## [186]     15782.72     83769.80     14973.35     11735.87      1335.46
## [191]      7284.33    193000.00     97000.00   3500000.00   1038100.00
## [196]   5306600.00    141500.00    350000.00   4521600.00   1850000.00
## [201]   3140000.00  19620000.00    500000.00  16000000.00   2124596.25
## [206]   1250000.00   4937157.00    356932.17   1500000.00       616.60
## [211]     46299.89  28499024.85   5393739.90   4928672.94   2000000.02
## [216]   8772835.78  30399749.15   5495408.74    891250.94 815060787.90
## [221] 376634414.00   1472448.11   2794988.34  67079528.30   6849360.11
## [226]  10949896.14   2390011.55  82748475.24 181042275.70   9499921.14
## [231]  39877690.65   7126724.98  82735138.83 504940260.80  53583367.03
## [236]   2406855.40 312003883.70  17353217.10   2219984.80    592570.46
## [241]    281838.29   3100000.00    387266.56   3799968.53  14617395.58
## [246]   4150018.19 361917847.30   7066103.59   4356522.89   9093477.57
## [251]  16755600.61   1323244.20    176750.24    899994.80    208929.61
## [256]   1000000.00    648858.50   4073802.78   3808816.06  10000000.00
## [261]   7809981.41     57500.29  11999965.57   2290867.65  11595000.00
## [266]     11410.11     35000.16       467.74       707.94    194236.12
## [271]     40377.55      3393.13     11999.97     29000.13       100.00
## [276]    116834.20     21133.43     25945.38      5011.87       234.42
## [281]      4786.30      5011.87       371.54       275.42        77.62
## [286]      3630.78      7428.65      3004.35      2828.59       700.00
## [291]       253.00    151000.00    114500.00      6476.00   1853000.00
## [296]    150600.00     46000.00    516375.00    110900.00    495000.00
## [301]    240400.00    429300.00     99000.00    131000.00     40000.00
## [306]     15400.00     17400.00    701000.00       600.00    374800.00
## [311]     77400.00     27379.00     38800.00     96000.00    171600.00
## [316]    119288.89     10655.00       200.00     60700.00     54200.00
## [321]     28000.00   2579600.00     34900.00   1178000.00     22900.00
## [326]    316000.00     34800.00      2613.69    245928.57      2400.00
## [331]    142000.00    115500.00     65300.00     31500.00     23800.00
## [336]     33000.00     35160.00     57500.00    186000.00      5180.00
## [341]     28000.00   1800000.00
```


```r
mean(carni_mass_vector)
```

```
## [1] 13039918
```


```r
herbi_mass_vector <- c(herbi$mean.hra.m2)
herbi_mass_vector
```

```
##   [1] 1.113000e+01 3.209286e+04 1.790000e+04 5.200000e-01 3.442300e+04
##   [6] 1.130000e+00 1.850000e+01 2.580000e+00 5.400000e-01 2.250000e+00
##  [11] 5.000000e-02 7.400000e+00 7.830000e+03 1.720000e+02 2.444278e+04
##  [16] 2.870000e+02 1.650000e+02 1.750000e+02 7.200000e+01 4.948214e+04
##  [21] 4.591200e+07 2.589980e+03 2.540000e+06 6.358500e+07 1.030000e+05
##  [26] 1.815822e+07 1.699677e+04 2.589984e+04 6.200000e+04 1.975000e+06
##  [31] 5.500000e+06 1.203000e+07 4.300000e+04 1.323320e+05 3.090000e+04
##  [36] 1.618740e+04 2.589984e+04 3.140000e+06 9.500000e+04 1.950000e+05
##  [41] 2.450000e+06 2.388000e+07 8.430000e+07 2.428110e+04 1.012535e+07
##  [46] 3.224482e+06 2.176858e+06 9.050030e+06 2.657786e+08 1.014051e+07
##  [51] 6.452827e+07 7.000032e+05 4.161693e+05 3.570015e+05 1.872837e+06
##  [56] 2.733317e+04 2.328574e+07 5.882474e+06 2.754482e+07 6.199976e+05
##  [61] 3.408003e+06 5.239984e+07 3.710906e+04 1.070040e+09 9.382531e+07
##  [66] 3.647707e+06 6.746368e+05 7.486520e+07 8.514909e+05 9.850086e+05
##  [71] 1.620541e+05 3.507276e+07 2.488342e+06 7.900053e+06 1.976651e+05
##  [76] 3.550831e+09 1.365369e+08 5.899973e+06 1.742007e+06 1.097994e+07
##  [81] 2.486223e+06 1.459990e+07 1.919995e+05 1.024991e+06 4.487040e+03
##  [86] 2.792480e+05 4.135043e+06 1.930012e+04 6.413277e+05 9.149977e+05
##  [91] 3.236533e+06 6.133382e+06 9.709123e+05 1.630009e+05 7.739983e+06
##  [96] 1.190008e+05 4.566674e+05 3.732759e+05 1.545468e+04 4.250010e+05
## [101] 3.566646e+04 1.385639e+05 1.527601e+05 5.013488e+04 2.500000e+05
## [106] 1.079991e+05 4.858810e+03 3.271674e+04 2.900013e+06 1.592759e+06
## [111] 5.300050e+05 2.866157e+05 1.389985e+04 4.532418e+05 6.299992e+04
## [116] 1.829995e+04 2.892011e+04 3.960044e+04 1.376000e+03 1.866340e+03
## [121] 2.229513e+07 1.591256e+07 4.206588e+07 4.499974e+04 1.099993e+08
## [126] 1.753759e+09 1.037410e+03 2.685840e+03 1.299990e+04 1.582010e+03
## [131] 2.720000e+02 1.720000e+02 5.400950e+03 9.769897e+05 1.000000e+04
## [136] 1.596980e+03 7.738910e+03 8.322040e+03 7.004200e+02 8.550000e+01
## [141] 1.516000e+02 6.744700e+02 4.117400e+02 3.667000e+01 4.192000e+02
## [146] 9.821300e+02 4.761678e+04 2.537000e+03 5.330000e+02 5.805500e+02
## [151] 5.995010e+03 2.132900e+03 4.549990e+03 3.127810e+03 5.788150e+03
## [156] 1.038510e+03 1.866680e+05 3.615014e+05 7.114000e+01 1.093000e+05
## [161] 4.369990e+03 4.317000e+02 7.374800e+03 1.769987e+04 3.008500e+03
## [166] 1.480440e+03 1.689779e+06 1.411985e+06 1.511785e+05 7.574950e+03
## [171] 1.305209e+04 1.327000e+02 6.300000e+02 3.500016e+04 9.499920e+03
## [176] 5.000000e+03 4.030000e+01 1.932810e+03 1.799990e+03 7.900053e+04
## [181] 2.919577e+04 5.696394e+04 1.653903e+05 2.749983e+04 1.308881e+05
## [186] 4.900040e+03 1.708205e+05 1.282655e+05 7.490831e+04 5.188400e+02
## [191] 5.355000e+02 1.684380e+05 3.010925e+04 1.504181e+04 1.559983e+04
## [196] 4.250500e+04 8.163190e+03 1.490219e+04 5.325005e+04 3.246011e+04
## [201] 4.753350e+03 1.239995e+05 1.176068e+04 8.594300e+03 7.066000e+04
## [206] 1.280000e+05 3.477778e+04 2.350000e+03 5.656000e+03 5.850000e+02
## [211] 5.750000e+01 1.350000e+02 1.464800e+04 8.727000e+03 3.418000e+04
## [216] 1.384160e+06 1.685000e+05 5.200000e+03 7.200000e+04 1.900000e+04
## [221] 9.640000e+04 3.197000e+05 2.050000e+06 1.710000e+04 3.790000e+04
## [226] 5.700000e+05 1.417000e+05
```


```r
mean(herbi_mass_vector)
```

```
## [1] 34137012
```

```r
mean(carni_mass_vector)
```

```
## [1] 13039918
```

The herbivores have a larger average "mass.hra.m2" compared to carnivores. I was able to calculate the mean without errors from NAs. 

**9. Make a new dataframe `deer` that is limited to the mean mass, log10 mass, family, genus, and species of deer in the database. The family for deer is cervidae. Arrange the data in descending order by log10 mass. Which is the largest deer? What is its common name?**  


```r
deer <- select(filter(homerange, family == "cervidae"), family:log10.mass, -primarymethod, -N)
arrange(deer, desc("log10.mass"))
```

```
## # A tibble: 12 × 5
##    family   genus      species     mean.mass.g log10.mass
##    <chr>    <chr>      <chr>             <dbl>      <dbl>
##  1 cervidae alces      alces           307227.       5.49
##  2 cervidae axis       axis             62823.       4.80
##  3 cervidae capreolus  capreolus        24050.       4.38
##  4 cervidae cervus     elaphus         234758.       5.37
##  5 cervidae cervus     nippon           29450.       4.47
##  6 cervidae dama       dama             71450.       4.85
##  7 cervidae muntiacus  reevesi          13500.       4.13
##  8 cervidae odocoileus hemionus         53864.       4.73
##  9 cervidae odocoileus virginianus      87884.       4.94
## 10 cervidae ozotoceros bezoarticus      35000.       4.54
## 11 cervidae pudu       puda              7500.       3.88
## 12 cervidae rangifer   tarandus        102059.       5.01
```

```r
deer
```

```
## # A tibble: 12 × 5
##    family   genus      species     mean.mass.g log10.mass
##    <chr>    <chr>      <chr>             <dbl>      <dbl>
##  1 cervidae alces      alces           307227.       5.49
##  2 cervidae axis       axis             62823.       4.80
##  3 cervidae capreolus  capreolus        24050.       4.38
##  4 cervidae cervus     elaphus         234758.       5.37
##  5 cervidae cervus     nippon           29450.       4.47
##  6 cervidae dama       dama             71450.       4.85
##  7 cervidae muntiacus  reevesi          13500.       4.13
##  8 cervidae odocoileus hemionus         53864.       4.73
##  9 cervidae odocoileus virginianus      87884.       4.94
## 10 cervidae ozotoceros bezoarticus      35000.       4.54
## 11 cervidae pudu       puda              7500.       3.88
## 12 cervidae rangifer   tarandus        102059.       5.01
```


```r
filter(homerange, species == "alces")
```

```
## # A tibble: 1 × 24
##   taxon   common.name class    order    family genus species primarymethod N    
##   <fct>   <chr>       <chr>    <fct>    <chr>  <chr> <chr>   <chr>         <chr>
## 1 mammals moose       mammalia artioda… cervi… alces alces   telemetry*    <NA> 
## # … with 15 more variables: mean.mass.g <dbl>, log10.mass <dbl>,
## #   alternative.mass.reference <chr>, mean.hra.m2 <dbl>, log10.hra <dbl>,
## #   hra.reference <chr>, realm <chr>, thermoregulation <chr>, locomotion <chr>,
## #   trophic.guild <chr>, dimension <dbl>, preymass <dbl>, log10.preymass <dbl>,
## #   PPMR <dbl>, prey.size.reference <chr>
```
The largest deer has a log10.mass of 5.48746 and goes by the common name of "moose". 

**10. As measured by the data, which snake species has the smallest homerange? Show all of your work, please. Look this species up online and tell me about it!** **Snake is found in taxon column** 


```r
snakes <- data.frame(filter(homerange, taxon == "snakes"))
snakes 
```

```
##     taxon                common.name    class    order     family         genus
## 1  snakes         western worm snake reptilia squamata colubridae     carphopis
## 2  snakes         eastern worm snake reptilia squamata colubridae     carphopis
## 3  snakes                      racer reptilia squamata colubridae       coluber
## 4  snakes       yellow bellied racer reptilia squamata colubridae       coluber
## 5  snakes             ringneck snake reptilia squamata colubridae     diadophis
## 6  snakes       eastern indigo snake reptilia squamata colubridae    drymarchon
## 7  snakes      great plains ratsnake reptilia squamata colubridae        elaphe
## 8  snakes           western ratsnake reptilia squamata colubridae        elaphe
## 9  snakes              hognose snake reptilia squamata colubridae     heterodon
## 10 snakes         European whipsnake reptilia squamata colubridae     hierophis
## 11 snakes          Eastern kingsnake reptilia squamata colubridae  lampropeltis
## 12 snakes                  milksnake reptilia squamata colubridae  lampropeltis
## 13 snakes                  coachwhip reptilia squamata colubridae   masticophis
## 14 snakes                grass snake reptilia squamata colubridae        natrix
## 15 snakes     copperbelly watersnake reptilia squamata colubridae       nerodia
## 16 snakes        Northern watersnake reptilia squamata colubridae       nerodia
## 17 snakes         redbacked ratsnake reptilia squamata colubridae    oocatochus
## 18 snakes               gopher snake reptilia squamata colubridae     pituophis
## 19 snakes                 pine snake reptilia squamata colubridae     pituophis
## 20 snakes       butlers garter snake reptilia squamata colubridae    thamnophis
## 21 snakes        giant garter snakes reptilia squamata colubridae    thamnophis
## 22 snakes          Aesculapian snake reptilia squamata colubridae       zamenis
## 23 snakes          broadheaded snake reptilia squamata   elapidae hoplocephalus
## 24 snakes                tiger snake reptilia squamata   elapidae      notechis
## 25 snakes                 blacksnake reptilia squamata   elapidae    pseudechis
## 26 snakes southwestern carpet python reptilia squamata pythonidae       morelia
## 27 snakes                 copperhead reptilia squamata  viperidae   agkistrodon
## 28 snakes                cottonmouth reptilia squamata  viperidae   agkistrodon
## 29 snakes        namaqua dwarf adder reptilia squamata  viperidae         bitis
## 30 snakes               fer-de-lance reptilia squamata  viperidae      bothrops
## 31 snakes        western diamondback reptilia squamata  viperidae      crotalus
## 32 snakes                 sidewinder reptilia squamata  viperidae      crotalus
## 33 snakes         timber rattlesnake reptilia squamata  viperidae      crotalus
## 34 snakes    blacktailed rattlesnake reptilia squamata  viperidae      crotalus
## 35 snakes   midget faded rattlesnake reptilia squamata  viperidae      crotalus
## 36 snakes   twin-spotted rattlesnake reptilia squamata  viperidae      crotalus
## 37 snakes         Mojave rattlesnake reptilia squamata  viperidae      crotalus
## 38 snakes          tiger rattlesnake reptilia squamata  viperidae      crotalus
## 39 snakes          chinese pit viper reptilia squamata  viperidae      gloydius
## 40 snakes             Armenian viper reptilia squamata  viperidae   montivipera
## 41 snakes            snubnosed viper reptilia squamata  viperidae        vipera
##                     species  primarymethod    N mean.mass.g log10.mass
## 1                    vermis       radiotag    1        3.46  0.5390761
## 2                   viridis       radiotag   10        3.65  0.5622929
## 3               constrictor      telemetry   15      556.15  2.7451919
## 4  constrictor flaviventris      telemetry   12      144.50  2.1598678
## 5                 punctatus mark-recapture <NA>        9.00  0.9542425
## 6                   couperi      telemetry    1      450.00  2.6532125
## 7            guttata emoryi      telemetry   12      256.70  2.4094259
## 8                  obsoleta      telemetry   18      642.80  2.8080759
## 9               platirhinos      telemetry    8      147.32  2.1682617
## 10             viridiflavus      telemetry   32      234.10  2.3694014
## 11            getula getula      telemetry   12      315.72  2.4993021
## 12               triangulum      telemetry   10      165.00  2.2174839
## 13                flagellum      telemetry    4      534.50  2.7279477
## 14                   natrix      telemetry    4       78.50  1.8948697
## 15            erythrogaster      telemetry   15      313.24  2.4958772
## 16                 sipeodon      telemetry   13      190.55  2.2800090
## 17             rufodorsatus      telemetry   21       62.50  1.7958800
## 18                catenifer      telemetry    4      375.00  2.5740313
## 19             melanoleucus      telemetry   12     1004.00  3.0017337
## 20                  butleri mark-recapture    1       21.51  1.3326404
## 21                    gigal      telemetry   11      306.00  2.4857214
## 22              longissimus      telemetry   32      249.30  2.3967223
## 23              bungaroides      telemetry   24       48.79  1.6883308
## 24                 scutatus      telemetry    5      330.00  2.5185139
## 25             porphyriacus      telemetry   44      479.00  2.6803355
## 26        spilota imbricata      telemetry   33     1226.85  3.0887915
## 27               contortrix      telemetry   18      231.12  2.3638375
## 28              piscivorous      telemetry   15      188.00  2.2741578
## 29               schneideri      telemetry   11       16.95  1.2291697
## 30                    asper      telemetry    6      826.23  2.9171010
## 31                    atrox      telemetry   14      319.90  2.5050142
## 32                 cerastes      telemetry   25      106.70  2.0281644
## 33                 horridus      telemetry    6     1020.00  3.0086002
## 34                 molossus      telemetry    3      414.00  2.6170003
## 35        oreganus concolor      telemetry   21      138.70  2.1420765
## 36                   pricei      telemetry    5       67.20  1.8273693
## 37               scutulatus      telemetry   19      280.30  2.4476231
## 38                   tigris      telemetry    3      234.70  2.3705131
## 39              shedaoensis      telemetry   16      196.81  2.2940472
## 40                   raddei      telemetry   14      162.14  2.2098902
## 41                 latastei      telemetry    7       97.40  1.9885590
##    alternative.mass.reference mean.hra.m2 log10.hra
## 1                        <NA>      700.00  2.845098
## 2                        <NA>      253.00  2.403121
## 3                        <NA>   151000.00  5.178977
## 4                        <NA>   114500.00  5.058805
## 5                        <NA>     6476.00  3.811307
## 6                        <NA>  1853000.00  6.267875
## 7                        <NA>   150600.00  5.177825
## 8                        <NA>    46000.00  4.662758
## 9                        <NA>   516375.00  5.712965
## 10                       <NA>   110900.00  5.044932
## 11                       <NA>   495000.00  5.694605
## 12                       <NA>   240400.00  5.380934
## 13                       <NA>   429300.00  5.632761
## 14                       <NA>    99000.00  4.995635
## 15                       <NA>   131000.00  5.117271
## 16                       <NA>    40000.00  4.602060
## 17                       <NA>    15400.00  4.187521
## 18                       <NA>    17400.00  4.240549
## 19                       <NA>   701000.00  5.845718
## 20                       <NA>      600.00  2.778151
## 21                       <NA>   374800.00  5.573800
## 22                       <NA>    77400.00  4.888741
## 23                       <NA>    27379.00  4.437418
## 24                       <NA>    38800.00  4.588832
## 25                       <NA>    96000.00  4.982271
## 26                       <NA>   171600.00  5.234517
## 27                       <NA>   119288.89  5.076600
## 28                       <NA>    10655.00  4.027553
## 29                       <NA>      200.00  2.301030
## 30                       <NA>    60700.00  4.783189
## 31                       <NA>    54200.00  4.733999
## 32                       <NA>    28000.00  4.447158
## 33                       <NA>  2579600.00  6.411552
## 34                       <NA>    34900.00  4.542825
## 35                       <NA>  1178000.00  6.071145
## 36                       <NA>    22900.00  4.359835
## 37                       <NA>   316000.00  5.499687
## 38                       <NA>    34800.00  4.541579
## 39                       <NA>     2613.69  3.417254
## 40                       <NA>   245928.57  5.390809
## 41                       <NA>     2400.00  3.380211
##                                                                                                                                                                                                                                             hra.reference
## 1                                                                                         Clark DR. 1970. Age-Specific "Reproductive Effort" in the Worm Snake Carphophis vermis (Kennicott). Transactions of the Kansas Academy of Science 73(1), 20-24.
## 2                                                                                          Barbour RW, Harvey MJ, and Hardin JW. 1969. Home Range, Movements, and Activity of the Eastern Worm Snake, Carphophis Amoenus Amoenus. Ecology 50(3), 470-476.
## 3                                                   Carfagno GLF, Weatherhead PJ. 2008. Energetics and space use: intraspecific and interspecific comparisons of movements and home ranges of two Colubrid snakes. Journal of Animal Ecology 77, 416-424.
## 4          Klug PE, Fill J, With KA. 2011. Spatial ecology of Eastern yellow-bellies racer (Coluber constrictor flaviventris) and great plains rat snake (Pantherophis emoryi) in a contiguous tallgrass-prairie landscape. Herpetologica 67(4), 428-439.
## 5                                                                             Fitch HS. 1975. A Demographic Study of the Ringneck Snake (Diadophis punctatus) in Kansas. University of Kansas Museum of Natural History Miscellaneous Publication No. 62.
## 6                                                                                                            Dodd CK, Barichivich WJ. 2007. Movements of large snakes (Drymarchon, Masticophis_ in North-central Florida. Florida Scientist 70(1), 83-94.
## 7          Klug PE, Fill J, With KA. 2011. Spatial ecology of Eastern yellow-bellies racer (Coluber constrictor flaviventris) and great plains rat snake (Pantherophis emoryi) in a contiguous tallgrass-prairie landscape. Herpetologica 67(4), 428-439.
## 8                                                   Carfagno GLF, Weatherhead PJ. 2008. Energetics and space use: intraspecific and interspecific comparisons of movements and home ranges of two Colubrid snakes. Journal of Animal Ecology 77, 416-424.
## 9                                                                                 Plummer MV, Mills NE. 2000. Spatial Ecology and Survivorship of Resident and Translocated Hognose Snakes (Heterodonplatirhinos). Journal of Herpetology 34(4), 565-575.
## 10                                                           Herbe L, Moreau C, Blouin-Demers G, Bonnet X, Lourdais O. 2012. Two Syntopic Colubrid Snakes Differ In Their Energetic Requirements and In Their Use of Space. Herpetologica 68(3), 358-364.
## 11                             Linehan JM, Smith LL, Steen DA. 2010. Ecology of the Eastern kingsnake (Lampropeltis getula getula) in a longleaf pine (Pinus palustris) forest in Southwestern Georgia. Herpetological Conservation Biology 5(1), 94-101.
## 12                                                                                                                             Row JR, Blouin-Demers G. 2006. Kernels are not Accurate Estimators of Home-Range Size For Herpetofauna. Copeia 4, 797-802.
## 13                                                                                                           Dodd CK, Barichivich WJ. 2007. Movements of large snakes (Drymarchon, Masticophis_ in North-central Florida. Florida Scientist 70(1), 83-94.
## 14                                                                                                        Madsen T. 1984. Movements, Home Range Size and Habitat Use of Radio-Tracked Grass Snakes (Natrix natrix) in Southern Sweden. Copeia 3, 707-713.
## 15                                                                     Roe JH, Kingsbury BA, Herbert NR. 2004. Comparative water snake ecology: conservation of mobile animals that use temporally dynamic resources. Biological Conservation 118, 79-89.
## 16                                                                     Roe JH, Kingsbury BA, Herbert NR. 2004. Comparative water snake ecology: conservation of mobile animals that use temporally dynamic resources. Biological Conservation 118, 79-89.
## 17                                                                       Lee H-J, Lee J-H, Park D. 2011. Habitat Use and Movement Patterns of the Viviparous Aquatic Snake, Oocatochus rufodorsatus, from Northeast Asia. Zoological Science 28, 593-599.
## 18                                                                                                                    Rodriguez-Robles JA. 2003. Home Ranges of Gopher Snakes (Pituophis catenifer, Colubridae) in Central California. Copeia 2, 391-396.
## 19                                                                                   Miller GJ, Smith LL, Johnson SA, Franz R. 2012. Home Range Size and Habitat Selection in the Florida Pine Snake (Pituophis melanoleucus mugitus). Copeia 4, 706-713.
## 20                                                                                                              Freedman B, Catling PM. 1979. Movements of sympatric species of snakes at Amherstburg, Ontatio. Canadian Field-Naturalist 93(4): 399-404.
## 21    Wylie G, Amarello M. 2006.  For the Bank Protection Project on the Left Bank of the Colusa Basin Drainage Canal in Reclamation District. Progress report for the U.S. Army Corps of Engineers.\nSacramento River Bank Protection Project, Phase II.
## 22                                                           Herbe L, Moreau C, Blouin-Demers G, Bonnet X, Lourdais O. 2012. Two Syntopic Colubrid Snakes Differ In Their Energetic Requirements and In Their Use of Space. Herpetologica 68(3), 358-364.
## 23                                                                                  Webb JK, Shine R. 1997. A Field Study of Spatial Ecology and Movements of a Threatened Snake Species, Hoplocephalus bungaroides. Biological Conservation 82, 203-217.
## 24                                                                   Butler H, Malone B, Clemann N. 2005. The effects of translocation on the spatial ecology of tiger snakes (Notechis scutatus) in a suburban landscape. Wildlife Research 32, 165-171.
## 25                                                            Shine R. 1987. Intraspecific Variation in Thermoregulation, Movements and Habitat Use by Australian Blacksnakes, Pseudechis porphyriacus (Elapidae). Journal of Herpetology 21(3), 165-177.
## 26                                                                 Pearson D, Shine R, Williams A. 2005. Spatial ecology of a threatened python (Morelia spilota imbricata) and the effects of anthropogenic habitat change. Austral Ecology 30, 261-274.
## 27                                        Smith CF, Schuett GW, Early RL, Schwenk K. 2009. The Spatial and Resproductive Ecology of the Copperhead (Agkistrodon contortrix) at the Northeastern Extrme of its Range. Herpetological Monographs 23, 45-73.
## 28                                                                                                             Roth ED. 2005. Spatial Ecology of a Cottonmouth (Agkistrodon piscivorus) Population in East Texas. Journal of Herpetology  39(2), 308-312.
## 29                                                                                                                  Maritz B, Alexander GJ. 2012. Dwarfs on the Move: Spatial Ecology of the World's Smallest Viper, Bitis schneideri. Copeia 1, 115-120.
## 30                                        Wasko DK, Sasa M. 2012. Food resources influence spatial ecology, habitat selection, and foraging behavior in an ambush-hunting snake (Viperidae: Bothrops asper): an experimental study. Zoology 115, 179-187.
## 31                                                                                                             Beck DD. 1995. Ecology and Energetics of Three Sympatric Rattlesnake Species in the Sonoran Desert. Journal of Herpetology 29(2), 211-223.
## 32                                                                                                                      Secor SM. 1994. Ecological Significance of Movements and Activity Range for the Sidewinder, Crotalus cerastes. Copeia 3, 631-645.
## 33 Bauder JM, Blodgett D, Briggs KV, Jenkins CL. 2011. The Ecology of Timber Rattlesnakes (Crotalus horridus) in Vermont: A First Year Progress Report Submitted to the Vermont Department of Fish and Wildlife. Vermont Department of Fish and Wildlife.
## 34                                                                                                             Beck DD. 1995. Ecology and Energetics of Three Sympatric Rattlesnake Species in the Sonoran Desert. Journal of Herpetology 29(2), 211-223.
## 35                                                                                           Parker JM, Anderson SH. 2007. Ecology and Behavior of the Midget Faded Rattlesnake (Crotalus Oreganus Concolor) in Wyoming. Journal of Herpetology 1, 41-51.
## 36                                                           Prival DB, Goode MJ, Swann DE, Schwalbe CR, Schroff MJ. 2002. Natural History of a Northern Population of Twin-Spotted Rattlesnakes, Crotalus pricei. Journal of Herpetology 36(4), 598-607.
## 37                                                                                                                                                     Cardwell MD. 2008. The reproductive ecology of Mohave rattlesnakes. Journal of Zoology 274, 65-76.
## 38                                                                                                             Beck DD. 1995. Ecology and Energetics of Three Sympatric Rattlesnake Species in the Sonoran Desert. Journal of Herpetology 29(2), 211-223.
## 39                                                                                           Shine R, Sun L-X. 2003. Attack strategy of an ambush predator: which attributes of the prey trigger a pit-viper\x92s strike? Functional Ecology 17, 340-348.
## 40                                                                                         Ettling JA, Aghasyan LA, Aghasyan AL, Parker PG. 2013. Spatial Ecology of Armenian Vipers, Montivipera raddei, in a Human-Modified Landscape. Copeia 1, 64-71.
## 41                                                                                      Brito JC. 2003. Seasonal Variation in Movements, Home Range, and Habitat Use by Male Vipera latastei in Northern Portugal. Journal of Herpetology 37(1), 155-160.
##          realm thermoregulation locomotion trophic.guild dimension preymass
## 1  terrestrial        ectotherm   crawling     carnivore         2       NA
## 2  terrestrial        ectotherm   crawling     carnivore         2       NA
## 3  terrestrial        ectotherm   crawling     carnivore         2   617.94
## 4  terrestrial        ectotherm   crawling     carnivore         2   160.56
## 5  terrestrial        ectotherm   crawling     carnivore         2       NA
## 6  terrestrial        ectotherm   crawling     carnivore         2       NA
## 7  terrestrial        ectotherm   crawling     carnivore         2       NA
## 8  terrestrial        ectotherm   crawling     carnivore         2    71.50
## 9  terrestrial        ectotherm   crawling     carnivore         2       NA
## 10 terrestrial        ectotherm   crawling     carnivore         2    40.78
## 11 terrestrial        ectotherm   crawling     carnivore         2       NA
## 12 terrestrial        ectotherm   crawling     carnivore         2       NA
## 13 terrestrial        ectotherm   crawling     carnivore         2       NA
## 14 terrestrial        ectotherm   crawling     carnivore         2    13.99
## 15 terrestrial        ectotherm   crawling     carnivore         2       NA
## 16 terrestrial        ectotherm   crawling     carnivore         2    97.72
## 17 terrestrial        ectotherm   crawling     carnivore         2       NA
## 18 terrestrial        ectotherm   crawling     carnivore         2       NA
## 19 terrestrial        ectotherm   crawling     carnivore         2    53.75
## 20 terrestrial        ectotherm   crawling     carnivore         2       NA
## 21 terrestrial        ectotherm   crawling     carnivore         2       NA
## 22 terrestrial        ectotherm    walking     carnivore         2       NA
## 23 terrestrial        ectotherm   crawling     carnivore         2       NA
## 24 terrestrial        ectotherm   crawling     carnivore         2    45.90
## 25 terrestrial        ectotherm   crawling     carnivore         2       NA
## 26 terrestrial        ectotherm   crawling     carnivore         2       NA
## 27 terrestrial        ectotherm   crawling     carnivore         2       NA
## 28 terrestrial        ectotherm   crawling     carnivore         2    51.93
## 29 terrestrial        ectotherm   crawling     carnivore         2       NA
## 30 terrestrial        ectotherm   crawling     carnivore         2   165.25
## 31 terrestrial        ectotherm   crawling     carnivore         2    12.80
## 32 terrestrial        ectotherm   crawling     carnivore         2       NA
## 33 terrestrial        ectotherm   crawling     carnivore         2  2684.21
## 34 terrestrial        ectotherm   crawling     carnivore         2       NA
## 35 terrestrial        ectotherm   crawling     carnivore         2    51.56
## 36 terrestrial        ectotherm   crawling     carnivore         2       NA
## 37 terrestrial        ectotherm   crawling     carnivore         2       NA
## 38 terrestrial        ectotherm   crawling     carnivore         2       NA
## 39 terrestrial        ectotherm   crawling     carnivore         2    14.00
## 40 terrestrial        ectotherm   crawling     carnivore         2       NA
## 41 terrestrial        ectotherm   crawling     carnivore         2     8.97
##    log10.preymass  PPMR
## 1              NA    NA
## 2              NA    NA
## 3       2.7909463  0.90
## 4       2.2056374  0.90
## 5              NA    NA
## 6              NA    NA
## 7              NA    NA
## 8       1.8543060  8.99
## 9              NA    NA
## 10      1.6104472  5.74
## 11             NA    NA
## 12             NA    NA
## 13             NA    NA
## 14      1.1458177  5.61
## 15             NA    NA
## 16      1.9899835 14.63
## 17             NA    NA
## 18             NA    NA
## 19      1.7303785 18.68
## 20             NA    NA
## 21             NA    NA
## 22             NA    NA
## 23             NA    NA
## 24      1.6618127  7.19
## 25             NA    NA
## 26             NA    NA
## 27             NA    NA
## 28      1.7154183  3.62
## 29             NA    NA
## 30      2.2181415  5.00
## 31      1.1072100 25.00
## 32             NA    NA
## 33      3.4288165  0.38
## 34             NA    NA
## 35      1.7123129  2.69
## 36             NA    NA
## 37             NA    NA
## 38             NA    NA
## 39      1.1461280 14.06
## 40             NA    NA
## 41      0.9527924 10.86
##                                                                                                                                                                                                                                                                               prey.size.reference
## 1                                                                                                                                                                                                                                                                                            <NA>
## 2                                                                                                                                                                                                                                                                                            <NA>
## 3                                                                                             Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 4                                                                                             Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 5                                                                                                                                                                                                                                                                                            <NA>
## 6                                                                                                                                                                                                                                                                                            <NA>
## 7                                                                                                                                                                                                                                                                                            <NA>
## 8                                                                                                               Weatherhead PJ, Blouin-Demers G, Cavey KM. 2003. Seasonal and Prey-size Dietary Patterns of Black Ratsnakes (Elaphe obsoleta obsoleta). American Midland Naturalist 150, 275-281.
## 9                                                                                                                                                                                                                                                                                            <NA>
## 10                                                                                            Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 11                                                                                                                                                                                                                                                                                           <NA>
## 12                                                                                                                                                                                                                                                                                           <NA>
## 13                                                                                                                                                                                                                                                                                           <NA>
## 14                                                                                                                               Gregory PT, Isaac LA. 2004. Food Habits of the Grass Snake in Southeastern England: Is Natrix natrix a Generalist Predator? Journal of Herpetology 38(1): 88-95.
## 15                                                                                                                                                                                                                                                                                           <NA>
## 16                                                                                                                                                                             King RB. 2002. Predicted and Observed Maximum Prey Size - Snake Size Allometry. Functional Ecology 16(6), 766-772.
## 17                                                                                                                                                                                                                                                                                           <NA>
## 18                                                                                                                                                                                                                                                                                           <NA>
## 19                                                                                            Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 20                                                                                                                                                                                                                                                                                           <NA>
## 21                                                                                                                                                                                                                                                                                           <NA>
## 22                                                                                                                                                                                                                                                                                           <NA>
## 23                                                                                                                                                                                                                                                                                           <NA>
## 24                                                                                            Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 25                                                                                                                                                                                                                                                                                           <NA>
## 26                                                                                                                                                                                                                                                                                           <NA>
## 27                                                                                                                                                                                                                                                                                           <NA>
## 28                                                                                                                   Vincent SW, Herrel A, Irschick DJ. 2004. Sexual dimorphism in head shape and diet in the cottonmouth. Journal of Zoology, London 264, 53-59.\nsnake (Agkistrodon piscivorus)
## 29                                                                                                                                                                                                                                                                                           <NA>
## 30 Martins M, Marques OAV, Sazima I. 2002. Ecological and phylogenetic correlates of feeding habits in Neotropical pitvipers of the genus Bothrops. In G. W. Schuett, M. E. Douglas, M. H\xf6ggren, and H. W. Greene (eds.), Biology of the Vipers. Eagle Mountain Publishing, Eagle Mountain, UT
## 31                                                                                            Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 32                                                                                                                                                                                                                                                                                           <NA>
## 33                                                                                                                                                                                      Clark RW. 2002. Diet of the timber rattlesnake, Crotalus horridus. Journal of Herpetology 36(3), 494-499.
## 34                                                                                                                                                                                                                                                                                           <NA>
## 35                                                                                            Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 36                                                                                                                                                                                                                                                                                           <NA>
## 37                                                                                                                                                                                                                                                                                           <NA>
## 38                                                                                                                                                                                                                                                                                           <NA>
## 39                                                                                                                                   Shine R, Sun L-X. 2003. Attack strategy of an ambush predator: which attributes of the prey trigger a pit-viper\x92s strike? Functional Ecology 17, 340-348.
## 40                                                                                                                                                                                                                                                                                           <NA>
## 41                                           Jaksic FM, Delibes M.1987. A Comparative Analysis of Food-Niche Relationships and Trophic Guild Structure in TwoAssemblages of Vertebrate Predators Differing in Species Richness: Causes, Correlations, and Consequences. Oecologia 71(3), 461-472.
```

```r
arrange(snakes, mean.hra.m2)
```

```
##     taxon                common.name    class    order     family         genus
## 1  snakes        namaqua dwarf adder reptilia squamata  viperidae         bitis
## 2  snakes         eastern worm snake reptilia squamata colubridae     carphopis
## 3  snakes       butlers garter snake reptilia squamata colubridae    thamnophis
## 4  snakes         western worm snake reptilia squamata colubridae     carphopis
## 5  snakes            snubnosed viper reptilia squamata  viperidae        vipera
## 6  snakes          chinese pit viper reptilia squamata  viperidae      gloydius
## 7  snakes             ringneck snake reptilia squamata colubridae     diadophis
## 8  snakes                cottonmouth reptilia squamata  viperidae   agkistrodon
## 9  snakes         redbacked ratsnake reptilia squamata colubridae    oocatochus
## 10 snakes               gopher snake reptilia squamata colubridae     pituophis
## 11 snakes   twin-spotted rattlesnake reptilia squamata  viperidae      crotalus
## 12 snakes          broadheaded snake reptilia squamata   elapidae hoplocephalus
## 13 snakes                 sidewinder reptilia squamata  viperidae      crotalus
## 14 snakes          tiger rattlesnake reptilia squamata  viperidae      crotalus
## 15 snakes    blacktailed rattlesnake reptilia squamata  viperidae      crotalus
## 16 snakes                tiger snake reptilia squamata   elapidae      notechis
## 17 snakes        Northern watersnake reptilia squamata colubridae       nerodia
## 18 snakes           western ratsnake reptilia squamata colubridae        elaphe
## 19 snakes        western diamondback reptilia squamata  viperidae      crotalus
## 20 snakes               fer-de-lance reptilia squamata  viperidae      bothrops
## 21 snakes          Aesculapian snake reptilia squamata colubridae       zamenis
## 22 snakes                 blacksnake reptilia squamata   elapidae    pseudechis
## 23 snakes                grass snake reptilia squamata colubridae        natrix
## 24 snakes         European whipsnake reptilia squamata colubridae     hierophis
## 25 snakes       yellow bellied racer reptilia squamata colubridae       coluber
## 26 snakes                 copperhead reptilia squamata  viperidae   agkistrodon
## 27 snakes     copperbelly watersnake reptilia squamata colubridae       nerodia
## 28 snakes      great plains ratsnake reptilia squamata colubridae        elaphe
## 29 snakes                      racer reptilia squamata colubridae       coluber
## 30 snakes southwestern carpet python reptilia squamata pythonidae       morelia
## 31 snakes                  milksnake reptilia squamata colubridae  lampropeltis
## 32 snakes             Armenian viper reptilia squamata  viperidae   montivipera
## 33 snakes         Mojave rattlesnake reptilia squamata  viperidae      crotalus
## 34 snakes        giant garter snakes reptilia squamata colubridae    thamnophis
## 35 snakes                  coachwhip reptilia squamata colubridae   masticophis
## 36 snakes          Eastern kingsnake reptilia squamata colubridae  lampropeltis
## 37 snakes              hognose snake reptilia squamata colubridae     heterodon
## 38 snakes                 pine snake reptilia squamata colubridae     pituophis
## 39 snakes   midget faded rattlesnake reptilia squamata  viperidae      crotalus
## 40 snakes       eastern indigo snake reptilia squamata colubridae    drymarchon
## 41 snakes         timber rattlesnake reptilia squamata  viperidae      crotalus
##                     species  primarymethod    N mean.mass.g log10.mass
## 1                schneideri      telemetry   11       16.95  1.2291697
## 2                   viridis       radiotag   10        3.65  0.5622929
## 3                   butleri mark-recapture    1       21.51  1.3326404
## 4                    vermis       radiotag    1        3.46  0.5390761
## 5                  latastei      telemetry    7       97.40  1.9885590
## 6               shedaoensis      telemetry   16      196.81  2.2940472
## 7                 punctatus mark-recapture <NA>        9.00  0.9542425
## 8               piscivorous      telemetry   15      188.00  2.2741578
## 9              rufodorsatus      telemetry   21       62.50  1.7958800
## 10                catenifer      telemetry    4      375.00  2.5740313
## 11                   pricei      telemetry    5       67.20  1.8273693
## 12              bungaroides      telemetry   24       48.79  1.6883308
## 13                 cerastes      telemetry   25      106.70  2.0281644
## 14                   tigris      telemetry    3      234.70  2.3705131
## 15                 molossus      telemetry    3      414.00  2.6170003
## 16                 scutatus      telemetry    5      330.00  2.5185139
## 17                 sipeodon      telemetry   13      190.55  2.2800090
## 18                 obsoleta      telemetry   18      642.80  2.8080759
## 19                    atrox      telemetry   14      319.90  2.5050142
## 20                    asper      telemetry    6      826.23  2.9171010
## 21              longissimus      telemetry   32      249.30  2.3967223
## 22             porphyriacus      telemetry   44      479.00  2.6803355
## 23                   natrix      telemetry    4       78.50  1.8948697
## 24             viridiflavus      telemetry   32      234.10  2.3694014
## 25 constrictor flaviventris      telemetry   12      144.50  2.1598678
## 26               contortrix      telemetry   18      231.12  2.3638375
## 27            erythrogaster      telemetry   15      313.24  2.4958772
## 28           guttata emoryi      telemetry   12      256.70  2.4094259
## 29              constrictor      telemetry   15      556.15  2.7451919
## 30        spilota imbricata      telemetry   33     1226.85  3.0887915
## 31               triangulum      telemetry   10      165.00  2.2174839
## 32                   raddei      telemetry   14      162.14  2.2098902
## 33               scutulatus      telemetry   19      280.30  2.4476231
## 34                    gigal      telemetry   11      306.00  2.4857214
## 35                flagellum      telemetry    4      534.50  2.7279477
## 36            getula getula      telemetry   12      315.72  2.4993021
## 37              platirhinos      telemetry    8      147.32  2.1682617
## 38             melanoleucus      telemetry   12     1004.00  3.0017337
## 39        oreganus concolor      telemetry   21      138.70  2.1420765
## 40                  couperi      telemetry    1      450.00  2.6532125
## 41                 horridus      telemetry    6     1020.00  3.0086002
##    alternative.mass.reference mean.hra.m2 log10.hra
## 1                        <NA>      200.00  2.301030
## 2                        <NA>      253.00  2.403121
## 3                        <NA>      600.00  2.778151
## 4                        <NA>      700.00  2.845098
## 5                        <NA>     2400.00  3.380211
## 6                        <NA>     2613.69  3.417254
## 7                        <NA>     6476.00  3.811307
## 8                        <NA>    10655.00  4.027553
## 9                        <NA>    15400.00  4.187521
## 10                       <NA>    17400.00  4.240549
## 11                       <NA>    22900.00  4.359835
## 12                       <NA>    27379.00  4.437418
## 13                       <NA>    28000.00  4.447158
## 14                       <NA>    34800.00  4.541579
## 15                       <NA>    34900.00  4.542825
## 16                       <NA>    38800.00  4.588832
## 17                       <NA>    40000.00  4.602060
## 18                       <NA>    46000.00  4.662758
## 19                       <NA>    54200.00  4.733999
## 20                       <NA>    60700.00  4.783189
## 21                       <NA>    77400.00  4.888741
## 22                       <NA>    96000.00  4.982271
## 23                       <NA>    99000.00  4.995635
## 24                       <NA>   110900.00  5.044932
## 25                       <NA>   114500.00  5.058805
## 26                       <NA>   119288.89  5.076600
## 27                       <NA>   131000.00  5.117271
## 28                       <NA>   150600.00  5.177825
## 29                       <NA>   151000.00  5.178977
## 30                       <NA>   171600.00  5.234517
## 31                       <NA>   240400.00  5.380934
## 32                       <NA>   245928.57  5.390809
## 33                       <NA>   316000.00  5.499687
## 34                       <NA>   374800.00  5.573800
## 35                       <NA>   429300.00  5.632761
## 36                       <NA>   495000.00  5.694605
## 37                       <NA>   516375.00  5.712965
## 38                       <NA>   701000.00  5.845718
## 39                       <NA>  1178000.00  6.071145
## 40                       <NA>  1853000.00  6.267875
## 41                       <NA>  2579600.00  6.411552
##                                                                                                                                                                                                                                             hra.reference
## 1                                                                                                                   Maritz B, Alexander GJ. 2012. Dwarfs on the Move: Spatial Ecology of the World's Smallest Viper, Bitis schneideri. Copeia 1, 115-120.
## 2                                                                                          Barbour RW, Harvey MJ, and Hardin JW. 1969. Home Range, Movements, and Activity of the Eastern Worm Snake, Carphophis Amoenus Amoenus. Ecology 50(3), 470-476.
## 3                                                                                                               Freedman B, Catling PM. 1979. Movements of sympatric species of snakes at Amherstburg, Ontatio. Canadian Field-Naturalist 93(4): 399-404.
## 4                                                                                         Clark DR. 1970. Age-Specific "Reproductive Effort" in the Worm Snake Carphophis vermis (Kennicott). Transactions of the Kansas Academy of Science 73(1), 20-24.
## 5                                                                                       Brito JC. 2003. Seasonal Variation in Movements, Home Range, and Habitat Use by Male Vipera latastei in Northern Portugal. Journal of Herpetology 37(1), 155-160.
## 6                                                                                            Shine R, Sun L-X. 2003. Attack strategy of an ambush predator: which attributes of the prey trigger a pit-viper\x92s strike? Functional Ecology 17, 340-348.
## 7                                                                             Fitch HS. 1975. A Demographic Study of the Ringneck Snake (Diadophis punctatus) in Kansas. University of Kansas Museum of Natural History Miscellaneous Publication No. 62.
## 8                                                                                                              Roth ED. 2005. Spatial Ecology of a Cottonmouth (Agkistrodon piscivorus) Population in East Texas. Journal of Herpetology  39(2), 308-312.
## 9                                                                        Lee H-J, Lee J-H, Park D. 2011. Habitat Use and Movement Patterns of the Viviparous Aquatic Snake, Oocatochus rufodorsatus, from Northeast Asia. Zoological Science 28, 593-599.
## 10                                                                                                                    Rodriguez-Robles JA. 2003. Home Ranges of Gopher Snakes (Pituophis catenifer, Colubridae) in Central California. Copeia 2, 391-396.
## 11                                                           Prival DB, Goode MJ, Swann DE, Schwalbe CR, Schroff MJ. 2002. Natural History of a Northern Population of Twin-Spotted Rattlesnakes, Crotalus pricei. Journal of Herpetology 36(4), 598-607.
## 12                                                                                  Webb JK, Shine R. 1997. A Field Study of Spatial Ecology and Movements of a Threatened Snake Species, Hoplocephalus bungaroides. Biological Conservation 82, 203-217.
## 13                                                                                                                      Secor SM. 1994. Ecological Significance of Movements and Activity Range for the Sidewinder, Crotalus cerastes. Copeia 3, 631-645.
## 14                                                                                                             Beck DD. 1995. Ecology and Energetics of Three Sympatric Rattlesnake Species in the Sonoran Desert. Journal of Herpetology 29(2), 211-223.
## 15                                                                                                             Beck DD. 1995. Ecology and Energetics of Three Sympatric Rattlesnake Species in the Sonoran Desert. Journal of Herpetology 29(2), 211-223.
## 16                                                                   Butler H, Malone B, Clemann N. 2005. The effects of translocation on the spatial ecology of tiger snakes (Notechis scutatus) in a suburban landscape. Wildlife Research 32, 165-171.
## 17                                                                     Roe JH, Kingsbury BA, Herbert NR. 2004. Comparative water snake ecology: conservation of mobile animals that use temporally dynamic resources. Biological Conservation 118, 79-89.
## 18                                                  Carfagno GLF, Weatherhead PJ. 2008. Energetics and space use: intraspecific and interspecific comparisons of movements and home ranges of two Colubrid snakes. Journal of Animal Ecology 77, 416-424.
## 19                                                                                                             Beck DD. 1995. Ecology and Energetics of Three Sympatric Rattlesnake Species in the Sonoran Desert. Journal of Herpetology 29(2), 211-223.
## 20                                        Wasko DK, Sasa M. 2012. Food resources influence spatial ecology, habitat selection, and foraging behavior in an ambush-hunting snake (Viperidae: Bothrops asper): an experimental study. Zoology 115, 179-187.
## 21                                                           Herbe L, Moreau C, Blouin-Demers G, Bonnet X, Lourdais O. 2012. Two Syntopic Colubrid Snakes Differ In Their Energetic Requirements and In Their Use of Space. Herpetologica 68(3), 358-364.
## 22                                                            Shine R. 1987. Intraspecific Variation in Thermoregulation, Movements and Habitat Use by Australian Blacksnakes, Pseudechis porphyriacus (Elapidae). Journal of Herpetology 21(3), 165-177.
## 23                                                                                                        Madsen T. 1984. Movements, Home Range Size and Habitat Use of Radio-Tracked Grass Snakes (Natrix natrix) in Southern Sweden. Copeia 3, 707-713.
## 24                                                           Herbe L, Moreau C, Blouin-Demers G, Bonnet X, Lourdais O. 2012. Two Syntopic Colubrid Snakes Differ In Their Energetic Requirements and In Their Use of Space. Herpetologica 68(3), 358-364.
## 25         Klug PE, Fill J, With KA. 2011. Spatial ecology of Eastern yellow-bellies racer (Coluber constrictor flaviventris) and great plains rat snake (Pantherophis emoryi) in a contiguous tallgrass-prairie landscape. Herpetologica 67(4), 428-439.
## 26                                        Smith CF, Schuett GW, Early RL, Schwenk K. 2009. The Spatial and Resproductive Ecology of the Copperhead (Agkistrodon contortrix) at the Northeastern Extrme of its Range. Herpetological Monographs 23, 45-73.
## 27                                                                     Roe JH, Kingsbury BA, Herbert NR. 2004. Comparative water snake ecology: conservation of mobile animals that use temporally dynamic resources. Biological Conservation 118, 79-89.
## 28         Klug PE, Fill J, With KA. 2011. Spatial ecology of Eastern yellow-bellies racer (Coluber constrictor flaviventris) and great plains rat snake (Pantherophis emoryi) in a contiguous tallgrass-prairie landscape. Herpetologica 67(4), 428-439.
## 29                                                  Carfagno GLF, Weatherhead PJ. 2008. Energetics and space use: intraspecific and interspecific comparisons of movements and home ranges of two Colubrid snakes. Journal of Animal Ecology 77, 416-424.
## 30                                                                 Pearson D, Shine R, Williams A. 2005. Spatial ecology of a threatened python (Morelia spilota imbricata) and the effects of anthropogenic habitat change. Austral Ecology 30, 261-274.
## 31                                                                                                                             Row JR, Blouin-Demers G. 2006. Kernels are not Accurate Estimators of Home-Range Size For Herpetofauna. Copeia 4, 797-802.
## 32                                                                                         Ettling JA, Aghasyan LA, Aghasyan AL, Parker PG. 2013. Spatial Ecology of Armenian Vipers, Montivipera raddei, in a Human-Modified Landscape. Copeia 1, 64-71.
## 33                                                                                                                                                     Cardwell MD. 2008. The reproductive ecology of Mohave rattlesnakes. Journal of Zoology 274, 65-76.
## 34    Wylie G, Amarello M. 2006.  For the Bank Protection Project on the Left Bank of the Colusa Basin Drainage Canal in Reclamation District. Progress report for the U.S. Army Corps of Engineers.\nSacramento River Bank Protection Project, Phase II.
## 35                                                                                                           Dodd CK, Barichivich WJ. 2007. Movements of large snakes (Drymarchon, Masticophis_ in North-central Florida. Florida Scientist 70(1), 83-94.
## 36                             Linehan JM, Smith LL, Steen DA. 2010. Ecology of the Eastern kingsnake (Lampropeltis getula getula) in a longleaf pine (Pinus palustris) forest in Southwestern Georgia. Herpetological Conservation Biology 5(1), 94-101.
## 37                                                                                Plummer MV, Mills NE. 2000. Spatial Ecology and Survivorship of Resident and Translocated Hognose Snakes (Heterodonplatirhinos). Journal of Herpetology 34(4), 565-575.
## 38                                                                                   Miller GJ, Smith LL, Johnson SA, Franz R. 2012. Home Range Size and Habitat Selection in the Florida Pine Snake (Pituophis melanoleucus mugitus). Copeia 4, 706-713.
## 39                                                                                           Parker JM, Anderson SH. 2007. Ecology and Behavior of the Midget Faded Rattlesnake (Crotalus Oreganus Concolor) in Wyoming. Journal of Herpetology 1, 41-51.
## 40                                                                                                           Dodd CK, Barichivich WJ. 2007. Movements of large snakes (Drymarchon, Masticophis_ in North-central Florida. Florida Scientist 70(1), 83-94.
## 41 Bauder JM, Blodgett D, Briggs KV, Jenkins CL. 2011. The Ecology of Timber Rattlesnakes (Crotalus horridus) in Vermont: A First Year Progress Report Submitted to the Vermont Department of Fish and Wildlife. Vermont Department of Fish and Wildlife.
##          realm thermoregulation locomotion trophic.guild dimension preymass
## 1  terrestrial        ectotherm   crawling     carnivore         2       NA
## 2  terrestrial        ectotherm   crawling     carnivore         2       NA
## 3  terrestrial        ectotherm   crawling     carnivore         2       NA
## 4  terrestrial        ectotherm   crawling     carnivore         2       NA
## 5  terrestrial        ectotherm   crawling     carnivore         2     8.97
## 6  terrestrial        ectotherm   crawling     carnivore         2    14.00
## 7  terrestrial        ectotherm   crawling     carnivore         2       NA
## 8  terrestrial        ectotherm   crawling     carnivore         2    51.93
## 9  terrestrial        ectotherm   crawling     carnivore         2       NA
## 10 terrestrial        ectotherm   crawling     carnivore         2       NA
## 11 terrestrial        ectotherm   crawling     carnivore         2       NA
## 12 terrestrial        ectotherm   crawling     carnivore         2       NA
## 13 terrestrial        ectotherm   crawling     carnivore         2       NA
## 14 terrestrial        ectotherm   crawling     carnivore         2       NA
## 15 terrestrial        ectotherm   crawling     carnivore         2       NA
## 16 terrestrial        ectotherm   crawling     carnivore         2    45.90
## 17 terrestrial        ectotherm   crawling     carnivore         2    97.72
## 18 terrestrial        ectotherm   crawling     carnivore         2    71.50
## 19 terrestrial        ectotherm   crawling     carnivore         2    12.80
## 20 terrestrial        ectotherm   crawling     carnivore         2   165.25
## 21 terrestrial        ectotherm    walking     carnivore         2       NA
## 22 terrestrial        ectotherm   crawling     carnivore         2       NA
## 23 terrestrial        ectotherm   crawling     carnivore         2    13.99
## 24 terrestrial        ectotherm   crawling     carnivore         2    40.78
## 25 terrestrial        ectotherm   crawling     carnivore         2   160.56
## 26 terrestrial        ectotherm   crawling     carnivore         2       NA
## 27 terrestrial        ectotherm   crawling     carnivore         2       NA
## 28 terrestrial        ectotherm   crawling     carnivore         2       NA
## 29 terrestrial        ectotherm   crawling     carnivore         2   617.94
## 30 terrestrial        ectotherm   crawling     carnivore         2       NA
## 31 terrestrial        ectotherm   crawling     carnivore         2       NA
## 32 terrestrial        ectotherm   crawling     carnivore         2       NA
## 33 terrestrial        ectotherm   crawling     carnivore         2       NA
## 34 terrestrial        ectotherm   crawling     carnivore         2       NA
## 35 terrestrial        ectotherm   crawling     carnivore         2       NA
## 36 terrestrial        ectotherm   crawling     carnivore         2       NA
## 37 terrestrial        ectotherm   crawling     carnivore         2       NA
## 38 terrestrial        ectotherm   crawling     carnivore         2    53.75
## 39 terrestrial        ectotherm   crawling     carnivore         2    51.56
## 40 terrestrial        ectotherm   crawling     carnivore         2       NA
## 41 terrestrial        ectotherm   crawling     carnivore         2  2684.21
##    log10.preymass  PPMR
## 1              NA    NA
## 2              NA    NA
## 3              NA    NA
## 4              NA    NA
## 5       0.9527924 10.86
## 6       1.1461280 14.06
## 7              NA    NA
## 8       1.7154183  3.62
## 9              NA    NA
## 10             NA    NA
## 11             NA    NA
## 12             NA    NA
## 13             NA    NA
## 14             NA    NA
## 15             NA    NA
## 16      1.6618127  7.19
## 17      1.9899835 14.63
## 18      1.8543060  8.99
## 19      1.1072100 25.00
## 20      2.2181415  5.00
## 21             NA    NA
## 22             NA    NA
## 23      1.1458177  5.61
## 24      1.6104472  5.74
## 25      2.2056374  0.90
## 26             NA    NA
## 27             NA    NA
## 28             NA    NA
## 29      2.7909463  0.90
## 30             NA    NA
## 31             NA    NA
## 32             NA    NA
## 33             NA    NA
## 34             NA    NA
## 35             NA    NA
## 36             NA    NA
## 37             NA    NA
## 38      1.7303785 18.68
## 39      1.7123129  2.69
## 40             NA    NA
## 41      3.4288165  0.38
##                                                                                                                                                                                                                                                                               prey.size.reference
## 1                                                                                                                                                                                                                                                                                            <NA>
## 2                                                                                                                                                                                                                                                                                            <NA>
## 3                                                                                                                                                                                                                                                                                            <NA>
## 4                                                                                                                                                                                                                                                                                            <NA>
## 5                                            Jaksic FM, Delibes M.1987. A Comparative Analysis of Food-Niche Relationships and Trophic Guild Structure in TwoAssemblages of Vertebrate Predators Differing in Species Richness: Causes, Correlations, and Consequences. Oecologia 71(3), 461-472.
## 6                                                                                                                                    Shine R, Sun L-X. 2003. Attack strategy of an ambush predator: which attributes of the prey trigger a pit-viper\x92s strike? Functional Ecology 17, 340-348.
## 7                                                                                                                                                                                                                                                                                            <NA>
## 8                                                                                                                    Vincent SW, Herrel A, Irschick DJ. 2004. Sexual dimorphism in head shape and diet in the cottonmouth. Journal of Zoology, London 264, 53-59.\nsnake (Agkistrodon piscivorus)
## 9                                                                                                                                                                                                                                                                                            <NA>
## 10                                                                                                                                                                                                                                                                                           <NA>
## 11                                                                                                                                                                                                                                                                                           <NA>
## 12                                                                                                                                                                                                                                                                                           <NA>
## 13                                                                                                                                                                                                                                                                                           <NA>
## 14                                                                                                                                                                                                                                                                                           <NA>
## 15                                                                                                                                                                                                                                                                                           <NA>
## 16                                                                                            Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 17                                                                                                                                                                             King RB. 2002. Predicted and Observed Maximum Prey Size - Snake Size Allometry. Functional Ecology 16(6), 766-772.
## 18                                                                                                              Weatherhead PJ, Blouin-Demers G, Cavey KM. 2003. Seasonal and Prey-size Dietary Patterns of Black Ratsnakes (Elaphe obsoleta obsoleta). American Midland Naturalist 150, 275-281.
## 19                                                                                            Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 20 Martins M, Marques OAV, Sazima I. 2002. Ecological and phylogenetic correlates of feeding habits in Neotropical pitvipers of the genus Bothrops. In G. W. Schuett, M. E. Douglas, M. H\xf6ggren, and H. W. Greene (eds.), Biology of the Vipers. Eagle Mountain Publishing, Eagle Mountain, UT
## 21                                                                                                                                                                                                                                                                                           <NA>
## 22                                                                                                                                                                                                                                                                                           <NA>
## 23                                                                                                                               Gregory PT, Isaac LA. 2004. Food Habits of the Grass Snake in Southeastern England: Is Natrix natrix a Generalist Predator? Journal of Herpetology 38(1): 88-95.
## 24                                                                                            Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 25                                                                                            Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 26                                                                                                                                                                                                                                                                                           <NA>
## 27                                                                                                                                                                                                                                                                                           <NA>
## 28                                                                                                                                                                                                                                                                                           <NA>
## 29                                                                                            Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 30                                                                                                                                                                                                                                                                                           <NA>
## 31                                                                                                                                                                                                                                                                                           <NA>
## 32                                                                                                                                                                                                                                                                                           <NA>
## 33                                                                                                                                                                                                                                                                                           <NA>
## 34                                                                                                                                                                                                                                                                                           <NA>
## 35                                                                                                                                                                                                                                                                                           <NA>
## 36                                                                                                                                                                                                                                                                                           <NA>
## 37                                                                                                                                                                                                                                                                                           <NA>
## 38                                                                                            Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 39                                                                                            Carbone C, Daryl C, Conrad S, Marcus C, Belby J. 2014. Geometric factors influencing the diet of vertebrate predators in marine and terrestrial environments. Ecology Letters 17(12): 1553\x961559.
## 40                                                                                                                                                                                                                                                                                           <NA>
## 41                                                                                                                                                                                      Clark RW. 2002. Diet of the timber rattlesnake, Crotalus horridus. Journal of Herpetology 36(3), 494-499.
```

```r
names(snakes)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```

The namaqua dwarf adder snake has the smallest homerange. It is venemous and lives in Southern Africa. 

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
