---
title: Grade Calculator
layout: default
description: Supports grade inputs and calculates average. 
permalink: /frontend/grade_calc
categories: [pbl]
tags: [javascript, input, onblur]
---

<div class="container bg-primary">
    <header class="pb-3 mb-4 border-bottom border-primary text-dark">
        <span class="fs-4">Grade Calculator</span>
    </header>
    Total   : <input type="number" name="total" id="total" readonly/>
    Average : <input type="number" name="average" id="average" readonly/>
    <br><br>
    Score1 : <input onblur="findTotal()" type="text" name="score" id="score1"/><br>
    Score2 : <input onblur="findTotal()" type="text" name="score" id="score2"/><br>
    Score3 : <input onblur="findTotal()" type="text" name="score" id="score3"/><br>
    Score4 : <input onblur="findTotal()" type="text" name="score" id="score4"/><br>
    Score5 : <input onblur="findTotal()" type="text" name="score" id="score5"/><br>
</div>

<script>
    function findTotal(){
        var array = document.getElementsByName('score');
        var total=0;
        var count = 0;
        for(var i=0;i<array.length;i++){
            if(parseInt(array[i].value))
                total += parseInt(array[i].value);
                count++;
        }
        document.getElementById('total').value = total;
        document.getElementById('average').value = average;
    }
</script>