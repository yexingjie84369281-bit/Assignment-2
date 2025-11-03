<html>
<head>
<title>CSIT128 Assignment 2</title>
<style>
              /* Part 2 */

            .partTwo table {
                margin-top: 50px;
                width: 50%;
                border-collapse: collapse;
            }
        
            .partTwo table tr td a {
                display: block;
                text-decoration: none;
            }
        
            .partTwo .digit {
                background-color: #dbebf3;
                font-weight: bold;
                padding: 15px;
            }
        
            .partTwo .score {
                font-weight: bold;
                background-color: #fff2cc;
                font-size: 120%;
            }
        
            .partTwo .heading {
                background-color: #f7caac;
                font-weight: bold;
                font-size: 190%;
                padding: 10px;
            }
        
            .partTwo .button {
                background-color: #99ff99;
            }
        
            .partTwo span.count {
                font-weight: bold;
                color: red;
                font-size: 150%;
            }
        
            .partTwo table tr.button td {
                padding: 10px;
            }
        
            .partTwo .start {
                margin-right: 95px
            }
        
            .partTwo .stop {
                margin-left: 95px
            }
        
            .partTwo .d1 {
                color: rgb(206, 128, 40);
                font-size: 150%;
            }
        
            .partTwo .d2 {
                color: blue;
                font-size: 150%;
            }
        
            .partTwo table tr td.d3 {
                color: #23621e;
                font-size: 150%;
            }

            #d1, #d2, #d3 {
                color: black;
                font-weight: bold;
            }
        
        </style>
    </head>

    <body>
</head>
      <body>
<!-- Part 1: Assignment 2 group member information -->
 <h1>Part 1 </h1>
<table border="1" width="100%" cellpadding="10">

  <tr>

    <td colspan="3" bgcolor="lightblue" align="center"><b>CSIT128: Assignment 2</b></td>
    <td colspan="3" bgcolor="lightblue" align="center"><b>Group G2T02</b></td>
  </tr>
     <td rowspan="4">
      Student Number / <br>
      Name / Email
    </td>

  <tr>
    <td>9754064</td>
    <td>Ye Xingjie</td>
    <td>ye84369281@gmail.com</td>
  </tr>

  <tr>
    <td>9906150</td>
    <td>Xia Yanxin</td>
    <td>sfms.sas.02@gmail.com</td>
  </tr>
      
</table>
  <!-- Part 2 -->
        <div class="partTwo">
            <table border="1">
                <th colspan="3" class="heading">Part 2: Game</th>
                
                <tr style="background-color: #ecc9ad;">
                    <td colspan="3" align="left">
                        <label style="font-weight: bold;">Name:</label>
                        <input type="text" id="playerName" style="width: 30%; margin-left: 20px;">
                    </td>
                </tr>
                
                
                <tr>
                    <td class="digit"><b>Your chosen character:</b></td>
            <td class="digit">
                <select id="option" disabled>
                    <option value=" " selected disabled>Choose</option>
                    <option value="B">B</option><option value="F">F</option>
                    <option value="$">$</option><option value="%">%</option>
                    <option value="3">3</option><option value="9">9</option>
                </select>
            </td>
            <td class="score">Current Score: <span class="count" id="count">0</span></td>
        </tr>
        <tr class="button">
            <td colspan="3" align="center">
                <input type="button" id="start" value="Start Game" onclick="start()" disabled>
                <input type="button" id="stop" value="Stop Game" onclick="stop()" disabled>
            </td>
        </tr>
        <tr>
            <td id="d1" class="d1"></td>
            <td id="d2" class="d2"></td>
            <td id="d3" class="d3"></td>
        </tr>
    </table>
    <tr>
        <td colspan="3" align="left">
            <div id="highscoreDisplay" style="font-size: 160%; color: red; font-weight: bold; padding: 10px;">
                Highest: 0 - None
            </div>
        </td>
    </tr>
    

<script>
    let timer;
    let highScore = 0;
    let highPlayer = "None";
    const characters = ["B", "F", "$", "%", "3", "9"];

    function checkName() {
        const name = document.getElementById("playerName").value.trim();
        const disabled = name === "";
        document.getElementById("option").disabled = disabled;
        document.getElementById("start").disabled = disabled;
        document.getElementById("stop").disabled = disabled;
    }

    document.addEventListener("DOMContentLoaded", () => {
        document.getElementById("playerName").addEventListener("input", checkName);
        checkName();
    });

    function random() {
        const selected = [];
        while (selected.length < 3) {
            const rand = characters[Math.floor(Math.random() * characters.length)];
            if (!selected.includes(rand)) selected.push(rand);
        }

        document.getElementById("d1").innerHTML = selected[0];
        document.getElementById("d2").innerHTML = selected[1];
        document.getElementById("d3").innerHTML = selected[2];

        document.getElementById("d1").style.backgroundColor = '#' + Math.floor(Math.random() * 16777215).toString(16);
        document.getElementById("d2").style.backgroundColor = '#' + Math.floor(Math.random() * 16777215).toString(16);
        document.getElementById("d3").style.backgroundColor = '#' + Math.floor(Math.random() * 16777215).toString(16);
    }

    function start() {
        document.getElementById("start").disabled = true;
        document.getElementById("option").disabled = true;
        document.getElementById("count").innerHTML = "0";

        document.getElementById("d1").setAttribute("onclick", "handleClick('d1')");
        document.getElementById("d2").setAttribute("onclick", "handleClick('d2')");
        document.getElementById("d3").setAttribute("onclick", "handleClick('d3')");

        random();
        timer = setInterval(random, 2000);
    }

    function stop() {
        clearInterval(timer);
        document.getElementById("start").disabled = false;
        document.getElementById("option").disabled = false;
        document.getElementById("d1").setAttribute("onclick", "");
        document.getElementById("d2").setAttribute("onclick", "");
        document.getElementById("d3").setAttribute("onclick", "");

        const current = parseInt(document.getElementById("count").innerHTML);
        const name = document.getElementById("playerName").value.trim();
        if (current > highScore) {
            highScore = current;
            highPlayer = name;
            if (document.getElementById("highscoreDisplay"))
                document.getElementById("highscoreDisplay").innerText = `Highest: ${highScore} ${highPlayer}`;
        }
    }

    function handleClick(cellId) {
        const charClicked = document.getElementById(cellId).innerHTML;
        const selected = document.getElementById("option").value;
        let count = parseInt(document.getElementById("count").innerHTML);

        if (charClicked === selected) {
            count += 4;
        } else {
            count -= 3;
        }
        document.getElementById("count").innerHTML = count;
    }
</script>

    </body>
</html>

