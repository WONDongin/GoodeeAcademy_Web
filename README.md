# GoodeeAcademy
GoodeeAcademy_ Boot Camp_Web

# D-DAY
25.03.13~25.03.31

## 2M (HTML, CSS, JavaScript) 
🏆[HTML,CSS 회고록](https://dongin97.tistory.com/category/HTML_CSS)
🏆[JavaScript 회고록](https://dongin97.tistory.com/category/JavaScript)
- 1주차 : HTML(기본구조,글자/목록/표/영역/하이퍼링크/폼/이미지 태그), CSS(Bootstrap)
- 2주차 : JavaScript(자료형,연산자,데이터 입출력,요소접근,문자열,조건/반복문,Math,Date)

## Blog
- https://dongin97.tistory.com/

<br>
<br>

#### **☑️ 개인 프로젝트\_최종완료**

**코인(매수/매도)차트** 게임

---

#### **☑️ 작업기간**

2025/02/25 ~ 2025/02/27

#### **☑️ 게임설명**

**턴 선택 후** 시작을 **눌러 원하는 구간에 매수**를 하고 원하는 **수익률 도달시 매도**하여 **시드를 늘리는 게임**

\- **코인종류 3종류**(난이도 개념)

\- **턴 제 방식 :** **1,10,30턴** 선택하여 차트를 볼수있다.

\- **턴 시작 후 코인 종목 선택 불가**

\- 상승/하락 **각각 50% 확률**, **초기 시드 30만**

\- **매도 시 게임종료(반복 매수X)**, 최종시드, 코인 등락률, 내 수익률을 확인 할 수 있다.

\- **게임 다시하겠습니까?** 

   **Y :** 수익율을 반영한 시드로 **다시 게임시작**

   **N :** 최종 시드 기준 **등급을 확인가능**(100만 이상(챌린저), 70만 이상(마스터), 50만 이상(플레티넘) 미만 코린이)

---

#### **☑️ 주요 메소드 / 태그**

**✅ canvars  :** 캔버스 태그를 사용하여 **x,y 값을 조정**하여 이전 차트 **y값 기준 랜덤 수치범위 안 으로** 차트가 나타나도록 설정

```
// 캔버스 크기 및 변수설정
let canvasWidth = 1200;
let canvasHeight = 800;
...
function createChart(chartY) {
	let newChart = document.createElement("span");
	newChart.style.position = 'absolute';
	newChart.style.width = '20px';
	newChart.style.height = '100px';
                
	let x = charts.length * 40; 
	let { newY, isUp } = getChangeAmount(chartY);
	newChart.style.backgroundColor = isUp ? '#ff3131' : '#7575ff';
    
	// x +=40px, y: 등락률*10px (신규생성)
	newChart.style.transform = `translate(${x}px, ${newY}px)`;

	document.querySelector("#main").appendChild(newChart);
	return newY;
}
```

****✅** 수치 계산식**

**등락률 누적 :** (등락률 - 1) \* 100  

```
 udRate *= (1 + rate / 100);
```

**수익률 :** ((현재 주가 - 평균단가 )/평균단가 ) \* 100 

```
 let myRate = shares > 0 ? ((stockPrice - averagePrice) / averagePrice) * 100 : 0;
```

**평가금액  :** 주식수 \* 현재주가

```
 myPrice = shares * stockPrice;
```

**최종 시드** 

```
let seed = parseFloat(document.getElementById("seed").value);
let profit = seed - updateSeed; // 기본값

let profit = seed - updateSeed; // 게임종료시 정산

updateSeed = seed; // 게임 리셋(초기설정)
```

**✅ toLocaleString**

**콤마가 포함된 문자열**로 변환

```
document.getElementById("seed").value = Math.round(seed).toLocaleString();
document.getElementById("stockPrice").value = Math.round(stockPrice).toLocaleString();
document.getElementById("myPrice").value = Math.round(shares * stockPrice).toLocaleString();
...
```

**\*정규식**을 사용하는 방법보다 **사용이 용이한 것 같다.**

```
toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
```

**✅ parseFlot** 

입력받은 **문자열을 실수로 변환 한 값**을 리턴

**실수로 변환할 수 없는 경우**에는 **NaN (Not a Number)** 값을 리턴

```
 stockPrice = parseFloat(document.getElementById("stock").value);
```

**✅ Math**

**Math.random()  < 0.5 :** 50%(0~1 미만의 실수 중 0.5보다 작은 경우)

```
let isUp = Math.random() < 0.5; // 상승/하락(50%)
```

**Math.floor(Math.random() \* 10) + 1 :** 1~10 사이 정수, 소수점 아래 버림(정수)

```
let rate = Math.floor(Math.random() * 10) + 1; // 1~10% 등락률 범위
```

**Math.max(0, Math.min(canvasHeight - 100, chartY))** :  **0 이상 (canvasHeight - 100) 이하**의 범위를 벗어나지 않는 chartY 값을 반환

**Math.min(canvasHeight - 100, chartY)** → chartY가 canvasHeight - 100보다 크면 그 값으로 제한

**Math.max(0, ...)** → chartY가 0보다 작아지지 않도록 보정

---

#### **☑️ 기능 별 함수**

**✅ 전역변수, 초기값, 캔버스 크기설정**

\- **가로,세로  :** 1200\*800 

\- 신규 생성되는 차트를 **charts \[\] 배열에 저장**

\- stock value 값이 변경 될때마다 **stockPrice value 값 변경**

\- 시드, 주식가격, 보유주식 수, 평균 매수가, 평가금액 초기설정

```
            // 캔버스 크기 및 변수설정
            let canvasWidth = 1200;
            let canvasHeight = 800;
            let charts = []; 
            let interval;
            // 상승률 (기본값 1)
            let udRate = 1; 
            // 시드 (10만원) - toLocaleString: 천단위 , 형식
            let seed = parseFloat(document.getElementById("seed").value);
            let updateSeed = seed;
            // 주식 가격
            let stockPrice = parseFloat(document.getElementById("stock").value);
             // 보유 주식 수, 평균 매수가, 평가금액
            let shares = 0;           
            let averagePrice = 0; 
            let myPrice = 0;         
            document.getElementById("seed").value = seed.toLocaleString();
            document.getElementById("stockPrice").value = stockPrice.toLocaleString();
            document.getElementById("myPrice").value = myPrice.toLocaleString();
            
            //  투자 종목별 주가(onchange)
            function stock() {
                stockPrice = parseFloat(document.getElementById("stock").value);
                document.getElementById("stockPrice").value = stockPrice.toLocaleString();
            }
```

---

**✅ 상승/하락 (주가 변동)**

\- **상승/하락 :** 50% 확률 설정

\- **상승/하락률 :** 1 ~ 10% 범위설정

\- **등락률 대비 \* 10px** 만큼 차트 **y 값 조정**

\- 등락률을 반영하여 평가금액, 수익률 , 종목주가 표기

\- 캔버스 영역 **상/하단 끝에 도달시 더이상 넘어가지 못하도록** 설정 **(캔버스높이 - 차트 높이(100px))**

```
            function getChangeAmount(chartY) {
                let isUp = Math.random() < 0.5; // 상승/하락(50%)
                let rate = Math.floor(Math.random() * 10) + 1; // 1~10%
                let change = rate * 10; // 등락률 * 10px 차트변동
                
                // isUp : true(상승) / false(하락)
                if (isUp) { 
                    chartY -= change;                    // 차트 y값  누적
                    udRate *= (1 + rate / 100);      //  등락률 누적
                    stockPrice *= (1 + rate / 100);//  종목주가 누적
                } else {
                    chartY += change;
                    udRate *= (1 - rate / 100);
                    stockPrice *= (1 - rate / 100);
                }
                // 평가금액  : 주식수 * 현재주가
                myPrice = shares * stockPrice;
                // 수익률 : ((현재 주가 - 평균단가 )/평균단가 ) * 100 
                let myRate = shares > 0 ? ((stockPrice - averagePrice) / averagePrice) * 100 : 0;
                // 등락률 표기 : (등락률 - 1) * 100  
                document.getElementById("udRate").value = ((udRate - 1) * 100).toFixed(2) + "%";
                document.getElementById("stockPrice").value = Math.round(stockPrice).toLocaleString();
                document.getElementById("myPrice").value = Math.round(myPrice).toLocaleString();
                document.getElementById("myRate").value = myRate.toFixed(2) + "%";
                //  캔버스 영역 제한
                return { newY: Math.max(0, Math.min(canvasHeight - 100, chartY)), isUp };
            }
```

---

**✅ 차트 생성**

\- **span** 태그로 차트 **객체(20\*100px)** 생성하여 **#main 부분에 appenChild**로 1개 생성

\- **x :** 40px 간격으로 위치 생성

**\- y:** 이전 차트 y +- (등락률\*10px) 위치 생성

\- **상승/하락**에 맞게 **배경색 변경**

```
            function createChart(chartY) {
                let newChart = document.createElement("span");
                newChart.style.position = 'absolute';
                newChart.style.width = '20px';
                newChart.style.height = '100px';
                
                let x = charts.length * 40; 
                let { newY, isUp } = getChangeAmount(chartY);
                // 상승/하락 : 배경색 지정
                newChart.style.backgroundColor = isUp ? '#ff3131' : '#7575ff';
                // x +=40px, y: 등락률*10px (신규생성)
                newChart.style.transform = `translate(${x}px, ${newY}px)`;
                // #main 차트영역 안 추가
                document.querySelector("#main").appendChild(newChart);
                return newY;
            }
```

---

**✅ 게임 실행**

\- 게임 시작 시 코인 선택 **비활성화**

\- 지정한 **턴 value 만큼 0.1초 마다** 신규 차트 생성

\- **첫 차트 생성위치 :** 높이 기준 **가운데 생성** (전체 캔버스 / 2 : 400px)

\- 평가금액이 **0이 아니고, 10,000원 미만**일 경우(어떠한 종목의 주식을 살수 없는 수치) → **강제 청산(강제종료, 재시작 불가)**

\- **30턴이 지날동안 매도를 안할 경우 :** 강제 매도(강제 게임종료)

\- **if (interval) clearInterval(interval) :** 인터벌 루프 방지(인터벌 초기화)

```
            function game() {
            	 // 코인 선택 비활성화
            	document.getElementById("stock").disabled = true; 
                // 5,10,30턴 만큼 실행
                let count = parseInt(document.querySelector(".cnt").value);
                let createdCount = 0;
                // 초기 첫 차트위치
                let chartY = charts.length > 0 ? charts[charts.length - 1] : 400;
                if (interval) clearInterval(interval); // 인터벌 루프방지

                // 0.1초 마다 신규 차트 생성 + 배열 저장 + y값 누적전달
                interval = setInterval(() => {
                    if (createdCount < count) {
                        chartY = createChart(chartY);
                        charts.push(chartY);
                        createdCount++;

                        myPrice = shares * stockPrice; 
                        // 청산 (시드 < 10000)
                        if (myPrice < 10000 && myPrice != 0 ) {
                            clearInterval(interval);
                            interval = null;
                            // 시작버튼 비활성화 
                            seed = 0;
                            document.getElementById("seed").value = "0";
                            document.getElementById("myPrice").value = "0";
                            document.getElementById("myRate").value = "0.00%";
                            alert("청산되었습니다. 게임을 다시 할 수 없습니다.");
                            return; 
                        }
                    }
                    // 30턴 (최대 턴)
                    if (charts.length == 30) {
                        clearInterval(interval);
                        interval = null;
                        // 30턴이 끝나면 자동 매도 
                        sell(); 
                    }
                }, 100);
            }
```

---

**✅ 매수/매도**

\- **매수 :** 시드 / 현재 주가 (**매수 가능한만큼 풀매수**) 빠른 게임 진행을 위해 설정 

\- **매수불가 :** 시드 보다 현재 주가가 높을 경우

\- 주가 변동률 적용하여 **시드, 평가금액 수치 변동적용**

```
            function buy() {
                let fullShares = Math.floor(seed / stockPrice); // 인생한방 풀매수
                let totalCost = fullShares * stockPrice;
                // 매수불가(시드 < 현재주가)
                if (fullShares <= 0) {
                    alert("시드가 부족하여 매수가 불가능 합니다.\n현재 시드 : " + seed);
                    return;
                }
                // 평균 매수가 계산
                if (shares === 0) {
                     averagePrice = stockPrice;
                }
                shares += fullShares;
                seed -= totalCost;

                document.getElementById("seed").value = Math.round(seed).toLocaleString();
                document.getElementById("myPrice").value = Math.round(shares * stockPrice).toLocaleString();
            }
```

\- **매도  :** 매도시 게임종료

\- 매수를 안하고 30턴이 종료 시 **0.00%로 설정 (NaN  에러 방지)**

\- **시드 += totalSellValue(주식을 판 금액) :** 시드 업데이트

\- 매도 시 : **보유주식 수, 평가금액 초기화 + 시드 업데이트 + 게임종료 알림창**

```
            function sell() {
                if (shares === 0) {
                    //  보유 주식이 없을 경우 수익률을 0%
                    document.getElementById("myRate").value = "0.00%";
                    endGame(0);
                    return;
                }
                let totalSellValue = shares * stockPrice;
                // 수익률  : ((현재주가-평균매수가)/평균매수가)*100
                let myRate = ((stockPrice - averagePrice) / averagePrice) * 100;
                seed += totalSellValue;
                 // 매도 후 초기화
                shares = 0;
                averagePrice = 0;
                document.getElementById("seed").value = Math.round(seed).toLocaleString();
                endGame(myRate);
            }
```

---

**✅ 게임 종료**

\- **최종 수익금 :** 시드 - 현재 턴 수익률 반영금액

\- 게임종료시 **코인 종류 선택가능**

\- 게임종료 알림창 생성 : **등락률,수익률,시드,개임 재시작 유뮤**

\- **게임종료시** 최종시드에 따라 **등급부여**  

```
            function endGame(myRate) { 
                // 최종 수익금
                let profit = seed - updateSeed;
                setTimeout(() => {
                	document.getElementById("stock").disabled = false; // 코인 선택 다시 가능
                    if (confirm("거래 종료!\n시드: " + Math.round(seed).toLocaleString() + "원\n코인 등락률: " + ((udRate - 1) * 100).toFixed(2) + "%\n내 수익률: " + myRate.toFixed(2) + "%\n수익: " + Math.round(profit).toLocaleString() + "원\n게임을 다시 하시겠습니까?\n취소 : 투자 티어를 확인 할 수 있습니다.")) {
                        resetGame();
                    } else {
                        document.getElementById("popup").style.display = "flex";
                        if (seed >= 1000000) {
                            document.getElementById("rank").innerHTML = "챌린저";
                            document.getElementById("rankImage").src = "images/icon_01.png";
                        } else if (seed >= 700000) {
                            document.getElementById("rank").innerHTML = "마스터";
                            document.getElementById("rankImage").src = "images/icon_02.png";
                        } else if (seed >= 500000) {
                            document.getElementById("rank").innerHTML = "플레티넘";
                            document.getElementById("rankImage").src = "images/icon_03.png";
                        } else{
                            document.getElementById("popup").style.background = "none";
                            document.getElementById("rankImage").src = "images/icon_05.png";
                            document.getElementById("rank").innerHTML = "코린이";
                        }
                      
                    }
                }, 100);
            }
```

[##_ImageGrid|kage@uyR2W/btsNkwpWUdY/JENPtFk1QvzYGkYjbu1ZFK/img.png,kage@XrXVk/btsNjCqVG94/akknKa0v1BBkK5S48I2nU1/img.png|data-origin-width="972" data-origin-height="780" data-is-animation="false" style="width: 49.2793%; margin-right: 10px;" data-widthpercent="49.86",data-origin-width="980" data-origin-height="782" data-is-animation="false" style="width: 49.5579%;" data-widthpercent="50.14"|_##][##_ImageGrid|kage@GhKn3/btsNkIjoHfz/eq1ll1EIk0HAvZttTVXYE0/img.png,kage@lU3pJ/btsNiCqH1vY/huDNFPHWItbPXc69fHOs9k/img.png|data-origin-width="973" data-origin-height="783" data-is-animation="false" style="width: 49.5191%; margin-right: 10px;" data-widthpercent="50.1",data-origin-width="974" data-origin-height="787" data-is-animation="false" style="width: 49.3181%;" data-widthpercent="49.9"|_##]

---

**✅ 게임리셋(초기설정)**

\- 시드, 평균단가,차트배열,보유주식 수, 차트객체삭제 등 **초기화 작업**

```
            function resetGame() {
                updateSeed = seed;
                seed = Math.round(seed);
                udRate = 1;
                shares = 0;
                averagePrice = 0;
                charts = [];
                document.querySelectorAll("#main span").forEach(chart => chart.remove());
                stockPrice = parseFloat(document.getElementById("stock").value);
                document.getElementById("seed").value = seed.toLocaleString();
                document.getElementById("myPrice").value = "0";
                document.getElementById("udRate").value = "0.00%";
                document.getElementById("myRate").value = "0.00%";
                document.getElementById("stockPrice").value = Math.round(stockPrice).toLocaleString();
            }
```

---

> **다음에는 은행 API를 사용하여 작업 해볼 예정이다.**

#### **☑️ 전체코드**

```
<!DOCTYPE html>
<html lang="ko">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>코인 시뮬레이터 게임</title>
        <style>
            @font-face {font-family: 'GmarketSansMedium'; src: url('https://fastly.jsdelivr.net/gh/projectnoonnu/noonfonts_2001@1.1/GmarketSansMedium.woff') format('woff'); font-weight: normal;font-style: normal;}
            *{margin: 0; padding: 0; font-family: 'GmarketSansMedium'; font-weight: normal; font-size: 16px; color: #333; letter-spacing: -0.05rem; box-sizing: border-box;}
            #wrap{max-width: 1200px; width: 100%; margin: 0 auto; padding: 50px 0;}
            #main{position:relative; width: 100%; height: 800px; border: 2px solid #333; overflow: hidden; border-radius: 5px;   background-color: #fff; box-shadow: 0px 0px 8px 2px #e2e2e2;}
            #main .cnt{position: absolute; left: 20px; bottom: 20px; width: 100px; height: 30px; padding: 5px; border: 1px solid #bebebe;}
            #nav{width: 100%; height: 50px; display: flex; align-items: center; justify-content: center;  margin-top: 30px; gap: 10px;}
            #nav #stock{width: 150px; height: 100%; padding: 5px;border: 1px solid #bebebe; cursor: pointer;}
            #nav .btn{width: 150px; height: 100%; border: 1px solid #333; background-color: #fff; cursor: pointer;}
            #nav .btn:nth-of-type(1):hover{background-color: #333; color: #fff;transition: 0.3s ease-in;}
            #nav .btn:nth-of-type(2):hover{background-color: #ff3131; border: 1px solid #ff3131; color: #fff;transition: 0.3s ease-in;}
            #nav .btn:nth-of-type(3):hover{background-color: #7575ff; border: 1px solid #7575ff; color: #fff;transition: 0.3s ease-in;}

            #boxList{width: 100%; height: 50px; display: flex; align-items: center; justify-content: space-between;  margin-top: 30px;}
            #boxList .box{height: 100%; display: flex; align-items: baseline; gap: 10px; vertical-align:middle;}
            #boxList .box input{width: 150px; height: 100%; padding: 15px; border: 1px solid #bebebe; border-radius: 50px;} 

            #main span{display: block;border-radius: 10px;}
            #main span::after{position: absolute; left: 0; top: 0; display: block; width: 2px; height: 120px; background-color: #333;} 
            #popup{ display: none; justify-content: center; align-items: center; flex-direction: column; width: 300px; height: 400px; position: absolute; top: 50%; left: 50%; transform: translate(-50%,-50%);; border-radius: 5px;  padding: 1rem; background:url(images/bk.gif) no-repeat center;} 
            #popup{font-size: 20px; text-align: center;}
       </style>
    </head>
    <body>
        <div id="wrap">
            <div id="main">
                <select class="cnt">
                    <option value="1">1턴</option>
                    <option value="10">10턴</option>
                    <option value="30">30턴</option>
                </select>
            </div>

            <div id="nav">
                <select name="stock" id="stock" onchange="stock();">
                    <option value="10000">GD코인</option>
                    <option value="50000">도지코인</option>
                    <option value="100000">비트코인</option>
                </select>

                <button class="btn" type="button" onclick="game();">시작</button>
                <button class="btn" type="button" onclick="buy();">매수</button>
                <button class="btn" type="button" onclick="sell();">매도</button> 
            </div>

            <div id="boxList">
                <div class="box"><span>시드 :</span><input type="text" id="seed" value="300000" readonly></div>
                <div class="box"><span>종목주가 :</span><input type="text" id="stockPrice" value="10000" readonly></div>
                <div class="box"><span>등락률 :</span><input type="text" id="udRate" value="0.00%" readonly></div>
                <div class="box"><span>평가금액 :</span><input type="text" id="myPrice"  readonly></div>
                <div class="box"><span>수익률 :</span><input type="text" id="myRate" value="0.00%" readonly></div>
            </div>
                
        </div>

        <div id="popup">
            <img id="rankImage" src="" alt="티어 이미지">
            <p>당신의 등급 : <span id="rank"></span></p>
        </div>

        <script>
            // 캔버스 크기 및 변수설정
            let canvasWidth = 1200;
            let canvasHeight = 800;
            let charts = []; 
            let interval;
            // 상승률 (기본값 1)
            let udRate = 1; 
            // 시드 (10만원) - toLocaleString: 천단위 , 형식
            let seed = parseFloat(document.getElementById("seed").value);
            let updateSeed = seed;
            // 주식 가격
            let stockPrice = parseFloat(document.getElementById("stock").value);
             // 보유 주식 수, 평균 매수가, 평가금액
            let shares = 0;           
            let averagePrice = 0; 
            let myPrice = 0;         
            document.getElementById("seed").value = seed.toLocaleString();
            document.getElementById("stockPrice").value = stockPrice.toLocaleString();
            document.getElementById("myPrice").value = myPrice.toLocaleString();
            
            //  투자 종목별 주가(onchange)
            function stock() {
                stockPrice = parseFloat(document.getElementById("stock").value);
                document.getElementById("stockPrice").value = stockPrice.toLocaleString();
            }

            // 상승/하락 (주가 변동)
            function getChangeAmount(chartY) {
                let isUp = Math.random() < 0.5; // 상승/하락(50%)
                let rate = Math.floor(Math.random() * 10) + 1; // 1~10%
                let change = rate * 10; // 등락률 * 10px 차트변동
                
                // isUp : true(상승) / false(하락)
                if (isUp) { 
                    chartY -= change;                    // 차트 y값  누적
                    udRate *= (1 + rate / 100);      //  등락률 누적
                    stockPrice *= (1 + rate / 100);//  종목주가 누적
                } else {
                    chartY += change;
                    udRate *= (1 - rate / 100);
                    stockPrice *= (1 - rate / 100);
                }
                // 평가금액  : 주식수 * 현재주가
                myPrice = shares * stockPrice;
                // 수익률 : ((현재 주가 - 평균단가 )/평균단가 ) * 100 
                let myRate = shares > 0 ? ((stockPrice - averagePrice) / averagePrice) * 100 : 0;
                // 등락률 표기 : (등락률 - 1) * 100  
                document.getElementById("udRate").value = ((udRate - 1) * 100).toFixed(2) + "%";
                document.getElementById("stockPrice").value = Math.round(stockPrice).toLocaleString();
                document.getElementById("myPrice").value = Math.round(myPrice).toLocaleString();
                document.getElementById("myRate").value = myRate.toFixed(2) + "%";
                //  캔버스 영역 제한
                return { newY: Math.max(0, Math.min(canvasHeight - 100, chartY)), isUp };
            }
        
            // 차트 생성
            function createChart(chartY) {
                let newChart = document.createElement("span");
                newChart.style.position = 'absolute';
                newChart.style.width = '20px';
                newChart.style.height = '100px';
                
                let x = charts.length * 40; 
                let { newY, isUp } = getChangeAmount(chartY);
                // 상승/하락 : 배경색 지정
                newChart.style.backgroundColor = isUp ? '#ff3131' : '#7575ff';
                // x +=40px, y: 등락률*10px (신규생성)
                newChart.style.transform = `translate(${x}px, ${newY}px)`;
                // #main 차트영역 안 추가
                document.querySelector("#main").appendChild(newChart);
                return newY;
            }
            
            //  게임 실행
            function game() {
                // 코인 선택 비활성화
                document.getElementById("stock").disabled = true; 
                // 5,10,30턴 만큼 실행
                let count = parseInt(document.querySelector(".cnt").value);
                let createdCount = 0;
                // 초기 첫 차트위치
                let chartY = charts.length > 0 ? charts[charts.length - 1] : 400;
                if (interval) clearInterval(interval); // 인터벌 루프방지

                // 0.1초 마다 신규 차트 생성 + 배열 저장 + y값 누적전달
                interval = setInterval(() => {
                    if (createdCount < count) {
                        chartY = createChart(chartY);
                        charts.push(chartY);
                        createdCount++;

                        myPrice = shares * stockPrice; 
                        // 청산 (시드 < 10000)
                        if (myPrice < 10000 && myPrice != 0 ) {
                            clearInterval(interval);
                            interval = null;
                            // 시작버튼 비활성화 
                            seed = 0;
                            document.getElementById("seed").value = "0";
                            document.getElementById("myPrice").value = "0";
                            document.getElementById("myRate").value = "0.00%";
                            alert("청산되었습니다. 게임을 다시 할 수 없습니다.");
                            return; 
                        }
                    }
                    // 30턴 (최대 턴)
                    if (charts.length == 30) {
                        clearInterval(interval);
                        interval = null;
                        // 30턴이 끝나면 자동 매도 
                        sell(); 
                    }
                }, 100);
            }

            // 매수
            function buy() {
                let fullShares = Math.floor(seed / stockPrice); // 인생한방 풀매수
                let totalCost = fullShares * stockPrice;
                // 매수불가(시드 < 현재주가)
                if (fullShares <= 0) {
                    alert("시드가 부족하여 매수가 불가능 합니다.\n현재 시드 : " + seed);
                    return;
                }
                // 평균 매수가 계산
                if (shares === 0) {
                     averagePrice = stockPrice;
                }
                shares += fullShares;
                seed -= totalCost;

                document.getElementById("seed").value = Math.round(seed).toLocaleString();
                document.getElementById("myPrice").value = Math.round(shares * stockPrice).toLocaleString();
            }
        
            // 매도
            function sell() {
                if (shares === 0) {
                    //  보유 주식이 없을 경우 수익률을 0%
                    document.getElementById("myRate").value = "0.00%";
                    endGame(0);
                    return;
                }
                let totalSellValue = shares * stockPrice;
                // 수익률  : ((현재주가-평균매수가)/평균매수가)*100
                let myRate = ((stockPrice - averagePrice) / averagePrice) * 100;
                seed += totalSellValue;
                 // 매도 후 초기화
                shares = 0;
                averagePrice = 0;
                document.getElementById("seed").value = Math.round(seed).toLocaleString();
                endGame(myRate);
            }

            //  게임종료
            function endGame(myRate) { 
                // 최종 수익금
                let profit = seed - updateSeed ;
                setTimeout(() => {
                    document.getElementById("stock").disabled = false; // 코인 선택 다시 가능
                    if (confirm("거래 종료!\n시드: " + Math.round(seed).toLocaleString() + "원\n코인 등락률: " + ((udRate - 1) * 100).toFixed(2) + "%\n내 수익률: " + myRate.toFixed(2) + "%\n수익: " + Math.round(profit).toLocaleString() + "원\n게임을 다시 하시겠습니까?\n취소 : 투자 티어를 확인 할 수 있습니다.")) {
                        resetGame();
                    } else {
                        document.getElementById("popup").style.display = "flex";
                        if (seed >= 1000000) {
                            document.getElementById("rank").innerHTML = "챌린저";
                            document.getElementById("rankImage").src = "images/icon_01.png";
                        } else if (seed >= 700000) {
                            document.getElementById("rank").innerHTML = "마스터";
                            document.getElementById("rankImage").src = "images/icon_02.png";
                        } else if (seed >= 500000) {
                            document.getElementById("rank").innerHTML = "플레티넘";
                            document.getElementById("rankImage").src = "images/icon_03.png";
                        } else{
                            document.getElementById("popup").style.background = "none";
                            document.getElementById("rankImage").src = "images/icon_05.png";
                            document.getElementById("rank").innerHTML = "코린이";
                        }
                      
                    }
                }, 100);
            }
        
            // 게임리셋(초기설정)
            function resetGame() {
                updateSeed = seed;
                seed = Math.round(seed);
                udRate = 1;
                shares = 0;
                averagePrice = 0;
                charts = [];
                document.querySelectorAll("#main span").forEach(chart => chart.remove());
                stockPrice = parseFloat(document.getElementById("stock").value);
                document.getElementById("seed").value = seed.toLocaleString();
                document.getElementById("myPrice").value = "0";
                document.getElementById("udRate").value = "0.00%";
                document.getElementById("myRate").value = "0.00%";
                document.getElementById("stockPrice").value = Math.round(stockPrice).toLocaleString();
            }
        </script>
    </body>
</html>
```
