<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Scientific Calculator</title>
  <style>
    body {
      background: #f2f2f2;
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }
    .calculator {
      width: 350px;
      background: #fff;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0px 0px 10px #aaa;
    }
    .display {
      width: 100%;
      height: 60px;
      font-size: 28px;
      text-align: right;
      padding-right: 10px;
      border: none;
      background: #eaeaea;
      border-radius: 5px;
      margin-bottom: 10px;
    }
    .keys {
      display: grid;
      grid-template-columns: repeat(6, 1fr);
      gap: 5px;
    }
    button {
      padding: 15px;
      font-size: 18px;
      border: none;
      background: #e6e6e6;
      border-radius: 5px;
      cursor: pointer;
      user-select: none;
    }
    button:hover {
      background: #d9d9d9;
    }
    button.operator {
      background: #ff9500;
      color: white;
    }
    button.function {
      background: #6c757d;
      color: white;
    }
    /* Span the "=" button to occupy two grid columns */
    button.equal {
      grid-column: span 2;
      background: #28a745;
      color: white;
    }
  </style>
</head>
<body>
  <div class="calculator">
    <input type="text" class="display" id="display" readonly>
    <div class="keys">
      <!-- First row: Scientific Functions and a couple operators -->
      <button class="function" data-value="sin(">sin</button>
      <button class="function" data-value="cos(">cos</button>
      <button class="function" data-value="tan(">tan</button>
      <button class="function" data-value="log(">log</button>
      <button class="operator" data-value="^">^</button>
      <button class="operator" data-value="⌫">⌫</button>

      <!-- Second row: 7, 8, 9, division, sqrt and ( -->
      <button data-value="7">7</button>
      <button data-value="8">8</button>
      <button data-value="9">9</button>
      <button class="operator" data-value="/">/</button>
      <button class="function" data-value="sqrt(">√</button>
      <button class="operator" data-value="(">(</button>

      <!-- Third row: 4, 5, 6, multiplication, ) and π -->
      <button data-value="4">4</button>
      <button data-value="5">5</button>
      <button data-value="6">6</button>
      <button class="operator" data-value="*">*</button>
      <button class="operator" data-value=")">)</button>
      <button class="operator" data-value="pi">π</button>

      <!-- Fourth row: 1, 2, 3, subtraction, comma (for potential multi-argument functions) and e -->
      <button data-value="1">1</button>
      <button data-value="2">2</button>
      <button data-value="3">3</button>
      <button class="operator" data-value="-">-</button>
      <button class="operator" data-value=",">,</button>
      <button class="operator" data-value="e">e</button>

      <!-- Fifth row: 0, dot, addition, Clear and Equal button spanning two columns -->
      <button data-value="0">0</button>
      <button data-value=".">.</button>
      <button class="operator" data-value="+">+</button>
      <button class="operator" data-value="C">C</button>
      <button class="equal" data-value="=">=</button>
    </div>
  </div>

  <script>
    const display = document.getElementById("display");
    const buttons = document.querySelectorAll("button");

    // Function to evaluate the mathematical expression in the display.
    function evaluateExpression(expr) {
      // Process custom tokens to JavaScript's Math functions and operators.
      // Convert exponentiation operator (^) to Math.pow() format.
      expr = expr.replace(/(\d+(\.\d+)?|\))\s*\^\s*(\d+(\.\d+)?|\()/g, "Math.pow($1,$3)");
      // Replace sqrt, sin, cos, tan, log with Math. equivalents.
      expr = expr.replace(/sqrt\(/g, "Math.sqrt(");
      expr = expr.replace(/sin\(/g, "Math.sin(");
      expr = expr.replace(/cos\(/g, "Math.cos(");
      expr = expr.replace(/tan\(/g, "Math.tan(");
      // Use base-10 logarithm for log
      expr = expr.replace(/log\(/g, "Math.log10(");
      // Replace constants: pi and e.
      expr = expr.replace(/pi/g, "Math.PI");
      expr = expr.replace(/e/g, "Math.E");
      // Return evaluated result.
      try {
        // Use Function constructor to safely evaluate the expression.
        return Function('"use strict"; return (' + expr + ')')();
      } catch (error) {
        return "Error";
      }
    }

    buttons.forEach(button => {
      button.addEventListener("click", () => {
        let value = button.getAttribute("data-value");
        if (value === "C") {
          display.value = "";
        } else if (value === "⌫") {
          display.value = display.value.slice(0, -1);
        } else if (value === "=") {
          let expr = display.value;
          let result = evaluateExpression(expr);
          display.value = result;
        } else {
          display.value += value;
        }
      });
    });
  </script>
</body>
</html>