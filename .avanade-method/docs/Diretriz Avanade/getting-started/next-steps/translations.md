# Translations

Managed solutions are growing quite bit when additional languages get enabled. Solution sizes naturally correlate to deployment times causing different problems in the delivery.

This concern should be addressed early, possibly with the following ideas:

* Research if the latest `pac.exe` allows handling of translations and language codes via `-loc` switch and `.resx` files in a dedicated `resources` folder (and add them separately in the pack process before import). (This approach has not yet been tested, it will be considered in a future release).

* Leave translations out and add via a dedicated customizing solution on top in a later stage (for example in DevInt).