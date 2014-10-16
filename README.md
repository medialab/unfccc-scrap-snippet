unfccc-scrap-snippet
====================

A code snippet used to scrape UNFCCC data during EMAPS project

We want to get the informations from this list:
http://unfccc.int/essential_background/library/items/3599.php?data=&such=j&title=&author=&keywords=&symbol=%2FCOM&meeting=&mo_from=&year_from=&mo_to=&year_to=&last_days=&sorted=&dirc=&anf=80&seite=1#beg

We build this snippet in order to display a CSV-like text in the Google Chrome console, and we copy-paste it in a file in order to get the CSV. We have to do this for each page.

The script:
```
var makeCell = function(text){text = ''+text;return '"'+text.replace('"', '""')+'"'}
var tables = document.getElementsByTagName('table')
var table = (Array.prototype.slice.call(tables).filter(function(t){return t.getAttribute('bgcolor') == '#CCCCCC'}))[0]
var trList = (Array.prototype.slice.call(table.getElementsByTagName('tr'))).filter(function(tr){return tr.getAttribute('bgcolor') == '#F4F4F4' || tr.getAttribute('bgcolor') == '#FFFFFF'})
var csv = 'symbol,publication_date,material_type,country,link_text_info,link_url_info,link_text_1,link_url_1,link_text_2,link_url_2,link_text_3,link_url_3\n'
+ trList.map(function(tr){if(tr.getAttribute('bgcolor') == '#F4F4F4'){ return makeCell(tr.childNodes[0].textContent)+','+makeCell(tr.childNodes[1].textContent)+','+makeCell(tr.childNodes[2].textContent) } else { return ','+makeCell(tr.childNodes[0].childNodes[1].childNodes[0].childNodes[0].childNodes[1].textContent.split('.')[0])+','+Array.prototype.slice.call(tr.getElementsByTagName('a')).map(function(a){return makeCell(a.textContent) + ',' + makeCell(a.getAttribute('href'))}).join(',')+'\n'}}).join('')
console.log(csv)
```
