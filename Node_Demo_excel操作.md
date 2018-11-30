excel操作

```
var Excel = require('exceljs');
var workbook = new Excel.Workbook();
workbook.xlsx.readFile('basic.xlsx').then(function(){
  var worksheet = workbook.getWorksheet('Sheet1');
  var workcell = worksheet.getCell('A1');
  console.log(workcell.value);
  workcell.value = 'jm';
  // console.log(Object.keys(workbook));
  worksheet.getCell('C8');
  // Add an array of rows
  var rows = [
    [5,'Bob',new Date()], // row by array
    {id:6, name: 'Barbara', dob: new Date()}
  ];
  worksheet.addRows(rows);
  workbook.xlsx.writeFile('basic.xlsx');
});

// worksheet = workbook.addWorksheet('My Sheet');
// worksheet.autoFilter = 'A1:C1';
// var cell = worksheet.getCell('C3');
// cell.value = new Date(1968, 5, 1);
// workbook.commit();

```
