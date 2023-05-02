# #1 [2022 UPDATE] THE CANVAS API

## ****#1.0 The Power of Canvas (04:10)****

- canvas API는 우리가 JavaScript로 그래픽을 그릴 수 있게 해주는 API
- WebGL API로 2D나 3D 그래픽을 그릴 수 있음

## #1.1 Our First Drawing (07:44)

- **canvas의 좌표 시스템은 left top 0(좌상단 끝부분)을 0, 0으로 시작함**
    
    ```jsx
    const canvas = document.querySelector('canvas');
    const ctx = canvas.getContext("2d"); // 기본적으로 붓과 같은 존재
    canvas.width = 800;
    canvas.height = 800;
    
    ctx.fillRect(50, 50, 100, 200);
    ```
    

## #1.2 Paths (06:59)

- fill은 단색으로 모양을 채우는 거고, stroke는 선만(ex) fillRect, strokeRect)
- fillRect 같은 건 shortcut function 이고, 개별적인 function을 통해서 선언할 수 있음
- 경로를 나눠야 개별적으로 style을 줄 수 있음
    
    ```jsx
    const canvas = document.querySelector('canvas');
    const ctx = canvas.getContext("2d"); // 기본적으로 붓과 같은 존재
    canvas.width = 800;
    canvas.height = 800;
    
    ctx.rect(50, 50, 100, 100);
    ctx.rect(150, 150, 100, 100);
    ctx.rect(250, 250, 100, 100);
    ctx.fill();
    
    ctx.beginPath(); // 새로운 경로 설정
    ctx.rect(350, 350, 100, 100);
    ctx.rect(450, 450, 100, 100);
    ctx.fillStyle = 'red';
    setTimeout(function() {
      ctx.fill();
    }, 1000);
    ```
    

## #1.3 moveTo and lineTo (06:10)

- 개별적인 function을 통해서 채워진 정사각형 만들기
    
    ```jsx
    const canvas = document.querySelector('canvas');
    const ctx = canvas.getContext("2d"); // 기본적으로 붓과 같은 존재
    canvas.width = 800;
    canvas.height = 800;
    
    ctx.moveTo(50, 50);
    ctx.lineTo(150, 50);
    ctx.lineTo(150, 150);
    ctx.lineTo(50, 150);
    ctx.lineTo(50, 50);
    ctx.fill();
    ```
    

## #1.4 Drawing Project One (06:20)

- fillRect를 쓴다는 건 shortcut function을 쓴다는 거고, 그 안에는 fill과 rect를 호출
- 집 모양 만들기
    
    ```jsx
    const canvas = document.querySelector('canvas');
    const ctx = canvas.getContext("2d"); // 기본적으로 붓과 같은 존재
    canvas.width = 800;
    canvas.height = 800;
    
    ctx.fillRect(200, 200, 50, 200);
    ctx.fillRect(400, 200, 50, 200);
    ctx.lineWidth = 2;
    ctx.fillRect(300, 300, 50, 100);
    ctx.fillRect(200, 200, 200, 20);
    ctx.moveTo(200, 200);
    ctx.lineTo(325, 100);
    ctx.lineTo(450, 200);
    ctx.fill();
    ```
    

## ****#1.5 Drawing Project Two (08:48)****

```jsx
const canvas = document.querySelector('canvas');
const ctx = canvas.getContext("2d"); // 기본적으로 붓과 같은 존재
canvas.width = 800;
canvas.height = 800;

ctx.fillRect(210 - 40, 200 - 30, 15, 100);
ctx.fillRect(350 - 40, 200 - 30, 15, 100);
ctx.fillRect(260 - 40, 200 - 30, 60, 200);

ctx.arc(250, 100, 50, 0, 2 * Math.PI);
ctx.fill();

ctx.beginPath(); // 별도의 무언가 색깔을 바꾸고 싶으면 새로운 path가 필요한지 체크
ctx.fillStyle = "white";
ctx.arc(260 + 10, 80, 8, Math.PI, 2 * Math.PI);
ctx.arc(220 + 10, 80, 8, Math.PI, 2 * Math.PI);
ctx.fill();
```

