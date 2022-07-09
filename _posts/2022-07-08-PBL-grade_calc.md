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
    <form>
        <div class="form-group row">
            Total : <span id="total" class="label label-primary">0.0</span>
            Count : <span id="count" class="label label-primary">0.0</span>
            Average : <span id="average" class="label label-primary">0.0</span>
        </div>
        <div class="form-group row">
            Input scores, press tab to add new number:
            <div id="scores">
                <input onblur="calculator()" type="text" name="score" id="score0"/><br>
                <!-- javascript generated inputs -->
            </div>
        </div>
    </form>
</div>

<script>
    const scoresContainer = document.getElementById("scores");

    function newInputLine(index) {
        // Prepare new input line
        var input = document.createElement("input");  // input element
        var br = document.createElement("br");  // line break element
        // Setup input line attributes
        input.setAttribute('onblur', "calculator()");
        input.setAttribute('type', "text");
        input.setAttribute('name', "score");
        input.setAttribute('id', "score" + index);
        // Add input and line break to page
        scoresContainer.appendChild(input);
        scoresContainer.appendChild(br);
        // Set cursor focus to new element
        document.getElementById("score" + index).focus();
    }

    function calculator(){
        var total = 0;  // running total
        var array = document.getElementsByName('score'); // setup array of scores
        for(var i = 0; i < array.length; i++){  // iterate through all matching input element
            if(parseInt(array[i].value))  // convert to int and 
                total += parseInt(array[i].value);  // running total update
        }
        // setup totals
        document.getElementById('total').innerHTML = total;
        document.getElementById('count').innerHTML = array.length;
        document.getElementById('average').innerHTML = total / array.length;
        // make a new input line
        newInputLine(array.length);
    }

</script>