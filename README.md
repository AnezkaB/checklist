# Pořádání soutěže

Pomocník pro pořadatele závodů v Jump Rope (závodní skákání přes švihadlo). Pořadatel si v pěti krocích naklikne, jaký závod pořádá. Aplikace z toho sestaví checklist: úkoly podle témat, personál, vybavení s množstvím, počet medailí a co zajistit v hale.

- Jeden soubor `index.html`, bez serveru a bez externích knihoven.
- Data zůstávají v prohlížeči pořadatele. Sdílet se dají odkazem nebo zálohou do souboru.
- Export do tabulky (CSV pro Excel) a do PDF přes tisk.
- Funguje na počítači i v mobilu.

Adresa: **https://anezkab.github.io/checklist/**

## Vložení na web (iframe)

```html
<iframe id="poradani-souteze"
        src="https://anezkab.github.io/checklist/"
        title="Checklist – pořádání soutěže"
        style="width:100%; height:900px; border:0; display:block;"></iframe>
<script>
(function () {
  /* Rám má vždy výšku podle obsahu (žádné prázdné místo pod ním) a lišta,
     menu témat i tlačítka průvodce se drží u okraje okna i při posouvání stránky. */
  var f = document.getElementById("poradani-souteze"), o = "https://anezkab.github.io";
  function posli() {
    var r = f.getBoundingClientRect();
    if (f.contentWindow) f.contentWindow.postMessage({ typ: "poradani-souteze:posun", top: r.top, vh: window.innerHeight }, o);
  }
  window.addEventListener("message", function (e) {
    if (e.origin !== o || !e.data) return;
    if (e.data.typ === "poradani-souteze:vyska") { f.style.height = e.data.vyska + "px"; posli(); }
    if (e.data.typ === "poradani-souteze:nahoru") { var t = f.getBoundingClientRect().top; if (t < 0) window.scrollBy(0, t); }
  });
  window.addEventListener("scroll", posli, { passive: true });
  window.addEventListener("resize", posli);
  f.addEventListener("load", posli);
})();
</script>
```

Pokud web nedovolí vložit `<script>`, stačí samotný `<iframe>` s pevnou výškou. Stránka se pak posouvá uvnitř rámu.

Starší vkládací kód (jen s úpravou výšky) funguje dál, ale lišta a menu témat se při posouvání nedrží.

Doporučuji pod rám přidat i přímý odkaz: „Otevřít v samostatném okně“. Některé prohlížeče (hlavně Safari) v rámu z cizí domény neukládají rozpracovaný checklist. Aplikace na to upozorní a v samostatném okně ukládá normálně.