# #2 [2022 UPDATE] PAINTING BOARD

## ****#2.0 Painting Lines (07:51)****

```jsx
const canvas = document.querySelector('canvas');
const ctx = canvas.getContext("2d"); // 기본적으로 붓과 같은 존재
canvas.width = 800;
canvas.height = 800;

ctx.lineWidth = 2;

const colors = [
  "#ff3838",
  "#ffb8b8",
  "#c56cf0",
  "#ff9f1a",
  "#fff200",
  "#32ff7e",
  "#7efff5",
  "#18dcff",
  "#7d5fff",
]

function onClick(event) {
  ctx.beginPath();
  ctx.moveTo(800, 800);
  const color = colors[Math.floor(Math.random() * colors.length)];
  ctx.strokeStyle = color;
  ctx.lineTo(event.offsetX, event.offsetY);
  ctx.stroke();
}

canvas.addEventListener("mousemove", onClick);
```

## ****#2.1 Mouse Painting (09:08)****

```jsx
const canvas = document.querySelector('canvas');
const ctx = canvas.getContext("2d"); // 기본적으로 붓과 같은 존재
canvas.width = 800;
canvas.height = 800;
ctx.lineWidth = 2;
let isPainting = false;

function onMove(event) {
  if(isPainting) {
    ctx.lineTo(event.offsetX, event.offsetY);
    ctx.stroke();
    return;
  }

  ctx.moveTo(event.offsetX, event.offsetY);
}

function startPainting() {
  isPainting = true;
}

function cancelPainting() {
  isPainting = false;
}

canvas.addEventListener("mousemove", onMove);
canvas.addEventListener('mousedown', startPainting);
canvas.addEventListener('mouseup', cancelPainting);
canvas.addEventListener('mouseleave', cancelPainting);
```

## #2.2 Line Width (07:10)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Meme Maker</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <canvas></canvas>
  <input id="line-width" type="range" min="1" max="10" value="5" step="0.1">
  <script src="app.js"></script>
</body>
</html>
```

```jsx
const canvas = document.querySelector('canvas');
const lineWidth = document.getElementById('line-width');
const ctx = canvas.getContext("2d"); // 기본적으로 붓과 같은 존재
canvas.width = 800;
canvas.height = 800;
ctx.lineWidth = lineWidth.value;
let isPainting = false;

function onMove(event) {
  if(isPainting) {
    ctx.lineTo(event.offsetX, event.offsetY);
    ctx.stroke();
    return;
  }

  ctx.beginPath();
  ctx.moveTo(event.offsetX, event.offsetY);
}

function startPainting() {
  isPainting = true;
}

function cancelPainting() {
  isPainting = false;
}

function onLineWidthChange(event) {
  ctx.lineWidth = event.target.value;
}

canvas.addEventListener("mousemove", onMove);
canvas.addEventListener('mousedown', startPainting);
canvas.addEventListener('mouseup', cancelPainting);
canvas.addEventListener('mouseleave', cancelPainting);

lineWidth.addEventListener('change', onLineWidthChange);
```

## #2.3 Paint Color part One (03:19)

```jsx
const color = document.getElementById('color');
const canvas = document.querySelector('canvas');
const lineWidth = document.getElementById('line-width');
const ctx = canvas.getContext("2d"); // 기본적으로 붓과 같은 존재
canvas.width = 800;
canvas.height = 800;
ctx.lineWidth = lineWidth.value;
let isPainting = false;

function onMove(event) {
  if(isPainting) {
    ctx.lineTo(event.offsetX, event.offsetY);
    ctx.stroke();
    return;
  }

  ctx.beginPath();
  ctx.moveTo(event.offsetX, event.offsetY);
}

function startPainting() {
  isPainting = true;
}

function cancelPainting() {
  isPainting = false;
}

function onLineWidthChange(event) {
  ctx.lineWidth = event.target.value;
}

function onColorChange(event) {
  ctx.strokeStyle = event.target.value;
  ctx.fillStyle = event.target.value;
}

