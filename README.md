# Náplň cvičenia
- zoznámenie sa s časovačom (TIM)
- zoznámenie sa s analógovo digitálnym prevodníkom (ADC)

# Konfigurácia prevodníka (ADC1)

<p align="center">
    <img src="https://github.com/VRS-Predmet/vrs_cvicenie_7/blob/master/images/adc_config.PNG" width="900">
</p>

- ADC je konfigurovaný v režime "single conversion" a využíva len jeden kanál
- využivaný ADC prevodník ma nastavené 10bit rozlíšenie (0 - 1023)
- prevod je spúšťaný časovačom s frekvenciou 1kHz
- prevedená hodnota je uložená do registra ADC: "DR" - data register

<p align="center">
    <img src="https://github.com/VRS-Predmet/vrs_cvicenie_7/blob/master/images/adc_dma_config.PNG" width="500">
</p>

- po ukončení konverzie, ADC vygeneruje "DMA request" 
- DMA kontrolér výsledok prevodu presunie z "DR" do vyhradenej pamäti

# Konfigurácia prevodníka (TIM1)

<p align="center">
    <img src="https://github.com/VRS-Predmet/vrs_cvicenie_7/blob/master/images/tim_config.PNG" width="900">
</p>

- TIM1 využiva interný zdroj hodín s taktom 128MHz
- TIM1 v pravidelnom časovom intervale (1kHz) spúšťa prevod ADC1
- potrebné správne nastavenie počítadla (counter) a predeličky (prescaler) časovača TIM1
- predelička zdroju hodinového signálu je nastavená na "4" (znížil som takt časovača zo 128MHz na 32MHz)
- predelička časovača nastavená na 31 znamená, že počítadlo časovača bude inkrementované každú mikrosekundu (znížil som frekvenciu inkrementovania počítadla z 32MHz na 1MHz)
- počítadlo nastavené na 999 znamená, že ak počítadlo napočíta do 999, tak nastane "update event". Na update event začne ADC1 konverziu a počítadlo časovača začne počítať od 0 (ak počítame nahor - up-counting)

# Spracovanie nameraných hodnôt

- výsledok prevodu ADC1 je DMA kontrolérom ukladaný do vyhradenej pamäti
- DMA generuje prerušnie "HT" a "TC"
- v prerušení sa výsledok konverzie prevedie na hodnotu reprezentujúcu merané napätie (0 - 3v3)
- v prípade, že maximálna úroveň meraného napätia presahuje úroveň referenčného, tak výpočet je potrebné patrične prispôsobiť 
