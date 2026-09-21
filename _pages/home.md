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
.rete-box{position:relative;overflow:visible;isolation:isolate;text-align:center;width:100%;max-width:none;margin:0;padding:3rem 0}
.rete-box canvas{position:absolute;inset:0;width:100%;height:100%;z-index:-1;display:block;pointer-events:none}
.rete-box > *{position:relative}
.rete-box h2{margin-top:0}
/* ===== MARTE START (css) - per rimuovere Marte: cancella da qui a MARTE END (css), il blocco MARTE nell'HTML, lo script MARTE (js) e assets/img/marte.webp ===== */
.rete-box .marte-orbita{position:absolute;z-index:-2;pointer-events:none;left:50%;top:50%;width:0;height:0;will-change:transform}
.rete-box .marte-orbita .marte-y{position:absolute;left:0;top:0;width:0;height:0;will-change:transform}
.rete-box .marte{position:absolute;--mt:clamp(72px,10vw,104px);width:var(--mt);height:var(--mt);
  left:calc(var(--mt) / -2);top:calc(var(--mt) / -2);border-radius:50%;display:block;
  opacity:.6;filter:saturate(.85);box-shadow:0 0 34px 10px rgba(150,150,158,.10)}
html[data-theme=dark] .rete-box .marte{opacity:.66;box-shadow:0 0 34px 10px rgba(150,150,160,.07)}
@media (max-width:600px){.rete-box .marte{--mt:17vw}}
/* ===== MARTE END (css) ===== */
</style>

<div class="rete-box" id="rete-box" markdown="1">
<!-- ===== MARTE START (html) ===== -->
<div class="marte-orbita" aria-hidden="true"><div class="marte-y"><canvas class="marte" width="208" height="208" aria-hidden="true"></canvas></div></div>
<!-- ===== MARTE END (html) ===== -->
<canvas id="rete-cv" aria-hidden="true"></canvas>

## Web Agency a Roma dal 2013, al fianco della crescita del tuo business.

Comunicazione, web marketing e sviluppo di piattaforme digitali: aiutiamo imprenditori, start up e grandi aziende a crescere, nel privato come nella Pubblica Amministrazione. Ogni progetto nasce da un'analisi su misura del business e degli obiettivi, combinando creatività e concretezza per ottenere risultati misurabili.

Un team unico di professionisti coordina ogni fase, dalla strategia al risultato: siti, e-commerce, campagne, brand identity e applicativi su misura. Rispondiamo entro 24 ore, festivi esclusi, e la prima consulenza è gratuita.

**Vuoi far crescere il tuo business?** Scrivici su WhatsApp o richiedi un preventivo: costruiamo insieme la soluzione giusta per te.

</div>

<script>
/* ===== MARTE START (js) ===== */
(function(){
  var o=document.querySelector('.marte-orbita'); if(!o) return;
  var y=o.querySelector('.marte-y'), cv=o.querySelector('.marte'); if(!y||!cv) return;
  var ridotto=matchMedia('(prefers-reduced-motion: reduce)').matches;
  var PX=173000, PY=131000, GIRO=240000, K='marte_t0';
  var t0=parseInt(localStorage.getItem(K),10); if(!t0||isNaN(t0)){ t0=Date.now(); try{localStorage.setItem(K,t0);}catch(e){} }
  function amp(){ return matchMedia('(max-width:600px)').matches?26:36; }
  function moto(){
    if(ridotto) return; var el=Date.now()-t0, ax=amp()+'vw';
    o.getAnimations().concat(y.getAnimations()).forEach(function(a){a.cancel();});
    o.animate([{transform:'translateX(-'+ax+')'},{transform:'translateX('+ax+')'}],{duration:PX,iterations:Infinity,direction:'alternate',easing:'ease-in-out'}).currentTime=el%(PX*2);
    y.animate([{transform:'translateY(-110px)'},{transform:'translateY(110px)'}],{duration:PY,iterations:Infinity,direction:'alternate',easing:'ease-in-out'}).currentTime=el%(PY*2);
  }
  moto(); var to; addEventListener('resize',function(){clearTimeout(to);to=setTimeout(moto,300);});
  var S=104, img=new Image(); cv.width=S*2; cv.height=S*2;
  img.onload=function(){
    var tc=document.createElement('canvas'); tc.width=img.width; tc.height=img.height; var tx=tc.getContext('2d'); tx.drawImage(img,0,0);
    var T=tx.getImageData(0,0,tc.width,tc.height).data, TW=tc.width, TH=tc.height;
    var g=cv.getContext('2d'), N=S*2, out=g.createImageData(N,N), D=out.data, map=new Float32Array(N*N*3);
    for(var yy=0;yy<N;yy++)for(var xx=0;xx<N;xx++){
      var nx=(xx+.5-S)/S, ny=(S-yy-.5)/S, r2=nx*nx+ny*ny, i=(yy*N+xx)*3;
      if(r2>1){map[i+2]=-1;continue;}
      var nz=Math.sqrt(1-r2); map[i]=Math.atan2(nx,nz); map[i+1]=(.5-Math.asin(ny)/Math.PI)*(TH-1); map[i+2]=nz;
    }
    function frame(ph){
      for(var i=0;i<N*N;i++){ var m=i*3,q=i*4; if(map[m+2]<0){D[q+3]=0;continue;}
        var u=(map[m]+ph)/(6.283185307); u-=Math.floor(u);
        var t=((Math.min(TH-1,map[m+1]|0))*TW+Math.min(TW-1,(u*TW)|0))*4, k=.35+.65*Math.pow(map[m+2],.6);
        D[q]=T[t]*k; D[q+1]=T[t+1]*k; D[q+2]=T[t+2]*k; D[q+3]=255; }
      g.putImageData(out,0,0);
    }
    var last=0;
    (function tick(now){
      var ph=((Date.now()-t0)%GIRO)/GIRO*6.283185307;   /* fase legata al tempo: dopo il refresh riprende da dove era */
      if(!ridotto&&now-last>66){ frame(ph); last=now; } else if(!last){ frame(ph); last=now; }
      if(!ridotto) requestAnimationFrame(tick);
    })(0);
  };
  img.src="{{ '/assets/img/marte.webp' | relative_url }}";
})();
/* ===== MARTE END (js) ===== */
</script>

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
