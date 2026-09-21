---
layout: about
title: Home
permalink: /
nav: true
nav_order: 0.3

selected_papers: true # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: true # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder

latest_posts:
  enabled: true
  scrollable: true # adds a vertical scroll bar if there are more than 3 new posts items
  limit: 3 # leave blank to include all the blog posts
---

<style>
.post-header{display:none}
.rete-box{position:relative;overflow:visible;isolation:isolate;text-align:center;max-width:800px;margin:0 auto;padding:3rem 1.2rem}
.rete-box canvas{position:absolute;inset:0;width:100%;height:100%;z-index:-1;display:block;pointer-events:none}
.rete-box > *{position:relative}
.rete-box h2{margin-top:0}
.rete-box .marte{position:absolute;z-index:-2;pointer-events:none;border-radius:50%;overflow:hidden;
  width:clamp(84px,12vw,124px);aspect-ratio:1;right:1%;top:-3%;
  box-shadow:0 0 46px 16px rgba(150,150,158,.12);opacity:.38;filter:saturate(.72) contrast(.9);
  background:url("{{ '/assets/img/marte.webp' | relative_url }}") center/100% 100% no-repeat;
  animation:marte-libra 90s ease-in-out infinite alternate}
.rete-box .marte::after{content:"";position:absolute;inset:0;border-radius:50%;
  background:radial-gradient(circle at 28% 30%,rgba(255,255,255,.12) 0%,rgba(0,0,0,0) 34%,rgba(0,0,0,.34) 100%);
  background-size:170% 170%;background-position:0% 0%;animation:marte-luce 150s ease-in-out infinite alternate}
@keyframes marte-libra{from{transform:rotate(-7deg)}to{transform:rotate(7deg)}}
@keyframes marte-luce{from{background-position:0% 0%}to{background-position:100% 60%}}
html[data-theme=dark] .rete-box .marte{opacity:.40;box-shadow:0 0 46px 16px rgba(150,150,160,.08)}
@media (max-width:600px){.rete-box .marte{width:20vw;right:1%;top:-5%}}
@media (prefers-reduced-motion:reduce){.rete-box .marte,.rete-box .marte::after{animation:none}}
</style>

<div class="rete-box" id="rete-box" markdown="1">
<div class="marte" aria-hidden="true"></div>
<canvas id="rete-cv" aria-hidden="true"></canvas>

## Web Agency a Roma dal 2013, al fianco della crescita del tuo business.

Comunicazione, web marketing e sviluppo di piattaforme digitali: aiutiamo imprenditori, start up e grandi aziende a crescere, nel privato come nella Pubblica Amministrazione. Ogni progetto nasce da un'analisi su misura del business e degli obiettivi, combinando creatività e concretezza per ottenere risultati misurabili.

Un team unico di professionisti coordina ogni fase, dalla strategia al risultato: siti, e-commerce, campagne, brand identity e applicativi su misura. Rispondiamo entro 24 ore, festivi esclusi, e la prima consulenza è gratuita.

**Vuoi far crescere il tuo business?** Scrivici su WhatsApp o richiedi un preventivo: costruiamo insieme la soluzione giusta per te.

</div>

<script>
(function(){
  var box=document.getElementById('rete-box'), cv=document.getElementById('rete-cv'); if(!box||!cv) return;
  var ctx=cv.getContext('2d'), ridotto=matchMedia('(prefers-reduced-motion: reduce)').matches;
  var punti=[], w=0, h=0, mouse={x:0,y:0,on:false}, raf=null, visibile=true;
  var DIST=140, DIST_M=190;
  function colore(){
    return document.documentElement.getAttribute('data-theme')==='dark' ? '105,105,115' : '150,150,160';
  }
  function dim(){
    var dpr=Math.min(devicePixelRatio||1,2); w=box.clientWidth; h=box.clientHeight;
    cv.width=w*dpr; cv.height=h*dpr; cv.getContext('2d').setTransform(dpr,0,0,dpr,0,0);
    var n=Math.round(Math.min(100,Math.max(34,(w*h)/7500))); punti=[];
    for(var i=0;i<n;i++) punti.push({x:Math.random()*w,y:Math.random()*h,vx:(Math.random()-.5)*.5,vy:(Math.random()-.5)*.5,r:1.3+Math.random()*1.5});
  }
  function disegna(){
    ctx.clearRect(0,0,w,h); var c=colore();
    for(var i=0;i<punti.length;i++){
      var a=punti[i];
      if(!ridotto){ a.x+=a.vx; a.y+=a.vy; if(a.x<0||a.x>w)a.vx*=-1; if(a.y<0||a.y>h)a.vy*=-1; }
      for(var j=i+1;j<punti.length;j++){
        var b=punti[j], dx=a.x-b.x, dy=a.y-b.y, d=Math.sqrt(dx*dx+dy*dy);
        if(d<DIST){ ctx.strokeStyle='rgba('+c+','+(0.55*(1-d/DIST)).toFixed(3)+')'; ctx.lineWidth=1;
          ctx.beginPath(); ctx.moveTo(a.x,a.y); ctx.lineTo(b.x,b.y); ctx.stroke(); }
      }
      if(mouse.on){ var mx=a.x-mouse.x,my=a.y-mouse.y,md=Math.sqrt(mx*mx+my*my);
        if(md<DIST_M){ ctx.strokeStyle='rgba('+c+','+(0.75*(1-md/DIST_M)).toFixed(3)+')'; ctx.lineWidth=1.1;
          ctx.beginPath(); ctx.moveTo(a.x,a.y); ctx.lineTo(mouse.x,mouse.y); ctx.stroke(); } }
      ctx.fillStyle='rgba('+c+',0.7)'; ctx.beginPath(); ctx.arc(a.x,a.y,a.r,0,6.2832); ctx.fill();
    }
  }
  function ciclo(){ disegna(); raf=(visibile&&!ridotto)?requestAnimationFrame(ciclo):null; }
  function avvia(){ if(!raf) raf=requestAnimationFrame(ciclo); }
  box.addEventListener('pointermove',function(e){ if(e.pointerType==='touch')return; var r=box.getBoundingClientRect(); mouse.x=e.clientX-r.left; mouse.y=e.clientY-r.top; mouse.on=true; });
  box.addEventListener('pointerleave',function(){ mouse.on=false; });
  document.addEventListener('visibilitychange',function(){ visibile=!document.hidden; if(visibile)avvia(); });
  if('IntersectionObserver' in window) new IntersectionObserver(function(en){ visibile=en[0].isIntersecting; if(visibile)avvia(); }).observe(box);
  var t; addEventListener('resize',function(){ clearTimeout(t); t=setTimeout(function(){dim();disegna();},120); });
  dim(); disegna(); if(!ridotto) avvia();
})();
</script>
