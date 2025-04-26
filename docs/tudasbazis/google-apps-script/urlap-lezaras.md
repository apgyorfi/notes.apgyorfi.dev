---
title: Google Forms űrlap lezárásának ütemezése
description: Google Forms űrlap válaszadási lehetőség ütemezett lezárása időalapú triggerrel meghívott Google Apps Script-el
sidebar_label: Űrlap ütemezett lezárása
---

# Google Forms űrlap lezárása Google Apps Script kóddal
1. Google Apps Script létrehozása
2. Kód beillesztése
```
function closeForm() {
  var form = FormApp.openById('FORM_ID');
  form.setAcceptingResponses(false);
}
```
3. `FORM_ID` cseréje az űrlap szerkesztési URL címéből történő kivágással
4. Űrlap mentése, és próbafuttatása (), jogosultságok megadása
5. Trigger beállítása időalapúra