---
layout: post
title: "Pinboard Extension für Chrome"
date: 2012-07-08 21:23
comments: true
categories: 
---
Nun bin ich auch unter die Chromenutzer gegangen. Da müssen auch einige Plugins mitwandern und dazu
gehört - als [Pinboard][3] Nutzer - auch die entsprechende Erweiterung. Leider fehlt mit bei der das
wichtigste: ein Shortcut! Und auch sonst sieht es bei der Erweiterung eher danach aus als wenn die Entwicklung stehen geblieben ist.

Also habe ich das Projekt mal geforkt. Ich habe ein wenig aufgeräumt, das Projekt kompatibel zur
[Manifest Version 2][4] gemacht und einen Shortcut auf CMD-e eingebaut. [Pull Request][2] gemacht und
los gehts! Wirklich? Ne, da tut sich nicht mehr so richtig viel. Na ja, solange bis der Autor den
Pullrequest animmt, kann man das Plugin [hier][1] herunterladen. Habe keine Lust die 5$ zu bezahlen,
also einfach selbst installieren.

Die interessanteste Erfahrung habe ich dabei mit dem Keyboardshortcut gemacht. Der Shortcut soll auf
jeder Webseite verfügbar sein. Dafür muss in der Chrome Extension eine [content.js][5] eingebunden
werden. Dadurch wird das enthaltene JavaScript auf jeder Seite eingebettet. Wie geht das? einfach
einen Eintrag in der manifest.json anlegen:

<pre><code class="language-json">
  {
    "content_scripts": [
      {
        "matches": ["http://*/*"],
        "js": ["content.js"]
      }
    ],
  }
  </code>
</pre>

Aus Sicherheitsgründen darf das content.js keine Funktionen aus dem Rest des Plugins(background.js
oder popup.js) aufrufen. Aber das geht über Messages! Dazu auf der sendenden Seite das machen:

<pre><code class="language-javascript">
  var cmdPressed = false;
  window.onkeydown = function(e){
    if(e.keyCode === 91){
      cmdPressed = true;
    }else if(e.keyCode === 69 && cmdPressed){
      chrome.extension.sendMessage({}, function(response) {});
    }
  };

  window.onkeyup = function(e){
    if(e.keyCode === 91){
      cmdPressed = false;
    }
  };
</code>
</pre>

und beim Empfänger das(hier die background.js):

<pre><code class="language-javascript">
  chrome.extension.onMessage.addListener(
    function(request, sender, sendResponse) {
      chrome.tabs.getSelected(null , function(tab) {
        var notification =
            webkitNotifications.createHTMLNotification("test.html");
        notification.show();
        setTimeout(function(){notification.cancel();}, 3000);
      });
    }
  );
</code></pre>


[1]: images/pinboard.crx
[2]: https://github.com/samclark/pinboard/pull/7
[3]: http://pinboard.in
[4]: http://developer.chrome.com/extensions/tut_migration_to_manifest_v2.html
[5]: http://developer.chrome.com/extensions/content_scripts.html