canvas.addEventListener("mousemove", onMove);
canvas.addEventListener('mousedown', startPainting);
canvas.addEventListener('mouseup', cancelPainting);
canvas.addEventListener('mouseleave', cancelPainting);

lineWidth.addEventListener('change', onLineWidthChange);
color.addEventListener('change', onColorChange);
```

## #2.4 Paint Color part Two (09:34)

- **ArrayLike는 Array가 아님 → 유사배열을 배열로 바꾸고 싶으면 Array.from()**
- HTML5부터는 data- 속성 사용 가능
    
    [https://velog.io/@yunsungyang-omc/HTML-데이터-속성-사용하기-data-attribute](https://velog.io/@yunsungyang-omc/HTML-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%86%8D%EC%84%B1-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-data-attribute)
    
- console로 구조를 보고 싶다면 `console.dir()`

```jsx
const colorOptions = Array.from(document.getElementsByClassName('color-option'));
const color = document.getElementById('color');
const canvas = document.querySelector('canvas');
const lineWidth = document.getElementById('line-width');
const ctx = canvas.getContext("2d"); // 기본적으로 붓과 같은 존재
canvas.width = 800;
canvas.height = 800;
ctx.lineWidth = lineWidth.value;
let isPainting = false;

function onMove(event) {
  if(isPainting) {
    ctx.lineTo(event.offsetX, event.offsetY);
    ctx.stroke();
    return;
  }

  ctx.beginPath();
  ctx.moveTo(event.offsetX, event.offsetY);
}

function startPainting() {
  isPainting = true;
}

function cancelPainting() {
  isPainting = false;
}

function onLineWidthChange(event) {
  ctx.lineWidth = event.target.value;
}

function onColorChange(event) {
  ctx.strokeStyle = event.target.value;
  ctx.fillStyle = event.target.value;
}

function onColorClick(event) {
  const colorValue = event.target.dataset.color;
  // console.dir(event.target.dataset.color);
  ctx.strokeStyle = colorValue;
  ctx.fillStyle = colorValue;
  color.value = colorValue;
  // ctx.strokeStyle = event.target.attributes[2].nodeValue;
  // ctx.fillStyle = event.target.attributes[2].nodeValue;
}

canvas.addEventListener("mousemove", onMove);
canvas.addEventListener('mousedown', startPainting);
canvas.addEventListener('mouseup', cancelPainting);
canvas.addEventListener('mouseleave', cancelPainting);

lineWidth.addEventListener('change', onLineWidthChange);
color.addEventListener('change', onColorChange);

colorOptions.forEach(color => color.addEventListener('click', onColorClick));
```

## #2.5 Filling Mode (06:29)

```jsx
const modeBtn = document.getElementById('mode-btn');
const colorOptions = Array.from(document.getElementsByClassName('color-option'));
const color = document.getElementById('color');
const canvas = document.querySelector('canvas');
const lineWidth = document.getElementById('line-width');
const ctx = canvas.getContext("2d"); // 기본적으로 붓과 같은 존재
canvas.width = 800;
canvas.height = 800;
ctx.lineWidth = lineWidth.value;
let isPainting = false;
let isFilling = false;

function onMove(event) {
  if(isPainting) {
    ctx.lineTo(event.offsetX, event.offsetY);
    ctx.stroke();
    return;
  }

  ctx.beginPath();
  ctx.moveTo(event.offsetX, event.offsetY);
}

function startPainting() {
  isPainting = true;
}

function cancelPainting() {
  isPainting = false;
}

function onLineWidthChange(event) {
  ctx.lineWidth = event.target.value;
}

function onColorChange(event) {
  ctx.strokeStyle = event.target.value;
  ctx.fillStyle = event.target.value;
}

function onColorClick(event) {
  const colorValue = event.target.dataset.color;
  // console.dir(event.target.dataset.color);
  ctx.strokeStyle = colorValue;
  ctx.fillStyle = colorValue;
  color.value = colorValue;
  // ctx.strokeStyle = event.target.attributes[2].nodeValue;
  // ctx.fillStyle = event.target.attributes[2].nodeValue;
}

