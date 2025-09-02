<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Avengers Battle — Thanos (Difficultés & Avatars)</title>
<style>
:root{
  --bg:#0f0f12; --panel:#121216; --muted:#9aa0a6; --accent:#6ef08a; --fg:#eef3ea;
  --hit:#ff4d4d; --crit:#ffd24d; --heal:#4dff88;
}
*{box-sizing:border-box}
body{ margin:0; font-family:Inter, Roboto, "Helvetica Neue", Arial; background:var(--bg); color:var(--fg); }
header{ padding:18px 12px; text-align:center; }
main{ max-width:900px; margin:0 auto; padding:12px; position:relative; }
.panel{ background:var(--panel); border-radius:12px; padding:14px; margin:10px 0; box-shadow:0 8px 28px rgba(0,0,0,0.6); position:relative; overflow:hidden;}
h1{ margin:0; font-size:20px; }
.muted{ color:var(--muted); font-size:13px; margin-top:6px; }
.controls{ display:flex; gap:12px; flex-wrap:wrap; margin-bottom:12px; }
.card{ background:#0d0d0f; border:1px solid #222; padding:10px; border-radius:8px; min-width:180px; }
#stats{ display:flex; gap:10px; flex-wrap:wrap; margin-bottom:8px; }
.stat{ background:#0d0d0f; border:1px solid #232323; padding:8px 10px; border-radius:8px; font-weight:700; color:var(--fg); min-width:150px; text-align:center; }
.bar-container{ background:#222; border-radius:8px; width:100%; height:14px; margin-top:6px; overflow:hidden; }
.bar{ height:100%; border-radius:8px; transition: width 0.35s ease; }
#story{ white-space:pre-wrap; font-size:15px; line-height:1.4; margin-bottom:8px; }
#log{ color:var(--muted); font-size:13px; margin-top:8px; min-height:100px; white-space:pre-wrap; overflow:auto; padding:6px; border-top:1px solid #232323; }
#choices{ display:grid; gap:8px; margin-top:12px; }
button.choice{ background:#141416; color:var(--fg); border:1px solid #2a2a2a; padding:10px 12px; border-radius:8px; text-align:center; cursor:pointer; font-weight:700; }
button.choice:hover{ border-color:var(--accent); color:var(--accent); transform:translateY(-2px); }
.avatar-row{ display:flex; justify-content:space-between; align-items:center; gap:12px; margin-bottom:12px; }
.avatar{ width:110px; height:110px; border-radius:12px; background:#141416; border:2px solid #333; display:flex; align-items:center; justify-content:center; text-align:center; padding:8px; font-weight:900; font-size:20px; }
.small{ font-size:13px; color:var(--muted); }
.projectile{ width:14px; height:14px; border-radius:50%; position:absolute; z-index:120; pointer-events:none; animation:proj 0.45s linear forwards; }
@keyframes proj{ 0%{transform:translate(0,0) scale(1);} 100%{transform:translate(260px,-18px) scale(0.95); opacity:1;} }
.shake{ animation:shake 0.35s; }
@keyframes shake{ 0%{transform:translate(0,0);} 25%{transform:translate(6px,0);} 50%{transform:translate(-6px,0);} 75%{transform:translate(6px,0);} 100%{transform:translate(0,0);} }
.floating-dmg{ position:absolute; font-weight:900; pointer-events:none; transition: all 0.9s ease-out; font-size:15px; text-shadow:0 2px 8px rgba(0,0,0,0.5); }
.footer-note{ text-align:center; color:var(--muted); margin-top:6px; font-size:13px; }

/* responsive */
@media (max-width:680px){
  .avatar{ width:88px; height:88px; font-size:16px; }
  .card{ min-width:140px; }
}
</style>
</head>
<body>
<header>
  <h1>Avengers Battle — Le Duel Final</h1>
  <div class="muted">Choisis ton héros, ton pouvoir (usage unique) et ton arme — puis choisis la difficulté.</div>
</header>

<main>
  <section class="panel">
    <div class="controls">
      <div class="card">
        <div style="font-weight:900">1) Choisir un Avenger</div>
        <div id="hero-list" style="margin-top:8px" class="small"></div>
      </div>
      <div class="card">
        <div style="font-weight:900">2) Choisir un pouvoir (usage unique)</div>
        <div id="power-list" style="margin-top:8px" class="small"></div>
      </div>
      <div class="card">
        <div style="font-weight:900">3) Choisir une arme</div>
        <div id="weapon-list" style="margin-top:8px" class="small"></div>
      </div>
      <div class="card">
        <div style="font-weight:900">4) Difficulté</div>
        <div id="difficulty-list" style="margin-top:8px" class="small"></div>
      </div>
    </div>

    <div class="avatar-row">
      <div>
        <div class="avatar" id="player-avatar">Héros</div>
        <div id="player-info" class="small" style="text-align:center;margin-top:6px"></div>
      </div>

      <div style="flex:1; margin-left:12px; margin-right:12px">
        <div id="stats" aria-live="polite"></div>
        <div id="story" class="small"></div>
      </div>

      <div>
        <div class="avatar" id="enemy-avatar">👑 Thanos</div>
        <div class="small" style="text-align:center; margin-top:6px">Boss final</div>
      </div>
    </div>

    <div id="choices"></div>
    <div id="log"></div>
  </section>

  <div style="text-align:center; margin-top:10px">
    <button id="restart" class="choice" style="max-width:260px">🔁 Recommencer</button>
  </div>

  <div class="footer-note">3 attaques par round → Thanos riposte ensuite. Critique 20% (x2). Lourde = 20% échec. Ajuste la difficulté pour changer PV/degats. Avatars = emojis.</div>
</main>

<script>
/* ---------- Config & données ---------- */
const HEROES = [
  { id:"iron", name:"Iron Man", emoji:"🤖" },
  { id:"thor", name:"Thor", emoji:"⚡" },
  { id:"widow", name:"Black Widow", emoji:"🕷️" },
  { id:"cap", name:"Captain America", emoji:"🛡️" },
  { id:"hulk", name:"Hulk", emoji:"🟢" },
  { id:"strange", name:"Doctor Strange", emoji:"🔮" }
];

const POWERS = {
  "Iron Man":["Répulseurs","Surcharge d'arc"],
  "Thor":["Mjolnir","Éclair divin"],
  "Black Widow":["Arts martiaux","Infiltration"],
  "Captain America":["Bouclier ricochet","Charge tactique"],
  "Hulk":["Poing destructeur","Rage inarrêtable"],
  "Doctor Strange":["Portail dimensionnel","Sort mystique"]
};

const WEAPONS = ["Grenade EMP","Bouclier énergétique","Arc plasma","Lance-filet","Épée asgardienne","Neuro-stun"];

/* difficulties */
const DIFFICULTIES = {
  "Facile":    { enemyPvMult: 0.8, enemyMin:3, enemyMax:6, enemyCritAdd:0 },
  "Normal":    { enemyPvMult: 1.0, enemyMin:4, enemyMax:8, enemyCritAdd:0 },
  "Difficile": { enemyPvMult: 1.2, enemyMin:6, enemyMax:12, enemyCritAdd:0 },
  "Cauchemar": { enemyPvMult: 1.5, enemyMin:8, enemyMax:15, enemyCritAdd:0.10 } // +10% enemy crit flavor (we can ignore player crit change)
};

/* baseline */
const PLAYER_BASE_PV = 30;
const THANO_BASE_PV = 50;

/* ---------- State ---------- */
let state = {
  player: { hero:null, power:null, weapon:null, emoji:"", pv:PLAYER_BASE_PV, maxPv:PLAYER_BASE_PV, usedUniquePowers:{} },
  enemy:  { name:"Thanos", pv:THANO_BASE_PV, maxPv:THANO_BASE_PV, minDmg:4, maxDmg:8, critAdd:0 },
  round: 1,
  attackCount: 0,
  log: [],
  difficulty: "Normal" // default
};

/* ---------- Utils ---------- */
const $ = s => document.querySelector(s);
function el(tag, attrs={}, txt=""){ const e=document.createElement(tag); for(const k in attrs) e.setAttribute(k, attrs[k]); e.textContent = txt; return e; }
function rnd(min,max){ return Math.floor(Math.random()*(max-min+1))+min; }
function clamp(v,a,b){ return Math.max(a, Math.min(b,v)); }

/* ---------- UI build ---------- */
function buildUI(){
  // heroes
  const heroList = $("#hero-list"); heroList.innerHTML = "";
  HEROES.forEach(h => {
    const b = el("button",{class:"choice"}, `${h.emoji} ${h.name}`);
    b.onclick = () => chooseHero(h);
    heroList.appendChild(b);
  });
  // powers empty until hero selected
  $("#power-list").innerHTML = "<div class='small'>Choisis un héros</div>";
  // weapons
  const weaponList = $("#weapon-list"); weaponList.innerHTML = "";
  WEAPONS.forEach(w => {
    const b = el("button",{class:"choice"}, w);
    b.onclick = () => chooseWeapon(w);
    weaponList.appendChild(b);
  });
  // difficulty
  const diffList = $("#difficulty-list"); diffList.innerHTML = "";
  Object.keys(DIFFICULTIES).forEach(d => {
    const b = el("button",{class:"choice"}, d);
    b.onclick = () => chooseDifficulty(d);
    diffList.appendChild(b);
  });
  setChoices([]); renderStatus();
}

/* ---------- Render status & helpers ---------- */
function renderStatus(){
  const p=state.player, e=state.enemy;
  const stats = $("#stats"); stats.innerHTML = "";
  stats.appendChild(el("div",{class:"stat"}, `Tes PV: ${p.pv}/${p.maxPv}`));
  stats.appendChild(bar("player-bar", p.pv, p.maxPv, "linear-gradient(90deg,var(--accent), #9bffb3)"));
  stats.appendChild(el("div",{class:"stat"}, `${e.name} PV: ${e.pv}/${e.maxPv}`));
  stats.appendChild(bar("enemy-bar", e.pv, e.maxPv, "linear-gradient(90deg,#ff6b6b, #ff2d2d)"));
  // story & info
  $("#log").innerHTML = state.log.join("\n");
  $("#player-avatar").textContent = p.emoji ? `${p.emoji}` : "Héros";
  $("#player-info").textContent = `${p.hero ? p.hero.name : ""} ${p.power?(" • "+p.power):""} ${p.weapon?(" • "+p.weapon):""} ${state.difficulty?(" • "+state.difficulty):""}`;
}

function bar(id,current,max, bg){
  const cont = document.createElement("div"); cont.className="bar-container";
  const b = document.createElement("div"); b.className="bar"; b.id = id;
  const pct = max>0 ? (current/max)*100 : 0;
  b.style.width = `${pct}%`; b.style.background = bg;
  cont.appendChild(b); return cont;
}

function pushLog(line){
  state.log.unshift(line);
  state.log = state.log.slice(0,8);
  renderStatus();
}

function setChoices(list){
  const c = $("#choices"); c.innerHTML="";
  list.forEach(opt => {
    const b = el("button",{class:"choice"}, opt.label);
    b.onclick = opt.onClick;
    c.appendChild(b);
  });
}

/* ---------- Selection flow ---------- */
function start(){
  // reset state
  state.player = { hero:null, power:null, weapon:null, emoji:"", pv:PLAYER_BASE_PV, maxPv:PLAYER_BASE_PV, usedUniquePowers:{} };
  state.difficulty = "Normal";
  applyDifficultySettings();
  state.enemy = { name:"Thanos", pv:state.enemy.maxPv, maxPv:state.enemy.maxPv, minDmg:state.enemy.minDmg, maxDmg:state.enemy.maxDmg, critAdd:state.enemy.critAdd };
  state.round = 1; state.attackCount = 0; state.log = [];
  $("#story").textContent = "Choisis ton Avenger, puis pouvoir, arme et difficulté.";
  buildUI();
  setChoices([]);
  renderStatus();
}

function chooseHero(h){
  state.player.hero = h;
  state.player.emoji = h.emoji;
  // populate powers for this hero
  const powerList = $("#power-list"); powerList.innerHTML = "";
  POWERS[h.name].forEach(pw => {
    const b = el("button",{class:"choice"}, pw);
    b.onclick = () => choosePower(pw);
    powerList.appendChild(b);
  });
  pushLog(`Héros choisi : ${h.emoji} ${h.name}`);
  renderStatus();
}

function choosePower(pw){
  state.player.power = pw;
  pushLog(`Pouvoir choisi : ${pw} (usage unique)`);
  renderStatus();
}

function chooseWeapon(w){
  state.player.weapon = w;
  pushLog(`Arme choisie : ${w}`);
  renderStatus();
  // show start combat button only when all chosen
  setChoices([{ label:"⚔️ Commencer le combat final", onClick: beginCombat }]);
}

function chooseDifficulty(d){
  state.difficulty = d;
  applyDifficultySettings();
  pushLog(`Difficulté : ${d}`);
  renderStatus();
}

/* Apply difficulty to enemy PV/damage */
function applyDifficultySettings(){
  const cfg = DIFFICULTIES[state.difficulty] || DIFFICULTIES["Normal"];
  const pv = Math.round(THANO_BASE_PV * cfg.enemyPvMult);
  const minD = cfg.enemyMin;
  const maxD = cfg.enemyMax;
  state.enemy.maxPv = pv;
  state.enemy.pv = pv;
  state.enemy.minDmg = minD;
  state.enemy.maxDmg = maxD;
  state.enemy.critAdd = cfg.enemyCritAdd || 0;
}

/* ---------- Combat (3 attacks per round) ---------- */
function beginCombat(){
  if(!state.player.hero){ pushLog("Choisis un héros d'abord."); return; }
  if(!state.player.power){ pushLog("Choisis un pouvoir d'abord."); return; }
  if(!state.player.weapon){ pushLog("Choisis une arme d'abord."); return; }
  state.round = 1;
  state.attackCount = 0;
  state.log = [];
  state.player.pv = state.player.maxPv;
  state.enemy.pv = state.enemy.maxPv;
  pushLog("⚔️ Combat engagé : Arrête Thanos !");
  renderStatus();
  playerAttackPrompt();
}

function playerAttackPrompt(){
  if(checkEnd()) return;
  $("#story").textContent = `Round ${state.round}/${state.enemy.maxDmg ? state.enemy.maxDmg : state.enemy.maxPv} — Attaque ${state.attackCount+1}/3`;
  setChoices([
    { label:"💨 Attaque rapide (3-5 PV)", onClick: ()=>playerAttack("rapide") },
    { label:"💥 Attaque lourde (5-9 PV, 20% échec)", onClick: ()=>playerAttack("lourde") },
    { label:`✨ Pouvoir (${state.player.power}) (usage unique)`, onClick: ()=>playerAttack("pouvoir") },
    { label:`⚔️ Utiliser arme (${state.player.weapon}) (4-7 PV)`, onClick: ()=>playerAttack("arme") }
  ]);
}

function playerAttack(type){
  if(checkEnd()) return;
  // projectile animation from player to enemy
  const panel = document.querySelector(".panel");
  const proj = document.createElement("div"); proj.className = "projectile";
  // color/type
  if(type==="pouvoir") proj.style.background = "#9b7dff";
  else if(type==="lourde") proj.style.background = "#ff9a66";
  else if(type==="arme") proj.style.background = "#a6e1ff";
  else proj.style.background = "var(--accent)";
  // place at player center
  const playerAvatar = $("#player-avatar");
  const startRect = playerAvatar.getBoundingClientRect();
  const panelRect = panel.getBoundingClientRect();
  proj.style.left = (startRect.left - panelRect.left + startRect.width/2) + "px";
  proj.style.top  = (startRect.top - panelRect.top + startRect.height/2) + "px";
  panel.appendChild(proj);
  proj.addEventListener("animationend", ()=> {
    setTimeout(()=>{ proj.remove(); applyPlayerDamage(type); }, 60);
  });
}

function applyPlayerDamage(type){
  const p = state.player;
  let dmg = 0;
  let miss = false;
  let crit = false;

  if(type==="rapide"){ dmg = rnd(3,5); }
  else if(type==="lourde"){ dmg = rnd(5,9); if(Math.random() < 0.20){ dmg = 0; miss = true; } }
  else if(type==="pouvoir"){
    if(UNIQUE_POWERS.includes(p.power)){
      if(p.usedUniquePowers[p.power]){ pushLog("⚠️ Pouvoir déjà utilisé ce combat ! Attaque rapide à la place."); dmg = rnd(3,5); }
      else { dmg = rnd(8,12); p.usedUniquePowers[p.power] = true; pushLog(`💥 ${p.power} activé ! (usage unique)`); }
    } else { dmg = rnd(6,10); }
  }
  else if(type==="arme"){
    dmg = rnd(4,7);
    if(p.weapon === "Épée asgardienne") dmg += 1;
  }

  // critique joueur 20%
  if(!miss && dmg > 0 && Math.random() < 0.20){
    crit = true; dmg = dmg * 2;
  }

  // apply damage
  state.enemy.pv = Math.max(0, state.enemy.pv - dmg);

  // visual: flash + shake
  flashBar("enemy-bar");
  const enemyAvatar = $("#enemy-avatar"); enemyAvatar.classList.add("shake");
  setTimeout(()=>enemyAvatar.classList.remove("shake"), 300);

  // floating damage
  if(!miss && dmg > 0) floatingDamage(enemyAvatar, `-${dmg}`, crit ? "yellow" : "red");
  else if(miss) floatingDamage(enemyAvatar, "Miss", "gray");

  // log
  if(miss) pushLog(`❌ Ta ${type} a raté !`);
  else pushLog(`${crit ? "✨ Critique ! " : ""}Tu infliges ${dmg} PV à Thanos (${type}).`);

  // increment attack count
  state.attackCount = (state.attackCount || 0) + 1;
  renderStatus();

  // check Thanos death
  if(state.enemy.pv <= 0){ setTimeout(()=>{ pushLog("🎉 Thanos est vaincu ! L'humanité est sauvée."); endGame(true); }, 300); return; }

  // after 3 attacks -> Thanos riposte
  if(state.attackCount >= 3){
    state.attackCount = 0;
    setTimeout(enemyTurn, 600);
  } else {
    setTimeout(playerAttackPrompt, 350);
  }
}

/* Floating damage helper */
function floatingDamage(targetEl, txt, colorName){
  const rect = targetEl.getBoundingClientRect();
  const f = document.createElement("div"); f.className = "floating-dmg"; f.textContent = txt;
  f.style.left = (rect.left + rect.width/2) + "px";
  f.style.top  = (rect.top - 18) + "px";
  if(colorName === "yellow") f.style.color = "var(--crit)";
  else if(colorName === "green") f.style.color = "var(--heal)";
  else if(colorName === "gray") f.style.color = "var(--muted)";
  else f.style.color = "var(--hit)";
  document.body.appendChild(f);
  setTimeout(()=>{ f.style.transform = "translateY(-42px)"; f.style.opacity = 0; }, 30);
  setTimeout(()=> f.remove(), 900);
}

/* Enemy turn (Thanos) */
function enemyTurn(){
  if(state.enemy.pv <= 0){ endGame(true); return; }
  // enemy damage with ramp
  const ramp = Math.floor(state.round / 2);
  let dmg = rnd(state.enemy.minDmg + ramp, state.enemy.maxDmg + ramp);
  // enemy crit chance addition treated as small chance to double
  if(Math.random() < (0.05 + (state.enemy.critAdd || 0))){ dmg = dmg * 2; pushLog("💥 Thanos a porté un coup critique !"); }

  state.player.pv = Math.max(0, state.player.pv - dmg);
  flashBar("player-bar");
  floatingDamage($("#player-avatar"), `-${dmg}`, "red");

  const playerAvatar = $("#player-avatar"); playerAvatar.classList.add("shake");
  setTimeout(()=>playerAvatar.classList.remove("shake"), 300);

  pushLog(`⚔️ Thanos riposte et inflige ${dmg} PV à ${state.player.hero?.name || "toi"}.`);
  state.round++;
  renderStatus();

  // end checks
  if(state.player.pv <= 0){ setTimeout(()=>{ pushLog("💀 Tu as été vaincu... Thanos gagne."); endGame(false); }, 250); return; }
  if(state.round > state.enemy.maxDmg /* dummy check avoided */ && state.round > state.enemy.maxDmg){} // no-op
  if(state.round > Math.ceil(state.enemy.maxPv / 10) + 5){} // safety

  if(state.round > state.enemy.maxPv){} // no-op

  // If Thanos had his allowed number of ripostes: use configured rounds
  if(state.round > Math.ceil(state.enemy.maxPv / 10) + 100){} // no-op

  // real end: number of ripostes limited by configured rounds
  if(state.round > Math.ceil(state.enemy.maxPv / 10) + 100){} // placeholder

  // check if we've exhausted allowed ripostes
  if(state.round > state.enemy.maxPv){} // no-op

  // use configured rounds (from applyDifficultySettings we set enemy.maxPv etc)
  if(state.round > (DIFFICULTIES[state.difficulty] ? DIFFICULTIES[state.difficulty].enemyMaxRounds || 5 : 5)){
    // unreachable default path — keep game going normally
  }

  // continue if not ended
  setTimeout(playerAttackPrompt, 400);
}

function checkEnd(){
  if(state.enemy.pv <= 0 || state.player.pv <= 0) { setChoices([]); return true; }
  return false;
}

/* End game */
function endGame(success){
  if(success){
    $("#story").textContent = "🏁 Victoire ! Thanos est vaincu — l'humanité est sauvée.";
    pushLog("🏆 Tu as gagné !");
  } else {
    $("#story").textContent = "☠️ Défaite... Thanos a presque atteint son objectif.";
    pushLog("❌ Partie terminée.");
  }
  setChoices([{ label:"🔁 Rejouer", onClick: start }]);
}

/* flash bar */
function flashBar(id){
  const b = $("#"+id);
  if(!b) return;
  b.classList.add("hit");
  setTimeout(()=>b.classList.remove("hit"), 300);
}

/* bind restart */
$("#restart").addEventListener("click", start);

/* initial build & start */
buildUI();
start();

</script>
</body>
</html>
