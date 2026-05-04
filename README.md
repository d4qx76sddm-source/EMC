# EMC
index.html
<!DOCTYPE html>

<html lang="zh">
<head>
<meta charset="UTF-8">
<title>EMC 数据库</title>

<style>
body {
  margin: 0;
  font-family: Arial;
  background: #ffffff;
  color: #222;
  overflow-x: hidden;
}

/* LOGO背景 */
body::before {
  content: "";
  position: fixed;
  top: 50%;
  left: 50%;
  width: 600px;
  height: 600px;
  transform: translate(-50%, -50%);
  background: url("logo.png") no-repeat center;
  background-size: contain;
  opacity: 0.06;
  z-index: 0;
}

.header {
  background: #dff6f5;
  padding: 15px;
  text-align: center;
  border-bottom: 2px solid #2aa198;
}

.container {
  max-width: 900px;
  margin: auto;
  padding: 20px;
  position: relative;
  z-index: 1;
  animation: fade 0.3s ease;
}

@keyframes fade {
  from {opacity:0;}
  to {opacity:1;}
}

/* 首页 */
.card {
  background: #f8ffff;
  border: 1px solid #cceeee;
  padding: 15px;
  margin: 15px 0;
  border-left: 5px solid #2aa198;
  cursor: pointer;
}

.oath {
  margin: 20px 0;
  padding: 15px;
  background: #f5ffff;
  border-left: 4px solid #2aa198;
  line-height: 1.6;
}

/* 列表 */
.list-item {
  padding: 6px;
  border-bottom: 1px solid #ddd;
  cursor: pointer;
}

.list-item:hover {
  background: #f5ffff;
}

input, textarea, select {
  width: 100%;
  margin-top: 5px;
}

button {
  margin-top: 8px;
  padding: 6px;
  border: none;
  background: #2aa198;
  color: white;
  cursor: pointer;
}

button.delete {
  background: #cc4444;
}

/* 星 */
.star {
  cursor: pointer;
  font-size: 20px;
  color: #ccc;
}

.star.active {
  color: gold;
}
</style>

</head>
<body>

<div class="header">
<h1>EMC 数据库</h1>
<p>Exception Management Company</p>
</div>

<div class="container" id="app"></div>

<script>
let data = JSON.parse(localStorage.getItem("emcData")) || {};

function save(){ localStorage.setItem("emcData", JSON.stringify(data)); }

function show(page){
  const app = document.getElementById("app");

  if(page==="home"){
    app.innerHTML = `
      <div class="oath">
      古老的传说并非偶然，超自然的力量裹挟着人类。我们并非特殊的存在，我们只是相对稳定；或许我们打搅了所谓的平衡，亦或许这世上从未有过平衡。以凡人之躯抵抗神明，以骨肉之身抵御黑暗，不畏惧生死，只为全人类的希望。
      </div>

      <div class="card" onclick="show('anomaly')">异常档案</div>
      <div class="card" onclick="show('event')">事件记录</div>
      <div class="card" onclick="show('story')">故事档案</div>
      <div class="card" onclick="show('lore')">设定数据库</div>
    `;
  }

  if(page==="anomaly"){
    let html = "<h2>异常档案</h2>";

    for(let i=1;i<=1000;i++){
      let id="EMC-"+String(i).padStart(3,"0");
      let a=data[id];
      let name=a ? " —— "+(a.name||"未命名") : "";

      html+=`
      <div class="list-item" onclick="openAnomaly('${id}')">
        ${id}${name}
      </div>`;
    }

    app.innerHTML=html;
  }

  if(page==="event") renderList("event");
  if(page==="story") renderList("story");
  if(page==="lore") renderList("lore");
}

function renderList(type){
  let list=data[type]||[];

  document.getElementById("app").innerHTML=`
    <h2>${type}</h2>
    ${list.map((i,index)=>`
      <div class="list-item" onclick="editItem('${type}',${index})">${i.title}</div>
    `).join("")}
    <button onclick="editItem('${type}',null)">新增</button>
  `;
}

function editItem(type,index){
  let item=index!==null?data[type][index]:{title:"",desc:"",rating:0};

  document.getElementById("app").innerHTML=`
    标题<input id="title" value="${item.title}">
    内容<textarea id="desc">${item.desc}</textarea>
    <button onclick="saveItem('${type}',${index})">保存</button>
    <button class="delete" onclick="deleteItem('${type}',${index})">删除</button>
    <button onclick="show('${type}')">返回</button>
  `;
}

function saveItem(type,index){
if(!data[type]) data[type]=[];

let obj={title:document.getElementById("title").value,desc:document.getElementById("desc").value};

if(index===null) data[type].push(obj);
else data[type][index]=obj;

save(); show(type);
}

function deleteItem(type,index){
if(index!==null){
data[type].splice(index,1);
save(); show(type);
}
}

function openAnomaly(id){
let a=data[id]||{name:"",level:"C1",desc:""};

document.getElementById("app").innerHTML=` <h2>${id}</h2>
    名称<input id="name" value="${a.name||""}">
    等级<select id="level">
      <option>C1</option><option>C2</option>
      <option>B1</option><option>B2</option>
      <option>A1</option><option>A2</option>
      <option>S</option><option>R</option>
      <option>无法分级</option>
    </select>
    内容<textarea id="desc">${a.desc}</textarea>
    <button onclick="saveAnomaly('${id}')">保存</button>
    <button class="delete" onclick="deleteAnomaly('${id}')">删除</button>
    <button onclick="show('anomaly')">返回</button>
`;

document.getElementById("level").value=a.level;
}

function saveAnomaly(id){
data[id]={
name:document.getElementById("name").value,
level:document.getElementById("level").value,
desc:document.getElementById("desc").value
};
save(); show("anomaly");
}

function deleteAnomaly(id){
delete data[id];
save(); show("anomaly");
}

show("home"); </script>

</body>
</html>
