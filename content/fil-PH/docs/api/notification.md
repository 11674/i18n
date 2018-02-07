# Ang paunawa

> Lumikha ng mga paunawa sa OS ng desktop

Proseso:[Main](../glossary.md#main-process)

## Ginagamit sa mga proseso ng tagasalin

Kung gusto mong ipakita ang mga Paunawa mula sa proseso ng tagasalin kailangan mong gamitin ang [HTML5 Notification API](../tutorial/notifications.md)

## Klase: Paunawa

> Lumikha ng mga paunawa sa OS ng desktop

Proseso:[Main](../glossary.md#main-process)

Ang `Notification` ay isang [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter).

Ito ay lumilikha ng isang bagong `Notification` na may sinaunang mga katangian na itinakda sa pamamagitan ng `options`.

### Mga istatikong pamamaraan

Ang klase ng `Notification` ay may mga sumusunod na mga istatikong pamamaraan:

#### `Notification.isSupported()`

Nagbabalik sa `Boolean` - Kung hindi man ang mga paunawa ng desktop ay sinusuportahan sa kasalukuyang sistema

### Ang `new Notification([options])` ay *Experimental*

* `mga pagpipilian` Bagay 
  * Ang `title` String - Ang isang pamagat sa mga paunawa, kung saan ay ipinakita sa ibabaw ng window ng paunawa kung saan ito ipinakita
  * Ang `subtitle` String - (opsyonal) Ang pangalawang pamagat para sa paunawa, kung saan ay makikita sa ilalim ng pamagat. *macOS*
  * Ang `body` String - Ang kabuuang teksto ng mga paunawa, kung saan ay makikita sa ilalim ng pamagat o pangalawang pamagat
  * Ang `silent` Boolean - (opsyonal) Kung hindi man ay maglalabas ng isang paunawang ingay kapag ipinapakita ang paunawa
  * `icon` (String | [NativeImage](native-image.md)) - (optional) An icon to use in the notification
  * Ang `hasReply` Boolean - (opsyonal) Kung hindi man ay magdagdag ng isang opsyon ng nasa linyang pagtugon sa mga paunawa. *macOS*
  * Ang `replyPlaceholder` String - (opsyonal) Ang placeholder para sumulat sa input field ng nasa linyang pagtugon. *macOS*
  * Ang `sound` String - (opsyonal) Ang pangalan ng file ng tunog ay tutunog kapag ang paunawa ay ipinakita. *macOS*
  * Ang `actions` [NotificationAction[]](structures/notification-action.md) - (opsyonal) Ang mga aksyon na idadagdag sa mga paunawa. Pakiusap basahin ang magagamit na mga aksyon at mga limitasyon sa mga dokumentasyon ng `NotificationAction` sa *macOS*

### Halimbawa ng mga Kaganapan

Ang mga bagay na nilikha na may `new Notification` ay naglalabas ng mga sumusunod na mga event:

**Tandaan:** Ang ilang mga kaganapan ay magagamit lamang sa partikular na mga operating system at na may label na.

#### Kaganapan: 'ipakita'

Ibinabalika ang:

* `kaganapan`Kaganapan

Ay lalabas kapag ang paunawa ay ipinakita sa mga gumagamit, tandaan na ito ay ititira ng maraming beses bilang isang paunawa na maaaring ipakita ng maraming beses sa pamamagitan ng pamamaraan ng `show()`.

#### Event: 'click'

Ibinabalika ang:

* `kaganapan` Kaganapan

Ay lalabas kapag ang paunawa ay na-klik na ng gumagamit.

#### Event: 'close'

Ibinabalika ang:

* `kaganapan` kaganapan

Ay lalabas kapag ang paunawa ay isinara sa pamamagitan ng manu-manong pakikialam mula sa mga gumagamit.

This event is not guaranteed to be emitted in all cases where the notification is closed.

#### Event: 'sumagot' sa *macOS*

Ibinabalika ang:

* `kaganapan` Kaganapan
* `reply` String - Ang string na ipinasok ng gumagamit sa field ng nasa linyang pagtugon

Ay lalabas kapag ang gumagamit ay na-klik ang pindutan ng "Reply" sa paunawa na may `hasReeply: true`.

#### Event: 'aksyon' sa *macOS*

Ibinabalika ang:

* `kaganapan` Kaganapan
* Ang `index` Number - Ang indise ng mga aksyon na na-aktibeyt na

### Mga halimbawa ng pamamaraan

Ang mga bagay na ginawa na may `new Notification` ay may mga sumusunod na pamamaraan ng instansya:

#### `ang notification.show()`

Ay agad na nagpapakita ng paunawa sa mga gumagamit, mangyaring tandaan na ito ay nangangahulugan na hindi katulad ng implementasyon ng HTML5 Notification, nagbibigay halimbawa lamang sa isang `new Notification` ay hindi agad ito ipinakita sa mga gumagamit, dapat mo munang tawagin ang pamamaraan na ito bago ang OS ay ipakita ito.

If the notification has been shown before, this method will dismiss the previously shown notification and create a new one with identical properties.

#### `notification.close()`

Dismisses the notification.

### Patugtugin ang mga tunog

Sa macOS, maaari mong tukuyin ang pangalan ng mga tunog na gusto mong patunugin kapag ang paunawa ay ipinakita. Kahit ano sa mga default na tunog (sa ilalim ng System Preferences > Sound) ay maaaring gamitin, sa karagdagan sa mga file ng costum sound. Maging sigurado na ang file ng tunog ay nakoya na sa ilalim ng bundle ng app (hal, `YourApp.app/Contents/Resources`), o isa sa mga sumusunod na lokasyon:

* `~/Library/Sounds`
* `/Library/Sounds`
* `/Network/Library/Sounds`
* `/System/Library/Sounds`

Tingnan ang docs ng [`NSSound`](https://developer.apple.com/documentation/appkit/nssound) para sa karagdagang impormasyon.