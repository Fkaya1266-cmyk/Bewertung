# Bewertung
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Danke für Ihren Besuch</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display&display=swap" rel="stylesheet">
<style>
:root{--bg:#12343b;--ink:#f3f7f6;--soft:#b9cfcc;--gold:#f2c14e;--btn-ink:#12343b}
*{box-sizing:border-box}
html,body{margin:0;min-height:100%}
body{background:var(--bg);color:var(--ink);font:18px/1.55 -apple-system,"Segoe UI",Roboto,sans-serif;display:flex;justify-content:center;padding:env(safe-area-inset-top) 0 env(safe-area-inset-bottom)}
main{width:100%;max-width:440px;padding:56px 28px;display:flex;flex-direction:column;min-height:100vh;justify-content:center}
.stars{display:flex;gap:6px;color:var(--gold);font-size:34px;margin-bottom:28px}
.stars span{animation:pop .5s both}
.stars span:nth-child(2){animation-delay:.08s}.stars span:nth-child(3){animation-delay:.16s}.stars span:nth-child(4){animation-delay:.24s}.stars span:nth-child(5){animation-delay:.32s}
@keyframes pop{from{opacity:0;transform:scale(.4)}to{opacity:1;transform:none}}
h1{font:400 40px/1.15 "DM Serif Display",Georgia,serif;margin:0 0 16px}
p{margin:0 0 32px;color:var(--soft);max-width:34ch}
.btn{display:block;text-align:center;background:var(--gold);color:var(--btn-ink);font-weight:700;font-size:19px;padding:18px 24px;border-radius:14px;text-decoration:none}
.btn:focus-visible,button:focus-visible,input:focus-visible{outline:3px solid var(--ink);outline-offset:3px}
small{margin-top:18px;color:var(--soft);font-size:14px}
label{display:block;margin:0 0 6px;font-weight:600}
input{width:100%;font:inherit;padding:14px;border-radius:10px;border:0;margin-bottom:20px;background:var(--ink);color:#12343b}
button{font:inherit;font-weight:700;padding:14px 20px;border-radius:12px;border:0;background:var(--gold);color:var(--btn-ink);width:100%}
#out{word-break:break-all;background:#0c262b;padding:14px;border-radius:10px;font-size:14px;margin:20px 0 12px;display:none}
@media (prefers-reduced-motion:reduce){.stars span{animation:none}}
</style>
</head>
<body>
<main id="app"></main>
<script>
(function(){
  var q=new URLSearchParams(location.search);
  var name=(q.get("n")||"").trim();
  var link=(q.get("l")||"").trim();
  var app=document.getElementById("app");
  function el(t,c,x){var e=document.createElement(t);if(c)e.className=c;if(x)e.textContent=x;return e}

  if(name && /^https:\/\//i.test(link)){
    document.title="Danke – "+name;
    var st=el("div","stars");
    for(var i=0;i<5;i++)st.appendChild(el("span","","★"));
    app.appendChild(st);
    app.appendChild(el("h1","","Vielen Dank für Ihren Besuch bei "+name+"!"));
    app.appendChild(el("p","","Hat es Ihnen geschmeckt? Ihr Feedback hilft uns, noch besser zu werden. Es dauert nur eine Minute."));
    var a=el("a","btn","Bewertung abgeben");
    a.href=link;a.rel="noopener";
    app.appendChild(a);
    app.appendChild(el("small","","Sie werden zu Google weitergeleitet."));
  } else {
    // Generator-Modus: Link für die NFC-Karte erstellen
    document.title="Link-Generator";
    app.appendChild(el("h1","","Link für die NFC-Karte erstellen"));
    var l1=el("label","","Name des Restaurants");l1.htmlFor="n";
    var i1=el("input");i1.id="n";i1.placeholder="z. B. Trattoria Roma";
    var l2=el("label","","Google-Bewertungslink");l2.htmlFor="l";
    var i2=el("input");i2.id="l";i2.placeholder="https://g.page/r/…/review";i2.inputMode="url";
    var b=el("button","","Link erstellen");
    var out=el("div");out.id="out";
    var cp=el("button","","Link kopieren");cp.style.display="none";
    [l1,i1,l2,i2,b,out,cp].forEach(function(x){app.appendChild(x)});
    var url="";
    b.onclick=function(){
      var n=i1.value.trim(),l=i2.value.trim();
      if(!n||!/^https:\/\//i.test(l)){out.style.display="block";out.textContent="Bitte Namen eintragen und einen Link, der mit https:// beginnt.";cp.style.display="none";return}
      url=location.origin+location.pathname+"?n="+encodeURIComponent(n)+"&l="+encodeURIComponent(l);
      out.style.display="block";out.textContent=url;cp.style.display="block";
    };
    cp.onclick=function(){
      if(navigator.clipboard){navigator.clipboard.writeText(url).then(function(){cp.textContent="Kopiert ✓"})}
      else{var r=document.createRange();r.selectNodeContents(out);var s=getSelection();s.removeAllRanges();s.addRange(r);cp.textContent="Markiert – jetzt kopieren"}
    };
  }
})();
</script>
</body>
</html>