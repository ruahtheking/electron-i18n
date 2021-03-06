# `<webview>` Tag

> Ipakita ang panlabas na nilalaman ng web sa mga liblib na anyu at proseso.

Proseso:[Tagabigay](../tutorial/quick-start.md#renderer-process)

Gumamit ng `webview` tag sa embed na 'panauhin' nilalaman(tulad ng pahina sa web) sa iyong Electron app. Ang nilalaman ng panauhin ay may nakalagay sa loob ng container na `webview`. Ang naka-embed na pahina sa loob ng iyong app kontrol kung paano ang nilalaman ng panauhin ay nailatag at naibigay.

Hindi tulad ng `iframe`, ang `webview` ay tumatakbo sa hiwalay na proseso kaysa ginagamit mong app. Ito ay walang parehang pahintulot tulad ng web na pahina at ang lahat ng mga interaskyon pagitan sa iyong app ang naka-embed na nilalaman ay magiging asinkrunos. Ito ay pinapanatili ang iyong app na ligtas sa mga naka-embed na nilalaman. **Paalala:** Lahat ng paraan ay tinatawag sa webview galing sa unang pahina na nangangailangan a asinkrunos na tawag papunta sa pangunahing proseso.

## Halimbawa

Para i-embed ang pahina ng web sa iyong app, Idagdag ang `webview` tag sa iyong app's na taga-embed na pahina (Ito ang app na pahina na makikita sa panauhin na nilalaman). Sa kanyang pinakasimpleng porma, ang `webview` tag kasama ang `src` sa pahina ng web at css na estilo na ikontrol ang itsura ng `webview` container:

```html
<webview id="foo" src="https://www.github.com/" style="display:inline-flex; width:640px; height:480px"></webview>
```

Kung gusto mong ikontrol ang nilalaman ng panauhin sa anumang paraan, maari kang sumulat sa JavaScript na nakikinig para `webview` pangyayari at tumutugon sa mga pangyayari gamit ang `webview` na pamamaraan. Ito ang mga halimbawang kod na may dalawang tagapakinig na pangyayari: isa na nakikinig sa pahina ng web upang simulan ang pagloloding, ang isa naman sa pahina ng web na patigilin ang pag loloding, at nagpapakita ng "loding..." na mensahe habang sa lod taym:

```html
<script>
  onload = () => {
    const webview = document.querySelector('webview')
    const indicator = document.querySelector('.indicator')

    const loadstart = () => {
      indicator.innerText = 'loading...'
    }

    const loadstop = () => {
      indicator.innerText = ''
    }

    webview.addEventListener('did-start-loading', loadstart)
    webview.addEventListener('did-stop-loading', loadstop)
  }
</script>
```

## Mga pamamaraan ng CSS notes

Paki tandaan na ang `webview` tag's na estilo ay gumagamit `displey:fleks;` internali upang makasiguro na ang batang `object` elemento pumuno sa buong taas at lapad ng `webview` container kung saan magagamit na may tradisyonal at layout na fleksbaks (mula v.0.36.11). Pakisuyo na hindi magpatung-sulat sa defolt `displey:fleks;` CSS na ari-arian, maliban sa isinasaad `displey:fleks;` para sa inlayn na layout.

`webview` ay may isyo habang nakatago gamit ang `hidden` katangian o gamit `displey: wala;`. Na makasanhi ng di-pangkaraniwang pagsasalin sa loob ng bata `browserplugin` objek at ang pahina ng web ay naka-relod habang `webview` ay hindi nakatago. Ang rekomendadong pamamaraan ay dapat itago ang `webview` gamit ang `pagpapakita: nakatago`.

```html
<style>
  webview {
    display:inline-flex;
    width:640px;
    height:480px;
  }
  webview.hide {
    visibility: hidden;
  }
</style>
```

## Katangian ng Tag

Ang `webview` na tag ay may sumusunod na katangian:

### `src`

```html
<webview src="https://www.github.com/"></webview>
```

Ibinabalik ang makikitang URL. Pagsulat sa mga katangian ang nagsimula ng top-lebel nabigasyon.

Pag-aatas `src` sa kanyang sariling balyu ay pagreload ng kasalukuyang pahina.

Ang `src` katangian ay maaring ding tumanggap ng mga datos sa URL, tulad ng `datos:text/plain,Hello,world!`.

### `autosize`

```html
<webview src="https://www.github.com/" autosize minwidth="576" minheight="432"></webview>
```

Habang ang katangian na ito ay naroroon sa `webview` kontayner ay magiging awtomatikong magbago ng laki sa loob ng hangganan na tinutukoy sa mga katangian `minwidth`, `minheight`. `maxwidth`, at `maxheight`. Mga limitasong hindi makakaapekto sa `webview` maliban `autosize` ay pinagana. Habang ang `autosize` ay pinagana, ang `webview` kontayner na laki ay hindi maging mababa kaysa sa minimum na balyos o hindi tataas sa pinakamataas.

### `nodeintegration`

```html
<webview src="http://www.google.com/" nodeintegration></webview>
```

Kapag itong katangian ay mayroon sa pahina ng panauhin sa `webview` ay magkakaroon ng integrasyon sa nod ang maaring maggamit sa node APIs tulad `require` at `process` para maka-access sa maliliit na lebel sa antas ng sistema. Integarason sa node ay hindi pinagana batay sa default ng pahina nag panauhin.

### `plugins`

```html
<webview src="https://www.github.com/" plugins></webview>
```

Habang ang katangian na ito ay mayroon sa pahina ng panauhin sa `webview` ay maaring maggamit samga browser plugins. Plugins ay hindi pinagana sa default.

### `preload`

```html
<webview src="https://www.github.com/" preload="./test.js"></webview>
```

Tinutukoy ang isang iskrip ma makakarga bago ang ibang iskrips na tumakbo sa pahina ng panauhin. Ang protokol ng iskrips URL ay dapat maging `file:` or `asar:`, dahil ito ay makakarga sa `require` sa pahina ng panauhin sa ilalim ng hood.

Habang ang pahina ng panauhin ay walang integrasyon na node ang iskrip na ito ay maari paring maka-access sa lahat ng Node APIs, pero gobal na objeks ay naka-injek kay Node at ito ay matatangal pagkatapos ng iskrip na matapos ang ginagawa.

**Paalala:** Ang opsyon na ito ay maaring lumitaw bilang `preloadURL`(hindi `preload`) sa `webPreferences` na tumutukoy sa `will-attach-webview` na pangyayari.

### `httpreferrer`

```html
<webview src="https://www.github.com/" httpreferrer="http://cheng.guru"></webview>
```

Nagtatakda ng referral na URL para sa pahina ng mga panauhin.

### `useragent`

```html
<webview src="https://www.github.com/" useragent="Mozilla/5.0 (Windows NT 6.1; WOW64; Trident/7.0; AS; rv:11.0) like Gecko"></webview>
```

Nagtatakda sa mga gumagamit na ahente para sa pahina ng panauhin bago i-navigate sa pahina. Kapag ang pahina ay nakarga na, gamitin and `setUserAgent` na paraan para palitan ang ahente ang gumagamit.

### `disablewebsecurity`

```html
<webview src="https://www.github.com/" disablewebsecurity></webview>
```

Habang itong katangian mayroon ang pahina ng panauhin hindi pinagana ang seguridad sa web. Ang seguridad sa web ay pinagana sa default.

### `partition`

```html
<webview src="https://github.com" partition="persist:github"></webview>
<webview src="https://electron.atom.io" partition="electron"></webview>
```

Itakda ang sesyon na ginamit sa pahina. Kapag `partition` ay nagsimula sa `persist:`, ang pahina ay gagamit ng masugid na sesyon na magagamit sa lahat ng pahina sa app na may kaparihang `partition`. kung wala naman `persist:` na panlapi, ang pahina ay gagamit na in-memory na sesyon. Sa pag-aatas ng kaparihang `partition`, maramihang pahina ang pwede maibahagi sa parehang sesyon. Kung ang `partition` ay di pa na set pagkatapos ang default na sesyon ng app ay magagamit.

Itong balyo lang ang pwede mabago bago ang unang nabigasyon, mula sa sesyon sa aktibong tagabahagi ang proseso ay di magbabago. Kasunod na pagtatangka na baguhin ang balyo ay mabibigo na may DOM eksepsyon.

### `allowpopups`

```html
<webview src="https://www.github.com/" allowpopups></webview>
```

Habang itong katangian ay mayroon ang pahina ng panauhin ay maaring buksan ang mga bagong bintana. Popups ay hindi pinagana sa default.

### `webpreferences`

```html
<webview src="https://github.com" webpreferences="allowRunningInsecureContent, javascript=no"></webview>
```

Isang listahan mg mga string na tumutukoy sa web preferencies na iiset sa webview, pinaghihiwaly ng `,`. Ang buong listahan ay supportado ng preference strings na makikita sa [BrowserWindow](browser-window.md#new-browserwindowoptions).

Ang string sumusunod sa kapareihang pormat bilang sa katangian ng string sa `window.open`. Ang pangalan ay ibinigay mismo ng `true` bollean balyo. Isang preference ay maaring mag takda ng ibang balyo kabilang ang `=`, kasunod ng balyo. Tumutukoy sa balyo na `yes` at `1` ay inerterprit sa `true`, habang `no` at `` ay binigyang-kahulugan ng `false`.

### `blinkfeatures`

```html
<webview src="https://www.github.com/" blinkfeatures="PreciseMemoryInfo, CSSVariables"></webview>
```

Ang lishtahan ng mga pisi na tumutukoy sa blink na katangian na pinagana ay pinaghihiwalay ng `,`. Ang buong listahan ng supportadong katangian ng pisi ay makikita sa [RuntimeEnabledFeatures.json5](https://cs.chromium.org/chromium/src/third_party/WebKit/Source/platform/RuntimeEnabledFeatures.json5?l=62) na fayl.

### `disableblinkfeatures`

```html
<webview src="https://www.github.com/" disableblinkfeatures="PreciseMemoryInfo, CSSVariables"></webview>
```

Ang listahan ng strings na tumutukoy sa blink na katangian ay maaring di-pinagana sa paghihiwalay ng `,`. Ang buong listahan ng supportadong katangian ng pisi ay makikita sa [RuntimeEnabledFeatures.json5](https://cs.chromium.org/chromium/src/third_party/WebKit/Source/platform/RuntimeEnabledFeatures.json5?l=62) na fayl.

### `guestinstance`

```html
<webview src="https://www.github.com/" guestinstance="3"></webview>
```

Ang balyo na nag-uugnay sa webview sa isang partikular na webContents. Kung ang webview ay unang kargahin ang bagong webContents ay nilikha ang mga katangian na itakda sa tagatukoy ng pagkakataon. Ang pagtatakda sa ganitong katangian sa bagong o umiiral na webview ay kumunekta sa mga umiiral webContents na kasalukuyang nagbibigay sa ibang webview.

Ang umiiral na webview ay makikita ang `destroy` na kaganapan at lilikha ng bagong webContents habang ang bagong url ay nakakarga.

### `disableguestresize`

```html
<webview src="https://www.github.com/" disableguestresize></webview>
```

Habang ang katangian na naroroon sa `webview` ang nilalaman ay nahadlang sa pag-resizing habang ang `webview` na elemento ay mismong naka resized.

Ito ay magagamit sa kombinasyon na may [`webContents.setSize`](web-contents.md#contentssetsizeoptions) na manu-manong baguhin ang laki sa nilalaman ng webview bilang reaksyon sa pagbago ng laki sa bintana. Ito ay maaring gawin na resizing na mas mabilis kompara kung nag-aasa sa elemento ng webview papunta sa awtomatic pag bago ng laki sa mga nilalaman.

```javascript
const {webContents} = require('electron')

// We assume that `win` points to a `BrowserWindow` instance containing a
// `<webview>` with `disableguestresize`.

win.on('resize', () => {
  const [width, height] = win.getContentSize()
  for (let wc of webContents.getAllWebContents()) {
    // Check if `wc` belongs to a webview in the `win` window.
    if (wc.hostWebContents &&
        wc.hostWebContents.id === win.webContents.id) {
      wc.setSize({
        normal: {
          width: width,
          height: height
        }
      })
    }
  }
})
```

## Pamamaraan

Nag `webview` na tag ay may mga susmusunod na pamamaraan:

**Paalala:** Ang webview na elemento ay dapat makarga bago gamitin ang mga pamamaraan.

**Halimbawa**

```javascript
const webview = document.querySelector('webview')
webview.addEventListener('dom-ready', () => {
  webview.openDevTools()
})
```

### `<webview>.loadURL(url[, options])`

* `url` Ang URL
* `mga pagpipilian` Mga bagay (opsyonal) 
  * `httpReferrer` String(opsyonal) - Ang tagabigay ng HTTP url.
  * `userAgent` String(opsyonal) - Ang ahente na gumagamit ng pinagmumulan ng kahilingan.
  * `extraHeaders` String(opsyonal) - Sobrang ulunan ay pinaghihiwalay sa "\n"
  * `postData`([UploadRawData[]](structures/upload-raw-data.md) | [UploadFile[]](structures/upload-file.md) | [UploadFileSystem[]](structures/upload-file-system.md) | [UploadBlob[]](structures/upload-blob.md)) - (optional) Context | Request Context
  * `baseURLForDataURL` String(opsyonal) - Basi nag url (may tagapahiwalay sa landas ng separator) para sa mga dokumento na kakargahin sa pamamagitan ng datos ng url. Ito ay kailangan kung ang tinutukoy ng `url` iy isang datos ng url at kailangan maikarga sa ibang dokumento.

Kakargahin ang `url` sa webview, ang `url` ay dapat magkaroon ng protokol na panlapi, e.g ang `http://` or `file://`.

### `<webview>.getURL()`

Nagbabalik `String` - Ang URL ng pahina ng panauhin.

### `<webview>.getTitle()`

Nagbabalik `String` - Ang titulo ng pahina ng panauhin.

### `<webview>.isLoading()`

Nagbabalik `Boolean` - Kung ang pahina ng panauhin ay nakakarga pa rin.

### `<webview>.isWaitingForResponse()`

Nagbabalik `Boolean` - Kung ang pahina ng panauhin ay naghihintay ng unang-sagot para sa pangunahing mapagkukunan ng pahina.

### `<webview>.stop()`

Nagtitigil ng anumang nakabingbing na nabigasyon.

### `<webview>.reload()`

Ikarga muli ang pahina ng panauhin.

### `<webview>.reloadIgnoringCache()`

Ikarga muli ang pahina ng panauhin at ang hindi napansin na cache.

### `<webview>.canGoBack()`

Nagbabalik `Boolean` - Kung ang pahina ng panauhin ay hindi makabalik.

### `<webview>.canGoForward()`

Nagbabalik `Boolean` - Kung ang pahina ng panauhin ay makaka-abanti.

### `<webview>.canGoToOffset(offset)`

* `offset` Integer

Nagbabalik `Boolean` - Kung ang pahina ng panauhin ay makakapunta sa `offset`.

### `<webview>.clearHistory()`

Linisin ang kasaysayan ng nabigasyon.

### `<webview>.goBack()`

Gawin na ang pahina ng panauhin na bumalik.

### `<webview>.goForward()`

Gawin ang pahina ng panauhin na sumulong.

### `<webview>.goToIndex(index)`

* `index` Integer

Ang nabigasyon sa tinutukoy na lubos na index.

### `<webview>.goToOffset(offset)`

* `offset` Integer

Ang nabigasyon sa tinutukoy na offset galing sa "kasalukuyang entri".

### `<webview>.isCrashed()`

Nagbabalik `Boolean` - Kung saan ang proseso ng tagapagbigay ay nasira.

### `<webview>.setUserAgent(userAgent)`

* `userAgent` String

Pumupatong sa gumagamit na ahente para sa pahina ng panauhin.

### `<webview>.getUserAgent()`

Nagbabalik `String` - Ang gumagamit na ahente para sa pahina ng panauhin.

### `<webview>.insertCSS(css)`

* `css` String

Paglagay ng CSS sa pahina ng panauhin.

### `<webview>.executeJavaScript(code, userGesture, callback)`

* `code` String
* `userGesture` Boolean - Default `false`.
* `pagbalik tawag` Gumagana (opsyonal) - Tinawag pagkatapos ang iskrip ay ginawa. 
  * `result` Any

Sinusuri `code` sa pahina. Kung `userGesture` ay nakatakda, ito ay lilikha ng konteksto ng kilos ng gugamit sa pahina. HTML APIs tulad ng `requestFullScreen`, na nangangailangan ng aksyon sa gugamit, ay maaring kumuha ng kalamangan para sa opsyon ng otomasyon.

### `<webview>.openDevTools()`

Ang pagbukas ng DevTools na bintana para sa pahina ng panauhin.

### `<webview>.closeDevTools()`

Ang pagsirado ng DevTools na bintana para sa pahina ng panauhin.

### `<webview>.isDevToolsOpened()`

Nagbabalik `Boolean` - Kung saan ang pahina ng panauhin ay mayroong DevTools na may bintana na nakalagay.

### `<webview>.isDevToolsFocused()`

Nagbabalik `Boolean` - Kung saan ang bintana ng DevTools ay ang pahina ng panauhin ay nakatuon.

### `<webview>.inspectElement(x, y)`

* `x` Integer
* `y` Integer

Naguumpisa sa pagsusi sa mga elemnto sa posisyon (`x`, `y`) sa pahina ng panauhin.

### `<webview>.inspectServiceWorker()`

Ang pagbukas ng DevTools para sa konteksto sa serbisyo ng trabahanti ay naroroon sa pahina ng panauhin.

### `<webview>.setAudioMuted(muted)`

* `muted` Boolean

Ang pagtakda nag pahina sa panauhin na naka tahimik.

### `<webview>.isAudioMuted()`

Nagbabalik `Boolean` - Kung saan ang pahina ng panauhin ay naka tahimik.

### `<webview>.undo()`

Paggawa ng pag-edit na utos `undo` sa pahina.

### `<webview>.redo()`

Ang paggawa ng pag-edit sa utos na `redo` sa pahina.

### `<webview>.cut()`

Paggawa ng pag-edit na utos na `cut` sa pahina.

### `<webview>.copy()`

Paggawa ng pag-edit sa utos na `copy` sa pahina.

### `<webview>.paste()`

Paggawa ng pag-edit sa utos na `paste` sa pahina.

### `<webview>.pasteAnd MatchStyle()`

Paggawa ng pag-edit sa utos na `pasteAndMatchStyle` sa pahina.

### `<webview>.delete()`

Paggawa ng pag-edit sa utos na `delete` sa pahina.

### `<webview>.selectAll()`

Paggawa ng pag-edit sa utos na `selectAll` sa pahina.

### `<webview>.unselect()`

Paggawa ng pag-edit sa utos na `unselect` sa pahina.

### `<webview>.replace(text)`

* `text` String

Paggawa ng pag-edit sa utos na `replace` sa pahina.

### `<webview>.replaceMisspelling(text)`

* `text` String

Paggawa ng pag-edit sa utos na `replaceMisspelling` sa pahina.

### `<webview>.insertText(text)`

* `text` String

Pagsingit `text` para sa nakapukos na elemento.

### `<webview>.findInPage(text[, options])`

* `text` String - Ang nilalaman na maaring suriin, ay dapat may laman.
* `mga pagpipilian` Mga bagay (opsyonal) 
  * `abanti` Boolean - (opsyonal) Kung mananaliksik ka ng paabanti o patalikod, defaults sa `true`.
  * `findNext` Boolean - (opsyonal) Kung ang operasyon isang kahilingan o isang pagsasagawang kasunod, mga defaults sa `false`.
  * `matchCase` Boolean - (opsyonal) Kung saan ang paghahanap ay dapat case-sensitive, mga defaults sa `false`.
  * `wordStart` Boolean - (opsyonal) Kung saan maghahanap ka lang ng simula ng salita. mga defaults sa `false`.
  * `medialCapitalAsWordStart` Boolean - (opsyonal) Kung ang pinagsama na may `wordStart`, tinatanggap ang kapareha sa gitna ng salita at kung ang kapareha nag nagsimula ng malaking titik at sinundan ng maliit na titik o walang-letter. Tinatanggap ang ilan na ibang intra-salitang magkapareha, mga defaults `false`.

Simulan ng kahilingan para makahanap ng lahat ng magkapareha para sa `text` sa pahina ng web ang nagbalik sa `integer` kumakatawan sa kahilingan na id na naggamit para sa kahilingan. Ang resulta ng mga kahilingan ay maaring makuha sa pag-subscribe sa [`matatagpuan-sa-pahina`](webview-tag.md#event-found-in-page) na kaganapan.

### `<webview>.stopFindInPage(action)`

* `aksyon` String - Tinitiyak ang aksyon na mangyayari sa katapusan [`<webview>.findInPage`](webview-tag.md#webviewtagfindinpage) hiling. 
  * `clearSelection` - Tanggalin ang mga napili.
  * `keepSelection` - I-translate ang mga napili para maging normal.
  * `activateSelection` - Ipukos at iclick ang node ng napili.

Itigil ang anumang `findInPage` na hinihiling para sa `webview` na may kaukulang `aksyon`.

### `<webview>.print([options])`

* `mga pagpipilian` Mga bagay (opsyonal) 
  * `silent` Boolean (opsyonal) - Huwag itanong sa user sa mga setting sa pagpapaimprinta. Ang naka-default ay `false`.
  * `printBackground` Boolean (opsyonal) - Iniimprinta rin ang kulay ng background at ang mukha ng web page. Ang naka-default ay `false`.
  * `deviceName` String (opsyonal) - Itakda ang pangalan ng gagamiting printer na gagamitin. Ang naka-default ay `"`.

Inimprinta ang web page ng `webview`. Pareho sa `webContents.print([options])`.

### `<webview>.printToPDF(options, callback)`

* `mga pagpipilian` Bagay 
  * `marginsType` Integer - (opsyonal) Itinatakda ang uri ng mga margin na gagamitin. Gumagamit ng 0 para sa naka-default na margin, 1 para sa walang margin, at 2 para sa pinakamaliit na maaaring gawing margin.
  * `pageSize` String - (opsyonal) Itinatakda ang sukat ng page ng nalilikhang PDF. Pwedeng `A3`, `A4`, `A5`, `Legal`, `Letter`, `Tabloid` o ang Objek na mayroong `height` at `width` na naka-micron.
  * `printBackground` Boolean - (opsyonal) Pwedeng i-imprinta ang mga background ng CSS.
  * `printSelectionOnly` Boolean - (opsyonal) Pwedeng i-imprinta ang mga napili lamang.
  * `landscape` Boolean - (opsyonal) `true` para sa landscape, `false` para sa portrait.
* `tumawag muli` Punsyon 
  * `error` Error
  * `data` Buffer

Iniimprinta ang web page ng `webview` bilang PDF, Pareho sa `webContents.printToPDF(options, callback)`.

### `<webview>.capturePage([rect, ]callback)`

* `rect` [Rectangle](structures/rectangle.md) (opsyonal) - Ang kabuuan ng page na kukuhanin
* `tumawag muli` Punsyon 
  * `image` [NativeImage](native-image.md)

Kumukuha ng larawan sa page ng `webview`. Pareho sa `webContents.capturePage([rect, ]callback)`.

### `<webview>.send(channel[, arg1][, arg2][, ...])`

* `channel` String
* `...args` anuman[]

Magpadala ng mensahe na asynchronous para maisagawa ang proseso sa pamamagitan ng `channel`. pwede mo ring ipadala ang mga argumento na arbitraryo. Ang magsasagawa ng proseso ay kayang i-handle ang mensahe sa pamamagitan ng pakikinig sa `channel` event kasama ang modulo ng `ipcRenderer`.

Tignan ang [webContents.send](web-contents.md#webcontentssendchannel-args) bilang mga halimbawa.

### `<webview>.sendInputEvent(event)`

* `event` Objek

Nagpapadala ng input na `event` sa page.

Tignan ang [webContents.sendInputEvent](web-contents.md#webcontentssendinputeventevent) para sa mga detalyadong paglalarawan ng objek na `event`.

### `<webview>.setZoomFactor(factor)`

* `factor` Numero - paktor ng zoom.

Binabago ang paktor ng zoom sa tiyak na paktor. Ang paktor ng zoom ay porsyento ng zoom na hinati sa 100, kaya 300% = 3.0.

### `<webview>.setZoomLevel(level)`

* `level` Numero - Lebel ng zoom

Binabago ang lebel ng zoom sa tiyak na lebel. Ang orihinal na sukat ay 0 at ang bawat increment na lampas o kulang ay nagrerepresenta ng 20% na laki o liit sa naka-default na limit na 300% at 50% sa orihinal na sukat, pagkakasunod-sunod.

### `.showDefinitionForSelection()` *macOS*

Pinapakita ang pop-up na diksyonaryo na naghahanap ng mga napiling salita sa page.

### `<webview>.getWebContents()`

Bumalik sa [`WebContents`](web-contents.md) - Ang laman ng web ay naka-ugnay sa `webview`.

## Mga event ng DOM

Ang mga sumusunod na event ng DOM ay nasa tanda ng `webview`:

### Event: 'load-commit'

Magbabalik ng:

* `url` String
* `isMainFrame` Boolean

Itinitigil agad kapag nacommit ang load. Sinasama nito ang nabigasyon na nasa kasalukuyang dokumento pati na ang mga load ng subframe na nasa lebel ng dokumento, pero di kasama ang mga load ng asynchronous resource.

### Event: 'did-finish-load'

Itigil kapag natapos na ang nabigasyon, i.e. ang taga-ikot ng tab ay huminto sa pag-ikot, at ang event na `onload` ay na-dispatch.

### Event: 'did-fail-load'

Magbabalik ng:

* `errorCode` Integer
* `errorDescription` String
* `validatedURL` String
* `isMainFrame` Boolean

Ang event na ito ay tulad ng `did-finish-load`, pero natigil nung nag-fail ang load o nakansela, e.g. `window.stop()` ay na-invoke.

### Event: 'did-frame-finish-load'

Magbabalik ng:

* `isMainFrame` Boolean

Itigil kapag natapos na ang nabigasyon ng frame.

### Event: 'did-start-loading'

Tumutugon sa mga puntos ng oras kung kailan nagsimulang umikot ang taga-ikot ng tab.

### Event: 'did-stop-loading'

Tumutugon sa mga puntos ng oras kung kailan huminto sa pag-ikot ang taga-ikot ng tab.

### Event: 'did-get-response-details'

Magbabalik ng:

* `status` Boolean
* `newURL` String
* `originalURL` String
* `httpResponseCode` Integer
* `requestMethod` String
* ang `referer` String
* `headers` Objek
* `resourceType` String

Itigil kapag ang mga detalye tungol sa hinihinging resource ay nahanap na. Ang `status` ay tumutukoy sa socket connection sa pagdownload ng resource.

### Event: 'did-get-redirect-request'

Magbabalik ng:

* `oldURL` String
* `newURL` String
* `isMainFrame` Boolean

Itigil kapag nakatanggap ng redirect habang himihingi ng resource.

### Event: 'dom-ready'

Itigil kapag ang dokumento ng sinasabing frame ay na-load.

### Event: 'page-title-updated'

Magbabalik ng:

* `title` String
* `explicitSet` Boolean

Itigil kapag ang titulo ng page ay naitakdang habang naka-nabigasyon. Ang `explicitSet` ay di totoo kapag ang titulo ay nabuo mula sa file url.

### Event: 'page-favicon-updated'

Magbabalik ng:

* `favicons` String[] - Hanay ng mga URL.

Itigil kapag nakatanggap ang page ng mga url na favicon.

### Event: 'enter-html-full-screen'

Itigil kapag ang page ay naka-fullscreen na dulot ng HTML API.

### Event: 'leave-html-full-screen'

Itigil kapag ang page ay hindi na naka-fullscreen na dulot ng HTML API.

### Event: 'console-message'

Magbabalik ng:

* `level` Integer
* `message` String
* `line` Integer
* `sourceId` String

Itigil kapag ang guest window ay nagtalaga ng mensahe na konsol.

Ang mga sumunod na halimbawa na mga code ay pinapasa ang lahat ng mga mensaheng nakatalaga sa konsol ng embedder na hindi nakapaloob sa nakatalagang lebel o iba pang mga katangian.

```javascript
const webview = document.querySelector('webview')
webview.addEventListener('console-message', (e) => {
  console.log('Guest page logged a message:', e.message)
})
```

### Event: 'found-in-page'

Magbabalik ng:

* `resulta` Bagay 
  * `requestId` Integer
  * `activeMatchOrdinal` Integer - Posisyon ng aktibong katugma.
  * `matches` Integer - Bilang ng mga Tugma.
  * `selectionArea` Objek - Mga coordinate ng unang tugmang parte.
  * `finalUpdate` Boolean

Itigil kung meron ng resulta sa hinihingi ng [`webview.findInPage`](webview-tag.md#webviewtagfindinpage).

```javascript
const webview = document.querySelector('webview')
webview.addEventListener('found-in-page', (e) => {
  webview.stopFindInPage('keepSelection')
})

const requestId = webview.findInPage('test')
console.log(requestId)
```

### Event: 'new-window'

Magbabalik ng:

* `url` String
* `frameName` String
* `disposition` String- Pwedeng `default`, `foreground-tab`, `background-tab`, `new-window`, `save-to-disk` and `other`.
* `options` Objek - Ang mga opsyon na kailangang gamitin para makagawa ng bagong `BrowserWindow`.

Itigil kapag ang guest page ay sinusubukang magbukas ng bagong browser window.

Ang mga sumusunod na halimbawa na code ay nagbubukas ng bagong url sa nakadefault na browser sa sistema.

```javascript
const {shell} = require('electron')
const webview = document.querySelector('webview')

webview.addEventListener('new-window', (e) => {
  const protocol = require('url').parse(e.url).protocol
  if (protocol === 'http:' || protocol === 'https:') {
    shell.openExternal(e.url)
  }
})
```

### Event: 'will-navigate'

Magbabalik ng:

* `url` String

Ilabas kapang ang user o ang mismong page ay gustong magsimula ng nabigasyon. Ito'y pwedeng mangyari kapag ang `window.location` ng objek ay nabago o ang kiniclick ng user ang link sa page.

Itong event na ito ay hindi ilalabas kapag ang nabigasyon ay na-istart na ang gamit ay programa na may mga API tulad ng `<webview>.loadURL` and `<webview>.back`.

Ito ay hindi rin ilalabas habang nasa nabigasyon sa loog ng page, gaya ng pag-click sa naka-ankor na mga link o naka-update ang `window.location.hash`. Gamitin ang event na `did-navigate-in-page` para sa layuning ito.

Tinatawag ang `event.preventDefault()` na may **NOT** mga epekto.

### Event: 'did-navigate'

Magbabalik ng:

* `url` String

Nilalabas kapag natapos na ang nabigasyon.

Ang event na ito ay hindi ilalabas habang nasa nabigasyon sa loog ng page, gaya ng pag-click sa naka-ankor na mga link o naka-update ang `window.location.hash`. Gamitin ang event na `did-navigate-in-page` para sa layuning ito.

### Event: 'did-navigate-in-page'

Magbabalik ng:

* `isMainFrame` Boolean
* `url` String

Ilalabas kapag nangyari ang nabigasyon sa loob ng page.

Kapag nangyari ang nabigasyon sa loob ng page, ang URL ng page ay nababago pero hindi ito magiging dahilan sa pag-nanavigate sa labas ng page. Mga halimbawa ng mga pangyayaring ito ay kapag ang naka-ankor na mga link ay naclick o kung ang mga na DOM na `hashchange` ay natrigger.

### Event: 'close'

Itigil kung ang guest page ay sinubukang isara ang sarili.

Ang mga sumusunod na halimbawa na mga code ay ninanavigate ang `webview` sa `about:blank` kapag ang bisita ay sinubukang isara ang sarili.

```javascript
const webview = document.querySelector('webview')
webview.addEventListener('close', () => {
  webview.src = 'about:blank'
})
```

### Event: 'ipc-message'

Magbabalik ng:

* `channel` String
* `args` Array

Itigil kapag ang guest page ay nagpadala ng mensahe na asynchronous sa embedder page.

Kasama ang paraang `sendToHost` at event na `ipc-message` magiging madali nalang ang iyong pakikipag-ugnayan sa pagitan ng guest page at embedder page:

```javascript
// In embedder page.
const webview = document.querySelector('webview')
webview.addEventListener('ipc-message', (event) => {
  console.log(event.channel)
  // Prints "pong"
})
webview.send('ping')
```

```javascript
// In guest page.
const {ipcRenderer} = require('electron')
ipcRenderer.on('ping', () => {
  ipcRenderer.sendToHost('pong')
})
```

### Event: 'crashed'

Itigil kapag ang nag-crash ang proseso na nagsumite.

### Event: 'gpu-crashed'

Itigil kapag ang ang proseso ng gpu ay nag-crash.

### Event: 'plugin-crashed'

Magbabalik ng:

* `name` String
* `version` String

Itigil kapag ang proseso na plug-in ay nagcrash.

### Event: 'destroyed'

Itigil kapag ang nasira ang mga WebContent.

### Event: 'media-started-playing'

Ilabas kapag ang medya ay nagsimula.

### Event: 'media-paused'

Ilabas kapag ang medya ay nahinto o natapos na.

### Event: 'did-change-theme-color'

Magbabalik ng:

* `themeColor` String

Ilabas kung ang kulay ng tema ng page ay nabago. Ito ay kadalasang dahil sa na-icounter ang tanda na meta:

```html
<meta name='theme-color' content='#ff0000'>
```

### Event: 'update-target-url'

Magbabalik ng:

* `url` String

Ilabas kapag ang mouse ay napunta sa link o ang keyboard ay nagalaw ang pukos sa link.

### Event: 'devtools-opened'

Ilabas kapag ang mga DevTool ay nabuksan.

### Event: 'devtools-closed'

Ilabas kapag ang mga DevTool ay nasarado.

### Event: 'devtools-focused'

Ilabas kapag ang mga DevTool ay napukos / nabuksan.