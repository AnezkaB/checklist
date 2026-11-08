# Pořádání soutěže

Pomocník pro pořadatele závodů ve skákání přes švihadlo. Pořadatel si v pěti krocích naklikne, jaký závod pořádá. Aplikace z toho sestaví checklist: úkoly podle témat, personál, vybavení s množstvím, počet medailí a co zajistit v hale.

- Jeden soubor `index.html`, bez serveru a bez externích knihoven.
- Data zůstávají v prohlížeči pořadatele. Sdílet se dají odkazem nebo zálohou do souboru.
- Export do tabulky (CSV pro Excel) a do PDF přes tisk.
- Funguje na počítači i v mobilu.

Adresa: **https://anezkab.github.io/checklist/**

## Vložení na web (iframe)

```html
<iframe id="poradani-souteze"
        src="https://anezkab.github.io/checklist/"
        title="Pořádání soutěže"
        style="width:100%; height:900px; border:0;"
        loading="lazy"></iframe>
<script>
  /* Výška rámu se přizpůsobí obsahu, takže nevznikne druhý posuvník. */
  window.addEventListener("message", function (e) {
    if (e.origin !== "https://anezkab.github.io") return;
    if (e.data && e.data.typ === "poradani-souteze:vyska") {
      document.getElementById("poradani-souteze").style.height = e.data.vyska + "px";
    }
  });
</script>
```

Pokud web nedovolí vložit `<script>`, stačí samotný `<iframe>` s pevnou výškou. Stránka se pak posouvá uvnitř rámu.

Doporučuji pod rám přidat i přímý odkaz: „Otevřít v samostatném okně“. Některé prohlížeče (hlavně Safari) v rámu z cizí domény neukládají rozpracovaný checklist. Aplikace na to upozorní a v samostatném okně ukládá normálně.