function onModeClick() {
  if(isFilling) {
    isFilling = false;
    modeBtn.innerText = 'Fill';
  } else {
    isFilling = true;
    modeBtn.innerText = 'Draw';
  }
}

function onCanvasClick() {
  if(isFilling) {
    ctx.fillRect(0, 0, 800, 800);
  }
}

canvas.addEventListener("mousemove", onMove);
canvas.addEventListener('mousedown', startPainting);
canvas.addEventListener('mouseup', cancelPainting);
canvas.addEventListener('mouseleave', cancelPainting);
canvas.addEventListener('click', onCanvasClick);
lineWidth.addEventListener('change', onLineWidthChange);
color.addEventListener('change', onColorChange);

colorOptions.forEach(color => color.addEventListener('click', onColorClick));

modeBtn.addEventListener('click', onModeClick);
```

## #2.6 Eraser (05:27)

```jsx
const modeBtn = document.getElementById('mode-btn');
const destroyBtn = document.getElementById('destroy-btn');
const eraserBtn = document.getElementById('eraser-btn');
const colorOptions = Array.from(document.getElementsByClassName('color-option'));
const color = document.getElementById('color');
const canvas = document.querySelector('canvas');
const lineWidth = document.getElementById('line-width');
const ctx = canvas.getContext("2d"); // 기본적으로 붓과 같은 존재

const CANVAS_WIDTH = 800;
const CANVAS_HEIGHT = 800;

canvas.width = CANVAS_WIDTH;
canvas.height = CANVAS_HEIGHT;
ctx.lineWidth = lineWidth.value;
let isPainting = false;
let isFilling = false;

function onMove(event) {
  if(isPainting) {
    ctx.lineTo(event.offsetX, event.offsetY);
    ctx.stroke();
    return;
  }

  ctx.beginPath();
  ctx.moveTo(event.offsetX, event.offsetY);
}

function startPainting() {
  isPainting = true;
}

function cancelPainting() {
  isPainting = false;
}

function onLineWidthChange(event) {
  ctx.lineWidth = event.target.value;
}

function onColorChange(event) {
  ctx.strokeStyle = event.target.value;
  ctx.fillStyle = event.target.value;
}

function onColorClick(event) {
  const colorValue = event.target.dataset.color;
  // console.dir(event.target.dataset.color);
  ctx.strokeStyle = colorValue;
  ctx.fillStyle = colorValue;
  color.value = colorValue;
  // ctx.strokeStyle = event.target.attributes[2].nodeValue;
  // ctx.fillStyle = event.target.attributes[2].nodeValue;
}

function onModeClick() {
  if(isFilling) {
    isFilling = false;
    modeBtn.innerText = 'Fill';
  } else {
    isFilling = true;
    modeBtn.innerText = 'Draw';
  }
}

function onCanvasClick() {
  if(isFilling) {
    ctx.fillRect(0, 0, CANVAS_WIDTH, CANVAS_HEIGHT);
  }
}

function onDestroyClick() {
  ctx.fillStyle = 'white';
  ctx.fillRect(0, 0, CANVAS_WIDTH, CANVAS_HEIGHT);
}

function onEraserClick() {
  ctx.strokeStyle = 'white';
  isFilling = false;
  modeBtn.innerText = 'Fill';
}

canvas.addEventListener("mousemove", onMove);
canvas.addEventListener('mousedown', startPainting);
canvas.addEventListener('mouseup', cancelPainting);
canvas.addEventListener('mouseleave', cancelPainting);
canvas.addEventListener('click', onCanvasClick);
lineWidth.addEventListener('change', onLineWidthChange);
color.addEventListener('change', onColorChange);

colorOptions.forEach(color => color.addEventListener('click', onColorClick));

