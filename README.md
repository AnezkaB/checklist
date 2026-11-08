# Pořádání soutěže

Pomocník pro pořadatele závodů v Jump Rope (závodní skákání přes švihadlo). Pořadatel si v pěti krocích naklikne, jaký závod pořádá. Aplikace z toho sestaví checklist: úkoly podle témat, personál, vybavení s množstvím, počet medailí a co zajistit v hale.

- Jeden soubor `index.html`, bez serveru a bez externích knihoven.
- Data zůstávají v prohlížeči pořadatele. Sdílet se dají odkazem nebo zálohou do souboru.
- Export do tabulky (CSV pro Excel) a do PDF přes tisk.
- Funguje na počítači i v mobilu.

Adresa: **https://anezkab.github.io/checklist/**

## Vložení na web (iframe)

```html
<div id="poradani-souteze-karta" style="display:none; font-family:inherit; background:#f2f4f9; border:1px solid #e1e5ef; border-radius:14px; padding:20px; margin:8px 0;">
  <p style="margin:0 0 4px; font-weight:700; font-size:1.15rem; color:#111833;">Checklist pro pořadatele soutěží</p>
  <p style="margin:0 0 16px; color:#57617e;">Naklikáte, jaký závod pořádáte, a aplikace sestaví úkoly, personál, vybavení i počet medailí.</p>
  <a href="https://anezkab.github.io/checklist/" style="display:inline-block; background:#315a92; color:#fff; font-weight:700; text-decoration:none; padding:14px 22px; border-radius:10px;">Otevřít checklist →</a>
</div>
<iframe id="poradani-souteze"
        title="Checklist – pořádání soutěže"
        style="width:100%; height:900px; border:0; display:block;"></iframe>
<script>
(function () {
  var o = "https://anezkab.github.io", url = o + "/checklist/";
  var f = document.getElementById("poradani-souteze"), k = document.getElementById("poradani-souteze-karta");
  /* Na mobilu se aplikace otevře přes celou obrazovku – v rámu se na telefonu špatně posouvá. */
  if (window.matchMedia("(max-width: 767px)").matches) { f.parentNode.removeChild(f); k.style.display = "block"; return; }
  f.src = url;
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
