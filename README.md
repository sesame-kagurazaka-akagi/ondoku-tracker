<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-title" content="音読記録">
<title>音読記録</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;}
body{font-family:-apple-system,sans-serif;background:#f5f5f5;max-width:480px;margin:0 auto;}
.top-bar{display:flex;background:#fff;border-bottom:1px solid #e0e0e0;overflow-x:auto;position:sticky;top:0;z-index:20;}
.class-tab{flex-shrink:0;padding:12px 14px;font-size:12px;color:#888;cursor:pointer;border:none;background:none;border-bottom:2px solid transparent;white-space:nowrap;}
.class-tab.on{color:#222;border-bottom-color:#222;font-weight:600;}
.tab-bar{display:flex;background:#f9f9f9;border-bottom:1px solid #e0e0e0;}
.tab{flex:1;padding:10px 2px;font-size:11px;text-align:center;color:#888;cursor:pointer;border:none;background:none;border-bottom:2px solid transparent;}
.tab.on{color:#222;border-bottom-color:#222;font-weight:600;}
.screen{display:none;padding:16px;}
.screen.active{display:block;}
.section-title{font-size:13px;color:#888;margin-bottom:12px;font-weight:600;}
.card{background:#fff;border-radius:14px;padding:16px;margin-bottom:12px;box-shadow:0 1px 4px rgba(0,0,0,0.07);}
.student-header{display:flex;align-items:center;gap:10px;margin-bottom:12px;}
.avatar{width:38px;height:38px;border-radius:50%;background:#dbeafe;display:flex;align-items:center;justify-content:center;font-size:15px;font-weight:600;color:#1d4ed8;flex-shrink:0;}
.avatar.green{background:#dcfce7;color:#166534;}
.student-name{font-size:16px;font-weight:600;color:#222;}
.cal-header{display:grid;grid-template-columns:repeat(7,1fr);gap:2px;margin-bottom:4px;}
.cal-header span{font-size:10px;text-align:center;color:#888;}
.cal-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:3px;margin-bottom:10px;}
.cal-day{min-height:36px;border-radius:8px;display:flex;flex-direction:column;align-items:center;justify-content:center;font-size:11px;cursor:pointer;border:1px solid #eee;background:#fafafa;}
.cal-day.empty{background:none;border:none;cursor:default;}
.cal-day.today{border-color:#60a5fa;}
.cal-day .day-num{font-size:10px;color:#888;margin-bottom:1px;}
.cal-day .dots{display:flex;gap:2px;}
.dot{width:7px;height:7px;border-radius:50%;}
.dot-read{background:#60a5fa;}
.dot-anime{background:#f59e0b;}
.cal-day.has-read{background:#eff6ff;}
.cal-day.has-anime{background:#fffbeb;}
.cal-day.has-both{background:#f0fdf4;}
.legend{display:flex;gap:12px;margin-bottom:12px;flex-wrap:wrap;}
.legend-item{display:flex;align-items:center;gap:4px;font-size:11px;color:#666;}
.legend-dot{width:8px;height:8px;border-radius:50%;}
.month-nav{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px;}
.month-nav button{width:32px;height:32px;border-radius:50%;border:1px solid #ddd;background:#fff;cursor:pointer;font-size:16px;color:#333;}
.month-nav span{font-size:14px;font-weight:600;color:#222;}
.summary-row{display:flex;gap:8px;margin-bottom:8px;}
.summary-box{flex:1;background:#f5f5f5;border-radius:10px;padding:8px;text-align:center;}
.summary-box .val{font-size:18px;font-weight:700;color:#222;}
.summary-box .lbl{font-size:10px;color:#888;margin-top:2px;}
.badge-ame{background:#fef9c3;color:#854d0e;font-size:11px;padding:3px 10px;border-radius:20px;margin-top:4px;display:inline-block;}
.season-card{background:#fff;border-radius:14px;padding:16px;margin-bottom:12px;box-shadow:0 1px 4px rgba(0,0,0,0.07);}
.season-title{font-size:13px;font-weight:600;color:#555;margin-bottom:8px;margin-top:12px;}
.season-row{display:flex;justify-content:space-between;align-items:center;padding:6px 0;border-bottom:1px solid #f0f0f0;font-size:13px;}
.season-yen{font-weight:700;color:#1d4ed8;}
.carry-row{background:#fffbeb;border-radius:8px;padding:8px 12px;font-size:12px;color:#854d0e;margin-top:8px;margin-bottom:8px;}
.msg-box{background:#fff;border-radius:14px;padding:16px;font-size:14px;line-height:1.8;color:#222;white-space:pre-wrap;box-shadow:0 1px 4px rgba(0,0,0,0.07);}
.copy-btn{margin-top:10px;width:100%;padding:12px;border-radius:10px;border:1px solid #ddd;background:#fff;cursor:pointer;font-size:14px;font-weight:600;color:#333;}
.month-select{font-size:14px;padding:6px 10px;border-radius:10px;border:1px solid #ddd;background:#fff;color:#222;margin-bottom:14px;width:100%;}
.settings-row{display:flex;align-items:center;gap:8px;margin-bottom:10px;}
.settings-row input[type=text]{flex:1;font-size:15px;padding:8px 12px;border-radius:10px;border:1px solid #ddd;background:#fff;color:#222;}
.del-btn{width:30px;height:30px;border-radius:50%;border:1px solid #ddd;background:none;cursor:pointer;font-size:16px;color:#888;}
.add-btn{width:100%;padding:10px;border:1.5px dashed #ddd;border-radius:10px;background:none;cursor:pointer;font-size:14px;color:#888;margin-top:4px;}
.save-btn{width:100%;padding:12px;border-radius:10px;border:none;background:#222;cursor:pointer;font-size:15px;font-weight:600;color:#fff;margin-top:14px;}
.modal-bg{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.4);z-index:100;}
.modal-bg.open{display:flex;align-items:center;justify-content:center;}
.modal{background:#fff;border-radius:18px;padding:20px;width:280px;}
.modal-title{font-size:15px;font-weight:600;margin-bottom:8px;color:#222;text-align:center;}
.modal-date{font-size:13px;color:#888;text-align:center;margin-bottom:16px;}
.modal-row{display:flex;align-items:center;justify-content:space-between;padding:10px 0;border-bottom:1px solid #f0f0f0;}
.modal-label{font-size:14px;color:#333;}
.modal-check{width:24px;height:24px;cursor:pointer;accent-color:#60a5fa;}
.modal-close{width:100%;padding:10px;border-radius:10px;border:none;background:#222;color:#fff;font-size:14px;font-weight:600;cursor:pointer;margin-top:12px;}
</style>
</head>
<body>
<div class="top-bar" id="class-tabs"></div>
<div class="tab-bar">
  <button class="tab on" onclick="showSub('calendar',this)">カレンダー</button>
  <button class="tab" onclick="showSub('season',this)">シーズン</button>
  <button class="tab" onclick="showSub('message',this)">メッセージ</button>
  <button class="tab" onclick="showSub('settings',this)">設定</button>
</div>
<div id="screen-calendar" class="screen active"></div>
<div id="screen-season" class="screen"></div>
<div id="screen-message" class="screen">
  <p class="section-title">保護者向けメッセージ</p>
  <select class="month-select" id="msg-month-sel" onchange="renderMessage()"></select>
  <div class="msg-box" id="msg-output"></div>
  <button class="copy-btn" onclick="copyMsg()">コピーする</button>
</div>
<div id="screen-settings" class="screen">
  <p class="section-title" id="settings-title">生徒名の編集</p>
  <div id="name-list"></div>
  <button class="add-btn" onclick="addStudent()">+ 生徒を追加</button>
  <button class="save-btn" onclick="saveNames()">保存する</button>
</div>
<div class="modal-bg" id="modal">
  <div class="modal">
    <div class="modal-title" id="modal-name"></div>
    <div class="modal-date" id="modal-date"></div>
    <div class="modal-row">
      <span class="modal-label">音読した</span>
      <input type="checkbox" class="modal-check" id="modal-read">
    </div>
    <div class="modal-row" id="modal-anime-row">
      <span class="modal-label">アニメ視聴（ゴールド★）</span>
      <input type="checkbox" class="modal-check" id="modal-anime">
    </div>
    <button class="modal-close" onclick="closeModal()">保存して閉じる</button>
  </div>
</div>
<script>
var MONTHS=['1月','2月','3月','4月','5月','6月','7月','8月','9月','10月','11月','12月'];
var DAYS=['日','月','火','水','木','金','土'];
var SEASONS=[
  {name:'春',months:[3,4,5],icon:'🌸'},
  {name:'夏',months:[6,7,8],icon:'☀️'},
  {name:'秋',months:[9,10,11],icon:'🍂'},
  {name:'冬',months:[12,1,2],icon:'⛄️'}
];
var classes=[
  {id:'p3',name:'P3教室',anime:true,ame:true,students:['あかり','けんた','まさき','しゅうや','そう','はやと']},
  {id:'p4',name:'P4オンライン',anime:true,ame:false,students:['岡埜朱里','石母田咲希','神戸はると','神田ともあき']}
];
var data={};
var currentClassIdx=0;
var currentYear=new Date().getFullYear();
var currentMonth=new Date().getMonth()+1;
var currentSub='calendar';
var modalInfo={};

try{var sc=localStorage.getItem('ondoku_classes');if(sc)classes=JSON.parse(sc);}catch(e){}
try{var sd=localStorage.getItem('ondoku_data');if(sd)data=JSON.parse(sd);}catch(e){}

function save(){
  try{localStorage.setItem('ondoku_data',JSON.stringify(data));}catch(e){}
  try{localStorage.setItem('ondoku_classes',JSON.stringify(classes));}catch(e){}
}

function daysInMonth(y,m){return new Date(y,m,0).getDate();}
function firstDay(y,m){return new Date(y,m-1,1).getDay();}

function getDay(cid,name,y,m,d){
  var k=cid+'|'+name+'|'+y+'-'+pad(m)+'-'+pad(d);
  return data[k]||{read:false,anime:false};
}
function setDay(cid,name,y,m,d,read,anime){
  var k=cid+'|'+name+'|'+y+'-'+pad(m)+'-'+pad(d);
  data[k]={read:read,anime:anime};
  save();
}
function pad(n){return String(n).padStart(2,'0');}

function monthRead(cid,name,y,m){
  var c=0,days=daysInMonth(y,m);
  for(var d=1;d<=days;d++)if(getDay(cid,name,y,m,d).read)c++;
  return c;
}
function monthAnime(cid,name,y,m){
  var c=0,days=daysInMonth(y,m);
  for(var d=1;d<=days;d++)if(getDay(cid,name,y,m,d).anime)c++;
  return c;
}
function weekRead(cid,name,y,m){
  var now=new Date();
  var ws=new Date(now);ws.setDate(now.getDate()-now.getDay());
  var c=0;
  for(var i=0;i<7;i++){
    var d=new Date(ws);d.setDate(ws.getDate()+i);
    if(d.getFullYear()===y&&d.getMonth()+1===m)
      if(getDay(cid,name,y,m,d.getDate()).read)c++;
  }
  return c;
}

function renderClassTabs(){
  var el=document.getElementById('class-tabs');
  el.innerHTML='';
  for(var i=0;i<classes.length;i++){
    (function(idx){
      var btn=document.createElement('button');
      btn.className='class-tab'+(idx===currentClassIdx?' on':'');
      btn.textContent=classes[idx].name;
      btn.onclick=function(){currentClassIdx=idx;renderClassTabs();renderSub();};
      el.appendChild(btn);
    })(i);
  }
}

function renderCalendar(){
  var cl=classes[currentClassIdx];
  var y=currentYear,m=currentMonth;
  var today=new Date();
  var html='<div class="month-nav">';
  html+='<button onclick="prevMonth()">&#8249;</button>';
  html+='<span>'+y+'年'+MONTHS[m-1]+'</span>';
  html+='<button onclick="nextMonth()">&#8250;</button>';
  html+='</div>';
  html+='<div class="legend">';
  html+='<div class="legend-item"><div class="legend-dot" style="background:#60a5fa"></div>音読</div>';
  if(cl.anime)html+='<div class="legend-item"><div class="legend-dot" style="background:#f59e0b"></div>アニメ(ゴールド★)</div>';
  html+='</div>';
  for(var si=0;si<cl.students.length;si++){
    var name=cl.students[si];
    var rc=monthRead(cl.id,name,y,m);
    var ac=cl.anime?monthAnime(cl.id,name,y,m):0;
    var wc=weekRead(cl.id,name,y,m);
    var ame=cl.ame&&wc>=5;
    html+='<div class="card">';
    html+='<div class="student-header"><div class="avatar'+(cl.ame?' green':'')+'">'+name.charAt(0)+'</div>';
    html+='<div><div class="student-name">'+name+'</div>';
    if(ame)html+='<span class="badge-ame">🍬 今週5回達成！飴あり</span>';
    html+='</div></div>';
    html+='<div class="cal-header">';
    for(var dw=0;dw<7;dw++)html+='<span>'+DAYS[dw]+'</span>';
    html+='</div><div class="cal-grid">';
    var fd=firstDay(y,m),td=daysInMonth(y,m);
    for(var b=0;b<fd;b++)html+='<div class="cal-day empty"></div>';
    for(var d=1;d<=td;d++){
      var dd=getDay(cl.id,name,y,m,d);
      var isT=today.getFullYear()===y&&today.getMonth()+1===m&&today.getDate()===d;
      var dc='cal-day';
      if(isT)dc+=' today';
      if(dd.read&&dd.anime)dc+=' has-both';
      else if(dd.read)dc+=' has-read';
      else if(dd.anime)dc+=' has-anime';
      var nn=name.replace(/\\/g,'\\\\').replace(/'/g,"\\'");
      html+='<div class="'+dc+'" onclick="openModal(\''+nn+'\','+y+','+m+','+d+')">';
      html+='<span class="day-num">'+d+'</span><div class="dots">';
      if(dd.read)html+='<div class="dot dot-read"></div>';
      if(dd.anime)html+='<div class="dot dot-anime"></div>';
      html+='</div></div>';
    }
    html+='</div>';
    html+='<div class="summary-row">';
    html+='<div class="summary-box"><div class="val">'+rc+'</div><div class="lbl">音読回数</div></div>';
    if(cl.anime)html+='<div class="summary-box"><div class="val">'+ac+'</div><div class="lbl">アニメ回数</div></div>';
    html+='<div class="summary-box"><div class="val">'+((rc+ac)*5)+'円</div><div class="lbl">今月の積立</div></div>';
    html+='</div></div>';
  }
  document.getElementById('screen-calendar').innerHTML=html;
}

function renderSeason(){
  var cl=classes[currentClassIdx];
  var now=new Date();
  var thisYear=now.getFullYear();
  var html='<p class="section-title">シーズン集計</p>';
  for(var si=0;si<cl.students.length;si++){
    var name=cl.students[si];
    html+='<div class="season-card">';
    html+='<div class="student-header"><div class="avatar'+(cl.ame?' green':'')+'">'+name.charAt(0)+'</div><div class="student-name">'+name+'</div></div>';
    for(var sei=0;sei<SEASONS.length;sei++){
      var season=SEASONS[sei];
      var total=0;
      html+='<div class="season-title">'+season.icon+' '+season.name+'シーズン</div>';
      for(var mi=0;mi<season.months.length;mi++){
        var mo=season.months[mi];
        var yr=thisYear;
        if(mo>=10&&now.getMonth()+1<=2)yr=thisYear-1;
        var rc2=monthRead(cl.id,name,yr,mo);
        var ac2=cl.anime?monthAnime(cl.id,name,yr,mo):0;
        var yen=(rc2+ac2)*5;
        total+=yen;
        html+='<div class="season-row"><span>'+MONTHS[mo-1]+'</span>';
        html+='<span>音読'+rc2+'回'+(cl.anime?' アニメ'+ac2+'回':'')+'</span>';
        html+='<span class="season-yen">'+yen+'円</span></div>';
      }
      var units=[1000,700,500,200];
      var card=0;
      for(var ui=0;ui<units.length;ui++){
        while(total-card>=units[ui])card+=units[ui];
      }
      var carry=total-card;
      html+='<div class="season-row" style="font-weight:600"><span>合計</span><span></span><span class="season-yen">'+total+'円</span></div>';
      html+='<div class="carry-row">図書カード '+card+'円 / 繰越 '+carry+'円</div>';
    }
    html+='</div>';
  }
  document.getElementById('screen-season').innerHTML=html;
}

function renderMessageScreen(){
  var now=new Date();
  var sel=document.getElementById('msg-month-sel');
  var opts='';
  for(var i=0;i<6;i++){
    var d=new Date(now.getFullYear(),now.getMonth()-i,1);
    var val=d.getFullYear()+'-'+pad(d.getMonth()+1);
    opts+='<option value="'+val+'">'+d.getFullYear()+'年'+MONTHS[d.getMonth()]+'</option>';
  }
  sel.innerHTML=opts;
  renderMessage();
}

function renderMessage(){
  var cl=classes[currentClassIdx];
  var sel=document.getElementById('msg-month-sel').value;
  var parts=sel.split('-');
  var y=parseInt(parts[0]),m=parseInt(parts[1]);
  var label=y+'年'+MONTHS[m-1];
  var lines='【'+label+'の音読チャレンジ結果】\n\nみなさん、今月もよくがんばりました！\n\n';
  for(var i=0;i<cl.students.length;i++){
    var name=cl.students[i];
    var rc=monthRead(cl.id,name,y,m);
    var ac=cl.anime?monthAnime(cl.id,name,y,m):0;
    var yen=(rc+ac)*5;
    lines+='⭐ '+name+' さん：音読'+rc+'回'+(cl.anime?' アニメ'+ac+'回':'')+' → '+yen+'円積立\n';
  }
  lines+='\nシーズン末に図書カードでお渡しします🎁\n引き続きよろしくお願いします🎵';
  document.getElementById('msg-output').textContent=lines;
}

function copyMsg(){
  var text=document.getElementById('msg-output').textContent;
  navigator.clipboard.writeText(text).then(function(){
    var btn=document.querySelector('.copy-btn');
    btn.textContent='コピーしました！';
    setTimeout(function(){btn.textContent='コピーする';},1500);
  });
}

function renderSettings(){
  var cl=classes[currentClassIdx];
  document.getElementById('settings-title').textContent=cl.name+' — 生徒名の編集';
  var html='';
  for(var i=0;i<cl.students.length;i++){
    html+='<div class="settings-row"><input type="text" value="'+cl.students[i]+'" id="sname-'+i+'" />';
    html+='<button class="del-btn" onclick="removeStudent('+i+')">x</button></div>';
  }
  document.getElementById('name-list').innerHTML=html;
}

function addStudent(){classes[currentClassIdx].students.push('新しい生徒');renderSettings();}
function removeStudent(i){classes[currentClassIdx].students.splice(i,1);renderSettings();}
function saveNames(){
  var inputs=document.querySelectorAll('[id^="sname-"]');
  var arr=[];
  for(var i=0;i<inputs.length;i++){var v=inputs[i].value.trim();if(v)arr.push(v);}
  classes[currentClassIdx].students=arr;
  save();renderSettings();renderCalendar();
  var btn=document.querySelector('.save-btn');
  btn.textContent='保存しました！';setTimeout(function(){btn.textContent='保存する';},1500);
}

function openModal(name,y,m,d){
  var cl=classes[currentClassIdx];
  var dd=getDay(cl.id,name,y,m,d);
  modalInfo={name:name,y:y,m:m,d:d};
  document.getElementById('modal-name').textContent=name;
  document.getElementById('modal-date').textContent=y+'年'+MONTHS[m-1]+d+'日';
  document.getElementById('modal-read').checked=dd.read;
  document.getElementById('modal-anime').checked=dd.anime;
  document.getElementById('modal-anime-row').style.display=cl.anime?'flex':'none';
  document.getElementById('modal').classList.add('open');
}

function closeModal(){
  var cl=classes[currentClassIdx];
  var read=document.getElementById('modal-read').checked;
  var anime=document.getElementById('modal-anime').checked;
  setDay(cl.id,modalInfo.name,modalInfo.y,modalInfo.m,modalInfo.d,read,anime);
  document.getElementById('modal').classList.remove('open');
  renderCalendar();
}

function prevMonth(){currentMonth--;if(currentMonth<1){currentMonth=12;currentYear--;}renderCalendar();}
function nextMonth(){currentMonth++;if(currentMonth>12){currentMonth=1;currentYear++;}renderCalendar();}

function showSub(name,tabEl){
  document.querySelectorAll('.screen').forEach(function(s){s.classList.remove('active');});
  document.querySelectorAll('.tab').forEach(function(t){t.classList.remove('on');});
  document.getElementById('screen-'+name).classList.add('active');
  tabEl.classList.add('on');
  currentSub=name;
  renderSub();
}

function renderSub(){
  if(currentSub==='calendar')renderCalendar();
  else if(currentSub==='season')renderSeason();
  else if(currentSub==='message')renderMessageScreen();
  else if(currentSub==='settings')renderSettings();
}

renderClassTabs();
renderCalendar();
</script>
</body>
</html>
