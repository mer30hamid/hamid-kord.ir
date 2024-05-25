---
title: پیوندهای مفید
description: "لینک برنامه ها، پروژه ها و سایت های مورد علاقه من"
draft: false
type: page
noComment: true
---

<table id="myCards">
  <thead>
    <tr>
      <th>Find in page</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <div class="card" dir=ltr>
          <p>TiddlyWiki5 in the Sky using <strong> Dropbox </strong>.</p>
          <p class="card-footer">
            <a class="icon" href="https://github.com/mer30hamid/tw5-dropbox" target="_blank" rel="noreferrer noopener me">
                {{< icon github >}}
            </a>
            <a class="icon" href="https://mer30hamid.github.io/tw5-dropbox/" target="_blank" rel="noreferrer noopener me">
                {{< icon external-link >}}
            </a>
          </p>
        </div>
      </td>
    </tr>
    <tr>
      <td>
        <div class="card" dir=ltr>
          <p>cryptgeon is a secure, open source note / file sharing service inspired by PrivNote <strong> written in rust </strong> & svelte.</p>
          <p class="card-footer">
            <a class="icon" href="https://github.com/cupcakearmy/cryptgeon" target="_blank" rel="noreferrer noopener me">
                {{< icon github >}}
            </a>
            <a class="icon" href="https://cryptgeon.org/" target="_blank" rel="noreferrer noopener me">
                {{< icon external-link >}}
            </a>
          </p>
        </div>
      </td>
    </tr>
  </tbody>
</table>



<div id="wrapper"></div>

<script src="https://cdn.jsdelivr.net/npm/gridjs@6.2.0/dist/gridjs.production.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gridjs@6.2.0/l10n/dist/l10n.umd.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/gridjs@6.2.0/dist/theme/mermaid.min.css">

<style>

/* Container needed to position the button. Adjust the width as needed */
.changBgBtn {
  position: relative;
  width: 12%;
}

/* Make the image responsive */
.changBgBtn img {
  width: 100%;
  height: auto;
}

/* Style the button and place it in the middle of the container/image */
.changBgBtn .btn {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
  background-color: #555;
  color: white;
  font-size: 16px;
  padding: 12px 24px;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.changBgBtn .btn:hover {
  background-color: black;
}



.card {
  font-size: 1.1em;
  color: #224;
  max-width: 500px;
  min-height: 200px;
  height: 300px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-bottom: 15px;
  padding: 0.5em;
  border: 1px solid rgba(255, 255, 255, .25);
  border-radius: 20px;
  background-color: rgba(255, 255, 255, 0.45);
  box-shadow: 0 0 10px 1px rgba(0, 0, 0, 0.25);
  backdrop-filter: blur(15px);
}

.card-footer a{
  font-size: 0.75em;
}

.card-footer a.icon {
  font-size: 1.5em;
  margin-right: 10px;
}



.gridjs-search{
  float: right;
}


.gridjs-wrapper, .gridjs-td, .gridjs-tr, .gridjs-tbody, .gridjs-table{
    background: transparent !important;
    border: none !important;
    max-width: 620px !important;
    box-shadow: none;
}

.gridjs-footer, .gridjs-pages button{
  max-width: 620px !important;
  color: #224;
  background-color: rgba(255, 255, 255, 0.45) !important;
  box-shadow: 0 0 10px 1px rgba(0, 0, 0, 0.25);
  backdrop-filter: blur(15px);
}


body{
    background-image: url("/bg/3.png") !important;
    background-attachment: fixed;
}


/* input.gridjs-input, .gridjs-search-input{
    min-height: 40px;
    margin: 0 20px 10px 0;
    width: 250px;
    padding: 0 15px 0 15px;
    border-radius: 25px;
    background-color: #e7fff1f5;
} */

.gridjs-message{
    color: oldlace
}

.gridjs-pagination .gridjs-pages button:first-child {
  border-bottom-left-radius: unset;
  border-top-left-radius: unset;
  border-bottom-right-radius: 6px;
  border-top-right-radius: 6px;
}

.gridjs-pagination .gridjs-pages button:last-child:focus {
  margin-right: unset;
  margin-left: 0;
}

.gridjs-pagination .gridjs-pages button:last-child {
  border-bottom-right-radius: unset;
  border-top-right-radius: unset;
  border-right: unset;
  border-bottom-left-radius: 6px;
  border-top-left-radius: 6px;
  border-left: 1px solid #d2d6dc;
}

</style>

<script>

// for(let i=1;i<=13;++i){
//   document.write('<div class="changBgBtn">');
//   document.write('<img src="/bg/'+ i +'.png" alt="Snow">');
//   document.write('<button class="btn" onclick="document.getElementsByClassName(\'app-container\')[0].style.background=\'url(/bg/' + i + '.png)\'">Set ' + i + '</button>');
//   document.write('</div>');
// }

myCards = document.getElementById("myCards");

const grid = new gridjs.Grid({
  from: myCards,
  language: gridjs.l10n.faIR,
  search: {
    ignoreHiddenColumns: false,
  },
  pagination: {
    limit: 5
  },
  style: {
    th: {'display': 'none'}}
});


let wrp = document.getElementById("wrapper");

grid.render(wrp);

window.onload = function() {
    wrp.getElementsByTagName("input")[0].focus();
};

</script>