**Schema di connessione dell'accelerometro**

Da un lato, vediamo il modulo Wi-Fi sulla scheda della stampante, dall'altro, l'accelerometro

![adxl345](adxl345.jpg)
Puoi posizionarlo sull'estrusore usando questo [supporto](adxl.stl) o questo [supporto](adxl2.stl)  

Per l'installazione, consiglio lo schema come nella foto

![tool](tool.jpg)

O opzione 2
![вариант2](v2_1.jpg)

Aggiungi queste righe al tuo ```printer.cfg``` 
```cfg
[adxl345]

cs_pin: PC15

axes_map: -x,y,z  # тут возможно придется править под ваше расположение осей

spi_software_sclk_pin: PB13

spi_software_mosi_pin: PC3

spi_software_miso_pin: PC2

[resonance_tester]

accel_chip: adxl345

probe_points:
  100, 100, 20
```
**Installazione software**

Andiamo alla nostra console OrangePI e scriviamo:

```bash
sudo apt update
sudo apt install python3-numpy python3-matplotlib libatlas-base-dev
~/klippy-env/bin/pip install -v numpy
```

**Controllo delle impostazioni**

Verificare la connessione dell'accelerometro 
con il seguente comando da console `ACCELEROMETER_QUERY`
Вы должны увидеть текущие измерения акселерометра, включая ускорение свободного падения, например
```
Recv: // adxl345 values (x, y, z): 470.719200, 941.438400, 9728.196800
```
Se si riceve un messaggio di errore, ad esempio `Invalid adxl345 id (got xx vs e5)`, dove xx è un altro identificatore, questo indica un problema di connessione al ADXL345 o un sensore difettoso. Ricontrolla l'alimentazione, il cablaggio (conformità allo schema, assenza di fili rotti o indeboliti, ecc.) o la qualità della saldatura. 

Quindi provare a eseguire `MEASURE_AXES_NOISE` nella console, si dovrebbero ottenere alcuni valori di base del rumore dell'accelerometro sugli assi (dovrebbe essere da qualche parte nell'intervallo ~1-100). Un rumore dell'asse troppo elevato (ad esempio, 1000 o più) può indicare un problema del sensore, un problema di alimentazione o ventole sbilanciate della stampante 3D troppo rumorose.

Puoi trovare l'accelerometro [qui](https://it.aliexpress.com/item/1005001621867550.html?spm=a2g0o.productlist.main.23.2c4eb94dvELjLf&algo_pvid=43af76c8-1adc-41cc-8371-d09dbe8b3860&algo_exp_id=43af76c8-1adc-41cc-8371-d09dbe8b3860-11&pdp_ext_f=%7B%22sku_id%22%3A%2212000016846764576%22%7D&pdp_npi=2%40dis%21EUR%211.67%211.41%21%21%21%21%21%402100b5dc16743376374608381d0710%2112000016846764576%21sea&curPageLogUid=2EFJsbfrDjkk)
