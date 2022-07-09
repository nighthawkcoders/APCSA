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
    Input scores, press tab to add new number:
    <div id="scores">
        <input onblur="calculate()" type="text" name="score" id="score0"/><br>
        <!-- javascript generated inputs -->
    </idv>
</div>

<script>
    const scoreContainer = document.getElementById("scores");

    function calculate(){
        // setup totals
        var total=0;
        // establish array for every input with name="score"
        var array = document.getElementsByName('score');
        for(var i = 0; i < array.length; i++){  // iterate through all matching input element
            if(parseInt(array[i].value))  // convert to int and 
                total += parseInt(array[i].value);  // running total on points
        }
        // calculate current total
        document.getElementById('total').value = total;
        document.getElementById('average').value = total / array.length;
        // make another input line
        var input = document.createElement("input");
        input.setAttribute('onblur', "calculate()");
        input.setAttribute('type', "text");
        input.setAttribute('name', "score");
        scoreContainer.appendChild(input);
    }
</script>