modeBtn.addEventListener('click', onModeClick);
destroyBtn.addEventListener('click', onDestroyClick);
eraserBtn.addEventListener('click', onEraserClick);
```

## #2.7 Recap (08:56)

- 기본값은 5를 갖게끔 되어 있음 → html이 js보다 먼저 파싱되기 때문
    
    ```jsx
    <input id="line-width" type="range" min="1" max="10" value="5" step="0.1">
    ```
    

- click은 mousedown + mouseup
- ArrayLike는 Array가 아님 → 유사배열을 배열로 바꾸고 싶으면 Array.from()

## #3.0 Adding Images (10:21)

- 브라우저는 항상 유저의 실제 파일 시스템과 격리되어 있음 ex) 자바스크립트 애플리케이션은 유저의 파일을 읽을 수 없음
- 해당 파일을 url로 담아서 표기할 수 있음
    
    ```jsx
    function onFileChange(event) {
      const file = event.target.files[0]; // 배열인 이유는 attribute로 multiple로 설정할 수 있기 때문
      const url = URL.createObjectURL(file); // 파일이 가리키는 url을 얻는 방식
      console.log(url);
      const image = new Image(); // new Image()는 html에서 <img src=""></img> 이것과 동일
      image.src = url;
      image.onload = function() { // event를 추가하는 다른 방식
        ctx.drawImage(image, 0, 0, CANVAS_WIDTH, CANVAS_HEIGHT);
      }
    }
    ```
    
    - *해당 코드들 모르는 요소들 체크해볼 것
    
    [https://kyounghwan01.github.io/blog/JS/JSbasic/Blob-url/](https://kyounghwan01.github.io/blog/JS/JSbasic/Blob-url/)
    

## ****#3.1 Adding Text (11:09)****

- 더블클릭을 통해서 좌표를 담고 그 좌표에 해당 텍스트를 뿌려주기
    
    ```jsx
    function onDoubleClick(event) {
      const text = textInput.value;
      if(text) {
        ctx.save(); // 이전 값을 저정하고, 중간에 변경값이 있더라도,
        ctx.lineWidth = 1; // 글자 입력 시 1로 변경
        ctx.font = '68px serif',
        ctx.fillText(text, event.offsetX, event.offsetY);
        ctx.restore(); // 끝에서 이전 저장값을 복구해줌
      } else {
        alert('텍스트를 입력하세요')
      }
    }
    ```
    

## ****#3.2 Saving Image (04:31)****

- html 버튼을 생성하고, 해당 버튼에 event 추가한 후에 해당 callback mothod에 다운로드가 가능하게끔 fake link 만들어서 처리
    
    ```jsx
    <button id="save" type="button">Save image</button>
    
    ...
    
    function onSaveClick() {
      const url = canvas.toDataURL(); // base64로 인코딩된 이미지를 돌려줌
      const a = document.createElement('a'); // fake link 생성한 후에 다운로드 세팅
      a.href = url; // href에 해당 url 부여
      a.download = "myDrawing.png"; // download 시 설정될 파일명
      a.click();
    }
    
    ...
    
    saveBtn.addEventListener('click', onSaveClick);
    ```
    
- #일반적인 다운로드 구현
    
    ```jsx
    function download() {
        var anchor = document.createElement('a');
        anchor.setAttribute('href', 'download.txt'); //path
        anchor.setAttribute('download', 'download.txt'); //file name
        document.body.appendChild(anchor);
        anchor.click(); //<a> tag click
        anchor.parentNode.removeChild(anchor);
    }
    ```
    
    [https://velog.io/@kkaemi/javascript-a-tag를-이용하여-javascript에서-파일-다운로드하기](https://velog.io/@kkaemi/javascript-a-tag%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-javascript%EC%97%90%EC%84%9C-%ED%8C%8C%EC%9D%BC-%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C%ED%95%98%EA%B8%B0)
    

## #3.3 Recap (08:53)

- input type=”file”의 event.target.files이 배열인 이유는 해당 file type의 경우 attribute로 multiple을 줄 수 있음 → multiple의 경우 사용자가 파일을 여러 개 올릴 수 있음

```css
/* 모든 css를 제거 */
button {
  all: unset; 
}
```